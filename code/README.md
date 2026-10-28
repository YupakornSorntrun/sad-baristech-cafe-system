# Baristech Cafe POS — โค้ดระบบ

ระบบ POS ร้านกาแฟหลายสาขา เขียนด้วย Node.js + Express + MySQL หน้าเว็บเป็น HTML/CSS/JavaScript ล้วน (ไม่ใช้ framework) อยู่ในโฟลเดอร์ `public/`

ภาพรวมโครงงานและวิธีรันอยู่ที่ [README ของ repository](../README.md) การตั้งค่าฐานข้อมูลอยู่ที่ [DATABASE_SETUP.md](DATABASE_SETUP.md)

## โครงสร้าง

| โฟลเดอร์/ไฟล์ | หน้าที่ |
|---|---|
| `src/app.js` | จุดเริ่มต้นของเซิร์ฟเวอร์ ตั้งค่า middleware ผูก route และตัวจัดการ error |
| `src/routes/` | กำหนด URL ของ API และตรวจสิทธิ์ตามบทบาท |
| `src/controllers/` | ตรวจข้อมูลที่ส่งมาและกฎธุรกิจ |
| `src/models/` | คำสั่ง SQL (`*Model.js`) และคลาสธุรกิจ `Order.js` |
| `src/middlewares/auth.js` | ตรวจ JWT และบทบาท (owner / cashier) |
| `public/` | หน้าเว็บ (`login`, `index` = POS, `orders`, `menu`, `stock`, `report`) |
| `schema.sql` / `seed.sql` | สร้างตารางและข้อมูลทดลอง (2 สาขา) |
| `tests/quality-review/` | สคริปต์ตรวจสอบคุณภาพของสัปดาห์ 14 (วิธีรันดู README ในโฟลเดอร์นั้น) |

## คำสั่งที่ใช้บ่อย

```bash
npm install     # ติดตั้งไลบรารี
npm start       # เปิดเซิร์ฟเวอร์ที่ http://localhost:3000
npm run dev     # เปิดแบบรีสตาร์ตอัตโนมัติเมื่อแก้ไฟล์
```
