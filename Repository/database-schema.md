#  سند طرح دیتابیس - ورکینو

| | |
|---|---|
| **نام پروژه** | ورکینو (workino) |
| **نسخهٔ سند** | 1.0|
| **تاریخ** | 1405 |
| **تهیه‌کننده** | تیم ورکینو |
| **وضعیت** | پیش‌نویس |

---

## ۱. مقدمه

### ۱.۱ هدف سند
این سند طرح دیتابیس پروژهٔ ورکینو را تعریف می‌کند.
شامل جداول، روابط، ایندکس‌ها و تصمیم‌های طراحی.

### ۱.۲ دامنه
- **در دامنه:** ساختار جداول، روابط، ایندکس‌ها.
- **خارج از دامنه:** Queryهای خاص، Optimization.

---

## ۲. نمای کلی

### ۲.۱ DBMS انتخاب‌شده
**PostgreSQL 16**

**دلیل:**
- Relational و قدرتمند
- پشتیبانی از JSON
- رایگان و اوپن‌سورس
- جامعهٔ بزرگ

### ۲.۲ ERD — نمای کلی

┌─────────────┐       ┌─────────────┐
│   users     │       │  customers  │
│  (admin)    │       │             │
└─────────────┘       └──────┬──────┘
                             │
                             │ 1:N
                             ▼
┌─────────────┐       ┌─────────────┐
│ categories  │──1:N──│   orders    │
└──────┬──────┘       └──────┬──────┘
       │                     │
       │ 1:N                 │ 1:N
       ▼                     ▼
┌─────────────┐       ┌─────────────┐
│ services │──N:M──│selected_services│
└─────────────┘       └─────────────┘

---

## ۳. جداول

### ۳.۱ جدول `users` (ادمین‌ها)

| ستون | نوع | توضیح |
|------|------|-------|
| id | SERIAL PRIMARY KEY | شناسه |
| email | VARCHAR(255) UNIQUE | ایمیل |
| password_hash | VARCHAR(255) | رمزنگاری‌شده |
| name | VARCHAR(100) | نام |
| role | VARCHAR(20) | admin / warehouse |
| created_at | TIMESTAMP | تاریخ ایجاد |

### ۳.۲ جدول `customers` (مشتریان)

| ستون | نوع | توضیح |
|------|------|-------|
| id | SERIAL PRIMARY KEY | شناسه |
| name | VARCHAR(100) | نام |
| phone | VARCHAR(20) | تلفن |
| email | VARCHAR(255) | ایمیل |
| address | TEXT | آدرس |
| created_at | TIMESTAMP | تاریخ ایجاد |

### ۳.۳ جدول `categories` (دسته‌بندی‌ها)

| ستون | نوع | توضیح |
|------|------|-------|
| id | SERIAL PRIMARY KEY | شناسه |
| name | VARCHAR(100) | نام |
| slug | VARCHAR(100) UNIQUE | شناسهٔ URL |
| parent_id | INTEGER | دستهٔ والد (اختیاری) |

### ۳.۴ جدول `services` (خدمات)

| ستون | نوع | توضیح |
|------|------|-------|
| id | SERIAL PRIMARY KEY | شناسه |
| name | VARCHAR(255) | نام خدمات |
| description | TEXT | توضیحات |
| price | DECIMAL(12,0) | قیمت (ریال) |
| stock | INTEGER | موجودی |
| brand | VARCHAR(100) | برند |
| category_id | INTEGER FK | دسته‌بندی |
| image_url | VARCHAR(500) | تصویر |
| created_at | TIMESTAMP | تاریخ ایجاد |

### ۳.۵ جدول `orders` (سفارش‌ها)

| ستون | نوع | توضیح |
|------|------|-------|
| id | SERIAL PRIMARY KEY | شناسه |
| customer_id | INTEGER FK | مشتری |
| total_price | DECIMAL(12,0) | مبلغ کل |
| status | VARCHAR(20) | pending / shipped / delivered |
| address | TEXT | آدرس تحویل |
| created_at | TIMESTAMP | تاریخ ثبت |

### ۳.۶ جدول `selected_services` (خدمات انتخاب شده)

| ستون | نوع | توضیح |
|------|------|-------|
| id | SERIAL PRIMARY KEY | شناسه |
| order_id | INTEGER FK | سفارش |
| product_id | INTEGER FK | خدمات |
| quantity | INTEGER | تعداد |
| unit_price | DECIMAL(12,0) | قیمت واحد |

---

## ۴. روابط

### ۴.۱ یک‌به‌چند (1:N)

| رابطه | توضیح |
|--------|-------|
| categories → products | هر دسته چند سفارش دارد |
| customers → orders | هر مشتری چند سفارش دارد |
| orders → order_items | هر سفارش چند قلم دارد |

### ۴.۲ چند‌به‌چند (N:M)

| رابطه | توضیح |
|--------|-------|
| orders ↔ products | از طریق selected_services |

---

## ۵. ایندکس‌ها

### ۵.۱ Primary Keys
- همهٔ جداول: `id`

### ۵.۲ Foreign Keys
- `products.category_id` → `categories.id`
- `orders.customer_id` → `customers.id`
- `order_items.order_id` → `orders.id`
- `order_items.product_id` → `products.id`

### ۵.۳ ایندکس‌های اضافی

| جدول | ستون | دلیل |
|------|------|-------|
| products | name | جست‌وجو |
| products | category_id | فیلتر |
| orders | customer_id | پیگیری |
| orders | status | گزارش |

---

## ۶. نرمال‌سازی

### ۶.۱ 1NF — حذف گروه‌های تکراری
- هر ستون یک مقدار اتمی دارد
- هیچ ستون چندمقداری نیست

### ۶.۲ 2NF — حذف وابستگی جزئی
- همهٔ ستون‌های غیرکلیدی به کل Primary Key وابسته‌اند

### ۶.۳ 3NF — حذف وابستگی انتقالی
- هیچ ستون غیرکلیدی به ستون غیرکلیدی دیگر وابسته نیست

**نتیجه:** دیتابیس تا 3NF نرمال است.

---

## ۷. امنیت داده

### ۷.۱ رمزنگاری
- گذرواژه‌ها با **bcrypt**
- ارتباط با DB با **SSL**

### ۷.۲ Backup
- **روزانه** به S3 یا فضای ابری
- نگهداری ۳۰ روز

### ۷.۳ دسترسی
- کاربر DB با حداقل دسترسی
- لاگ همهٔ Queryهای حساس

---

## ۸. نمونهٔ Queryها

### ۸.۱ لیست خدمات یک دسته

```sql
SELECT p.id, p.name, p.price, p.stock
FROM services s
JOIN categories c ON s.category_id = s.id
WHERE c.slug = 'yakhchal';
SELECT o.id, o.total_price, o.status, o.created_at
FROM orders o
WHERE o.customer_id = 5
ORDER BY o.created_at DESC;
SELECT DATE(created_at) as date, COUNT(*) as orders, SUM(total_price) as 
revenue
FROM orders
WHERE status = 'delivered'
GROUP BY DATE(created_at)
ORDER BY date DESC;

---

نسخه	تاریخ	تغییرات	تهیه‌کننده
 1.0    1405    تیم ورکینو   نسخه اولیه
