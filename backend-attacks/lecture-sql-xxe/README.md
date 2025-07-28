# المحاضرة الأولى: SQL Injection + XXE Injection

## 📋 معلومات المحاضرة

- **العنوان:** SQL Injection + XXE Injection
- **المدة:** ساعتان
- **التاريخ:** ديسمبر 2024
- **المختبرات:** 6 مختبرات من PortSwigger
- **المستوى:** متوسط إلى متقدم

## 🎯 الأهداف التعليمية

بنهاية هذه المحاضرة، ستكون قادراً على:

1. **فهم SQL Injection:**
   - تحديد نقاط الضعف في التطبيقات
   - تنفيذ هجمات Error-Based SQLi
   - استخدام Union-Based SQLi لاستخراج البيانات
   - تطبيق تقنيات Blind SQLi

2. **فهم XXE Injection:**
   - التعامل مع ملفات XML بشكل آمن
   - استغلال External Entities لقراءة الملفات
   - تنفيذ SSRF عبر XXE
   - فهم مخاطر XML Bomb

3. **طرق الحماية:**
   - تطبيق Prepared Statements
   - تعطيل External Entities
   - استخدام مكتبات آمنة

## 📚 المحتوى

### الجزء الأول: SQL Injection (60 دقيقة)
1. **المفاهيم الأساسية** (15 دقيقة)
   - تعريف SQL Injection
   - أسباب حدوث الثغرة
   - أمثلة على الكود الضعيف

2. **أنواع SQL Injection** (30 دقيقة)
   - Error-Based SQLi
   - Union-Based SQLi  
   - Boolean-Based Blind SQLi

3. **المختبرات العملية** (15 دقيقة)
   - 3 مختبرات من PortSwigger

### الجزء الثاني: XXE Injection (60 دقيقة)
1. **مقدمة في XML وXXE** (15 دقيقة)
   - أساسيات XML
   - External Entities
   - مخاطر معالجة XML

2. **أنواع هجمات XXE** (30 دقيقة)
   - File Disclosure
   - SSRF via XXE
   - XML Bomb (DoS)
   - Out-of-Band XXE

3. **المختبرات العملية** (15 دقيقة)
   - 3 مختبرات من PortSwigger

## 🧪 المختبرات العملية

### SQL Injection Labs:
1. **Lab 1:** [SQL injection vulnerability in WHERE clause](https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data)
2. **Lab 2:** [SQL injection UNION attack](https://portswigger.net/web-security/sql-injection/union-attacks/lab-determine-number-of-columns)
3. **Lab 3:** [Blind SQL injection with conditional responses](https://portswigger.net/web-security/sql-injection/blind/lab-conditional-responses)

### XXE Injection Labs:
4. **Lab 4:** [Exploiting XXE using external entities](https://portswigger.net/web-security/xxe/lab-exploiting-xxe-to-retrieve-files)
5. **Lab 5:** [Exploiting XXE to perform SSRF attacks](https://portswigger.net/web-security/xxe/lab-exploiting-xxe-to-perform-ssrf)
6. **Lab 6:** [Blind XXE with out-of-band interaction](https://portswigger.net/web-security/xxe/blind/lab-xxe-with-out-of-band-interaction)

## 🛠️ الأدوات المطلوبة

- **Burp Suite Community Edition** (مجاني)
- **متصفح ويب** (Firefox/Chrome)
- **حساب PortSwigger Academy** (مجاني)
- **محرر نصوص** (اختياري)

## 📖 المراجع والمصادر

- [OWASP SQL Injection](https://owasp.org/www-community/attacks/SQL_Injection)
- [OWASP XXE](https://owasp.org/www-community/vulnerabilities/XML_External_Entity_(XXE)_Processing)
- [PortSwigger Web Security Academy](https://portswigger.net/web-security)
- [CWE-89: SQL Injection](https://cwe.mitre.org/data/definitions/89.html)
- [CWE-611: XXE](https://cwe.mitre.org/data/definitions/611.html)

## 🎓 التقييم

- **المشاركة في المختبرات:** 60%
- **فهم المفاهيم:** 25%
- **تطبيق طرق الحماية:** 15%

## 📝 ملاحظات للمحاضر

- تأكد من وجود اتصال إنترنت مستقر
- اطلب من الطلاب تسجيل حسابات PortSwigger مسبقاً
- احرص على التطبيق العملي أكثر من النظري
- اترك وقت للأسئلة في نهاية كل قسم

## 🔗 الروابط المفيدة

- **الصفحة الرئيسية:** [../index.html](../index.html)
- **المحاضرة التفاعلية:** [lecture-clean.html](lecture-clean.html)
- **GitHub Repository:** [https://github.com/abode1234/lecture-server-side-attack](https://github.com/abode1234/lecture-server-side-attack)

---

*تم إعداد هذه المحاضرة بعناية لتوفير تجربة تعليمية شاملة وعملية في مجال أمان تطبيقات الويب.* 