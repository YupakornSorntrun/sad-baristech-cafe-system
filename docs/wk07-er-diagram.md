# Week 7 — ER Diagram (ฉบับปรับปรุงให้ตรงกับ schema ล่าสุด)

> ER Diagram ของระบบ Baristech Cafe POS ใช้เป็นต้นแบบของ `schema.sql`
> ตอนนี้มี **11 ตาราง** ตรงกับ `code/schema.sql` ที่ใช้งานจริง

ภาพ ER Diagram:

- ฉบับสัปดาห์ 7 (10 ตาราง, วาดด้วย MySQL Workbench): `images/wk07-er-diagram.png`
- **ฉบับปัจจุบัน (11 ตาราง)**: `images/wk07-er-diagram-final.png`

![ER Diagram ฉบับปัจจุบัน](images/wk07-er-diagram-final.png)

## สิ่งที่เพิ่มหลังสัปดาห์ 7 (ทำในสัปดาห์ 9–10)

| ส่วนที่เพิ่ม | ตาราง | เหตุผล |
|---|---|---|
| คอลัมน์ `image_url` | `menu_items` | แสดงรูปสินค้าบนหน้า POS |
| คอลัมน์ `username`, `password_hash` และค่า role `owner` | `employees` | ระบบล็อกอินและแยกสิทธิ์เจ้าของ/แคชเชียร์ |
| คอลัมน์ `queue_date`, `queue_no` (ไม่ซ้ำต่อสาขาต่อวัน) | `orders` | เลขคิวรายวัน A001, A002, ... |
| ตารางใหม่ `branch_daily_counters` | — | เก็บเลขคิวล่าสุดของแต่ละสาขาในแต่ละวัน |
| ค่า `voided` ใน `payment_status` และคอลัมน์ `branch_id` ใน `menu_items` (สัปดาห์ 9) | `orders`, `menu_items` | ยกเลิกบิลแบบไม่ลบข้อมูล และกันการเข้าถึงเมนูข้ามสาขา |

## Mermaid

```mermaid
erDiagram
    BRANCHES ||--o{ EMPLOYEES : "มีพนักงาน"
    BRANCHES ||--o{ MENU_ITEMS : "มีเมนู"
    BRANCHES ||--o{ ORDERS : "รับออเดอร์"
    BRANCHES ||--o{ BRANCH_DAILY_COUNTERS : "นับเลขคิว"
    EMPLOYEES ||--o{ ORDERS : "บันทึก"
    CATEGORIES ||--o{ MENU_ITEMS : "จัดกลุ่ม"
    ORDERS ||--|{ ORDER_ITEMS : "ประกอบด้วย"
    MENU_ITEMS ||--o{ ORDER_ITEMS : "ถูกสั่ง"
    MENU_ITEMS ||--o{ MENU_ITEM_INGREDIENTS : "ใช้สูตร"
    INGREDIENTS ||--o{ MENU_ITEM_INGREDIENTS : "ถูกใช้ใน"
    INGREDIENTS ||--o{ STOCK_MOVEMENTS : "มีประวัติ"
    ORDERS ||--o{ STOCK_MOVEMENTS : "ทำให้เกิด"
    ORDERS ||--o| RECEIPTS : "ออกใบเสร็จ"

    BRANCHES {
        int branch_id PK
        varchar name
        varchar address
    }
    EMPLOYEES {
        int employee_id PK
        int branch_id FK
        varchar name
        enum role "owner/barista/cashier"
        varchar username UK
        varchar password_hash
    }
    CATEGORIES {
        int category_id PK
        varchar name
    }
    MENU_ITEMS {
        int menu_id PK
        int branch_id FK
        int category_id FK
        varchar name
        decimal price
        varchar image_url
    }
    INGREDIENTS {
        int ingredient_id PK
        varchar name
        enum unit "g/ml/pcs"
        decimal stock_quantity
        decimal low_stock_threshold
    }
    MENU_ITEM_INGREDIENTS {
        int menu_id PK,FK
        int ingredient_id PK,FK
        decimal quantity_used
    }
    ORDERS {
        int order_id PK
        int branch_id FK
        int employee_id FK
        enum order_type "dine_in/takeaway"
        varchar table_number
        varchar payment_method
        enum payment_status "unpaid/paid/voided"
        decimal discount_amount
        decimal amount_received
        enum barista_status "pending/preparing/completed"
        datetime created_at
        date queue_date
        int queue_no
    }
    ORDER_ITEMS {
        int order_item_id PK
        int order_id FK
        int menu_id FK
        int quantity
        decimal unit_price
    }
    STOCK_MOVEMENTS {
        int movement_id PK
        int ingredient_id FK
        int order_id FK
        decimal quantity_change
        enum reason "sale/restock/adjustment"
        datetime moved_at
    }
    RECEIPTS {
        int receipt_id PK
        int order_id FK
        varchar receipt_number UK
        datetime printed_at
    }
    BRANCH_DAILY_COUNTERS {
        int branch_id PK,FK
        date queue_date PK
        int last_no
    }
```

หมายเหตุ: ยอดรวม ยอดสุทธิ และเงินทอนของออเดอร์ **ไม่เก็บในตาราง** `orders` (เป็นค่าที่คำนวณได้ จึงคำนวณสดจาก `order_items` เพื่อกันข้อมูลไม่ตรงกัน ตามหลัก Normalization 3NF)
