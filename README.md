# Final Exam: Software Testing with Playwright

ระบบที่ทดสอบ: https://www.saucedemo.com/
เครื่องมือ: Playwright Test (TypeScript)

## IT-01: Top-Down Integration Testing ด้วย Stub

แนวคิด: Top-Down เริ่มทดสอบจากโมดูลระดับบน (Login) โดยใช้ของจริง
ส่วนโมดูลระดับล่างที่ยังไม่พร้อมหรือไม่ต้องการใช้ของจริง (Inventory) จะแทนด้วย Stub

| ส่วน | สถานะ | รายละเอียด |
| --- | --- | --- |
| หน้า Login ของ SauceDemo | **Real** | เปิดเว็บจริง กรอก standard_user / secret_sauce และกด Login จริง |
| การตรวจ URL inventory.html | **Real** | ยืนยันว่าระบบ Login จริงพาเข้าหน้า Inventory สำเร็จ |
| หน้า Inventory | **Stub** | สร้างด้วย `page.setContent()` แสดง Mock Inventory Ready, Mock Backpack, Mock Bike Light |

หมายเหตุ: Stub นี้จำลอง DOM ระดับ Browser หลัง Login จริง
ไม่ใช่การแทน Internal Module ของ SauceDemo โดยตรง เพราะเราเข้าถึง Source Code ภายในไม่ได้


## IT-02: Bottom-Up Integration Testing ด้วย Driver

แนวคิด: Bottom-Up เริ่มทดสอบจากโมดูลระดับล่าง (Inventory, Cart) โดยใช้ของจริง
ส่วนโมดูลระดับบนที่ยังไม่พร้อม (Login) จะแทนด้วย Driver ที่เตรียมสถานะให้

| ส่วน | สถานะ | รายละเอียด |
| --- | --- | --- |
| `driverOpenInventory()` | **Driver** | ตั้ง Cookie `session-username=standard_user` ด้วย `BrowserContext.addCookies()` แล้วเปิด inventory.html ตรง ๆ โดยไม่ผ่าน Login Form |
| หน้า Inventory | **Real** | แสดงสินค้าจริงครบ 6 รายการ |
| การเพิ่มสินค้าและ Cart Badge | **Real** | เพิ่ม Backpack และ Bike Light แล้ว Badge เป็น 2 |
| หน้า Shopping Cart | **Real** | แสดงสินค้าทั้งสองรายการจริง |

บทบาทของ Driver: ทำหน้าที่เรียกและป้อนสถานะเริ่มต้นให้โมดูลระดับล่างทำงานได้
โดยไม่ต้องพึ่งโมดูล Login จริง