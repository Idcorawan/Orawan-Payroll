# ระบบเงินเดือน (Google Apps Script)

ระบบเงินเดือนพนักงานรายชั่วโมง/Part-time บน Google Apps Script + Google Sheet + หน้าเว็บ
ชื่อบริษัท โลโก้ ค่าแรง และกติกา OT ตั้งเองในเมนู **ตั้งค่าระบบ** หลังติดตั้ง (ไม่มีฝังในโค้ด)

| ไฟล์ | คืออะไร |
|---|---|
| `Code.gs` | ฝั่งเซิร์ฟเวอร์: สร้างชีต/โฟลเดอร์, API หน้าเว็บ, คำนวณเงิน, สลิป, โลโก้ |
| `Index.html` | หน้าเว็บทั้งระบบ (16 เมนู) |
| `TimesheetOcr.gs` | AI (Gemini) อ่านรูปใบลงเวลาฟอร์ม `PAY-TS` — ต้องตั้ง Script Property `GEMINI_API_KEY` |
| `appsscript.json` · `.claspignore` | manifest (Asia/Bangkok, V8, web app) · push ขึ้น Google เฉพาะ 4 ไฟล์ |
| `tools/make-preview.js` | สร้าง `preview.html` ดูหน้าเว็บด้วยข้อมูลสมมติ: `node tools/make-preview.js` |

## ติดตั้ง
```bash
cp appsscript.json appsscript.json.bak              # create-script เขียนทับ manifest
npx clasp create-script --type standalone --title "ระบบเงินเดือน"
cp appsscript.json.bak appsscript.json
npx clasp push -f                                   # ต้องเห็น "Pushed 4 files"
npx clasp create-deployment -d "ติดตั้งครั้งแรก"      # เก็บ deploymentId ไว้ redeploy ทับ URL เดิม
```
ไม่ใช้ clasp: script.google.com → New project → วาง `Code.gs`, สร้าง script `TimesheetOcr` และ HTML `Index` → แก้ manifest ตาม `appsscript.json` → Deploy เป็น Web app

1. เจ้าของบัญชีเปิดเอดิเตอร์ → Run `setup` → อนุญาตสิทธิ์ (ก่อนนี้ /exec ตอบ 403)
2. เปิด URL /exec → **ตั้งค่าระบบ**: อัปโหลดโลโก้ · ข้อมูลบริษัท · กติกาคำนวณ
3. ตำแหน่งงาน (ค่าแรง/ชม.) → พนักงาน → พิมพ์ใบลงเวลา → เริ่มใช้งาน
4. (ถ้าใช้ AI) สร้างคีย์ที่ https://aistudio.google.com/apikey → Script Properties `GEMINI_API_KEY` → หน้าตั้งค่ากด **ทดสอบคีย์ AI** ต้องขึ้น ✓
5. (ถ้าใช้บอท LINE) Run `setupBotSecret` แล้วเอารหัสจาก Execution log ไปตั้งฝั่งบอท

## อัปเดตเว็บเดิม (URL ไม่เปลี่ยน)
```bash
npx clasp push -f
npx clasp deploy -i <deploymentId> -d "อธิบายสั้นๆ"
```
