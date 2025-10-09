# E-Commerce-Production
ออกแบบและวางโครงสร้างระบบให้ยืดหยุ่นต่อการต่อยอด สามารถพัฒนาต่อในอนาคตได้ง่าย ตามแนวทางระบบ Production ขนาดใหญ่

## 📄 โครงสร้างระบบ

### Frontend
- React.js (JSX, ES6+) พร้อม Component แบบ reusable
- React Router, Bootstrap + SCSS + Material UI
- Context API + Custom Hooks
- Modal CRUD, Pagination, Search & Filter

### Backend
- Node.js + Express.js พร้อม Controller-Service-Model structure
- JWT Authentication + Middleware (role-based access)
- Async/Await MySQL queries + parameterized queries
- Centralized error handling & logging

### Features
- Fullstack REST API development
- Role-Based Access Control (RBAC)
- Realtime session updates via WebSocket
- Designed for scalability & maintainability

---

# 📦 Feature ทั้งหมด

---

## 🏰 MIDDLEWARE & Security
### 🔸 verifyToken
- ตรวจสอบความถูกต้องของ token ที่ร้องขอ API
- ถ้าไม่มี token หรือ token ไม่ถูกต้อง จะถูกปฏิเสธคำร้องขอ API

### 🔸 verifyRole
- ตรวจสอบว่า role ของสมาชิกตรงกับ role ที่อนุญาตหรือไม่
- หากไม่ตรงกับ role ที่อนุญาตจะถูกปฏิเสธคำร้องขอ api

### 🔸 checkPermissionCrud(modules.code, actions.code)
- ตรวจสอบสิทธิ์ดำเนินการของ role ของสมาชิกที่ร้องขอ api ว่ามีสิทธิ์ในการดำเนินการดังกล่าวตาม modules.code, actions.code หรือไม่
- ถ้าไม่มีสิทธิ์ จะแจ้งชื่อ permission ที่เกี่ยวข้องในข้อความ error

### 🔸 checkPermissionRead(modules.code, actions.code)
- ตรวจสอบสิทธิ์ของ role ของสมาชิก ว่าสามารถ อ่านข้อมูล (Read) ใน module นั้น ๆ ได้หรือไม่
- อนุญาตถ้ามี action view-all หรือสิทธิ์ action เฉพาะนั้น
- ถ้าไม่มีสิทธิ์ จะส่ง 403 Forbidden ทั่วไป

### 🔸 ensureGuestSessionMiddleware
- สร้าง Guest Session สำหรับผู้ที่ยังไม่ล็อกอิน
- เก็บ session_id ใน cookie

---

## 🏰 AUTO-CRON JOBS
### 🔸 AUTO EXPIRED CARTS
- เปลี่ยนสถานะตะกร้าสินค้าที่ไม่ได้มีการอัพเดทเกิน 7 วันไปเป็นสถานะ "ตะกร้าหมดอายุ"
- จะดำเนินการทุกเที่ยงคืน

### 🔸 AUTO CANCEL PAYMENT
- เปลี่ยนสถานะออร์เดอร์ที่ไม่ได้ชำระเงินเกิน 3 วันหลังจากยืนยันการสั่งซื้อออร์เดอร์
- จะดำเนินการทุกเที่ยงคืน

### 🔸 AUTO REVOKE EXPIRED SESSIONS
- Revoke User Session ที่ Token หมดอายุเมื่อเทียบกับเวลาปัจจุบัน
- จะดำเนินการทุกเที่ยงคืน

### 🔸 AUTO UNBAN EXPIRED USERS
- ปลดแบนสมาชิกอัตโนมัติเมื่อระยะเวลาการแบนครบตามเวลาที่กำหนด เมื่อเทียบกับเวลาปัจจุบัน
- จะดำเนินการทุกเที่ยงคืน

---

## 🏰 AUTH PAGE
- ระบบรองรับการตรวจสอบ และเก็บประวัติการเปลี่ยนรหัสผ่านของสมาชิก

### 🔸 Login
- เข้าสู่ระบบด้วย Email และรหัสผ่าน
- ตรวจสอบรหัสผ่านกับ Hashed Password ที่เก็บในฐานข้อมูล
- รองรับการจดจำฉัน (Remember Me)

### 🔸 Register
- สมัครสมาชิกใหม่ในระบบ
- เข้ารหัสรหัสผ่านก่อนเก็บลงฐานข้อมูล (Hashed Password) ด้วย bcrypt + salt เพื่อความปลอดภัย
- รองรับการเก็บประวัติรหัสผ่าน (Password History) สำหรับตรวจสอบไม่ให้ใช้รหัสผ่านซ้ำย้อนหลัง
- ตรวจสอบความถูกต้องของข้อมูลก่อนบันทึก (Validator + Sanitizer)

### 🔸 Forgot Password
- การกู้คืนรหัสผ่านอีเมลที่ใช้สมัครในระบบ

---

## 🏰 PUBLIC PAGE
- ระบบรองรับ GUEST USER ผ่าน GUEST SESSIONS และ MIGRATE CART จาก GUEST CART สู่ USER CART

### 🔸 Guest User
- เลือกดูรายการสินค้า และรายละเอียดสินค้าตามคุณสมบัติย่อยของสินค้าได้
- เพิ่มสินค้าเข้าตะกร้าสินค้า (รองรับการเพิ่ม ลด หรือลบ รายการสินค้าในตะกร้าสินค้า)

---

## 🏰 CUSTOMER PAGE
- ระบบรองรับการ MIGRATE CART จาก GUEST CART สู่ USER CART

### 🔸 Profile CRUD (Update)
- จัดการข้อมูลส่วนตัวสมาชิก
- รองรับการเปลี่ยนรหัสผ่าน (Update Password)

### 🔸 User Address Book CRUD (Create, Update, Hard-Delete)
- จัดการที่อยู่สำหรับจัดส่งสินค้า
- ระบบรองรับการตั้งค่าเริ่มต้นของที่อยู่สำหรับจัดส่ง (Set-Default, Reset-Default)

### 🔸 Checkout
- แสดงแบบฟอร์มสำหรับดำเนินการสั่งซื้อ
- ระบบรองรับการเซ็ตค่าที่อยู่สำหรับจัดส่งอัตโนมัติ
- ระบบรองรับการเลือกช่องทางการชำระเงิน (พัฒนาต่อได้)
- ระบบรองรับ Stock Reservation (ตรวจสอบจำนวนก่อนดำเนินการสั่งซื้อออร์เดอร์)

### 🔸 Order History (Read)
- แสดงรายการออร์เดอร์ที่สมาชิกดำเนินการซื้อสินค้า
- ระบบรองรับการยกเลิกรายการออร์เดอร์ (Cancel)
- ระบบรองรับการส่งหลักฐานชำระเงิน (Payment Proof)

### 🔸 Order Detail (Read)
- แสดงรายการสินค้าภายในออร์เดอร์ของสมาชิก

### 🔸 Payment Proof (Update)
- ระบบรองรับการส่งหลักฐานการชำระเงิน

### 🔸 Shipping Tracking History (Read)
- แสดงความคืบหน้าของการจัดส่งสินค้า เช่น พัสดุกำลังถูกขนส่ง (In Transit), พัสดุออกจากศูนย์กระจายสินค้าเพื่อส่งผู้รับ (Out of Delivery), พัสดุถูกส่งถึงผู้รับแล้ว (Delivered)

---

## 🏰 ADMIN PAGE
- ปัจจุบันมีบทบาท (Role) คือ SUPER ADMIN, ADMIN, LOGISTICS

### 📦 DASHBOARD
- ระบบรองรับการเก็บข้อมูล State ที่เกี่ยวข้องกับ E-Commerce เช่น ออร์เดอร์ที่ส่งซื้อ หรือยอดขายรายวัน/รายเดือน

### 📦 AUDIT LOG
- ระบบรองรับการเก็บประวัติการดำเนินการ (LOG) ต่าง ๆ ในระบบ ไว้ในตาราง history ของแต่ละข้อมูล
- ระบบรองรับการตรวจสอบรายละเอียดการเปลี่ยนแปลงข้อมูล เช่น การเปลี่ยนชื่อสมาชิก (name: from "testUpdate" to "testUpdate2")

### 📦 ORDERS MANAGEMENT
- ระบบรองรับการจัดการข้อมูลสินค้าอย่างเป็นระบบ พร้อมการกู้คืนสินค้าที่ถูกลบ (Soft-Delete)

#### 🔸 Order CRUD (Read)
- รองรับการปฏิเสธออร์เดอร์ (Reject)

#### 🔸 Order Detail CRUD (Assign, Update, Hard-Delete)
- จัดการรายการสินค้าภายในออร์เดอร์

#### 🔸 Order Status CRUD (Read, Create, Update, Soft-Delete, Reactivate)
- จัดการสถานะรายการออร์เดอร์

#### 🔸 Order Address Type CRUD (Read, Create, Update, Soft-Delete, Reactivate, Hard-Delete)
- รองรับการ Soft Delete เพื่อกู้คืนภายหลัง และ Hard-Delete เมื่อไม่มีการใช้งานในสินค้าใด ๆ

---

## 📦 PRODUCTS MANAGEMENT
- ระบบรองรับสินค้าเดียว สามารถมีได้หลายคุณสมบัติ เช่น เสื้อยืด (ไซส์ S, สีขาว หรือ ไซส์ M, สีขาว) โดยเก็บในตาราง product_variant
- ระบบรองรับการจัดการข้อมูลสินค้าอย่างเป็นระบบ พร้อมการกู้คืนสินค้าที่ถูกลบ (Soft-Delete)

#### 🔸 Product Categories CRUD (Read, Create, Update, Soft-Delete, Reactivate)
- จัดการหมวดหมู่สินค้า และกู้คืนข้อมูลที่ถูกลบได้

#### 🔸 Product Categories Detail CRUD (Assign, Hard-Delete)
- กำหนดรายละเอียดความสัมพันธ์ของหมวดหมู่ และรายการสินค้า

#### 🔸 Product Category Default Attribute Key Images CRUD (Read, Create, Update, Soft-Delete, Reactivate)
- ตั้งค่ารูปภาพ Default สำหรับ Attribute Key ในแต่ละหมวดหมู่สินค้า แบบ 1 : 1

#### 🔸 Product CRUD (Create, Read, Update, Soft-Delete, Reactivate)
- รองรับการเปลี่ยนสถานะสินค้า (Active / Inactive)
- รองรับการตั้งค่ารูปภาพหลักสำหรับแสดงบนหน้าร้านค้า (Set-Default-Image, Reset-Default-Image)

#### 🔸 Product VARIANT CRUD (Read, Create, Update, Soft-Delete, Reactivate)
- รองรับการเปลี่ยนสถานะสินค้า (Active / Inactive)
- รองรับการตั้งค่ารูปภาพหลักสำหรับแสดงบนหน้าร้านค้า (Set-Default-Image, Reset-Default-Image)

#### 🔸 Product Variant Attributes CRUD (Read, Create, Update, Soft-Delete, Reactivate)
- จัดการข้อมูลคุณสมบัติของ Product Variant แบบ 1 : 1

#### 🔸 Product Variant Attribute Keys CRUD (Read, Create, Update, Soft-Delete, Reactivate, Hard-Delete)
- จัดการประเภทคุณสมบัติ (Key) ของคุณสมบัติสินค้า เช่น สี, ขนาด, วัสดุ
- รองรับการ Soft-Delete เพื่อกู้คืนภายหลัง และ Hard-Delete เมื่อไม่มีการใช้งานในสินค้าใด ๆ

#### 🔸 Product Variant Attribute Values CRUD (Read, Create, Update, Soft-Delete, Reactivate, Hard-Delete)
- จัดการค่าคุณสมบัติ (Value) ของแต่ละคุณสมบัติสินค้า
- รองรับการ Soft-Delete เพื่อกู้คืนภายหลัง และ Hard-Delete เมื่อไม่มีการใช้งานในสินค้าใด ๆ

#### 🔸 Product Variant Attribute Images CRUD (Hard-Delete)
- จัดการรูปภาพของแต่ละ Attribute ของสินค้า

#### 🔸 Product Variant Attribute Images Detail CRUD (Update, Hard-Delete)
- จัดการรายละเอียดการเชื่อมโยงรูปภาพกับ Attribute

#### 🔸 Product Status CRUD (Read, Create, Update, Soft-Delete, Reactivate)
- จัดการสถานะของรายการสินค้า
- รองรับการ Soft Delete เพื่อกู้คืนภายหลัง และ Hard Delete เมื่อไม่มีการใช้งานในสินค้าใด ๆ


---

## 📦 CARTS
### 🔸 Cart CRUD (Read, Hard-Delete)
- จัดการตะกร้าสินค้า

### 🔸 Cart Detail CRUD (Assign, Update, Hard-Delete)
- จัดการรายการสินค้าในตะกร้าสินค้า

### 🔸 Cart Status CRUD (Read, Create, Update, Soft-Delete, Reactivate)
- จัดการสถานะตะกร้าสินค้า

---

## 📦 PAYMENTS
### 🔸 Payment CRUD (Read)
- จัดการการชำระเงิน และตรวจสอบหลักฐานการชำระเงิน
- รองรับการอนุมัติ และปฏิเสธการชำระเงิน (Confirm, Reject)

### 🔸 Payment Method CRUD (Read, Create, Update, Soft-Delete, Reactivate)
- จัดการช่องทางการชำระเงิน

### 🔸 Payment Status CRUD (Read, Create, Update, Soft-Delete, Reactivate)
- จัดการสถานะการชำระเงิน

---

## 📦 INVENTORY
### 🔸 Warehouse CRUD (Read, Create, Update, Soft-Delete, Reactivate)
- จัดการคลังสินค้า

### 🔸 Warehouse Detail CRUD (Assign, Hard-Delete)
- จัดการรายการสินค้าภายในคลังสินค้า
- รองรับการจัดการสต๊อกสินค้าในคลังสินค้า (Receive, Reduct, Set-min)

### 🔸 Stock CRUD
- จัดการสต๊อกสินค้าทุกคลังสินค้า (Create, Update, Soft-Delete, Reactivate)
- รองรับการจัดการสต๊อกสินค้าในคลังสินค้า (Receive, Reduct, Set-min)

---

## 📦 SHIPPING
### 🔸 Shipping Tracking CRUD (Read, Update)
- จัดการการติดตามการขนส่งของออร์เดอร์ แบบ 1 : 1

### 🔸 Shipping Rate CRUD (Read, Create, Update, Soft-Delete, Reactivate)
- จัดการเรทค่าบริการจัดส่ง

### 🔸 Shipping Zone CRUD (Read, Create, Update, Soft-Delete, Reactivate)
- จัดการโซนการจัดส่ง

### 🔸 Shipping Method CRUD (Read, Create, Update, Soft-Delete, Reactivate)
- จัดการวิธี/ช่องทางในการจัดส่ง

### 🔸 Shipping Status CRUD (Read, Create, Update, Soft-Delete, Reactivate)
- จัดการสถานะการจัดส่ง

---

## 📦 LOCATIONS
- ระบบมีการจัดการข้อมูลเกี่ยวกับพื้นที่ตามลำดับชั้นของข้อมูล (Country → Province → District → Subdistrict → Postal Code)

### 🔸 Country CRUD (Read, Create, Update, Soft-Delete, Reactivate)
- จัดการข้อมูลประเทศ

### 🔸 Provinces CRUD (Read, Create, Update, Soft-Delete, Reactivate)
- จัดการข้อมูลจังหวัด

### 🔸 Disicts CRUD (Read, Create, Update, Soft-Delete, Reactivate)
- จัดการเขต/อำเภอ

### 🔸 Subdistricts CRUD (Read, Create, Update, Soft-Delete, Reactivate)
- จัดการตำบล

### 🔸 Postal Codes CRUD (Read, Create, Update, Soft-Delete, Reactivate)
- จัดการรหัสไปรษณีย์

---

## 📦 USER MANAGEMENT
### 🔸 User CRUD (Read, Create, Update)
- จัดการสมาชิกในระบบ

---

## 📦 BANS MANAGEMENT
- รองรับการเก็บประวัติการแบนของสมาชิกไว้ในตาราง ban_history
- Developer จะเป็นผู้กำหนดขอบเขตการแบน (Ban Scope) และเชื่อมต่อ Middleware สำหรับตรวจสอบสถานะแบนในแต่ละ Route

### 🔸 Bans CRUD (Ban, Update, Unban)
- จัดการเกี่ยวกับการแบนสมาชิกในระบบ

### 🔸 Ban Durations (Read, Create, Update, Soft-Delete, Reactivate)
- ใช้จัดการระยะเวลาการแบน เช่น ระยะสั้น / ระยะยาว / ถาวร

### 🔸 Ban Scope (Read, Update)
- ใช้จัดการขอบเขตการแบน เช่น USER (ห้ามเข้าระบบ) หรือ CHAT (ห้ามส่งข้อความ)
- Ban Scope ถูกกำหนดโดย Developer เท่านั้น
- ผู้ใช้ระดับ SUPER ADMIN สามารถแก้ไขเฉพาะข้อมูลทั่วไป (เช่น ชื่อหรือรายละเอียด) แต่ ไม่สามารถแก้ไข code ของ Ban Scope ได้ผ่านระบบ

---

## 📦 ROLE MANAGEMENT
- ระบบรองรับการกำหนดสิทธิ์และบทบาทของผู้ใช้งานในระบบ
- ใช้การกำหนดสิทธิ์ผ่านตาราง modules และกำหนดการกระทำผ่านตาราง actions
  - modules.code แทนสิทธิ์การเข้าถึงเพจ เช่น users-page หรือ products-page
  - actions.code แทนการกระทำ เช่น create, update หรือ soft-delete
- ตาราง permissions จะเชื่อมโยง module_id และ action_id เพื่อระบุสิทธิ์ที่เฉพาะเจาะจง
  - Permission จะถูกจัดเตรียมและดูแลโดย Developer เท่านั้น (ไม่เปิดให้แก้ไขผ่านระบบ)
  - Developer จะเป็นคนเชื่อมโยง และกำหนด Middleware สำหรับตรวจสอบ Role Permission ใน routes
- ตาราง role_permissions ใช้เพื่อกำหนดสิทธิ์เหล่านี้ให้กับแต่ละ Role
  - Administrator สามารถจัดการสิทธิ์ของ Role ได้ผ่าน Role Permission CRUD  

### 🔸 Role CRUD (Read, Create, Update, Soft-Delete, Reactivate)
- จัดการบทบาท (Role) ของสมาชิกในระบบ
- รองรับการ Soft-Delete เพื่อกู้คืนภายหลัง และ Hard-Delete เมื่อไม่มีการใช้งานในระบบใด ๆ  

### 🔸 Role Permission CRUD
- จัดการการกำหนดสิทธิ์ (Permission) ให้กับ Role  
- รองรับการกำหนดสิทธิ์หลายรายการพร้อมกัน และการเพิกถอนสิทธิ์ออกจาก Role

---

## 📦 USER SESSIONS
- ระบบรองรับการ refresh token เมื่อ Access Token เก่าหมดอายุจะต่อ Access Token ใหม่ในระยะเวลาที่กำหนด
- ระบบรองรับการ revoke สมาชิกในระบบ

---

## 🖼️ UI Preview

### 🔸 System Overview Diagram
<img width="828" height="1072" alt="SystemOverviewDiagram" src="https://github.com/user-attachments/assets/9ba1a197-a27f-430a-bfc0-2103e679d3d2" />

### 🔸 Login Page
<img width="2554" height="1298" alt="Screenshot 2025-10-07 161012" src="https://github.com/user-attachments/assets/b7940715-9d3d-403a-84ad-d8a5e5f11171" />

### 🔸 Homepage Page
<img width="2559" height="1294" alt="Screenshot 2025-10-07 161310" src="https://github.com/user-attachments/assets/48362c43-a4f6-4bb3-8dbd-dadfc9f7070a" />

### 🔸 Dashboard Overview
<img width="2559" height="1292" alt="Screenshot 2025-10-07 162723" src="https://github.com/user-attachments/assets/1bfb2b05-c260-4df4-8160-5905272f254a" />

### 🔸 Product CRUD Page
<img width="2559" height="1294" alt="Screenshot 2025-10-07 162440" src="https://github.com/user-attachments/assets/8ef9b4d4-86d6-4d5e-b16b-bc68a40dc732" />

### 🔸 Product CRUD Modal
<img width="2559" height="1296" alt="Screenshot 2025-10-07 162604" src="https://github.com/user-attachments/assets/7dd35cc3-95ea-4181-81e1-35dc1144f69b" />

### 🔸 Role Permissions CRUD Page
<img width="2559" height="1297" alt="Screenshot 2025-10-07 162632" src="https://github.com/user-attachments/assets/cfded5c5-ce44-4f1f-b3f1-3968dfea63be" />




