
# Class Specification — ورکینو

## Customer

| بخش | توضیح |
|---|---|
| مسئولیت | ثبت درخواست خدمات و پیگیری آن‌ها |
| ویژگی‌ها | id, full_name, email, phone, password_hash, address, is_active |
| متدها | register(), login(), logout(), createRequest(), viewRequests(), submitReview() |
| روابط | هر مشتری می‌تواند چند درخواست و چند نظر داشته باشد. |

## Specialist

| بخش | توضیح |
|---|---|
| مسئولیت | ارائه‌ی خدمات و مدیریت درخواست‌های دریافتی |
| ویژگی‌ها | id, full_name, email, phone, password_hash, bio, skills, service_area, estimated_rate, is_active |
| متدها | register(), login(), logout(), viewRequests(), acceptRequest(), rejectRequest(), updateProfile() |
| روابط | هر متخصص می‌تواند چند درخواست، نظر و بازه‌ی زمانی داشته باشد و در چند دسته‌بندی فعالیت کند. |

## Admin

| بخش | توضیح |
|---|---|
| مسئولیت | مدیریت کاربران و دسته‌بندی خدمات |
| ویژگی‌ها | id, full_name, email, password_hash, is_active |
| متدها | login(), logout(), manageUsers(), manageCategories(), viewStatistics() |
| روابط | مدیریت حساب مشتریان، متخصصان و دسته‌بندی‌های خدمات |

## ServiceCategory

| بخش | توضیح |
|---|---|
| مسئولیت | دسته‌بندی خدمات قابل ارائه |
| ویژگی‌ها | id, name, description |
| متدها | addCategory(), updateCategory(), deleteCategory() |
| روابط | هر دسته‌بندی می‌تواند چند درخواست و چند متخصص داشته باشد. |

## ServiceRequest

| بخش | توضیح |
|---|---|
| مسئولیت | نگهداری و پیگیری درخواست‌های خدمات |
| ویژگی‌ها | id, customer_id, specialist_id, category_id, description, location, budget, requested_at, status |
| متدها | assignSpecialist(), updateStatus(), cancelRequest() |
| روابط | هر درخواست متعلق به یک مشتری و یک دسته‌بندی است. تخصیص متخصص اختیاری است. هر درخواست می‌تواند حداکثر یک نظر داشته باشد. |
| نوع وضعیت | `RequestStatus (enum)` |
| مقادیر وضعیت | PENDING, ACCEPTED, REJECTED, COMPLETED, CANCELLED |

## Review

| بخش | توضیح |
|---|---|
| مسئولیت | ثبت امتیاز و نظر مشتری درباره‌ی متخصص |
| ویژگی‌ها | id, customer_id, specialist_id, request_id, rating, comment, created_at |
| متدها | submitReview(), updateReview() |
| روابط | هر نظر به یک مشتری، یک متخصص و یک درخواست مربوط است. |

## AvailabilitySlot

| بخش | توضیح |
|---|---|
| مسئولیت | ثبت زمان‌های در دسترس بودن متخصص |
| ویژگی‌ها | id, specialist_id, start_time, end_time, is_available |
| متدها | addSlot(), updateSlot(), removeSlot() |
| روابط | هر بازه‌ی زمانی متعلق به یک متخصص است و هر متخصص می‌تواند چند بازه داشته باشد. |
