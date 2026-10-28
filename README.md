# โครงงาน: ระบบจัดการร้านกาแฟ (Cafe Ordering System) — Baristech Cafe POS

ระบบหน้าร้าน (POS) สำหรับรับออเดอร์ การจัดการสต็อกวัตถุดิบ และรายงานยอดขาย ของร้านกาแฟหลายสาขา งานทุกชิ้นทำเป็นกลุ่ม (5 คน) ตั้งแต่สัปดาห์ที่ 1 และพัฒนาต่อเนื่องกันไปจนถึงวันนำเสนอในสัปดาห์ที่ 15 สิ่งที่วิเคราะห์หรือออกแบบไว้ในแต่ละสัปดาห์ จะถูกนำไปต่อยอดเป็นโค้ดที่ทำงานได้จริงในสัปดาห์ถัดไปทันที

## สมาชิกกลุ่ม BarisTech

| รหัสนิสิต | ชื่อ-นามสกุล |
|:-----|:-----|
| 67160026 | นางสาวฑิมพิกา ภุมรินทร์ |
| 67160046 | นางสาวอภิชญา กันประดับ |
| 67160389 | นางสาวจิดาภา สิทธะนะ |
| 67160401 | นางสาวปภาวรินทร์ ทัศนางกูร |
| 67160408 | นางสาวยุพากร สอนเถื่อน |

## เอกสารโครงงาน

เริ่มอ่านจาก **[Project Report (เอกสารรวมเล่ม)](docs/project-report.pdf)** ([ไฟล์ Word](docs/project-report.docx)) แล้วดูเอกสารต้นทางของแต่ละสัปดาห์ได้ตามตารางนี้

| สัปดาห์ | เอกสาร |
|:--|:--|
| 1 | [Team Charter](docs/wk01-team-charter.md) |
| 2 | [Feasibility Report + BPMN As-Is / To-Be](docs/wk02-feasibility-report.md) |
| 3 | [Requirements (บันทึกสัมภาษณ์ + FR/NFR)](docs/wk03-requirements.md) |
| 4 | [User Stories + Use Case](docs/wk04-user-stories.md) |
| 6 | [Class Diagram](docs/wk06-class-diagram.png) · [Object Diagram](docs/wk06-object-diagram.png) |
| 7 | ER Diagram: [ภาพฉบับสัปดาห์ 7](docs/wk07-er-diagram.png) · [ภาพฉบับปัจจุบัน 11 ตาราง](docs/images/wk07-er-diagram-final.png) · [คำอธิบาย](docs/wk07-er-diagram.md) · [Schema สัปดาห์ 7](docs/wk07-schema.sql) |
| 9 | [Sequence Diagram](docs/wk09-sequence-diagram.png) · [Business Rule Flowchart](docs/images/wk09-business%20rule-flowchart.png) |
| 10 | [Input/Output Design](docs/wk10-input-output-design.docx) · [Wireframe](docs/wk10-wireframe.docx) · [SRS ฉบับสมบูรณ์](docs/wk10-srs.md) |
| 11 | [Architecture (4+1 View)](docs/wk11-architecture.md) |
| 12 | [Usability Testing](docs/wk12-usability-testing.md) |
| 13 | ระบบที่รวมแล้ว: โฟลเดอร์ [code/](code/) · [AI Disclosure สัปดาห์ 13](docs/ai-disclosure/wk13-ai-disclosure.md) |
| 14 | [รายงานตรวจสอบคุณภาพและความปลอดภัย](docs/wk14-quality-review.md) |


### AI Disclosure

[สัปดาห์ 5](docs/ai-disclosure/wk05-ai-disclosure.md) · [สัปดาห์ 7](docs/ai-disclosure/wk07-ai-disclosure.md) · [สัปดาห์ 9](docs/ai-disclosure/wk09-ai-disclosure.md) · [สัปดาห์ 10](docs/ai-disclosure/wk10-ai-disclosure.md) · [สัปดาห์ 13](docs/ai-disclosure/wk13-ai-disclosure.md) (ของสัปดาห์ 12 อยู่ท้ายไฟล์ [Usability Testing](docs/wk12-usability-testing.md))

## โครงสร้างโฟลเดอร์

```
sad-baristech-cafe-system/
├── code/          โค้ดระบบทั้งหมด (Node.js + Express + MySQL)
│   ├── src/         routes → controllers → models
│   ├── public/      หน้าเว็บ (HTML/CSS/JavaScript)
│   ├── tests/quality-review/   สคริปต์ตรวจสอบคุณภาพ (สัปดาห์ 14)
│   ├── schema.sql   สร้างฐานข้อมูล 11 ตาราง
│   └── seed.sql     ข้อมูลทดลอง (2 สาขา)
├── docs/          เอกสารรายสัปดาห์ รูป diagram และเอกสารรวมเล่ม
│   ├── images/
│   ├── ai-disclosure/
│   ├── work-summary/
│   └── code-guide/
└── README.md
```

## วิธีรันระบบ

1. ติดตั้ง Node.js และ MySQL แล้วสร้างฐานข้อมูล: นำเข้า `code/schema.sql` แล้วตามด้วย `code/seed.sql` (รายละเอียดใน [DATABASE_SETUP.md](code/DATABASE_SETUP.md))
2. คัดลอก `code/.env.example` เป็น `code/.env` แล้วใส่รหัสผ่าน MySQL และ `JWT_SECRET`
3. รันคำสั่งในโฟลเดอร์ `code/`

```bash
npm install
npm start
```

4. เปิด `http://localhost:3000/login.html`

| บัญชีทดลอง | ชื่อผู้ใช้ / รหัสผ่าน | เข้าได้ |
|---|---|---|
| เจ้าของร้าน | `owner` / `owner123` | จัดการเมนู สต็อก รายงาน (ทุกสาขา) |
| แคชเชียร์ สาขาบางแสน | `cashier1` / `cashier123` | POS และประวัติออเดอร์ของสาขาบางแสน |
| แคชเชียร์ สาขาชลบุรี | `cashier2` / `cashier123` | POS และประวัติออเดอร์ของสาขาชลบุรี |

## สถานะของระบบ

- ทำงานครบ flow สั่งออเดอร์ → ชำระเงิน → ใบเสร็จ → รายงานยอดขาย ผ่านการทดสอบ usability (สัปดาห์ 12) และการตรวจสอบคุณภาพ/ความปลอดภัย (สัปดาห์ 14)
- **ข้อจำกัดที่ทราบ:** ยังไม่มีหน้าจอฝั่งบาริสต้า (FR-03), วัตถุดิบใช้ร่วมกันทุกสาขา, ความเสี่ยงระดับ Low ที่ยังไม่ได้แก้ตามแผนใน [wk14-quality-review.md](docs/wk14-quality-review.md)
