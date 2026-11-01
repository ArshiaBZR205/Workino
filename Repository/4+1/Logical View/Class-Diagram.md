# Class Diagram — Workino

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
| category_id | int |
| description | string |
| location | string |
| budget | decimal |
| requested_at | datetime |
| status | RequestStatus (enum) |

مقادیر مجاز وضعیت درخواست:

- `PENDING` — در انتظار دریافت پیشنهاد
- `IN_PROGRESS` — در حال انجام
- `COMPLETED` — تکمیل‌شده
- `CANCELLED` — لغوشده

### ۲.۶ Quote

| ویژگی | نوع |
|---|---|
| id | int |
| request_id | int |
| specialist_id | int |
| proposed_price | decimal |
| description | string |
| created_at | datetime |
| status | QuoteStatus (enum) |

مقادیر مجاز وضعیت پیشنهاد قیمت:

- `PENDING` — در انتظار پاسخ مشتری
- `ACCEPTED` — پذیرفته‌شده
- `REJECTED` — ردشده

### ۲.۷ Review

| ویژگی | نوع |
|---|---|
| id | int |
| customer_id | int |
| specialist_id | int |
| request_id | int |
| rating | int |
| comment | string |
| created_at | datetime |

### ۲.۸ AvailabilitySlot

| ویژگی | نوع |
|---|---|
| id | int |
| specialist_id | int |
| start_time | datetime |
| end_time | datetime |
| is_available | bool |

### ۲.۹ Complaint

| ویژگی | نوع |
|---|---|
| id | int |
| customer_id | int |
| specialist_id | int |
| request_id | int |
| description | string |
| status | ComplaintStatus (enum) |
| created_at | datetime |

مقادیر مجاز وضعیت شکایت:

- `PENDING` — در انتظار بررسی
- `REVIEWED` — بررسی‌شده
- `RESOLVED` — رسیدگی‌شده

### ۲.۱۰ Notification

| ویژگی | نوع |
|---|---|
| id | int |
| specialist_id | int |
| request_id | int |
| message | string |
| is_read | bool |
| created_at | datetime |

## ۳. روابط

- `Customer 1:N ServiceRequest` — هر مشتری می‌تواند چند درخواست ثبت کند؛ هر درخواست متعلق به یک مشتری است.
- `ServiceCategory 1:N ServiceRequest` — هر دسته‌بندی می‌تواند چند درخواست داشته باشد؛ هر درخواست متعلق به یک دسته‌بندی است.
- `ServiceRequest 1:N Quote` — هر درخواست می‌تواند چند پیشنهاد قیمت دریافت کند؛ هر پیشنهاد قیمت متعلق به یک درخواست است.
- `Specialist 1:N Quote` — هر متخصص می‌تواند چند پیشنهاد قیمت ارسال کند؛ هر پیشنهاد قیمت توسط یک متخصص ارسال می‌شود.
- `Customer 1:N Review` — هر مشتری می‌تواند چند نظر ثبت کند؛ هر نظر توسط یک مشتری ثبت می‌شود.
- `Specialist 1:N Review` — هر متخصص می‌تواند چند نظر دریافت کند؛ هر نظر درباره یک متخصص است.
- `ServiceRequest 1:0..1 Review` — هر درخواست می‌تواند بدون نظر باشد یا حداکثر یک نظر داشته باشد.
- `Specialist 1:N AvailabilitySlot` — هر متخصص می‌تواند چند بازه زمانی داشته باشد؛ هر بازه متعلق به یک متخصص است.
- `Specialist N:M ServiceCategory` — هر متخصص می‌تواند در چند دسته‌بندی فعالیت کند و هر دسته‌بندی نیز می‌تواند چند متخصص داشته باشد.
- `Customer 1:N Complaint` — هر مشتری می‌تواند چند شکایت ثبت کند؛ هر شکایت توسط یک مشتری ثبت می‌شود.
- `Specialist 1:N Complaint` — هر متخصص می‌تواند موضوع چند شکایت باشد.
- `ServiceRequest 1:N Complaint` — هر درخواست می‌تواند موضوع چند شکایت باشد.
- `Specialist 1:N Notification` — هر متخصص می‌تواند چند اعلان دریافت کند؛ هر اعلان برای یک متخصص است.
- `ServiceRequest 1:N Notification` — هر درخواست می‌تواند باعث ایجاد چند اعلان شود.
