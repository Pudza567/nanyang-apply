# ใบสมัครงานออนไลน์ - นันยาง มาร์เก็ตติ้ง
## FM-HR-002 | Online Job Application Form

---

## 📁 ไฟล์ที่ได้รับ
- `index.html` — เว็บฟอร์มใบสมัครงาน (ไฟล์เดียวจบ)

---

## 🚀 วิธี Deploy บน Netlify (ใช้เวลา ~3 นาที)

1. ไปที่ [netlify.com](https://netlify.com) → Login
2. ลาก folder `nanyang-application` วางใน Netlify Drop zone
3. ได้ลิงก์ เช่น `https://nanyang-apply.netlify.app`
4. แชร์ลิงก์ให้ผู้สมัครได้เลย ✅

---

## 📊 วิธีเชื่อมต่อ Google Sheets (รับข้อมูลอัตโนมัติ)

### ขั้นตอนที่ 1: สร้าง Google Sheets
1. ไปที่ [sheets.google.com](https://sheets.google.com) → สร้าง Sheet ใหม่
2. ตั้งชื่อ เช่น "ใบสมัครงาน Nanyang 2569"
3. คัดลอก **Spreadsheet ID** จาก URL
   - URL ตัวอย่าง: `https://docs.google.com/spreadsheets/d/`**`1BxiMVs0XRA5nFMdKvBdBZjgmUUqptlbs74OgVE2upms`**`/edit`

### ขั้นตอนที่ 2: สร้าง Google Apps Script
1. ใน Google Sheets → เมนู **Extensions → Apps Script**
2. ลบโค้ดเดิมทั้งหมด แล้ว **วางโค้ดด้านล่างนี้แทน**:

```javascript
const SHEET_ID = 'ใส่ Spreadsheet ID ของคุณตรงนี้';
const SHEET_NAME = 'Sheet1';

function doPost(e) {
  const sheet = SpreadsheetApp.openById(SHEET_ID).getSheetByName(SHEET_NAME);
  
  // สร้าง Header ถ้ายังไม่มี
  if (sheet.getLastRow() === 0) {
    const headers = [
      'วันที่ส่ง','ตำแหน่ง 1','ตำแหน่ง 2','เงินเดือนที่หวัง',
      'ชื่อ','นามสกุล','ชื่อเล่น','First Name','Surname',
      'วันเกิด','อายุ','เลขบัตรประชาชน',
      'โทรบ้าน','มือถือ','Email','Line ID',
      'บ้านเลขที่','หมู่','ถนน','ตำบล','อำเภอ','จังหวัด','รหัสไปรษณีย์',
      'สถานะทหาร','สถานภาพ',
      'ชื่อบิดา','ชื่อมารดา','ชื่อคู่สมรส','จำนวนบุตร',
      'น้ำหนัก','ส่วนสูง',
      'การศึกษา 1','สถานศึกษา 1','สาขา 1','ปีสำเร็จ 1',
      'การศึกษา 2','สถานศึกษา 2','สาขา 2','ปีสำเร็จ 2',
      'การศึกษา 3','สถานศึกษา 3','สาขา 3','ปีสำเร็จ 3',
      'การศึกษา 4','สถานศึกษา 4','สาขา 4','ปีสำเร็จ 4',
      'ฝึกงาน','อบรม',
      'บริษัท 1','จาก 1','ถึง 1','ตำแหน่งเริ่ม 1','ตำแหน่งสุดท้าย 1','เงินเดือน 1','สาเหตุ 1',
      'บริษัท 2','จาก 2','ถึง 2','ตำแหน่งเริ่ม 2','ตำแหน่งสุดท้าย 2','เงินเดือน 2','สาเหตุ 2',
      'บริษัท 3','ตำแหน่งสุดท้าย 3','เงินเดือน 3',
      'บริษัท 4','ตำแหน่งสุดท้าย 4','เงินเดือน 4',
      'เริ่มงานได้ภายใน (วัน)',
      'ภาษา 1','พูด 1','อ่าน 1',
      'ภาษา 2','พูด 2',
      'ทักษะคอมพิวเตอร์','พิมพ์ดีดไทย','พิมพ์ดีดอังกฤษ',
      'งานอดิเรก',
      'อ้างอิง 1 ชื่อ','อ้างอิง 1 โทร',
      'อ้างอิง 2 ชื่อ','อ้างอิง 2 โทร',
      'ฉุกเฉิน 1 ชื่อ','ฉุกเฉิน 1 โทร',
      'รายละเอียดเพิ่มเติม'
    ];
    sheet.appendRow(headers);
    
    // จัดรูปแบบ Header
    const headerRange = sheet.getRange(1, 1, 1, headers.length);
    headerRange.setBackground('#1a2744');
    headerRange.setFontColor('white');
    headerRange.setFontWeight('bold');
  }
  
  const data = JSON.parse(e.postData.contents);
  
  const row = [
    data.timestamp, data.pos1, data.pos2, data.salary_expect,
    data.fname, data.lname, data.nickname, data.fname_en, data.lname_en,
    data.birthdate, data.age, data.id_card,
    data.phone_home, data.phone_mobile, data.email, data.line_id,
    data.addr_no, data.addr_moo, data.addr_road, data.addr_sub, data.addr_dist, data.addr_prov, data.addr_zip,
    data.military, data.marital,
    data.father_name, data.mother_name, data.spouse_name, data.children,
    data.weight, data.height,
    data.edu_level_1, data.edu_school_1, data.edu_major_1, data.edu_year_1,
    data.edu_level_2, data.edu_school_2, data.edu_major_2, data.edu_year_2,
    data.edu_level_3, data.edu_school_3, data.edu_major_3, data.edu_year_3,
    data.edu_level_4, data.edu_school_4, data.edu_major_4, data.edu_year_4,
    data.internship, data.training,
    data.work_co_1, data.work_from_1, data.work_to_1, data.work_pos_start_1, data.work_pos_end_1, data.work_salary_1, data.work_reason_1,
    data.work_co_2, data.work_from_2, data.work_to_2, data.work_pos_start_2, data.work_pos_end_2, data.work_salary_2, data.work_reason_2,
    data.work_co_3, data.work_pos_end_3, data.work_salary_3,
    data.work_co_4, data.work_pos_end_4, data.work_salary_4,
    data.start_days,
    data.lang1, data.lang1_speak, data.lang1_read,
    data.lang2, data.lang2_speak,
    data.skills, data.type_th, data.type_en,
    data.hobby,
    data.ref1_name, data.ref1_tel,
    data.ref2_name, data.ref2_tel,
    data.emr1_name, data.emr1_tel,
    data.self_desc
  ];
  
  sheet.appendRow(row);
  
  return ContentService
    .createTextOutput(JSON.stringify({ status: 'success' }))
    .setMimeType(ContentService.MimeType.JSON);
}
```

3. กด **Save** (💾)
4. กด **Deploy → New deployment**
5. เลือก Type: **Web app**
6. ตั้งค่า:
   - Execute as: **Me**
   - Who has access: **Anyone**
7. กด **Deploy** → Copy **Web app URL**

### ขั้นตอนที่ 3: ใส่ URL ใน index.html
เปิดไฟล์ `index.html` หาบรรทัดนี้:
```javascript
const SCRIPT_URL = 'YOUR_GOOGLE_APPS_SCRIPT_URL_HERE';
```
แล้วเปลี่ยนเป็น URL ที่ได้จากขั้นตอนที่ 2

---

## 🖨️ การพิมพ์ / บันทึก PDF
- กด **พิมพ์ใบสมัคร** หรือ **บันทึก PDF** หลังจากส่งฟอร์มแล้ว
- เบราว์เซอร์จะเปิด Print Dialog ขึ้นมา
- ถ้าต้องการ PDF ให้เลือก **"Save as PDF"** แทนเครื่องพิมพ์

---

## 📞 ติดต่อ
HR Department — Nanyang Marketing Co., Ltd.
