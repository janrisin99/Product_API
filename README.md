# แบบฝึกหัดที่ 17 - Vue.js แสดงข้อมูลสินค้าและผู้ใช้ผ่าน API

เว็บแอป Vue 3 สำหรับแสดงข้อมูลจาก Public API ตามโจทย์ โดยมีหน้าจอหลัก 3 รูปแบบ

- สินค้าแบบคอลัมน์ (การ์ด)
- สินค้าแบบตาราง
- ข้อมูลผู้ใช้แบบตาราง

## API ที่ใช้

- สินค้า: `https://dummyjson.com/products?limit=12`
- ผู้ใช้: `https://jsonplaceholder.typicode.com/users`

## เทคโนโลยี

- Vue 3 (Composition API)
- Vite
- Bootstrap 5

## วิธีติดตั้งและรัน

ต้องติดตั้ง Node.js รุ่น LTS ก่อน แล้วเปิด Terminal ในโฟลเดอร์โปรเจกต์และรัน

```powershell
npm install
npm run dev
```

เปิด URL ที่ Vite แสดง เช่น `http://localhost:5173`

## คำสั่งตรวจสอบก่อนส่ง

```powershell
npm run build
```

หาก build สำเร็จ จะได้โฟลเดอร์ `dist` สำหรับนำไป deploy

## สิ่งที่ต้องส่ง

1. PDF ที่มีภาพหน้าจอสินค้าแบบคอลัมน์ สินค้าแบบตาราง และข้อมูลผู้ใช้แบบตาราง
2. ลิงก์ GitHub repository ของโปรเจกต์

ดูขั้นตอนเพิ่มเติมได้ใน `SUBMISSION.md`
