# MEGAwatt Raikhing — ระบบบันทึกเวลาทำงาน (Time Attendance)

เว็บแอปสแกนใบหน้าบันทึกเวลา **เข้า-ออกงาน** สไตล์ Tech & Cyberpunk (โทน Teal/Cyan บนพื้นเข้ม)

- สแกนครั้งแรกของวัน = **เวลาเข้างาน**
- สแกนครั้งถัดไปของวันเดียวกัน = **เวลาออกงาน** (สแกนออกซ้ำจะอัปเดตเวลาออกล่าสุด)
- ตรวจพิกัด GPS ก่อนสแกน (ตั้งรัศมีได้ / ใส่ 0 = ไม่จำกัดพื้นที่)
- หน้าแรกมีตารางสรุปเวลาเข้า-ออกของทุกคน "วันนี้" รีเฟรชอัตโนมัติทุก 1 นาที

## โครงสร้างหน้า

| ไฟล์ | หน้าที่ |
|---|---|
| `index.html` | เมนูหลัก + ตารางเวลาเข้า-ออกงานวันนี้ |
| `scan.html` | สแกนใบหน้าเข้า-ออกงาน (face-api.js + GPS) |
| `register.html` | ลงทะเบียนใบหน้าพนักงานใหม่ |
| `config.html` | ตั้งค่า API URL / App Key / จุด GPS (ต้องใส่รหัสผู้ดูแล) |
| `js/api-config.js` | ค่า URL ของ Google Apps Script API |

## Backend (ใช้ร่วมกับระบบเช็กอินหน้างาน)

ระบบนี้**ใช้ Google Apps Script + Google Sheet ตัวเดียวกัน**กับ repo ระบบเช็กอินหน้างาน
(ไฟล์ `apps-script/Code.gs` ใน repo นั้น) — ไม่ต้อง deploy backend ใหม่

ข้อมูลแยกแท็บกันใน Google Sheet เดิม:

| แท็บ | ใช้โดย |
|---|---|
| `Faces` | ฐานข้อมูลใบหน้า (แชร์กันทั้งสองระบบ) |
| `Attendance` | **บันทึกเวลาเข้า-ออกงาน (ระบบนี้)** |
| `Site_CheckIn` | เช็กอินหน้างาน (อีกระบบ) |
| `Config` | จุด GPS + รัศมี (แชร์กัน) |

API ที่ใช้:
- `GET ?action=getTodayAttendance` — ตารางวันนี้
- `GET ?action=getKnownFaces` — ฐานใบหน้า
- `GET ?action=getConfig` — พิกัด GPS
- `POST {action: 'logAttendance', name, lat, lng, note}` — บันทึกเวลา (ไม่ส่ง `sheetTarget` = ลงแท็บ Attendance)

## Deploy

เป็น static site ล้วน — deploy ขึ้น Vercel / Netlify / GitHub Pages ได้ทันที ไม่ต้อง build

## ลิงก์ตั้งค่าอัตโนมัติ

ส่งลิงก์ให้พนักงานครั้งแรกแบบตั้งค่าให้อัตโนมัติ:

```
https://<โดเมน>/?apiurl=<GAS_URL>&appkey=<รหัสลับ>
```
