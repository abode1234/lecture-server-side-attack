# محاضرة File Upload + Path Traversal

## 📚 نظرة عامة

المحاضرة الثالثة من سلسلة أمان تطبيقات الويب - الهجمات من جانب الخادم. تركز هذه المحاضرة على نوعين خطيرين من الثغرات: File Upload Vulnerabilities و Path Traversal (Directory Traversal).

## 🎯 أهداف التعلم

بنهاية هذه المحاضرة، ستكون قادراً على:

- فهم آليات عمل ثغرات رفع الملفات وأنواعها المختلفة
- إتقان تقنيات Path Traversal للوصول للملفات الحساسة
- تطبيق تقنيات تجاوز الفلاتر والحماية
- تنفيذ Web Shells للسيطرة على الخوادم
- تطبيق طرق الحماية الفعالة ضد هذه الهجمات

## 📖 محتوى المحاضرة

### 📤 File Upload Vulnerabilities
- **الأساسيات:** مفهوم ثغرات رفع الملفات وآلية العمل
- **الأنواع:**
  - Unrestricted File Upload
  - Content-Type Bypass
  - Extension Blacklist Bypass
  - Path Traversal in Upload
  - Magic Bytes Bypass
  - Race Conditions
  - PUT Method Upload

### 📁 Path Traversal Attacks
- **الأساسيات:** آلية التنقل في المجلدات
- **التقنيات:**
  - Simple Path Traversal
  - Absolute Path Bypass
  - URL Encoding Bypass
  - Non-Recursive Filter Bypass
  - Start Path Validation Bypass
  - File Extension Validation Bypass

## 🧪 المختبرات العملية - 13 مختبر

### File Upload Labs (7 مختبرات)
1. **Remote code execution via web shell upload** - الاستغلال الأساسي
2. **Content-Type restriction bypass** - تجاوز فحص MIME type
3. **Web shell upload via path traversal** - استخدام Path Traversal
4. **Extension blacklist bypass** - تجاوز القائمة السوداء
5. **Obfuscated file extension** - استخدام Null Byte
6. **Race condition upload** - استغلال Race Conditions
7. **PUT method upload** - استخدام HTTP PUT

### Path Traversal Labs (6 مختبرات)
1. **Simple case** - الحالة الأساسية
2. **Absolute path bypass** - تجاوز بالمسار المطلق
3. **Start of path validation** - تجاوز فحص بداية المسار
4. **Non-recursive stripping** - تجاوز الحذف غير المتكرر
5. **Superfluous URL-decode** - Double URL encoding
6. **Null byte bypass** - تجاوز فحص الامتداد

## 🛡️ طرق الحماية

### حماية File Upload
- Whitelist للامتدادات المسموحة
- فحص MIME type وMagic Bytes
- إعادة تسمية الملفات
- تقييد صلاحيات التنفيذ
- استخدام مجلدات منفصلة

### حماية Path Traversal
- تنظيف المدخلات من traversal sequences
- استخدام Whitelist للملفات المسموحة
- التحقق من المسار باستخدام realpath()
- تقييد الوصول للمجلدات الحساسة

## 🔧 متطلبات المحاضرة

### الأدوات المطلوبة
- **Burp Suite** - لاعتراض وتعديل الطلبات
- **Browser** - للوصول للمختبرات
- **Text Editor** - لإنشاء Web Shells

### المعرفة المسبقة
- أساسيات HTTP وprotocols
- فهم بنية الملفات والمجلدات
- معرفة أساسية بـ PHP/scripting

## ⚡ نصائح للنجاح

1. **ابدأ بالأساسيات** - افهم آلية العمل قبل التقنيات المتقدمة
2. **مارس على كل مختبر** - التطبيق العملي أهم من النظرية
3. **اجمع التقنيات** - استخدم File Upload مع Path Traversal
4. **اختبر الفلاتر** - جرب طرق تجاوز متعددة
5. **راقب الاستجابات** - الأخطاء تحتوي على معلومات مفيدة

## 🚀 التطبيق العملي

### إعداد بيئة الاختبار
```bash
# إنشاء مجلد للتجارب
mkdir file-upload-labs
cd file-upload-labs

# إنشاء web shells بسيطة
echo '<?php echo system($_GET["cmd"]); ?>' > shell.php
echo '<?php echo file_get_contents($_GET["file"]); ?>' > reader.php
```

### أمثلة على Payloads
```bash
# File Upload Payloads
shell.php
shell.php%00.jpg
shell.php.jpg
../../../var/www/html/shell.php

# Path Traversal Payloads
../../../etc/passwd
/etc/passwd
....//....//etc/passwd
..%252f..%252fetc/passwd
```

## 📊 الإحصائيات

- **المدة:** ساعتان تدريبيتان
- **المختبرات:** 13 مختبر عملي من PortSwigger
- **التقنيات:** 12+ تقنية متقدمة
- **مستوى الصعوبة:** متوسط إلى متقدم

## 🔗 روابط مفيدة

- [PortSwigger File Upload Labs](https://portswigger.net/web-security/file-upload)
- [PortSwigger Path Traversal Labs](https://portswigger.net/web-security/file-path-traversal)
- [OWASP File Upload Security](https://owasp.org/www-community/vulnerabilities/Unrestricted_File_Upload)
- [Path Traversal Prevention Guide](https://owasp.org/www-community/attacks/Path_Traversal)

## 📝 ملاحظات إضافية

هذه المحاضرة تعتبر أساسية لأي متخصص في أمان التطبيقات. ثغرات File Upload وPath Traversal شائعة جداً في التطبيقات الحقيقية وتتيح للمهاجمين الحصول على صلاحيات عالية على الخوادم.

**تذكر:** الهدف من هذه المحاضرة تعليمي بحت لفهم الثغرات وطرق الحماية منها. استخدم هذه المعرفة لتأمين تطبيقاتك وليس لأغراض ضارة. 