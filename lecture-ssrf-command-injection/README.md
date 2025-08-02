# محاضرة SSRF + Command Injection

## 📚 نظرة عامة

المحاضرة الثانية من سلسلة أمان تطبيقات الويب - الهجمات من جانب الخادم. تركز هذه المحاضرة على نوعين خطيرين من الثغرات: Server-Side Request Forgery (SSRF) و Command Injection.

## 🎯 أهداف التعلم

بنهاية هذه المحاضرة، ستكون قادراً على:

- فهم آليات عمل هجمات SSRF وأنواعها المختلفة
- اكتشاف واستغلال ثغرات Command Injection
- تطبيق تقنيات تجاوز الفلاتر والحماية
- تنفيذ هجمات متقدمة على الخوادم السحابية
- تطبيق طرق الحماية الفعالة

## 📖 محتوى المحاضرة

### 🌐 SSRF (Server-Side Request Forgery)
- **الأساسيات:** مفهوم SSRF وآلية العمل
- **الأنواع:**
  - Regular SSRF
  - Blind SSRF
  - Cloud Metadata SSRF
- **التقنيات المتقدمة:**
  - تجاوز الفلاتر
  - استغلال AWS Metadata
  - Out-of-band Detection

### 💻 Command Injection
- **الأساسيات:** آلية تنفيذ الأوامر
- **الأنواع:**
  - In-band Command Injection
  - Blind Command Injection
- **تقنيات الاكتشاف:**
  - Time-based Detection
  - Out-of-band Detection
  - Output Redirection

## 🧪 المختبرات العملية

### SSRF Labs
1. **Basic SSRF against localhost** - الوصول للخوادم المحلية
2. **SSRF against backend systems** - مسح الشبكة الداخلية
3. **SSRF with blacklist filters** - تجاوز الفلاتر

### Command Injection Labs
1. **Simple OS command injection** - الاستغلال الأساسي
2. **Blind command injection with time delays** - Time-based detection
3. **Blind command injection with output redirection** - استخراج النتائج

## 🛡️ طرق الحماية

### حماية SSRF
- Whitelist المجالات المسموحة
- حظر Private IP ranges
- DNS resolution validation
- Network segmentation

### حماية Command Injection
- تجنب system calls مباشرة
- Input validation وfiltering
- استخدام parameterized commands
- Principle of least privilege

## 🚀 كيفية الاستخدام

1. افتح المحاضرة: [lecture-clean.html](lecture-clean.html)
2. اتبع المحتوى خطوة بخطوة
3. طبق المختبرات العملية من PortSwigger
4. استخدم أدوات Burp Suite للتطبيق العملي

## 📋 المتطلبات

- فهم أساسي لبروتوكولات HTTP
- معرفة بأساسيات أمان تطبيقات الويب
- حساب في PortSwigger Web Security Academy
- أدوات: Burp Suite أو OWASP ZAP

## 🔗 روابط مفيدة

- [PortSwigger SSRF Labs](https://portswigger.net/web-security/ssrf)
- [PortSwigger Command Injection Labs](https://portswigger.net/web-security/os-command-injection)
- [OWASP Command Injection](https://owasp.org/www-community/attacks/Command_Injection)
- [AWS Instance Metadata Service](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html)

## ⏱️ مدة المحاضرة

- **النظري:** 45 دقيقة
- **العملي:** 75 دقيقة
- **المجموع:** ساعتان

## 🏷️ العلامات

`SSRF` `Command Injection` `Server-Side Attacks` `Web Security` `Penetration Testing` `AWS` `Blind Attacks` `Filter Bypass`

---

**تم إنشاؤها بـ ❤️ للمجتمع التقني العربي** 