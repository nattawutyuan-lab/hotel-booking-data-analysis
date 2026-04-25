## สมาชิกกลุ่ม
* **6610201013** ณัฐวุฒิ เคลือบทอง
* **66102010142** ธนัท กันหารี
* **66102010571** อนันตสิน ลาภวิสุทธิสาโรจน์

---

## เกี่ยวกับโปรเจกต์ (Project Overview)
**Background & Pain Points**
ปัจจุบันโรงแรม **Azure Stay** กำลังเผชิญกับปัญหา **"ต้นทุนการจัดจำหน่ายที่สูงเกินไป (High Distribution Costs)"** แม้โรงแรมจะสามารถขายห้องพักได้และมียอดจอง (Volume) เข้ามาอย่างต่อเนื่อง แต่กลับต้องสูญเสียรายได้ไปกับค่าคอมมิชชันจำนวนมหาศาลให้กับตัวแทนขายออนไลน์ (OTAs) ส่งผลให้เกิดภาวะ **“รายได้สูง แต่กำไรสุทธิต่ำ” (Profit Margin Erosion)**

โปรเจกต์นี้เป็นการวิเคราะห์ข้อมูลการจองห้องพักของโรงแรม โดยใช้ชุดข้อมูลจำลอง (AI-generated dataset) ในรูปแบบ **Star Schema** เพื่อระบุให้ชัดเจนว่า **ช่องทางใดสร้างกำไรสุทธิ (Net Revenue)** ให้กับธุรกิจอย่างแท้จริง เพื่ออุดรอยรั่วไหลของกำไร และเพิ่มประสิทธิภาพการจัดสรรช่องทางการขาย (Channel Mix Optimization)

---

## วัตถุประสงค์ (SMART Objectives)
* **Specific:** ปรับสมดุลสัดส่วนช่องทางการจัดจำหน่าย ลดการพึ่งพา OTA ที่มีค่าคอมมิชชันสูง และเพิ่มยอดจองผ่านช่องทางตรง (Direct Channel)
* **Measurable:** เพิ่ม Net RevPAR สูงขึ้น **10%** และลดต้นทุนค่าคอมมิชชันรวมลง **5%**
* **Achievable:** ใช้ Data หา Profitable Channels เพื่อจัดสรรโควตาห้องพักใหม่
* **Relevant:** สอดคล้องกับเป้าหมายหลักคือ Maximize Net Revenue และ Optimize Customer Acquisition Cost (CAC)
* **Time-bound:** เริ่มดำเนินการและวัดผลสำเร็จได้ภายใน **1 ไตรมาส (3 เดือน)**

---

## สมมติฐานและวิธีการวิเคราะห์ (Hypotheses & Methods)

### 1. Direct Channel Profitability Advantage
* **สมมติฐาน:** ช่องทาง Direct ทำกำไรสุทธิต่อห้อง (Net ADR) ได้สูงกว่าช่องทาง OTA อย่างมีนัยสำคัญ
* **วิธีวิเคราะห์:** เปรียบเทียบ `Net ADR (OTA)` กับ `Net ADR (Direct)` 
* **เกณฑ์ตัดสินใจ:** หากส่วนต่างของ Net ADR (Direct) สูงกว่าชัดเจน ควรดึงโควตาห้องพักมาฝั่ง Direct ให้มากขึ้น

### 2. Commission Model Efficiency
* **สมมติฐาน:** การจ่ายค่าคอมมิชชันแบบ **Flat Fee** ต้นทุนต่ำกว่าและคุ้มค่ากว่าแบบ **Percentage**
* **วิธีวิเคราะห์:** คำนวณ `Cost %` ของแต่ละโมเดล `[Sum(commission_amount) / Sum(gross_room_revenue)] × 100`
* **เกณฑ์ตัดสินใจ:** โมเดลใดมีต้นทุน % ต่ำกว่า ควรหาทางเจรจาหรือผลักดันยอดจองให้ไปตกที่โมเดลนั้น

### 3. Weekend Commission Waste
* **สมมติฐาน:** ในช่วงวันหยุดสุดสัปดาห์ (ศุกร์–เสาร์) ที่ Demand สูง โรงแรมจ่ายค่าคอมมิชชันให้ OTA มากเกินความจำเป็น
* **วิธีวิเคราะห์:** ดู `Weekend Occupancy %` หาก > 85% (High Demand) ให้คำนวณ `Commission Waste` จากยอด OTA
* **เกณฑ์ตัดสินใจ:** หาก Demand สูงจนเต็มอยู่แล้ว ควรลดโควตา OTA ในช่วง Weekend เพื่อลด Commission Waste

### 4. Cancellation Impact 
* **สมมติฐาน:** ช่องทางหรือประเภทราคาที่มีการยกเลิก (Cancellation) สูง ทำให้โรงแรมเสียโอกาสในการขาย
* **วิธีวิเคราะห์:** คำนวณ `Cancellation Rate by Channel/Rate` `(Cancelled / Total Bookings)`
* **เกณฑ์ตัดสินใจ:** หากช่องทางใด Rate การยกเลิกสูง ควรจำกัด Inventory หรือปรับนโยบายการจองให้รัดกุมขึ้น

---

## โครงสร้างข้อมูล (Data Execution & Schema)
ชุดข้อมูลประกอบด้วย 4 ตารางหลักที่จำลองในรูปแบบ **Star Schema**:

### Table 1: `fact_bookings` (Transaction Data)
| Field | Description |
|---|---|
| `booking_id` (PK) | รหัสการจอง |
| `booking_date` | วันที่จอง |
| `check_in_date` | วันที่เข้าพัก |
| `channel_id` (FK) | อ้างอิงช่องทางการขาย |
| `rate_code_id` (FK)| อ้างอิงประเภทราคา |
| `gross_room_revenue`| รายได้ก่อนหักคอมมิชชัน |
| `commission_amount` | ค่าคอมมิชชัน |
| `net_room_revenue` | รายได้สุทธิ |
| `status` | Confirmed / Cancelled / Checked-Out |

### Table 2: `dim_channels` (Cost Definitions)
| Field | Description |
|---|---|
| `channel_id` (PK) | รหัสช่องทาง |
| `channel_name` | ชื่อช่องทาง |
| `channel_type` | OTA / Direct / Wholesale |
| `commission_model` | Percentage / Flat Fee / Net Rate / Merchant |
| `default_commission_rate` | อัตราค่าธรรมเนียม |

### Table 3: `dim_rate_codes` & Table 4: `fact_marketing_spend`
* **`dim_rate_codes`**: เก็บข้อมูลรหัสราคา (`rate_code_id`), ชื่อราคา, และเงื่อนไขว่าคิดคอมมิชชันหรือไม่ (`is_commissionable`)
* **`fact_marketing_spend`**: เก็บข้อมูลค่าใช้จ่ายทางการตลาด ประกอบด้วย วันที่, รหัสช่องทางโฆษณา (Google Ads / Facebook), ค่าใช้จ่าย (`cost_amount`), และจำนวนคลิก (`clicks`)

---

## สรุปผลวิเคราะห์และข้อเสนอแนะ (Insights & Impact)

### Insight 1: The Commission Trap (ต้นทุนแฝงที่กลืนกินกำไร)
* **ปัญหา:** แม้ OTA จะสร้างยอดจองสูงที่สุด แต่กำไรสุทธิลดลงอย่างมาก เพราะมีค่าคอมมิชชันสูงถึง **15–20%** ในขณะที่ Direct Web ไม่มีค่าคอมมิชชัน และมีอัตราการยกเลิกต่ำกว่า
* **Action:** * จัดสรรงบการตลาดกระตุ้น Direct Booking มากขึ้น
  * ทำแคมเปญแจกโค้ดส่วนลด 5–10% หรือแถมฟรีอาหารเช้า (Value Add) สำหรับลูกค้าที่จองตรง
  * ปรับปรุงหน้าเว็บไซต์ให้ทำรายการง่ายขึ้น
* **Impact:** ลดต้นทุนในระยะยาว และเก็บ First-party Data ของลูกค้าได้เอง

### Insight 2: The Promotional Cancellation Trap (ปัญหาการยกเลิกจากราคาโปรโมชัน)
* **ปัญหา:** ราคาประเภท **Promotional** มีอัตราการยกเลิกสูงถึง **24.5%** สะท้อนพฤติกรรมลูกค้ากลุ่ม “จองกั๊ก”
* **Action:** * ปรับเงื่อนไขราคาโปรโมชันให้รัดกุมขึ้น เช่น เปลี่ยนเป็น **Non-refundable** * บังคับชำระมัดจำล่วงหน้า 30–50%
* **Impact:** ลดปริมาณการจองหลอก, Forecast รายได้แม่นยำขึ้น และลดปัญหาห้องว่างกะทันหันเสียโอกาสในการขาย

### Insight 3: Booking Quality by Channel (คุณภาพการจองของกลุ่ม B2B)
* **ปัญหา/โอกาส:** พบว่าลูกค้ากลุ่ม **Corporate Rate** และ **B2B / Wholesalers** มีอัตราการยกเลิกต่ำที่สุด และมียอดจองสม่ำเสมอตลอดทั้งปี
* **Action:** * เพิ่มความร่วมมือและทำสัญญาระยะยาวกับบริษัท/องค์กรต่างๆ
  * ทำ Tiered Pricing จูงใจพาร์ทเนอร์
  * ขยายช่องทางเครือข่ายผ่าน GDS หรือ Hotelbeds
* **Impact:** สร้างกระแสเงินสดที่มั่นคง ช่วยพยุงรายได้ในช่วง Low Season และลดการพึ่งพาลูกค้ารายย่อยจาก OTA เพียงอย่างเดียว
