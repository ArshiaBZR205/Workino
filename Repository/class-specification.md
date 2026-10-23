
# Class Specification

در این بخش، مسئولیت، ویژگی‌ها، متدها و روابط هر کلاس مشخص شده است.

## 1. Customer

| مورد | توضیحات |
|---|---|
| مسئولیت | ثبت درخواست خدمات و ثبت نظر درباره متخصص |
| ویژگی‌ها | id, full_name, email, phone, password_hash, address, is_active |
| متدها | register(), login(), updateProfile(), createRequest(), addReview() |
| روابط | Customer 1:N ServiceRequest, Customer 1:N Review |

## 2. Specialist

| مورد | توضیحات |
|---|---|
| مسئولیت | مدیریت اطلاعات تخصصی، دریافت درخواست و تعیین زمان‌های در دسترس |
| ویژگی‌ها | id, full_name, email, phone, password_hash, bio, skills, service_area, estimated_rate, is_active |
| متدها | register(), login(), updateProfile(), acceptRequest(), rejectRequest(), updateAvailability() |
| روابط | Specialist 1:N ServiceRequest, Specialist 1:N Review, Specialist 1:N AvailabilitySlot, Specialist N:M ServiceCategory |

## 3. Admin

| مورد | توضیحات |
|---|---|
| مسئولیت | مدیریت کاربران، متخصصان و دسته‌بندی خدمات |
| ویژگی‌ها | id, full_name, email, password_hash, is_active |
| متدها | login(), manageCustomers(), manageSpecialists(), manageCategories() |
| روابط | — |

## 4. ServiceCategory

| مورد | توضیحات |
|---|---|
| مسئولیت | دسته‌بندی خدمات قابل ارائه در سیستم |
| ویژگی‌ها | id, name, description |
| متدها | create(), update(), delete() |
| روابط | ServiceCategory 1:N ServiceRequest, Specialist N:M ServiceCategory |

## 5. ServiceRequest

| مورد | توضیحات |
|---|---|
| مسئولیت | نگهداری اطلاعات درخواست خدمات و وضعیت آن |
| ویژگی‌ها | id, customer_id, specialist_id, category_id, description, location, budget, requested_at, status |
| متدها | create(), assignSpecialist(), accept(), reject(), complete(), cancel() |
| روابط | Customer 1:N ServiceRequest, Specialist 1:N ServiceRequest, ServiceCategory 1:N ServiceRequest, ServiceRequest 1:0..1 Review |

## 6. Review

| مورد | توضیحات |
|---|---|
| مسئولیت | ثبت امتیاز و نظر مشتری درباره متخصص |
| ویژگی‌ها | id, customer_id, specialist_id, request_id, rating, comment, created_at |
| متدها | create(), update(), delete() |
| روابط | Customer 1:N Review, Specialist 1:N Review, ServiceRequest 1:0..1 Review |

## 7. AvailabilitySlot

| مورد | توضیحات |
|---|---|
| مسئولیت | نگهداری زمان‌های در دسترس متخصص |
| ویژگی‌ها | id, specialist_id, start_time, end_time, is_available |
| متدها | create(), update(), delete(), setAvailability() |
| روابط | Specialist 1:N AvailabilitySlot |
