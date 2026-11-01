# Class Specification

در این بخش، مسئولیت، ویژگی‌ها، متدها و روابط هر کلاس مشخص شده است.

## 1. Customer

| مورد | توضیحات |
|---|---|
| مسئولیت | ثبت درخواست خدمات، پیگیری درخواست‌ها و ثبت نظر درباره متخصص |
| ویژگی‌ها | id, full_name, email, phone, password_hash, address, is_active |
| متدها | register(), login(), updateProfile(), createRequest(), cancelRequest(), addReview(), createComplaint() |
| روابط | Customer 1:N ServiceRequest, Customer 1:N Review, Customer 1:N Complaint |

## 2. Specialist

| مورد | توضیحات |
|---|---|
| مسئولیت | مدیریت اطلاعات تخصصی، مشاهده درخواست‌ها، ارسال پیشنهاد قیمت و تعیین زمان‌های در دسترس |
| ویژگی‌ها | id, full_name, email, phone, password_hash, bio, skills, service_area, estimated_rate, is_active |
| متدها | register(), login(), updateProfile(), sendQuote(), updateAvailability(), viewRequests() |
| روابط | Specialist 1:N Quote, Specialist 1:N Review, Specialist 1:N AvailabilitySlot, Specialist N:M ServiceCategory, Specialist 1:N Complaint, Specialist 1:N Notification |

## 3. Admin

| مورد | توضیحات |
|---|---|
| مسئولیت | مدیریت کاربران، متخصصان، دسته‌بندی خدمات و بررسی شکایت‌ها |
| ویژگی‌ها | id, full_name, email, password_hash, is_active |
| متدها | login(), manageCustomers(), manageSpecialists(), manageCategories(), manageComplaints(), blockUser(), viewReports() |
| روابط | — |

## 4. ServiceCategory

| مورد | توضیحات |
|---|---|
| مسئولیت | دسته‌بندی خدمات و تخصص‌های قابل ارائه در سیستم |
| ویژگی‌ها | id, name, description |
| متدها | create(), update(), delete() |
| روابط | ServiceCategory 1:N ServiceRequest, Specialist N:M ServiceCategory |

## 5. ServiceRequest

| مورد | توضیحات |
|---|---|
| مسئولیت | نگهداری اطلاعات درخواست خدمات و وضعیت آن |
| ویژگی‌ها | id, customer_id, category_id, description, location, budget, requested_at, status |
| متدها | create(), updateStatus(), complete(), cancel() |
| روابط | Customer 1:N ServiceRequest, ServiceCategory 1:N ServiceRequest, ServiceRequest 1:N Quote, ServiceRequest 1:0..1 Review, ServiceRequest 1:N Complaint, ServiceRequest 1:N Notification |

## 6. Quote

| مورد | توضیحات |
|---|---|
| مسئولیت | ثبت و مدیریت پیشنهادهای قیمت ارسال‌شده توسط متخصصان |
| ویژگی‌ها | id, request_id, specialist_id, proposed_price, description, created_at, status |
| متدها | create(), update(), accept(), reject() |
| روابط | ServiceRequest 1:N Quote, Specialist 1:N Quote |

## 7. Review

| مورد | توضیحات |
|---|---|
| مسئولیت | ثبت امتیاز و نظر مشتری درباره متخصص |
| ویژگی‌ها | id, customer_id, specialist_id, request_id, rating, comment, created_at |
| متدها | create(), update(), delete() |
| روابط | Customer 1:N Review, Specialist 1:N Review, ServiceRequest 1:0..1 Review |

## 8. AvailabilitySlot

| مورد | توضیحات |
|---|---|
| مسئولیت | نگهداری زمان‌های در دسترس متخصص |
| ویژگی‌ها | id, specialist_id, start_time, end_time, is_available |
| متدها | create(), update(), delete(), setAvailability() |
| روابط | Specialist 1:N AvailabilitySlot |

## 9. Complaint

| مورد | توضیحات |
|---|---|
| مسئولیت | ثبت و پیگیری شکایت‌های مشتریان درباره متخصصان |
| ویژگی‌ها | id, customer_id, specialist_id, request_id, description, status, created_at |
| متدها | create(), updateStatus(), resolve() |
| روابط | Customer 1:N Complaint, Specialist 1:N Complaint, ServiceRequest 1:N Complaint |

## 10. Notification

| مورد | توضیحات |
|---|---|
| مسئولیت | اطلاع‌رسانی درخواست‌های جدید به متخصصان |
| ویژگی‌ها | id, specialist_id, request_id, message, is_read, created_at |
| متدها | create(), markAsRead(), delete() |
| روابط | Specialist 1:N Notification, ServiceRequest 1:N Notification |
