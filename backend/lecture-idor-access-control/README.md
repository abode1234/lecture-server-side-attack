# محاضرة IDOR + Access Control Vulnerabilities

## 📋 نظرة عامة

هذه هي المحاضرة الرابعة من سلسلة أمان تطبيقات الويب، وتركز على ثغرات **IDOR (Insecure Direct Object References)** وثغرات **Access Control**. هذه الثغرات من أخطر الثغرات في تطبيقات الويب حيث تسمح للمهاجمين بالوصول إلى بيانات وموارد لا يحق لهم الوصول إليها.

## 🎯 أهداف التعلم

بنهاية هذه المحاضرة ستكون قادراً على:

- فهم آلية عمل ثغرات IDOR وأنواعها المختلفة
- التعرف على ثغرات Access Control وتصنيفاتها
- تطبيق تقنيات الاستغلال المختلفة عملياً
- حل 10 مختبرات من PortSwigger Academy
- تطبيق طرق الحماية الفعالة ضد هذه الثغرات

## 📚 المحتوى التعليمي

### 1. مقدمة
- تعريف IDOR وAccess Control
- أهمية هذه الثغرات في الأمان السيبراني
- الإحصائيات والتأثير على الشركات

### 2. أساسيات IDOR
- آلية عمل ثغرات IDOR
- المخاطر والتأثيرات الأمنية
- علامات وجود الثغرات

### 3. أنواع ثغرات IDOR
- **Horizontal IDOR**: الوصول لبيانات مستخدمين في نفس المستوى
- **Vertical IDOR**: رفع الصلاحيات والوصول لمستويات أعلى
- **Blind IDOR**: الاستغلال غير المباشر
- **Mass Assignment IDOR**: تعديل خصائص غير مسموحة
- **Wrapped IDOR**: التعامل مع المعرفات المشفرة

### 4. أساسيات Access Control
- مكونات أنظمة التحكم بالوصول
- نماذج Access Control المختلفة
- أفضل الممارسات

### 5. أنواع ثغرات Access Control
- Broken Authentication
- Missing Access Control
- Privilege Escalation

## 🧪 المختبرات العملية

### مختبرات IDOR (5 مختبرات):
1. **Unprotected admin functionality** - العثور على لوحة إدارة مخفية
2. **Unprotected admin functionality with unpredictable URL** - URL غير متوقع
3. **User role controlled by request parameter** - تحكم بالأدوار عبر المعاملات
4. **User role can be modified in user profile** - تعديل الأدوار في الملف الشخصي
5. **User ID controlled by request parameter** - التحكم بمعرف المستخدم

### مختبرات Access Control (5 مختبرات):
6. **User ID controlled by request parameter, with unpredictable user IDs** - معرفات غير متوقعة
7. **User ID controlled by request parameter with data leakage in redirect** - تسريب في إعادة التوجيه
8. **User ID controlled by request parameter with password disclosure** - كشف كلمات المرور
9. **Insecure direct object references** - مراجع مباشرة غير آمنة
10. **Multi-step process with no access control on one step** - عملية متعددة الخطوات

## 🛡️ طرق الحماية

### حماية IDOR:
- استخدام معرفات غير متتالية (UUID)
- التحقق من الصلاحيات لكل طلب
- استخدام مراجع غير مباشرة
- ربط الموارد بالجلسات

### حماية Access Control:
- تطبيق مبدأ أقل الصلاحيات
- المنع الافتراضي
- نظام صلاحيات مركزي
- المراجعة الدورية للصلاحيات

## 💻 أمثلة الكود

المحاضرة تتضمن أمثلة عملية بلغات مختلفة:
- PHP: تطبيق آمن للتحقق من الصلاحيات
- Node.js: Middleware للتحقق من الأدوار
- أمثلة على الاستغلال والحماية

## 🔗 روابط مفيدة

- [PortSwigger Web Security Academy - Access Control](https://portswigger.net/web-security/access-control)
- [OWASP Access Control Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Access_Control_Cheat_Sheet.html)
- [OWASP Top 10 - Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)

## ⏱️ مدة المحاضرة

- **المدة الإجمالية**: ساعتان
- **الجزء النظري**: 45 دقيقة
- **المختبرات العملية**: 75 دقيقة

## 📋 المتطلبات المسبقة

- فهم أساسي لبروتوكول HTTP
- معرفة بأساسيات تطبيقات الويب
- خبرة مع Burp Suite أو أدوات مشابهة
- إكمال المحاضرات السابقة في السلسلة

## 🏆 شهادة الإتمام

لكسب شهادة إتمام هذه المحاضرة:
1. مشاهدة المحاضرة كاملة
2. حل جميع المختبرات العملية (10/10)
3. اجتياز الاختبار النهائي بنسبة 80% أو أكثر

---

*هذه المحاضرة جزء من سلسلة شاملة لأمان تطبيقات الويب. للحصول على أقصى استفادة، يُنصح بإكمال جميع المحاضرات بالترتيب.* 