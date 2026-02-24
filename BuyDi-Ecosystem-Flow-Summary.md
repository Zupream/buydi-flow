# BuyDi Ecosystem & Core Loop Architecture

ระบบ Fulloop ครบจบทุก Flow ตั้งแต่ Buyer, Seller, Affiliate จนถึง Admin

---

## 1. Ecosystem Overview — 4 Roles

| Role | หน้าที่หลัก | Core Loop |
|------|------------|-----------|
| **Buyer** | เลือกสินค้า (ผ่าน Aff Link), สั่งซื้อ, ชำระเงิน, ติดตามสถานะ, คืนสินค้า/ยกเลิก | Traffic → Payment |
| **Seller** | สมัคร (ข้อมูลร้าน/ภาษี/ธนาคาร/บัตร/ใบอนุญาต), ลงสินค้า, จัดส่ง, ถอนเงิน | Register → Revenue |
| **Affiliate** | สมัคร → Admin อนุมัติ → สร้าง Aff Link → แชร์ → รับ Commission → ถอนเงิน | Share → Commission |
| **Admin** | อนุมัติ Vendor/Affiliate, ตรวจสินค้า, จัดการถอนเงิน/Payout, ดูแลระบบ | Verify → Payout |

---

## 2. Buyer Journey

```
สินค้า (Aff Link) → Add To Cart → Checkout (กรอกที่อยู่ + เลือกขนส่ง) → ชำระเงิน
    → ติดตามออเดอร์ → รับสินค้า (Delivered) → Order Completed
```

---

## 3. Order Status State Machine

### Happy Path

```
Pending Payment → Pending → Processing → Shipped → Delivered → Completed
```

### Branching States

- **Cancelled**
  - Buyer/Seller ยกเลิกก่อน Processing
  - Timeout ไม่ชำระภายในเวลา

- **Return Flow**
  - Return Approval → Waiting Product → Product Returned → Return Success

- **Debate (ไกล่เกลี่ย)**
  - เมื่อ Seller ไม่อนุมัติคืนสินค้า
  - Admin ตัดสิน

---

## 4. Return, Refund & Dispute Resolution

### ขั้นตอนหลัก

```
Buyer ยื่นคำร้อง → System ตรวจสอบ → Seller ตอบรับ/ปฏิเสธ
```

### กรณี Seller ตอบรับ

```
Buyer ส่งคืนสินค้า → Seller ยืนยันรับ → Refund ดำเนินการ
```

### กรณี Seller ปฏิเสธ → Dispute

```
Admin Review → ตรวจหลักฐาน → ตัดสินผล
```

| ผลลัพธ์ | รายละเอียด |
|---------|-----------|
| **Buyer ชนะ** | Full/Partial Refund, Seller ถูกเตือน/แบน |
| **Seller ชนะ** | ปิดเคส ไม่คืนเงิน, แจ้ง Buyer เหตุผล |

---

## 5. Seller Registration & Verification Pipeline

### ขั้นตอนสมัคร

```
สมัครร้านค้า (ข้อมูลร้าน/ภาษี/ธนาคาร/บัตร/ใบอนุญาต) → Admin Review → Approved/Rejected
```

### ขั้นตอนลงสินค้า

```
ลงสินค้า/แก้ไขสินค้า → Admin ตรวจสอบ → Approved → เปิดขาย
```

### เอกสารที่ต้องยื่น

| หมวด | รายละเอียด |
|------|-----------|
| ข้อมูลร้าน | ชื่อร้าน, หมวดหมู่สินค้า, ที่อยู่ |
| ภาษี/ธนาคาร | เลขประจำตัวผู้เสียภาษี, บัญชีธนาคาร |
| บัตร/ใบอนุญาต | บัตรประชาชน, ใบอนุญาตหลักฐาน |

---

## 6. Seller Fulfillment & Revenue Trigger

### ขั้นตอนจัดส่ง

```
รับ Order → ยืนยันออเดอร์ → Pack สินค้า → ส่งพัสดุ
    → Tracking Update → Delivered → Hold Period (3 วัน) → Revenue Released
```

### Revenue Deductions (สูตรคำนวณ)

```
Revenue = Order Amount
         − Platform Fee %
         − Payment Gateway Fee
         − Affiliate Commission (ถ้ามี)
         = Net Seller Income
```

---

## 7. Affiliate User Journey & Attribution

### เส้นทาง Affiliate

```
สมัคร Affiliate → Admin อนุมัติ → สร้าง Affiliate Link → แชร์/โปรโมท
    → User คลิกลิงก์ → Cookie Attribution → User ซื้อสินค้า
    → Order Completed → Commission คำนวณ → Withdraw
```

### Attribution Model

| Model | รายละเอียด |
|-------|-----------|
| **Last Click** | คลิกสุดท้ายก่อนซื้อ ได้ Commission ทั้งหมด |
| **Cookie Window** | 30 วัน — ถ้าซื้อภายใน 30 วันหลังคลิก |

---

## 8. Commission Lifecycle State Machine

### Happy Path

```
Create Order → Order Completed → อนุมัติ → ได้ Balance → ถอน
```

### Exceptions

| กรณี | ผลลัพธ์ |
|------|--------|
| **Order Cancelled** | ไม่อนุมัติ Commission |
| **ถอนเงิน** | Admin อนุมัติ → จ่ายแล้ว / Admin ไม่อนุมัติ → คืน Balance |

---

## 9. Withdrawal & Payout Flow

### Withdrawal Sources

| Source | Trigger | ประเภท |
|--------|---------|--------|
| **Wallet Withdrawal** | Affiliate ถอน Commission / Seller ถอนจาก Wallet | Manual → Admin Review |
| **Buyer Triggers** | Cancel (จ่ายแล้ว) / Return success / Dispute ชนะ | Auto Approved |
| **Seller Triggers** | Seller cancel (buyer จ่ายแล้ว) / Accept return / Dispute Buyer wins | Auto Approved |

### Admin Review (Wallet Withdrawal)

```
Pending Approval → Admin Reviews → Approve หรือ Reject (คืน Wallet)
```

### Admin Payout Process

```
Approved → Create Batch → Export to Bank → Bank โอนเงิน → Close Batch
```

### After Close Batch

| ผลลัพธ์ | รายละเอียด |
|---------|-----------|
| **Transfer OK** | Completed + Payment Voucher |
| **Transfer Failed** | กลับเป็น Approved รอรอบถัดไป |

---

## 10. Non-COD Shipping Logic & Weight Check

### ขั้นตอนหลัก

```
Seller ยืนยันจัดส่ง → ขนส่งรับพัสดุ → ตรวจน้ำหนัก
```

### Decision: Actual vs Estimated Weight

| กรณี | ผลลัพธ์ |
|------|--------|
| **น้ำหนักตรงกัน** | RS Document → ส่งปกติ |
| **Actual > Estimated** | Seller จ่ายส่วนต่าง → RSA Document |
| **Actual < Estimated** | ส่วนต่าง = BuyDi Revenue |

### After Delivery

```
สำเร็จ → Delivered → Completed (3 วัน) → Credit to Seller Wallet
```

### กรณีส่งไม่สำเร็จ

```
ส่งไม่สำเร็จ → Retry? → คืนสินค้าให้ Seller → ยกเลิก/คืนเงิน Buyer
```

---

## 11. COD Shipping & Money Reconciliation

### ขั้นตอนหลัก

```
COD Order → จัดส่ง → Buyer จ่ายเงิน COD
    → ขนส่งเก็บเงิน → ปณ.โอน COD ให้ BuyDi → Generate Transaction ID + Validate
    → Update Status → Waiting COD Settlement → Order Completed → หักค่าขนส่ง → Available for Withdrawal
```

### COD Risks

| ความเสี่ยง | รายละเอียด |
|------------|-----------|
| **ปฏิเสธรับพัสดุ** | ส่งคืน Seller — ค่าส่งไป-กลับ หัก Seller |
| **Remittance Delay** | ขนส่งโอนช้า 3-7 วัน ระบบต้อง track |
| **ยอดไม่ตรง** | Auto-flag + Manual review ถ้ายอดไม่ match |
| **ยกเลิก COD** | ไม่มี Refund (ยังไม่จ่ายเงิน) |

---

## 12. Shipping Cost Adjustment Decision Tree

เมื่อ GetShip Webhook ได้น้ำหนักจริง:

| กรณี | Document | ผลลัพธ์ |
|------|----------|--------|
| **1. น้ำหนักตรง** | RS Document | ไม่ต้องปรับ |
| **2. Actual > Estimated** | RS + RSA Document | หักจาก Seller Wallet, แจ้ง Seller |
| **3. Actual < Estimated** | RS Document | ส่วนต่าง = BuyDi Revenue, ไม่ต้องแจ้ง Seller |

---

## 13. Admin Gatekeeper & Verification Pipelines

| Pipeline | รายละเอียด | ประเภท |
|----------|-----------|--------|
| **Seller Verification** | ตรวจเอกสาร ID / ใบอนุญาต → Approve / Reject | Manual |
| **Affiliate Verification** | ตรวจ Social Media / Bank → Approve / Reject | Manual |
| **Product Approval** | ตรวจสินค้า / คุณภาพ / ข้อมูล → Approve / Reject | Manual |
| **Withdrawal Approval** | ตรวจสอบยอดถอน ≥ threshold → Manual review สำหรับยอดสูง | Manual |
