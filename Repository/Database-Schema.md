# سند طرح دیتابیس - ورکینو

| | |
|---|---|
| **نام پروژه** | ورکینو (Workino) |
| **نسخه سند** | 1.0 |
| **تاریخ** | 1405 |
| **تهیه‌کننده** | تیم ورکینو |
| **وضعیت** | پیش‌نویس |

---

## ۱. مقدمه

### ۱.۱ هدف سند

هدف این سند، تعریف ساختار دیتابیس پروژه ورکینو است.
این سند شامل جدول‌ها، ستون‌ها، کلیدهای اصلی و خارجی، روابط، ایندکس‌ها و تصمیم‌های طراحی دیتابیس است.

### ۱.۲ دامنه

- **در دامنه:** ساختار جدول‌ها، روابط، کلیدهای اصلی و خارجی، ایندکس‌ها و نرمال‌سازی.
- **خارج از دامنه:** پیاده‌سازی Queryهای پیچیده و بهینه‌سازی پیشرفته دیتابیس.

---

## ۲. نمای کلی

### ۲.۱ DBMS انتخاب‌شده

**PostgreSQL 16**

**دلایل انتخاب:**
- پشتیبانی از ساختار رابطه‌ای و ارتباط بین جدول‌ها
- پشتیبانی از انواع داده مختلف
- پایداری و قابلیت اطمینان
- رایگان و متن‌باز بودن
- پشتیبانی از کلیدهای خارجی و محدودیت‌های داده

### ۲.۲ موجودیت‌های اصلی

دیتابیس ورکینو شامل جدول‌های زیر است:

| جدول | توضیحات |
|---|---|
| customers | اطلاعات مشتریان |
| specialists | اطلاعات متخصصان |
| admins | اطلاعات مدیران سایت |
| service_categories | دسته‌بندی خدمات |
| specialist_categories | ارتباط متخصصان با دسته‌بندی‌ها |
| service_requests | درخواست‌های ثبت‌شده توسط مشتریان |
| quotes | پیشنهادهای قیمت متخصصان |
| reviews | امتیازها و نظرات مشتریان |
| availability_slots | زمان‌های در دسترس متخصصان |
| complaints | شکایت‌های ثبت‌شده |
| notifications | اعلان‌های ارسال‌شده به متخصصان |

### ۲.۳ روابط کلی

- هر مشتری می‌تواند چند درخواست خدمات ثبت کند.
- هر دسته‌بندی می‌تواند شامل چند درخواست خدمات باشد.
- هر درخواست می‌تواند چند پیشنهاد قیمت دریافت کند.
- هر متخصص می‌تواند چند پیشنهاد قیمت ارسال کند.
- هر متخصص می‌تواند در چند دسته‌بندی فعالیت داشته باشد.
- هر مشتری می‌تواند چند نظر و شکایت ثبت کند.
- هر متخصص می‌تواند چند نظر دریافت کند.
- هر درخواست می‌تواند حداکثر یک نظر داشته باشد.
- هر متخصص می‌تواند چند بازه زمانی در دسترس داشته باشد.
- هر درخواست می‌تواند چند شکایت و اعلان مرتبط داشته باشد.

---

## ۳. جداول

### ۳.۱ جدول `customers` (مشتریان)

| ستون | نوع | توضیح |
|---|---|---|
| id | SERIAL PRIMARY KEY | شناسه مشتری |
| full_name | VARCHAR(150) | نام و نام خانوادگی |
| email | VARCHAR(255) UNIQUE | ایمیل |
| phone | VARCHAR(20) | شماره تلفن |
| password_hash | VARCHAR(255) | رمز عبور هش‌شده |
| address | TEXT | آدرس |
| is_active | BOOLEAN | وضعیت فعال بودن حساب |
| created_at | TIMESTAMP | تاریخ ایجاد حساب |

### ۳.۲ جدول `specialists` (متخصصان)

| ستون | نوع | توضیح |
|---|---|---|
| id | SERIAL PRIMARY KEY | شناسه متخصص |
| full_name | VARCHAR(150) | نام و نام خانوادگی |
| email | VARCHAR(255) UNIQUE | ایمیل |
| phone | VARCHAR(20) | شماره تلفن |
| password_hash | VARCHAR(255) | رمز عبور هش‌شده |
| bio | TEXT | معرفی و توضیحات متخصص |
| skills | TEXT | مهارت‌های متخصص |
| service_area | VARCHAR(255) | محدوده ارائه خدمات |
| estimated_rate | DECIMAL(12,0) | اجرت تخمینی |
| is_active | BOOLEAN | وضعیت فعال بودن حساب |
| created_at | TIMESTAMP | تاریخ ایجاد حساب |

### ۳.۳ جدول `admins` (مدیران سایت)

| ستون | نوع | توضیح |
|---|---|---|
| id | SERIAL PRIMARY KEY | شناسه مدیر |
| full_name | VARCHAR(150) | نام و نام خانوادگی |
| email | VARCHAR(255) UNIQUE | ایمیل |
| password_hash | VARCHAR(255) | رمز عبور هش‌شده |
| is_active | BOOLEAN | وضعیت فعال بودن حساب |
| created_at | TIMESTAMP | تاریخ ایجاد حساب |

### ۳.۴ جدول `service_categories` (دسته‌بندی خدمات)

| ستون | نوع | توضیح |
|---|---|---|
| id | SERIAL PRIMARY KEY | شناسه دسته‌بندی |
| name | VARCHAR(100) | نام دسته‌بندی |
| description | TEXT | توضیحات دسته‌بندی |

### ۳.۵ جدول `specialist_categories` (ارتباط متخصصان و دسته‌بندی‌ها)

این جدول برای پیاده‌سازی رابطه چندبه‌چند بین متخصصان و دسته‌بندی خدمات استفاده می‌شود.

| ستون | نوع | توضیح |
|---|---|---|
| specialist_id | INTEGER FK | شناسه متخصص |
| category_id | INTEGER FK | شناسه دسته‌بندی |

**کلید اصلی مرکب:** `(specialist_id, category_id)`

### ۳.۶ جدول `service_requests` (درخواست‌های خدمات)

| ستون | نوع | توضیح |
|---|---|---|
| id | SERIAL PRIMARY KEY | شناسه درخواست |
| customer_id | INTEGER FK | شناسه مشتری |
| category_id | INTEGER FK | شناسه دسته‌بندی |
| description | TEXT | توضیحات درخواست |
| location | TEXT | محل انجام خدمات |
| budget | DECIMAL(12,0) | بودجه پیشنهادی مشتری |
| requested_at | TIMESTAMP | زمان ثبت درخواست |
| status | VARCHAR(20) | وضعیت درخواست |

**مقادیر مجاز status:**
- pending
- in_progress
- completed
- cancelled

### ۳.۷ جدول `quotes` (پیشنهادهای قیمت)

این جدول پیشنهادهای قیمت متخصصان برای درخواست‌های ثبت‌شده را نگهداری می‌کند.

| ستون | نوع | توضیح |
|---|---|---|
| id | SERIAL PRIMARY KEY | شناسه پیشنهاد |
| request_id | INTEGER FK | شناسه درخواست |
| specialist_id | INTEGER FK | شناسه متخصص |
| proposed_price | DECIMAL(12,0) | قیمت پیشنهادی |
| description | TEXT | توضیحات پیشنهاد |
| created_at | TIMESTAMP | زمان ثبت پیشنهاد |
| status | VARCHAR(20) | وضعیت پیشنهاد |

**مقادیر مجاز status:**
- pending
- accepted
- rejected

### ۳.۸ جدول `reviews` (نظرات و امتیازها)

| ستون | نوع | توضیح |
|---|---|---|
| id | SERIAL PRIMARY KEY | شناسه نظر |
| customer_id | INTEGER FK | شناسه مشتری |
| specialist_id | INTEGER FK | شناسه متخصص |
| request_id | INTEGER FK UNIQUE | شناسه درخواست |
| rating | SMALLINT | امتیاز مشتری |
| comment | TEXT | متن نظر |
| created_at | TIMESTAMP | زمان ثبت نظر |

### ۳.۹ جدول `availability_slots` (زمان‌های در دسترس)

| ستون | نوع | توضیح |
|---|---|---|
| id | SERIAL PRIMARY KEY | شناسه بازه زمانی |
| specialist_id | INTEGER FK | شناسه متخصص |
| start_time | TIMESTAMP | زمان شروع |
| end_time | TIMESTAMP | زمان پایان |
| is_available | BOOLEAN | وضعیت در دسترس بودن |

### ۳.۱۰ جدول `complaints` (شکایت‌ها)

| ستون | نوع | توضیح |
|---|---|---|
| id | SERIAL PRIMARY KEY | شناسه شکایت |
| customer_id | INTEGER FK | شناسه مشتری |
| specialist_id | INTEGER FK | شناسه متخصص |
| request_id | INTEGER FK | شناسه درخواست |
| description | TEXT | توضیحات شکایت |
| status | VARCHAR(20) | وضعیت شکایت |
| created_at | TIMESTAMP | زمان ثبت شکایت |

**مقادیر مجاز status:**
- pending
- reviewed
- resolved

### ۳.۱۱ جدول `notifications` (اعلان‌ها)

| ستون | نوع | توضیح |
|---|---|---|
| id | SERIAL PRIMARY KEY | شناسه اعلان |
| specialist_id | INTEGER FK | شناسه متخصص دریافت‌کننده |
| request_id | INTEGER FK | شناسه درخواست مرتبط |
| message | TEXT | متن اعلان |
| is_read | BOOLEAN | وضعیت خوانده شدن |
| created_at | TIMESTAMP | زمان ایجاد اعلان |

---

## ۴. روابط بین جدول‌ها

### ۴.۱ روابط یک‌به‌چند (1:N)

| رابطه | توضیح |
|---|---|
| customers → service_requests | هر مشتری می‌تواند چند درخواست ثبت کند. |
| service_categories → service_requests | هر دسته‌بندی می‌تواند شامل چند درخواست باشد. |
| service_requests → quotes | هر درخواست می‌تواند چند پیشنهاد قیمت دریافت کند. |
| specialists → quotes | هر متخصص می‌تواند چند پیشنهاد قیمت ارسال کند. |
| customers → reviews | هر مشتری می‌تواند چند نظر ثبت کند. |
| specialists → reviews | هر متخصص می‌تواند چند نظر دریافت کند. |
| specialists → availability_slots | هر متخصص می‌تواند چند بازه زمانی ثبت کند. |
| customers → complaints | هر مشتری می‌تواند چند شکایت ثبت کند. |
| specialists → complaints | هر متخصص می‌تواند چند شکایت دریافت کند. |
| service_requests → complaints | هر درخواست می‌تواند چند شکایت داشته باشد. |
| specialists → notifications | هر متخصص می‌تواند چند اعلان دریافت کند. |
| service_requests → notifications | هر درخواست می‌تواند چند اعلان مرتبط داشته باشد. |

### ۴.۲ روابط چندبه‌چند (N:M)

| رابطه | جدول واسط | توضیح |
|---|---|---|
| specialists ↔ service_categories | specialist_categories | هر متخصص می‌تواند در چند دسته‌بندی فعالیت کند و هر دسته‌بندی می‌تواند شامل چند متخصص باشد. |

### ۴.۳ روابط یک‌به‌یک یا یک‌به‌صفر و یک

| رابطه | توضیح |
|---|---|
| service_requests → reviews | هر درخواست می‌تواند حداکثر یک نظر داشته باشد. |

---

## ۵. کلیدهای اصلی و خارجی

### ۵.۱ کلیدهای اصلی (Primary Keys)

- جدول‌های customers، specialists، admins، service_categories، service_requests، quotes، reviews، availability_slots، complaints و notifications دارای ستون `id` به‌عنوان کلید اصلی هستند.
- جدول specialist_categories دارای کلید اصلی مرکب `(specialist_id, category_id)` است.

### ۵.۲ کلیدهای خارجی (Foreign Keys)

| جدول | ستون | جدول مرجع |
|---|---|---|
| specialist_categories | specialist_id | specialists.id |
| specialist_categories | category_id | service_categories.id |
| service_requests | customer_id | customers.id |
| service_requests | category_id | service_categories.id |
| quotes | request_id | service_requests.id |
| quotes | specialist_id | specialists.id |
| reviews | customer_id | customers.id |
| reviews | specialist_id | specialists.id |
| reviews | request_id | service_requests.id |
| availability_slots | specialist_id | specialists.id |
| complaints | customer_id | customers.id |
| complaints | specialist_id | specialists.id |
| complaints | request_id | service_requests.id |
| notifications | specialist_id | specialists.id |
| notifications | request_id | service_requests.id |

---

## ۶. ایندکس‌ها

### ۶.۱ ایندکس‌های اصلی

کلیدهای اصلی و ستون‌های دارای محدودیت UNIQUE به‌صورت خودکار ایندکس می‌شوند.

### ۶.۲ ایندکس‌های اضافی

| جدول | ستون | دلیل |
|---|---|---|
| customers | email | جست‌وجوی مشتری |
| specialists | email | جست‌وجوی متخصص |
| specialists | service_area | فیلتر بر اساس محدوده خدمات |
| service_requests | customer_id | مشاهده درخواست‌های مشتری |
| service_requests | category_id | فیلتر درخواست‌ها بر اساس دسته‌بندی |
| service_requests | status | فیلتر بر اساس وضعیت درخواست |
| quotes | request_id | مشاهده پیشنهادهای یک درخواست |
| quotes | specialist_id | مشاهده پیشنهادهای متخصص |
| reviews | specialist_id | مشاهده نظرات متخصص |
| complaints | status | بررسی شکایت‌ها |
| notifications | specialist_id | مشاهده اعلان‌های متخصص |

---

## ۷. نرمال‌سازی

### ۷.۱ فرم نرمال اول (1NF)

- هر ستون یک مقدار مشخص و اتمی دارد.
- اطلاعات چندمقداری در یک ستون ذخیره نمی‌شوند.
- برای ارتباط چندبه‌چند متخصصان و دسته‌بندی‌ها از جدول واسط استفاده شده است.

### ۷.۲ فرم نرمال دوم (2NF)

- تمام ستون‌های غیرکلیدی به کلید اصلی وابسته هستند.
- در جدول specialist_categories هیچ وابستگی جزئی به بخشی از کلید مرکب وجود ندارد.

### ۷.۳ فرم نرمال سوم (3NF)

- ستون‌های غیرکلیدی به ستون‌های غیرکلیدی دیگر وابستگی انتقالی ندارند.
- اطلاعات مشتریان، متخصصان، درخواست‌ها و پیشنهادهای قیمت در جدول‌های جداگانه نگهداری می‌شوند.

**نتیجه:** ساختار پیشنهادی دیتابیس با هدف رعایت اصول نرمال‌سازی تا فرم نرمال سوم (3NF) طراحی شده است.

---

## ۸. امنیت داده‌ها

### ۸.۱ رمز عبور

- رمزهای عبور به‌صورت هش‌شده ذخیره می‌شوند.
- از الگوریتم‌های امن هش رمز عبور مانند bcrypt استفاده می‌شود.
- رمز عبور خام در دیتابیس ذخیره نمی‌شود.

### ۸.۲ ارتباط با دیتابیس

- در محیط عملیاتی، ارتباط با دیتابیس باید از طریق اتصال امن برقرار شود.
- دسترسی کاربران دیتابیس باید متناسب با مسئولیت آن‌ها محدود شود.

### ۸.۳ پشتیبان‌گیری

- تهیه نسخه پشتیبان به‌صورت منظم توصیه می‌شود.
- نسخه‌های پشتیبان باید در محل امن نگهداری شوند.

### ۸.۴ کنترل دسترسی

- دسترسی به اطلاعات بر اساس نقش کاربران کنترل می‌شود.
- مدیر سایت امکان مدیریت کاربران و بررسی شکایت‌ها را دارد.
- مشتری و متخصص فقط به امکانات و اطلاعات مجاز خود دسترسی دارند.

---

## ۹. نمونه Queryها

### ۹.۱ مشاهده درخواست‌های یک مشتری

```sql
SELECT id, description, location, budget, status, requested_at
FROM service_requests
WHERE customer_id = 1
ORDER BY requested_at DESC;
