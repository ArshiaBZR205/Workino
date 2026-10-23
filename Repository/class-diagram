
# Class Diagram — ورکینو

## ۱. مقدمه

این دیاگرام کلاس‌های اصلی سیستم ورکینو، ویژگی‌های آن‌ها و روابط بین آن‌ها را نشان می‌دهد.

## ۲. کلاس‌ها

### ۲.۱ Customer

| ویژگی | نوع |
|---|---|
| id | int |
| full_name | string |
| email | string |
| phone | string |
| password_hash | string |
| address | string |
| is_active | bool |

### ۲.۲ Specialist

| ویژگی | نوع |
|---|---|
| id | int |
| full_name | string |
| email | string |
| phone | string |
| password_hash | string |
| bio | string |
| skills | string |
| service_area | string |
| estimated_rate | decimal |
| is_active | bool |

### ۲.۳ Admin

| ویژگی | نوع |
|---|---|
| id | int |
| full_name | string |
| email | string |
| password_hash | string |
| is_active | bool |

### ۲.۴ ServiceCategory

| ویژگی | نوع |
|---|---|
| id | int |
| name | string |
| description | string |

### ۲.۵ ServiceRequest

| ویژگی | نوع |
|---|---|
| id | int |
| customer_id | int |
| specialist_id | int (nullable) |
| category_id | int |
| description | string |
| location | string |
| budget | decimal |
| requested_at | datetime |
| status | RequestStatus (enum) |

مقادیر مجاز وضعیت درخواست:

- `PENDING` — در انتظار پاسخ متخصص
- `ACCEPTED` — پذیرفته‌شده
- `REJECTED` — ردشده
- `COMPLETED` — تکمیل‌شده
- `CANCELLED` — لغوشده

### ۲.۶ Review

| ویژگی | نوع |
|---|---|
| id | int |
| customer_id | int |
| specialist_id | int |
| request_id | int |
| rating | int |
| comment | string |
| created_at | datetime |

### ۲.۷ AvailabilitySlot

| ویژگی | نوع |
|---|---|
| id | int |
| specialist_id | int |
| start_time | datetime |
| end_time | datetime |
| is_available | bool |

## ۳. روابط

- `Customer 1:N ServiceRequest` — هر مشتری می‌تواند چند درخواست ثبت کند؛ هر درخواست متعلق به یک مشتری است.
- `Specialist 1:N ServiceRequest` — هر متخصص می‌تواند چند درخواست دریافت کند؛ تخصیص متخصص به درخواست اختیاری است.
- `ServiceCategory 1:N ServiceRequest` — هر دسته‌بندی می‌تواند چند درخواست داشته باشد؛ هر درخواست متعلق به یک دسته‌بندی است.
- `Customer 1:N Review` — هر مشتری می‌تواند چند نظر ثبت کند؛ هر نظر توسط یک مشتری ثبت می‌شود.
- `Specialist 1:N Review` — هر متخصص می‌تواند چند نظر دریافت کند؛ هر نظر درباره‌ی یک متخصص است.
- `ServiceRequest 1:0..1 Review` — هر درخواست می‌تواند بدون نظر باشد یا حداکثر یک نظر داشته باشد.
- `Specialist 1:N AvailabilitySlot` — هر متخصص می‌تواند چند بازه‌ی زمانی داشته باشد؛ هر بازه متعلق به یک متخصص است.
- `Specialist N:M ServiceCategory` — هر متخصص می‌تواند در چند دسته‌بندی فعالیت کند و هر دسته‌بندی نیز می‌تواند چند متخصص داشته باشد.

