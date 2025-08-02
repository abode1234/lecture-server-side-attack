# محاضرة SQL Injection + XXE Injection

## 📚 نظرة عامة

المحاضرة الأولى من سلسلة أمان تطبيقات الويب - الهجمات من جانب الخادم. تركز هذه المحاضرة على نوعين من أخطر الثغرات في تطبيقات الويب: SQL Injection و XXE Injection.

## 🎯 أهداف التعلم

بنهاية هذه المحاضرة، ستكون قادراً على:

- فهم آليات عمل هجمات SQL Injection وأنواعها المختلفة
- اكتشاف واستغلال ثغرات XXE Injection
- تطبيق تقنيات تجاوز الفلاتر والحماية
- تنفيذ هجمات متقدمة للحصول على بيانات حساسة
- تطبيق طرق الحماية الفعالة

## 📖 محتوى المحاضرة

### 💉 SQL Injection
- **الأساسيات:** مفهوم SQL Injection وأنواعه
- **التقنيات:**
  - Error-Based SQL Injection
  - Union-Based SQL Injection
  - Blind SQL Injection
  - Time-Based Blind SQL Injection
- **أدوات الاستغلال:** sqlmap, Burp Suite

### 🔄 XXE Injection
- **الأساسيات:** مفهوم XXE وآلية العمل
- **أنواع الاستغلال:**
  - File Disclosure
  - SSRF via XXE
  - XML Bomb (Billion Laughs)
  - Out-of-Band XXE
- **تقنيات متقدمة:** Blind XXE

## 🧪 المختبرات العملية

### SQL Injection Labs
1. **SQL injection vulnerability in WHERE clause** - استغلال أساسي
2. **SQL injection UNION attack** - استخراج البيانات
3. **Blind SQL injection with conditional responses** - Blind techniques

### XXE Labs
1. **Exploiting XXE using external entities** - File disclosure
2. **Exploiting XXE to perform SSRF attacks** - SSRF via XXE
3. **Blind XXE with out-of-band interaction** - Out-of-band techniques

## 🛡️ طرق الحماية

### حماية SQL Injection
- استخدام Prepared Statements
- Input validation وsanitization
- Principle of least privilege للقواعد
- Web Application Firewalls

### حماية XXE
- تعطيل external entities
- استخدام safe XML parsers
- Input validation للXML
- Network segmentation

## 🚀 كيفية الاستخدام

1. افتح المحاضرة: [lecture-clean.html](lecture-clean.html)
2. اتبع المحتوى خطوة بخطوة
3. طبق المختبرات العملية من PortSwigger
4. استخدم أدوات Burp Suite للتطبيق العملي

## ⏱️ مدة المحاضرة

- **النظري:** 45 دقيقة
- **العملي:** 75 دقيقة
- **المجموع:** ساعتان

## 🏷️ العلامات

`SQL Injection` `XXE` `Server-Side Attacks` `Database Security` `XML Security` `OWASP Top 10`

---

**تم إنشاؤها بـ ❤️ للمجتمع التقني العربي** 