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
* **เป้าหมาย:** เปรียบเทียบรายได้สุทธิต่อการจองหลังจากหักค่าคอมมิชชันและค่าการตลาด เพื่อประเมินประสิทธิภาพเชิงกำไรของแต่ละช่องทาง
* **ผลการวิเคราะห์:** จากกราฟพบว่า OTA มี True Net ADR อยู่ที่ 3,661.18 บาท ซึ่งสูงกว่า Direct ที่มีค่า 2,862.92 บาท
* **การนำเสนอ:** ใช้ Bar Chart เพื่อให้เห็นความแตกต่างของค่าเฉลี่ยรายได้สุทธิระหว่างกลุ่มได้อย่างชัดเจนและตีความง่ายจากความสูงของแท่งกราฟ
<img width="984" height="584" alt="image" src="https://github.com/user-attachments/assets/bdeca9c0-22cf-4791-a77e-cdfe7dc7e3ab" />

### 2. Final Net Profit by Channel (After ALL Costs)
* **เป้าหมาย:** วิเคราะห์ผลกำไรที่แท้จริงของแต่ละช่องทาง โดยหักต้นทุนผันแปรทั้งหมด ทั้งค่าคอมมิชชันและค่าใช้จ่ายการตลาด
* **ผลการวิเคราะห์:** Booking.com เป็นผู้ชนะในเชิงกำไรสุทธิรวมสูงสุดที่ 651,325 บาท ตามมาด้วย Expedia (545,880 บาท) และ Direct Web (485,577 บาท)
* **การนำเสนอ:** ใช้ Bar Chart แบบเรียงลำดับ เพื่อระบุช่องทางที่สร้างกำไรสูงสุดให้โรงแรมได้อย่างชัดเจนที่สุดสำหรับผู้บริหาร
<img width="984" height="584" alt="image" src="https://github.com/user-attachments/assets/f753ac54-d8cc-4a00-9bd0-563f573eb8e0" />

### 3. Customer Acquisition Cost (COA) Analysis
* **เป้าหมาย:** ต้องการหาต้นทุนการได้ลูกค้าจริง (COA) โดยนำค่าใช้จ่ายการตลาดมารวมกับค่า Commission เพื่อพิสูจน์ว่าช่องทาง Direct Web ที่ไม่ต้องเสียค่า Commission นั้น มีความคุ้มค่าจริงหรือไม่เมื่อต้องจ่ายค่าโฆษณาเอง
* **ผลวิเคราะห์เบื้องต้น:** ช่วยให้เห็นภาพรวมของต้นทุนแฝงในแต่ละช่องทาง โดยเปรียบเทียบว่าเงินที่จ่ายให้ OTA (ในรูปคอมมิชชัน) กับเงินที่โรงแรมจ่ายเอง (ในรูปค่าแอด) ฝั่งไหนสูงกว่ากัน
* **การนำเสนอ:** เลือกใช้ Bar Chart เพื่อเปรียบเทียบสัดส่วน COA ให้เห็นชัดเจนว่าช่องทางไหนมีต้นทุนรวมสูงที่สุด
<img width="984" height="584" alt="image" src="https://github.com/user-attachments/assets/cf2b36fb-ab05-4b65-88dd-4391b9875d8a" />

### 4. Booking Volume and Quality by Channel
* **เป้าหมาย:** วิเคราะห์ผลกำไรที่แท้จริงของแต่ละช่องทาง โดยนำรายได้รวมมาหักออกด้วยต้นทุนผันแปรทั้งหมด (ค่าคอมมิชชัน + ค่าใช้จ่ายการตลาด)
* **ผลวิเคราะห์เบื้องต้น:** ระบุ "ผู้ชนะ" ในเชิงกำไรสุทธิ (Net Profit) เพื่อหาความได้เปรียบเชิงพาณิชย์ ช่วยให้ผู้บริหารตัดสินใจได้ทันทีว่าควรจัดสรรทรัพยากรและงบประมาณลงทุนในช่องทางใดต่อเพื่อให้ได้ผลตอบแทนสูงสุด
* **การนำเสนอ:** เลือกใช้ Bar Chart เรียงลำดับ (Sorted) เพื่อให้เห็นการจัดอันดับช่องทางที่ทำกำไรได้ดีที่สุดไปจนถึงน้อยที่สุดอย่างชัดเจน
<img width="1184" height="684" alt="image" src="https://github.com/user-attachments/assets/91dcf2d1-5458-47bd-832c-0377ba32b768" />

---

## สรุปผลวิเคราะห์และข้อเสนอแนะ (Insights & Impact)

### Insight 1: The Profit Gap (ยอดขายรวม vs กำไรที่แท้จริง)
* **ปัญหา:** หลังจากนำรายได้มาหักลบต้นทุนแฝงทั้งหมด (Commission ของ OTA และ Marketing ของ Direct Web) พบว่าแม้ Direct Web จะไม่เสียค่าคอมมิชชัน แต่ต้นทุนการตลาดที่สูงมากทำให้กำไรสุทธิ (Net Profit) ลดลงอย่างมีนัยสำคัญ เมื่อเทียบกับยอดขายรวม (Gross Revenue)
* **Recommendation:**
  * **Audit Marketing Efficiency:** ตรวจสอบประสิทธิภาพการยิงโฆษณาของ Direct Web ทันที เนื่องจากค่าใช้จ่ายสูงแต่ Conversion อาจไม่คุ้มค่า เพื่อลดงบประมาณที่สูญเปล่า
  * **Convert OTA to Direct Loyalty:** นำเสนอสิทธิพิเศษ (In-stay incentives) ให้แก่ลูกค้าที่จองผ่าน OTA เมื่อมาเช็คอิน เพื่อดึงเข้าสู่ระบบสมาชิก (Loyalty Program) หวังผลให้เกิดการจองตรงในครั้งถัดไป
* **Impact:** ลดต้นทุนการหาลูกค้า (CAC) ในระยะยาว และเพิ่มอัตรากำไรสุทธิจากการจองโดยตรงที่ไม่มีทั้งค่าคอมมิชชันและค่าแอด
<img width="1184" height="683" alt="image" src="https://github.com/user-attachments/assets/e361fb52-3545-449c-b615-1f110330c37e" />

### Insight 2: The Promotional Cancellation Trap (หลุมพรางการยกเลิกจากราคาโปรโมชัน)
* **ปัญหา:** จากการวิเคราะห์ Rate Code พบว่ากลุ่มลูกค้าที่ใช้ "Promotional Rate" มีพฤติกรรมการจองแบบเผื่อเลือก โดยมียอดการยกเลิกสูงถึง 24.5% ซึ่งสูงกว่าราคาประเภทอื่นอย่างผิดปกติ
* **Recommendation:** ปรับเงื่อนไขราคาประเภท Promotional ให้มีความเข้มงวดมากขึ้น เช่น เปลี่ยนนโยบายเป็น "Non-refundable" (ไม่คืนเงิน) หรือบังคับ "Pre-payment" (ชำระเงินมัดจำล่วงหน้า) ทันทีที่ทำการจอง
* **Impact:** ลดการเสียโอกาสในการขาย (Opportunity Cost) ป้องกันการกั๊กห้องพัก และช่วยให้โรงแรมพยากรณ์รายได้ (Revenue Forecast) ได้แม่นยำยิ่งขึ้น
<img width="976" height="583" alt="image" src="https://github.com/user-attachments/assets/2b6fb6ef-cf31-4e0a-ae22-6bb6eb1014f5" />

### Insight 3: Booking Quality & Direct Value (คุณภาพการจองและความคุ้มค่าของ Direct Web)
* **ปัญหา:** เมื่อเปรียบเทียบคุณภาพการจองพบว่า Direct Web เป็นช่องทางที่มีความน่าเชื่อถือสูงที่สุด (ยอดเข้าพักจริงสูง และอัตราการยกเลิกต่ำที่สุดเพียง 6.5%) ในขณะที่ OTA มียอดการยกเลิกที่ผันผวนสูงกว่ามาก
* **Recommendation:** ผลักดันแคมเปญ "Book Direct & Get More" เพื่อจูงใจลูกค้าที่มีความตั้งใจพักจริง (High-intent customers) เช่น มอบสิทธิ Early Check-in หรือ Food Credit และควรพิจารณาเรียกเก็บเงินมัดจำในทุกช่องทางเพื่อลดความเสี่ยงจากการยกเลิกกะทันหัน
* **Impact:** เพิ่มอัตราการเข้าพักจริง (Occupancy Rate) ลดปัญหาห้องว่างกะทันหัน และลดภาระค่าคอมมิชชันสะสมจาก OTA ส่งผลให้ธุรกิจเติบโตอย่างยั่งยืน
<img width="1181" height="683" alt="image" src="https://github.com/user-attachments/assets/ac994764-ec5a-4e7f-a1ec-69961ac706f0" />
