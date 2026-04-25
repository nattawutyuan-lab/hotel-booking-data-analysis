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
S (Specific): ปรับสัดส่วนช่องทางการจัดจำหน่ายห้องพักโดยเพิ่มการเข้าพักของ Direct Booking


M (Measurable): เพิ่ม Net RevPAR 10%, ลดค่า Commission&Marketing รวมกัน 15%


A (Achievable): ใช้ข้อมูลการเข้าพักย้อนหลังเพื่อทำ Customer Segmentation และระบุกลุ่มลูกค้าที่มีโอกาสจองตรงสูง


R (Relevant): เพื่อลดการพึ่งพา OTA และเพิ่มกำไรสุทธิสูงสุด


T (Time-bound): วัดผลภายใน 1 ไตรมาส (3 เดือน)

---

## สมมติฐานและวิธีการวิเคราะห์ (Hypotheses & Methods)

### 1. Hypothesis 1: ช่องทาง Direct ให้ Net ADR สูงกว่า OTA
* **What to explore:** ต้องการเปรียบเทียบว่าช่องทางการขายช่องทางใดสร้างรายได้สุทธิต่อการจองได้สูงกว่าหลังจากหักค่า commission และ marketing เพื่อประเมินประสิทธิภาพเชิงกำไรของแต่ละช่องทาง 
* **Why this chart is appropriate:** ใช้ Bar Chart เนื่องจากเหมาะสำหรับการเปรียบเทียบค่าระหว่างกลุ่มอย่างชัดเจน ทำให้เห็นความแตกต่างของ Net ADR ระหว่างแต่ละ channel ได้ทันที และช่วยให้ตีความได้ง่ายจากความสูงของแท่งกราฟ
  <img width="715" height="492" alt="image" src="https://github.com/user-attachments/assets/616822b7-4b44-4be8-9131-1af63c1d3582" />


### 2. Hypothesis 2: Model ค่า Commission แบบ Flat Fee  คุ้มค่ากว่าแบบ Percentage
* **What to explore:** วิเคราะห์ว่า Model ค่าคอมมิชชั่นแบบ Flat Fee และ Percentage มีสัดส่วนต้นทุนต่อรายได้ แตกต่างกันอย่างไร เพื่อเข้าใจว่า Model ใดมี impact ต่อรายได้มากกว่า
* **Why this chart is appropriate:** ใช้ Bar chart เพื่อเปรียบเทียบสัดส่วนระหว่าง Model ทำให้เห็นชัดว่า Model ใดมีต้นทุนสูงกว่ากันเมื่อเทียบกับรายได้รวม
  <img width="784" height="483" alt="image" src="https://github.com/user-attachments/assets/2ef06c78-66c8-4537-adf6-3fe4eb149956" />


### 3. Hypothesis 3: วิเคราะห์พฤติกรรมการเข้าพักและผลตอบแทนตามช่วงเวลา
* **What to explore:**  ต้องการวิเคราะห์พฤติกรรมการเข้าพักระหว่าง "วันธรรมดา" และ "วันหยุด" ว่ามีความแตกต่างกันอย่างไร ทั้งในปริมาณการจองผ่านแต่ละช่องทาง และความสามารถในการทำกำไรต่อห้อง เพื่อใช้เป็นแนวทางในแบ่งกลุ่มลูกค้า
* **Why this chart is appropriate:** ใช้ Bar Chart 2 กราฟควบคู่กัน โดยกราฟแรกแสดงระดับความเสี่ยง และกราฟที่สองแสดงผลกระทบทางธุรกิจ ทำให้สามารถวิเคราะห์ได้ทั้งเชิงสัดส่วนและมูลค่าพร้อมกันอย่างครบถ้วน 
<img width="1783" height="584" alt="Hypothesis 3 Weekend vs Weekday Performance" src="https://github.com/user-attachments/assets/b4877af3-557c-4610-8fe0-07c7c2abcb68" />

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

### Table 3: `dim_rate_codes` (Rate Conditions)
| Field | Description |
|---|---|
| `rate_code_id` (PK) | รหัสราคา |
| `rate_name` | ชื่อราคา |
| `is_commissionable` | เงื่อนไขว่าคิดคอมมิชชันหรือไม่ |

### Table 4: `fact_marketing_spend` (Marketing Costs)
| Field | Description |
|---|---|
| `spend_id` (PK) | รหัสรายการใช้จ่าย |
| `spend_date` | วันที่ใช้จ่าย |
| `channel_id` (FK) | รหัสช่องทางโฆษณา (Google Ads / Facebook) |
| `cost_amount` | ค่าใช้จ่ายทางการตลาด |
| `clicks` | จำนวนคลิกที่ได้รับ |
---

## การสำรวจและวิเคราะห์ข้อมูล (Exploratory Data Analysis - EDA)

เพื่อตอบสมมติฐานและทำความเข้าใจพฤติกรรมข้อมูล ได้ทำการวิเคราะห์ผ่าน Data Visualization ดังนี้:

### 1. True Net ADR Comparison (Direct vs OTA)
* **เป้าหมาย:** เจาะลึกเปรียบเทียบกำไรต่อบุ๊กกิ้ง (Net ADR) ระหว่างช่องทาง Direct และ OTA โดยหักลบ "ต้นทุนแฝงทั้งหมด" ได้แก่ ค่าคอมมิชชันและค่าการตลาด (Marketing Spend / CAC)
* **การนำเสนอ:** ใช้ **Bar Chart** เปรียบเทียบเพื่อให้เห็นภาพชัดเจนว่าเมื่อนำต้นทุนค่าโฆษณาเฉลี่ยเข้าไปในแต่ละบุ๊กกิ้งแล้ว กำไรสุทธิต่อห้องที่แท้จริงเปลี่ยนไปอย่างไร
<img width="984" height="584" alt="image" src="https://github.com/user-attachments/assets/bdeca9c0-22cf-4791-a77e-cdfe7dc7e3ab" />

### 2. Final Net Profit by Channel (After ALL Costs)
* **เป้าหมาย:** หาผลกำไรที่แท้จริง (Contribution Margin) ของแต่ละช่องทาง โดยนำรายได้รวมมาหักออกด้วยต้นทุนผันแปรทั้งหมด
* **การนำเสนอ:** ใช้ **Bar Chart** แบบเรียงลำดับ (Sorted) เพื่อระบุ "ผู้ชนะ" ในเชิงกำไรอย่างชัดเจน ช่วยให้ผู้บริหารตัดสินใจได้ทันทีว่าควรจัดสรรงบลงทุนในช่องทางใดต่อ
<img width="984" height="584" alt="image" src="https://github.com/user-attachments/assets/cf2b36fb-ab05-4b65-88dd-4391b9875d8a" />

### 3. High Cancellation Channels Cause Revenue Loss (Cancellation Impact)
* **เป้าหมาย:** ระบุช่องทางที่มีอัตราการยกเลิก (Cancellation Rate) สูง และประเมินการสูญเสียรายได้ (Lost Revenue) เพื่อหาช่องทางที่มีความเสี่ยงสูง (High-risk channel)
* **ผลวิเคราะห์เบื้องต้น:** พบว่า OTA ยักษ์ใหญ่อย่าง Booking.com (30.8%) และ Expedia (28.6%) มีอัตราการยกเลิกสูงสุด นำไปสู่ความเสียหายด้านโอกาสทางรายได้มหาศาล ในขณะที่ Direct Web มีการยกเลิกเพียง 6.5%
* **การนำเสนอ:** ใช้ **Bar Chart 2 กราฟควบคู่กัน** กราฟแรกแสดงระดับความเสี่ยง (Cancellation Rate) และกราฟที่สองแสดงผลกระทบทางธุรกิจ (Lost Revenue) เพื่อมิติการวิเคราะห์ที่ครบถ้วน
<img width="984" height="584" alt="image" src="https://github.com/user-attachments/assets/cf2b36fb-ab05-4b65-88dd-4391b9875d8a" />
<img width="984" height="584" alt="image" src="https://github.com/user-attachments/assets/f753ac54-d8cc-4a00-9bd0-563f573eb8e0" />
### 4. Booking Volume and Quality by Channel
* **เป้าหมาย:** ตรวจสอบ "คุณภาพของการจอง" โดยเปรียบเทียบปริมาณการจองที่เข้าพักสำเร็จ (Checked-Out) กับการจองที่ถูกยกเลิก (Cancelled)
* **การนำเสนอ:** ใช้ **Grouped Bar Chart** เผยให้เห็น Insight สำคัญว่า ช่องทางที่สร้าง Volume ยอดจองได้สูงสุด มักเป็นช่องทางที่มีการยกเลิกสูงสุดตามไปด้วย สะท้อนพฤติกรรมการ "จองแบบเผื่อเลือก" ของฝั่ง OTA เมื่อเทียบกับ Direct Web ที่ลูกค้ามีความตั้งใจเข้าพักจริงมากกว่า
<img width="1184" height="684" alt="image" src="https://github.com/user-attachments/assets/91dcf2d1-5458-47bd-832c-0377ba32b768" />

---

## สรุปผลวิเคราะห์และข้อเสนอแนะ (Insights & Impact)

### Insight 1: The Commission Trap (ต้นทุนแฝงที่กลืนกินกำไร)
* **ปัญหา:** แม้ OTA จะสร้างยอดจองสูงที่สุด แต่กำไรสุทธิลดลงอย่างมาก เพราะมีค่าคอมมิชชันสูงถึง **15–20%** ในขณะที่ Direct Web ไม่มีค่าคอมมิชชัน และมีอัตราการยกเลิกต่ำกว่า
* **Action:** * จัดสรรงบการตลาดกระตุ้น Direct Booking มากขึ้น
  * ทำแคมเปญแจกโค้ดส่วนลด 5–10% หรือแถมฟรีอาหารเช้า (Value Add) สำหรับลูกค้าที่จองตรง
  * ปรับปรุงหน้าเว็บไซต์ให้ทำรายการง่ายขึ้น
* **Impact:** ลดต้นทุนในระยะยาว และเก็บ First-party Data ของลูกค้าได้เอง
<img width="1184" height="683" alt="image" src="https://github.com/user-attachments/assets/e361fb52-3545-449c-b615-1f110330c37e" />

### Insight 2: The Promotional Cancellation Trap (ปัญหาการยกเลิกจากราคาโปรโมชัน)
* **ปัญหา:** ราคาประเภท **Promotional** มีอัตราการยกเลิกสูงถึง **24.5%** สะท้อนพฤติกรรมลูกค้ากลุ่ม “จองกั๊ก” หรือ Serial Cancellers
* **Action:** * ปรับเงื่อนไขราคาโปรโมชันให้รัดกุมขึ้น เช่น เปลี่ยนเป็น **Non-refundable**
  * บังคับชำระมัดจำล่วงหน้า 30–50%
* **Impact:** ลดปริมาณการจองหลอก (Opportunity Cost), Forecast รายได้แม่นยำขึ้น และลดปัญหาห้องว่างกะทันหันเสียโอกาสในการขาย
<img width="976" height="583" alt="image" src="https://github.com/user-attachments/assets/2b6fb6ef-cf31-4e0a-ae22-6bb6eb1014f5" />

### Insight 3: Booking Quality by Channel (คุณภาพการจองของกลุ่ม B2B)
* **ปัญหา/โอกาส:** พบว่าลูกค้ากลุ่ม **Corporate Rate** และ **B2B / Wholesalers** มีอัตราการยกเลิกต่ำที่สุด และมียอดจองสม่ำเสมอตลอดทั้งปี
* **Action:** * เพิ่มความร่วมมือและทำสัญญาระยะยาวกับบริษัท/องค์กรต่างๆ
  * ทำ Tiered Pricing จูงใจพาร์ทเนอร์
  * ขยายช่องทางเครือข่ายผ่าน GDS หรือ Hotelbeds
* **Impact:** สร้างกระแสเงินสดที่มั่นคง ช่วยพยุงรายได้ในช่วง Low Season และลดการพึ่งพาลูกค้ารายย่อยจาก OTA เพียงอย่างเดียว
<img width="1181" height="683" alt="image" src="https://github.com/user-attachments/assets/ac994764-ec5a-4e7f-a1ec-69961ac706f0" />
