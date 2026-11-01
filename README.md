# Job Kingdom - مملكة الفرص والعمل

تطبيق توظيف حقيقي مبني بـ Flutter + Firebase.

## الميزات الموجودة حالياً (تعمل فعلياً)
- تسجيل دخول وإنشاء حساب آمن (Firebase Auth)
- نوعان من المستخدمين: باحث عن عمل / صاحب عمل
- عرض الوظائف مع فلترة حسب المحافظة
- **المطابقة الذكية (AI Matching)**: ترتيب الوظائف حسب تطابق المهارات
- **السيرة الذاتية بالفيديو (Video CV)**: رفع فيديو تعريفي
- **الخريطة الذكية (QR Map)**: QR Code لكل محافظة
- هيكل قاعدة البيانات جاهز للتوسع
- تصميم بألوان ملكية (أزرق ملكي + ذهبي)

## كيفية التشغيل

### 1. إعداد Firebase
1. اذهب إلى [Firebase Console](https://console.firebase.google.com)
2. أنشئ مشروع جديد باسم `job-kingdom`
3. أضف تطبيق Android و iOS
4. فعّل Authentication (Email/Password)
5. أنشئ قاعدة بيانات Firestore
6. فعّل Storage (مهم لرفع Video CV)
7. حمل ملف `google-services.json` (Android) و `GoogleService-Info.plist` (iOS)

### 2. تشغيل المشروع
```bash
cd job_kingdom
flutter pub get
flutter run
```

### 3. قواعد الأمان المقترحة لـ Firestore
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
    match /jobs/{jobId} {
      allow read: if true;
      allow create, update: if request.auth != null;
    }
    match /applications/{appId} {
      allow read, write: if request.auth != null;
    }
  }
}
```

### 4. قواعد Storage (لرفع الفيديو)
```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /video_cvs/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

## الخطوات التالية المقترحة
- إضافة شاشة تفاصيل الوظيفة والتقديم الفعلي
- إضافة شاشة إنشاء وظيفة لصاحب العمل
- نظام الإشعارات
- تحسين المطابقة الذكية باستخدام Cloud Functions
- دعم مسح QR Code من الكاميرا

## ملاحظات
هذا أساس قوي وقابل للتطوير. يمكن إضافة ذكاء اصطناعي أكثر تقدمًا لاحقًا عبر Cloud Functions بدون إعادة بناء التطبيق.
