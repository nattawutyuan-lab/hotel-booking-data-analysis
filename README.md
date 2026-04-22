
## สมาชิกกลุ่ม

* 6610201013 ณัฐวุฒิ เคลือบทอง
* 66102010142 ธนัท กันหารี
* 66102010571 อนันตสิน ลาภวิสุทธิสาโรจน์

---

## เกี่ยวกับโปรเจกต์

โปรเจกต์นี้เป็นการวิเคราะห์ข้อมูลการจองห้องพักของโรงแรม (Hotel Booking) โดยใช้ชุดข้อมูลที่สร้างขึ้นผ่าน AI (AI-generated dataset) ในรูปแบบ **Star Schema** เพื่อจำลองสถานการณ์จริงของธุรกิจโรงแรม

โดยมุ่งเน้นการวิเคราะห์ในประเด็นสำคัญ เช่น:

* การเปรียบเทียบช่องทางการจอง (**OTA vs Direct**)
* ค่าคอมมิชชั่น (Commission)
* งบประมาณทางการตลาด (Marketing Spend)

---

## สมมติฐานการวิเคราะห์ (Hypotheses & Methods)

### 1. Direct Channel Profitability Advantage

**Hypothesis**
ช่องทาง **Direct (เว็บไซต์โรงแรม)** ทำกำไรสุทธิต่อห้อง (Net ADR) ได้สูงกว่าช่องทาง OTA อย่างชัดเจน

**Method**
ใช้คอลัมน์ `net_room_revenue` และ `channel_type` เพื่อคำนวณค่าเฉลี่ยรายได้สุทธิต่อการจอง

```
Net ADR (OTA)
= Sum(net_room_revenue) / Count(booking_id)
(เฉพาะ channel_type = 'OTA')

Net ADR (Direct)
= Sum(net_room_revenue) / Count(booking_id)
(เฉพาะ channel_type = 'Direct')
```

**Decision Rule**

```
Net ADR (Direct) - Net ADR (OTA)
```

* ถ้าค่ามากกว่า 0 อย่างมีนัยสำคัญ
  → ควรเพิ่มสัดส่วนการขายผ่าน Direct
  → เพราะสร้างกำไรสุทธิต่อห้องได้มากกว่า

---

### 2. Commission Model Efficiency

**Hypothesis**
ช่องทางที่คิดค่าคอมมิชชั่นแบบ **Flat Fee** คุ้มค่ากว่าแบบ **Percentage**

**Method**
ใช้คอลัมน์ `commission_amount`, `gross_room_revenue`, และ `commission_model`

```
Cost % (Flat Fee)
= [ Sum(commission_amount) / Sum(gross_room_revenue) ] × 100

Cost % (Percentage)
= [ Sum(commission_amount) / Sum(gross_room_revenue) ] × 100
```

**Decision Rule**

* เปรียบเทียบค่า Cost %
* ช่องทางที่มี % ต่ำกว่า = ต้นทุนต่ำกว่า
  → ควรส่งเสริมช่องทางนั้น

---

### 3. Weekend Commission Waste

**Hypothesis**
ในช่วงวันหยุดสุดสัปดาห์ (ศุกร์–เสาร์) โรงแรมจ่ายค่าคอมมิชชั่นให้ OTA มากเกินความจำเป็น ทั้งที่ Demand สูงอยู่แล้ว

---

**Method**

**(1) Weekend Occupancy %**

```
Weekend Occupancy %
= [ Count(booking_id) / Total Rooms Available ] × 100
```

เงื่อนไข:

* เฉพาะวันศุกร์–เสาร์
* status = 'Checked-Out'

---

**(2) Commission Waste**

```
Commission Waste
= Sum(commission_amount)
```

เงื่อนไข:

* channel_type = 'OTA'
* เฉพาะวันศุกร์–เสาร์

---

**Decision Rule**

* ถ้า Occupancy > 85%
  → แปลว่าห้องเกือบเต็ม (High Demand)

* ค่า Commission Waste
  → คือ “ต้นทุนที่สามารถลดได้”

---


# The Azure Stay Dilemma: Hotel Booking Data Analysis

## การวิเคราะห์ข้อมูลเพื่อปรับสมดุลช่องทางการจัดจำหน่ายและเพิ่มกำไรสุทธิให้กับโรงแรม Azure Stay

---

# Section 1: Strategic Alignment

## Background & Pain Points (ความท้าทายของโรงแรม)

ปัจจุบันโรงแรม **Azure Stay** กำลังเผชิญกับปัญหา **ต้นทุนการจัดจำหน่ายที่สูงเกินไป (High Distribution Costs)** แม้โรงแรมจะสามารถขายห้องพักได้และมียอดจอง (Volume) เข้ามาอย่างต่อเนื่อง แต่กลับต้องสูญเสียรายได้ไปกับค่าคอมมิชชันจำนวนมหาศาลให้กับตัวแทนขายออนไลน์ (OTAs เช่น Expedia หรือ Booking.com)

ส่งผลให้เกิดภาวะ **“รายได้สูง แต่กำไรสุทธิต่ำ” (Profit Margin Erosion)**

โปรเจกต์นี้จึงถูกสร้างขึ้นเพื่อวิเคราะห์เชิงลึก และระบุให้ชัดเจนว่า **ช่องทางใดสร้างกำไรสุทธิ (Net Revenue)** ให้กับธุรกิจอย่างแท้จริง เพื่ออุดรอยรั่วไหลของกำไร และเพิ่มประสิทธิภาพการขายห้องพัก

---

## SMART Objectives

### S — Specific

ปรับสมดุลสัดส่วนช่องทางการจัดจำหน่าย (**Channel Mix Optimization**) โดยลดการพึ่งพาช่องทาง OTA ที่มีค่าคอมมิชชันสูง และเพิ่มยอดจองผ่านช่องทางตรง (**Direct Channel**) หรือช่องทางต้นทุนต่ำ

### M — Measurable

* เพิ่ม **Net RevPAR** สูงขึ้น **10%**
* ลดต้นทุนค่าคอมมิชชันรวมลง **5%**

### A — Achievable

ใช้ข้อมูลเพื่อหา **Profitable Channels** และจัดสรรโควตาห้องพักใหม่ โดยลดห้องในช่องทางกำไรต่ำ และเพิ่มในช่องทางกำไรสูง

### R — Relevant

สอดคล้องกับเป้าหมายหลักของ Azure Stay คือ

* เพิ่มกำไรสุทธิสูงสุด (**Maximize Net Revenue**)
* ควบคุมต้นทุนการได้มาซึ่งลูกค้า (**Customer Acquisition Cost**)

### T — Time-bound

เริ่มดำเนินการและวัดผลสำเร็จได้ภายใน **1 ไตรมาส (3 เดือน)**

---

# Section 2: Analytical Design

## Hypothesis & Method (สมมติฐานและวิธีการวิเคราะห์)

---

## 1. ช่องทาง Direct ทำกำไรสุทธิต่อห้องสูงกว่า OTA

### สูตรคำนวณ

**Net ADR (OTA)**

```text
Sum(net_room_revenue) / Count(booking_id)
เฉพาะ channel_type = 'OTA'
```

**Net ADR (Direct)**

```text
Sum(net_room_revenue) / Count(booking_id)
เฉพาะ channel_type = 'Direct'
```

### จุดตัดสินใจ

หาก Direct สูงกว่าอย่างมีนัยสำคัญ ควรเพิ่มโควตาห้องพักให้ช่องทาง Direct

---

## 2. Flat Fee คุ้มค่ากว่า Percentage

### สูตรคำนวณ

**Cost % (Flat Fee)**

```text
[ Sum(commission_amount) / Sum(gross_room_revenue) ] × 100
เฉพาะ commission_model = 'Flat Fee'
```

**Cost % (Percentage)**

```text
[ Sum(commission_amount) / Sum(gross_room_revenue) ] × 100
เฉพาะ commission_model = 'Percentage'
```

### จุดตัดสินใจ

ช่องทางใดมีต้นทุนต่ำกว่า ควรผลักดันยอดจองผ่านช่องทางนั้น

---

## 3. ช่องทางที่ยกเลิกสูง ทำให้เสียโอกาสขายห้อง

### สูตรคำนวณ

```text
Cancellation Rate by Channel =
COUNT(booking_id WHERE status = 'Cancelled')
/
COUNT(Total booking_id)
GROUP BY channel_id
```

### จุดตัดสินใจ

ช่องทางที่ยกเลิกสูงควรถูกจำกัด Inventory หรือปรับนโยบายการจอง

---

# Section 3: Data Execution & EDA

ข้อมูลจำลองที่ใช้ในการวิเคราะห์ถูกออกแบบให้สอดคล้องกับธุรกิจโรงแรม ประกอบด้วย **4 ตารางหลัก**

---

## Table 1: fact_bookings (Transaction Data)

| Field              | Description                         |
| ------------------ | ----------------------------------- |
| booking_id (PK)    | รหัสการจอง                          |
| booking_date       | วันที่จอง                           |
| check_in_date      | วันที่เข้าพัก                       |
| channel_id (FK)    | อ้างอิงช่องทางการขาย                |
| rate_code_id (FK)  | อ้างอิงประเภทราคา                   |
| gross_room_revenue | รายได้ก่อนหักคอมมิชชัน              |
| commission_amount  | ค่าคอมมิชชัน                        |
| net_room_revenue   | รายได้สุทธิ                         |
| status             | Confirmed / Cancelled / Checked-Out |

---

## Table 2: dim_channels (Cost Definitions)

| Field                   | Description                      |
| ----------------------- | -------------------------------- |
| channel_id (PK)         | รหัสช่องทาง                      |
| channel_name            | ชื่อช่องทาง                      |
| channel_type            | OTA / Direct                     |
| commission_model        | Percentage / Flat Fee / Net Rate |
| default_commission_rate | อัตราค่าธรรมเนียม                |
| contract_owner          | ผู้ดูแลช่องทาง                   |

---

## Table 3: dim_rate_codes

| Field             | Description        |
| ----------------- | ------------------ |
| rate_code_id (PK) | รหัสราคา           |
| rate_name         | ชื่อราคา           |
| is_commissionable | มีคอมมิชชันหรือไม่ |

---

## Table 4: fact_marketing_spend

| Field           | Description           |
| --------------- | --------------------- |
| spend_id (PK)   | รหัสรายการใช้จ่าย     |
| spend_date      | วันที่ใช้จ่าย         |
| channel_id (FK) | ช่องทางอ้างอิง        |
| platform        | Google Ads / Facebook |
| cost_amount     | ค่าใช้จ่าย            |
| clicks          | จำนวนคลิก             |

---

# Section 4: Insights & Impact

---

## Insight 1: ต้นทุนแฝงที่กลืนกินกำไร

### The Commission Trap

แม้ OTA จะสร้างยอดจองสูงที่สุด แต่กำไรสุทธิลดลงอย่างมาก เพราะมีค่าคอมมิชชันสูงถึง **15–20%**

ในขณะที่ **Direct Web** ไม่มีค่าคอมมิชชัน และมีอัตราการยกเลิกต่ำกว่า

### Recommendation

เพิ่มงบการตลาดเพื่อกระตุ้น Direct Booking

### Action

* แจกโค้ดส่วนลด 5–10%
* ฟรีอาหารเช้า
* ปรับหน้าเว็บให้จองง่ายขึ้น
* ทำ A/B Testing

### Impact

* ลดต้นทุนระยะยาว
* เก็บ First-party Data ของลูกค้าได้เอง

---

## Insight 2: ปัญหาการยกเลิกจากราคาโปรโมชัน

### The Promotional Cancellation Trap

ราคาประเภท **Promotional** มีอัตราการยกเลิกสูงถึง **24.5%**

สะท้อนพฤติกรรมลูกค้ากลุ่ม “จองกั๊ก”

### Recommendation

ปรับเงื่อนไขราคาให้รัดกุมขึ้น

### Action

* เปลี่ยนเป็น Non-refundable
* ชำระมัดจำล่วงหน้า 30–50%

### Impact

* ลดการจองหลอก
* Forecast รายได้แม่นยำขึ้น
* ลดปัญหาห้องว่างกะทันหัน

---

## Insight 3: คุณภาพการจองแยกตามช่องทาง

### Booking Quality by Channel

ลูกค้ากลุ่ม **Corporate Rate** และ **B2B / Wholesalers** มีอัตรายกเลิกต่ำ และจองสม่ำเสมอตลอดปี

### Recommendation

เพิ่มความร่วมมือกับกลุ่มองค์กร

### Action

* ทำสัญญากับบริษัทต่าง ๆ
* ทำ Tiered Pricing
* ขยายผ่าน GDS / Hotelbeds

### Impact

* กระแสเงินสดมั่นคง
* รักษารายได้ช่วง Low Season
* ลดการพึ่งพาลูกค้ารายย่อย

---


