# เช็กลิสต์ส่งแบบฝึกหัดที่ 17

## ไฟล์หลักฐาน PDF

PDF ควรมีภาพหน้าจอจากเว็บที่รันสำเร็จทั้งหมด 3 หน้า

1. หน้าสินค้า โดยเลือก **แบบคอลัมน์** และรอให้ข้อมูลจาก API โหลดครบ
2. หน้าสินค้า โดยเลือก **แบบตาราง** และรอให้ข้อมูลจาก API โหลดครบ
3. หน้า **ข้อมูลผู้ใช้** แบบตาราง และรอให้ข้อมูลจาก API โหลดครบ

ควรตั้งชื่อไฟล์ให้สื่อความหมาย เช่น `แบบฝึกหัด17_สินค้าและผู้ใช้.pdf`

## GitHub

สร้าง Public repository ชื่อ `Vue-Products-Users-API` แล้ว push โค้ดด้วยคำสั่ง

```powershell
git init
git add .
git commit -m "Add Vue products and users API assignment"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/Vue-Products-Users-API.git
git push -u origin main
```

เปลี่ยน `YOUR_USERNAME` เป็นชื่อบัญชี GitHub ของผู้ส่ง แล้วแนบลิงก์ repository ไปพร้อม PDF

## ตรวจครั้งสุดท้าย

- ภาษาไทยแสดงผลถูกต้อง ไม่เป็นอักขระเพี้ยน
- ทั้ง 3 หน้าจอแสดงข้อมูลจริงจาก API
- `npm run build` สำเร็จ
- PDF เปิดได้และอ่านข้อความ/ตารางได้ชัดเจน
- GitHub repository เปิดดูได้จากลิงก์ที่ส่ง
