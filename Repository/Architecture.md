# سند معماری — ورکینو

| | |
|---|---|
| **نام پروژه** | ورکینو (workino) |
| **نسخهٔ سند** | 1.0 |
| **تاریخ** | 1405 |
| **تهیه‌کننده** | تیم ورکینو |
| **وضعیت** | پیش‌نویس |

---

## ۱. مقدمه

### ۱.۱ هدف سند
این سند معماری نرم‌افزار پروژهٔ ورکینو را تعریف می‌کند.
هدف آن ایجاد یک درک مشترک از ساختار سیستم، تصمیم‌های کلیدی و
محدودیت‌ها است.

### ۱.۲ دامنه
- **در دامنه:** معماری سطح بالا، لایه‌ها، Endpointها، امنیت.
- **خارج از دامنه:** جزئیات پیاده‌سازی، کد.

### ۱.۳ ذی‌نفعان
| ذی‌نفع | نقش |
|--------|------|
| تیم توسعه | پیاده‌سازی |
| استاد درس | ارزیابی |
| مشتری فرضی | تأیید |

---

## ۲. نمای کلی معماری

### ۲.۱ سبک معماری
**Monolith + Layered Architecture**

- یک برنامهٔ واحد
- سه لایهٔ اصلی: Presentation, Business, Data
- مناسب تیم کوچک (۳-۴ نفر)
- ساده برای شروع، قابل گسترش

### ۲.۲ دیاگرام لایه‌ها
┌─────────────────────────────────────────┐
│ Presentation Layer │
│ React (SPA) │
└─────────────────┬───────────────────────┘
│ HTTP/REST
┌─────────────────▼───────────────────────┐
│ Business Layer │
│ Node.js + Express │
└─────────────────┬───────────────────────┘
│ SQL
┌─────────────────▼───────────────────────┐
│ Data Layer │
│ PostgreSQL │
└─────────────────────────────────────────┘
### ۲.۳ جریان داده

---

## ۳. لایه‌ها

### ۳.۱ Presentation Layer (Frontend)

| مورد | انتخاب |
|------|--------|
| فناوری | React 18 |
| Routing | React Router |
| State | Redux Toolkit |
| UI | Tailwind CSS |
| Build | Vite |

### ۳.۲ Business Layer (Backend)

| مورد | انتخاب |
|------|--------|
| فناوری | Node.js 20 + Express 4 |
| ORM | Prisma |
| Validation | Zod |
| Auth | JWT |
| Logging | Winston |

### ۳.۳ Data Layer (Database)

| مورد | انتخاب |
|------|--------|
| فناوری | PostgreSQL 16 |
| Migration | Prisma Migrate |
| Backup | روزانه |
| Hosting | Railway |

---

## ۴. تصمیم‌های معماری (ADR)

### ADR-01: چرا Monolith؟

**تصمیم:** معماری Monolith

**دلیل:**
- تیم کوچک (۳-۴ نفر)
- پیچیدگی کم
- سرعت توسعه بالاتر
- استقرار ساده

### ADR-02: چرا React؟

**تصمیم:** React برای Frontend

**دلیل:**
- محبوب و مستند
- اکوسیستم بزرگ
- SPA برای UX بهتر

### ADR-03: چرا PostgreSQL؟

**تصمیم:** PostgreSQL برای Database

**دلیل:**
- Relational و قدرتمند
- پشتیبانی از JSON
- رایگان و اوپن‌سورس

---

## ۵. REST API

### ۵.۱ Endpointهای عمومی

| Method | Endpoint | توضیح |
|--------|----------|-------|
| GET | /api/products | لیست متخصصان |
| GET | /api/products/:id |جزئیات متخصصان |
| GET | /api/categories | لیست دسته‌بندی‌ها |
| GET | /api/products/search | جست‌وجو |

### ۵.۲ Endpointهای سفارش

| Method | Endpoint | توضیح |
|--------|----------|-------|
| POST | /api/orders | ثبت سفارش |
| GET | /api/orders/:id | جزئیات سفارش |
| GET | /api/orders/:id/status | وضعیت سفارش |

### ۵.۳ Endpointهای ادمین

| Method | Endpoint | توضیح |
|--------|----------|-------|
| POST | /api/admin/products | ثبت متخصص |
| PUT | /api/admin/products/:id | ویرایش متخصص |
| DELETE | /api/admin/products/:id |حذف متخصص |
| GET | /api/admin/orders | لیست سفارش‌ها |
| PUT | /api/admin/orders/:id/status | تغییر وضعیت |

### ۵.۴ نمونه Response

```json
{
  "id": 1,
  "name": "یخچال ساید بای ساید",
  "price": 45000000,
  "stock": 5,
  "brand": "سامسونگ"
}

---

## ۶. امنیت

### ۶.۱ احراز هویت
- **JWT** برای API
- **bcrypt** برای رمزنگاری گذرواژه
- **Refresh Token** برای تمدید

### ۶.۲ مجوزدهی
- **Role-Based Access Control (RBAC)**
- نقش‌ها: Customer, Admin, Warehouse

### ۶.۳ امنیت API
- **HTTPS** اجباری
- **Rate Limiting** (۱۰۰ درخواست در دقیقه)
- **CORS** محدود
- **Input Validation** با Zod

---

## ۷. مقیاس‌پذیری

### ۷.۱ محدودیت‌های فعلی
- Monolith → مقیاس‌پذیری محدود
- یک سرور → تک نقطهٔ شکست

### ۷.۲ مسیر رشد
1. فاز ۱ (MVP): Monolith + یک سرور
2. فاز ۲: Load Balancer + چند سرور
3. فاز ۳: Database Replication
4. فاز ۴: جدا کردن سرویس‌های پرمصرف

---

## ۸. ریسک‌ها و بدهی فنی

| ریسک | احتمال | اثر | راهکار |
|------|--------|-----|---------|
| رشد سریع کاربران | متوسط | بالا | Load Balancer |
| نفوذ امنیتی | کم | بالا | OWASP |
| از دست دادن داده | کم | بالا | Backup روزانه |
| بدهی فنی | بالا | متوسط | Refactor در Sprint 3 |

---

## ۹. فناوری‌ها

| لایه | فناوری |
|------|--------|
| Frontend | React 18 + Vite + Tailwind |
| Backend | Node.js 20 + Express 4 |
| Database | PostgreSQL 16 |
| ORM | Prisma |
| Auth | JWT + bcrypt |
| Testing | Jest + Supertest |
| CI/CD | GitHub Actions |
| Deploy | Railway |

---

## پیوست الف — تاریخچهٔ تغییرات

| نسخه | تاریخ | تغییرات | تهیه‌کننده |
|-------|-------|----------|--------------|
| ۱.۰ | ۱۴۰۴ | نسخهٔ اولیه | ابراهیم فضلی |
