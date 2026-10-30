# 🏪 Bazarna — منصة تجارة إلكترونية متعددة التجار

منصة تجارة إلكترونية مبنية بـ **Django** تدعم التجار والعملاء والمشرفين، مع محافظ رقمية، بوابات دفع، إشعارات فورية، ودعم متعدد اللغات.

![Python](https://img.shields.io/badge/Python-3.12-blue)
![Django](https://img.shields.io/badge/Django-6.0-092E20)
![DRF](https://img.shields.io/badge/DRF-3.16-red)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-supported-336791)

---

# 🇬🇧 English

## Overview

**Bazarna** is a multi-vendor e-commerce platform built with **Django**. It supports customers, merchants and supervisors, with digital wallets, payment gateways, push notifications and multi-language content.

### Features

- **Three dashboards**: customer storefront, merchant panel (`/merchant/`), supervisor panel (`/super/`), plus Django Admin.
- **Catalog**: categories, multiple images, sizes, offers, banners, promo popups.
- **Orders**: cart, orders/items, per-merchant shipping rates by governorate, returns, delivery complaints.
- **Wallets**: per-user wallet, transactions, deposit/withdrawal requests, merchant cashback, personal vouchers.
- **Payments**: **Paymob** (card / mobile wallet) and **Fawaterk**.
- **Users**: custom user model, custom roles, addresses, countries, referral rewards.
- **Auth**: email/password + Google via `django-allauth`; JWT for the API.
- **Notifications**: Firebase Cloud Messaging with notification logs.
- **Support**: ticketing system with messages.
- **Reviews & favorites**, and multi-language content via `django-modeltranslation`.

### Tech Stack

| Layer | Technology |
|---|---|
| Backend | Django 6.0.2, Django REST Framework, SimpleJWT |
| Auth | django-allauth (Google OAuth) |
| Database | PostgreSQL (production) via `dj-database-url` / SQLite (development) |
| Static files | WhiteNoise |
| Server | Gunicorn |
| Push | Firebase Admin SDK |
| Other | django-tinymce, django-cors-headers, Pillow |

### Project Structure

```
bazar/
├── bazarna/          # Project settings (settings, urls, wsgi, asgi)
├── accounts/         # Users, roles, addresses, referrals, FCM tokens
├── store/            # Products, orders, wallets, offers, payments
├── merchant_panel/   # Merchant dashboard
├── supervisor/       # Supervisor dashboard
├── support/          # Support tickets
├── templates/        # HTML templates
├── locale/           # Translation files
├── manage.py
├── requirements.txt
├── Procfile          # web: gunicorn bazarna.wsgi
└── runtime.txt       # python-3.12.8
```

### Getting Started

```bash
git clone https://github.com/swe-abdelrahman/bazar.git
cd bazar

python -m venv venv
# Windows
venv\Scripts\activate
# Linux / macOS
source venv/bin/activate

pip install -r requirements.txt
```

Create a `.env` file (see below), then:

```bash
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

- App: http://localhost:8000
- Admin: http://localhost:8000/admin/

> Firebase initialization requires a `firebase-key.json` service-account file in the project root.

### Environment Variables

| Variable | Description |
|---|---|
| `SECRET_KEY` | Django secret key (required, unique and long) |
| `DEBUG` | Must be `False` in production |
| `ALLOWED_HOSTS` | Comma-separated list of allowed domains |
| `DATABASE_URL` | e.g. `postgres://USER:PASSWORD@HOST:PORT/DB_NAME` |
| `GOOGLE_CLIENT_ID` / `GOOGLE_CLIENT_SECRET` | Google sign-in |
| `PAYMOB_API_KEY` | Paymob API key |
| `PAYMOB_INTEGRATION_ID_CARD` | Card integration ID |
| `PAYMOB_INTEGRATION_ID_WALLET` | Mobile-wallet integration ID |
| `PAYMOB_IFRAME_ID` | Paymob iframe ID |
| `FAWATERK_API_KEY` | Fawaterk API key |

### Production Deployment

Ready for platforms that support a `Procfile` (Heroku / Railway / Render):

```bash
pip install -r requirements.txt
python manage.py migrate --noinput
python manage.py collectstatic --noinput
gunicorn bazarna.wsgi --log-file -
```

Checklist:

- [ ] `DEBUG=False`
- [ ] Fresh, secret `SECRET_KEY`
- [ ] `ALLOWED_HOSTS` set to your real domain
- [ ] `DATABASE_URL` points to PostgreSQL
- [ ] Fawaterk success/webhook URLs point to the production domain
- [ ] `firebase-key.json` provided as a secret file, not via Git
- [ ] `media/` on persistent storage (S3 / GCS); cloud filesystems are ephemeral
- [ ] HTTPS enforced (`SECURE_SSL_REDIRECT`) behind the proxy

### Security — Before Going Live

> ⚠️ **Issues found in the current repository that must be fixed before a public launch:**

1. **Sensitive files are tracked by Git**: `.env`, `firebase-key.json` and `db.sqlite3`, despite being listed in `.gitignore`.
   ```bash
   git rm --cached .env firebase-key.json db.sqlite3
   git commit -m "Stop tracking secrets and local db"
   ```
   Then **rotate every exposed secret** (Firebase, Paymob, Google, Fawaterk, `SECRET_KEY`) since they remain in Git history.
2. **`FAWATERK_API_KEY` and ngrok URLs are hardcoded** in `settings.py` — move them to environment variables.
3. **`CORS_ALLOW_ALL_ORIGINS = True`** — restrict to your own origins in production.
4. **`SECURE_SSL_REDIRECT = False`** — enable it in production along with `SESSION_COOKIE_SECURE` and `CSRF_COOKIE_SECURE`.
5. Use PostgreSQL instead of SQLite in production.

### Internationalization

```bash
python manage.py makemessages -a
python manage.py compilemessages
python manage.py update_translation_fields
```

### Contributing

1. Fork the repository
2. Create a branch: `git checkout -b feature/my-feature`
3. Commit: `git commit -m "Add my feature"`
4. Push and open a Pull Request

### License

No license specified yet — add a `LICENSE` file as needed.

---

# 🇪🇬 العربية

## 📋 المحتويات

1. [المميزات](#-المميزات)
2. [التقنيات المستخدمة](#-التقنيات-المستخدمة)
3. [هيكل المشروع](#-هيكل-المشروع)
4. [التشغيل محلياً](#-التشغيل-محلياً)
5. [متغيرات البيئة](#-متغيرات-البيئة)
6. [النشر على الإنتاج](#-النشر-على-الإنتاج)
7. [الأمان — قبل الإطلاق](#-الأمان--قبل-الإطلاق)
8. [الترجمة](#-الترجمة-i18n)
9. [المساهمة](#-المساهمة)

---

## ✨ المميزات

- **ثلاث لوحات تحكم**: العميل (المتجر)، التاجر (`/merchant/`)، المشرف (`/super/`) + لوحة Django Admin.
- **إدارة المنتجات**: تصنيفات، صور متعددة، مقاسات، عروض (Offers)، بانرات، نوافذ ترويجية.
- **الطلبات**: سلة، طلبات وعناصر طلبات، أسعار شحن لكل تاجر حسب المحافظة، طلبات إرجاع، شكاوى توصيل.
- **المحافظ الرقمية**: محفظة لكل مستخدم، معاملات، طلبات إيداع وسحب، كاش باك للتاجر، قسائم شخصية.
- **المدفوعات**: تكامل **Paymob** (بطاقة / محفظة) و **Fawaterk**.
- **المستخدمون**: نموذج مستخدم مخصص، أدوار مخصصة (`CustomRole`)، عناوين، دول، نظام إحالات ومكافآت.
- **تسجيل الدخول**: بريد/كلمة مرور + Google عبر `django-allauth`، و JWT للـ API.
- **الإشعارات**: Firebase Cloud Messaging (Push) مع سجل إشعارات.
- **الدعم الفني**: تذاكر ورسائل (`support`).
- **التقييمات والمفضلة** للمنتجات.
- **متعدد اللغات** عبر `django-modeltranslation`.

## 🧰 التقنيات المستخدمة

| الطبقة | التقنية |
|---|---|
| Backend | Django 6.0.2, Django REST Framework, SimpleJWT |
| Auth | django-allauth (Google OAuth) |
| قاعدة البيانات | PostgreSQL (إنتاج) عبر `dj-database-url` / SQLite (تطوير) |
| Static | WhiteNoise |
| Server | Gunicorn |
| إشعارات | Firebase Admin SDK |
| محرر نصوص | django-tinymce |
| أخرى | django-cors-headers, Pillow |

## 📁 هيكل المشروع

```
bazar/
├── bazarna/          # إعدادات المشروع (settings, urls, wsgi, asgi)
├── accounts/         # المستخدمون، الأدوار، العناوين، الإحالات، FCM
├── store/            # المنتجات، الطلبات، المحافظ، العروض، الدفع
├── merchant_panel/   # لوحة التاجر
├── supervisor/       # لوحة المشرف
├── support/          # تذاكر الدعم الفني
├── templates/        # قوالب HTML
├── locale/           # ملفات الترجمة
├── md/               # وثائق إضافية
├── manage.py
├── requirements.txt
├── Procfile          # web: gunicorn bazarna.wsgi
└── runtime.txt       # python-3.12.8
```

## 🚀 التشغيل محلياً

```bash
git clone https://github.com/swe-abdelrahman/bazar.git
cd bazar

python -m venv venv
# Windows
venv\Scripts\activate
# Linux / macOS
source venv/bin/activate

pip install -r requirements.txt
```

أنشئ ملف `.env` (انظر [متغيرات البيئة](#-متغيرات-البيئة)) ثم:

```bash
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

- الموقع: http://localhost:8000
- لوحة الإدارة: http://localhost:8000/admin/

> يتطلب تهيئة Firebase وجود ملف `firebase-key.json` (Service Account) في جذر المشروع.

## 🔐 متغيرات البيئة

| المتغير | الوصف |
|---|---|
| `SECRET_KEY` | مفتاح Django السري (إجباري، فريد وطويل) |
| `DEBUG` | `False` في الإنتاج |
| `ALLOWED_HOSTS` | النطاقات المسموحة مفصولة بفاصلة |
| `DATABASE_URL` | مثال: `postgres://USER:PASSWORD@HOST:PORT/DB_NAME` |
| `GOOGLE_CLIENT_ID` / `GOOGLE_CLIENT_SECRET` | تسجيل الدخول بجوجل |
| `PAYMOB_API_KEY` | مفتاح Paymob |
| `PAYMOB_INTEGRATION_ID_CARD` | معرّف تكامل البطاقات |
| `PAYMOB_INTEGRATION_ID_WALLET` | معرّف تكامل المحافظ |
| `PAYMOB_IFRAME_ID` | معرّف الـ iframe |
| `FAWATERK_API_KEY` | مفتاح Fawaterk |

## ☁️ النشر على الإنتاج

المشروع جاهز للنشر على منصات تدعم `Procfile` (Heroku / Railway / Render):

```bash
pip install -r requirements.txt
python manage.py migrate --noinput
python manage.py collectstatic --noinput
gunicorn bazarna.wsgi --log-file -
```

قائمة التحقق:

- [ ] `DEBUG=False`
- [ ] `SECRET_KEY` جديد وسري
- [ ] `ALLOWED_HOSTS` مضبوط على النطاق الفعلي
- [ ] `DATABASE_URL` يشير إلى PostgreSQL
- [ ] ضبط روابط Fawaterk (success / webhook) على نطاق الإنتاج
- [ ] رفع `firebase-key.json` بشكل آمن (Secret File) وليس عبر Git
- [ ] تخزين `media/` على خدمة دائمة (S3 / GCS) لأن أنظمة الملفات في المنصات السحابية مؤقتة
- [ ] تفعيل HTTPS (`SECURE_SSL_REDIRECT`) خلف الـ proxy

## 🛡️ الأمان — قبل الإطلاق

> ⚠️ **تنبيهات مهمة اكتُشفت في المستودع الحالي ويجب معالجتها قبل أي إطلاق عام:**

1. **ملفات حساسة مرفوعة على Git**: `.env` و `firebase-key.json` و `db.sqlite3` موجودة داخل المستودع رغم وجودها في `.gitignore`. 
   ```bash
   git rm --cached .env firebase-key.json db.sqlite3
   git commit -m "Stop tracking secrets and local db"
   ```
   ثم **غيّر (Rotate) كل المفاتيح** المكشوفة (Firebase, Paymob, Google, Fawaterk, SECRET_KEY) لأنها بقيت في تاريخ Git.
2. **`FAWATERK_API_KEY` وروابط ngrok مكتوبة مباشرة في `settings.py`** — انقلها إلى متغيرات البيئة.
3. **`CORS_ALLOW_ALL_ORIGINS = True`** — قيّدها بنطاقاتك فقط في الإنتاج.
4. **`SECURE_SSL_REDIRECT = False`** — فعّلها في الإنتاج مع `SESSION_COOKIE_SECURE` و `CSRF_COOKIE_SECURE`.
5. استخدم PostgreSQL بدلاً من SQLite في الإنتاج.

## 🌍 الترجمة (i18n)

```bash
python manage.py makemessages -a
python manage.py compilemessages
```

الملفات في مجلد `locale/`. ترجمة حقول الموديلات تتم عبر `django-modeltranslation`:

```bash
python manage.py update_translation_fields
```

## 🤝 المساهمة

1. Fork للمستودع
2. أنشئ فرعاً: `git checkout -b feature/my-feature`
3. Commit: `git commit -m "Add my feature"`
4. Push ثم افتح Pull Request

## 📄 الترخيص

لم يتم تحديد ترخيص بعد — أضف ملف `LICENSE` حسب احتياجك.
