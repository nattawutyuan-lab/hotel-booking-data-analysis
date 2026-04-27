# CP372 Data Analytics and Business Intelligence Project

## Azure Stay: High Distribution Costs (Channel Profitability)

การวิเคราะห์เพื่อเพิ่มประสิทธิภาพช่องทางการจัดจำหน่ายและเพิ่มกำไรสุทธิสูงสุดให้กับธุรกิจโรงแรม
<img width="1030" height="576" alt="image" src="https://github.com/user-attachments/assets/2e0e5703-ae9f-4899-8d61-074e74cc7f1e" />


---

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

## SMART Objectives
- **S (Specific):** ปรับสัดส่วนช่องทางการจัดจำหน่ายห้องพักโดยเพิ่มการเข้าพักที่มีคุณภาพและลดต้นทุนแฝง
- **M (Measurable):** เพิ่ม Net RevPAR 10% และ ลดค่า Commission & Marketing รวมกัน 15%
- **A (Achievable):** ทำ Customer Segmentation ตามช่วงเวลา (Weekday/Weekend) และปรับนโยบายราคา (Rate Code) เพื่อป้องกันการยกเลิกห้อง
- **R (Relevant):** ลดการพึ่งพา OTA สร้างฐานลูกค้าประจำ (Direct Booking) เพื่อเพิ่มอัตรากำไรสุทธิสูงสุด
- **T (Time-bound):** วัดผลการเปลี่ยนแปลงและบรรลุเป้าหมายภายใน 1 ไตรมาส (3 เดือน)

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

## Data Dictionary (โครงสร้างแบบ Galaxy Schema / Fact Constellation)

ชุดข้อมูลถูกออกแบบในลักษณะ **Galaxy Schema** เนื่องจากมี Fact Table 2 ตาราง (ยอดจอง และ ค่าโฆษณา) ที่ใช้งาน Dimension Table ร่วมกัน ประกอบไปด้วย:

### Table 1: `fact_bookings` (ตารางบันทึกการจอง)
| Attribute | Description | Data Type | Valid Range/Example |
| :--- | :--- | :--- | :--- |
| `booking_id` (PK) | รหัสการจองห้องพักแต่ละรายการ | Nominal | BK_00001, BK_00050 |
| `booking_date` | วันที่ทำการจอง | Interval (Date) | 2022-12-06, 2023-04-22 |
| `check_in_date` | วันที่ลูกค้าเข้าพักจริง | Interval (Date) | 2023-01-10, 2023-05-11 |
| `channel_id` (FK) | รหัสช่องทางการจัดจำหน่าย | Nominal | CH_01, CH_03 |
| `Channel Name` | ชื่อช่องทาง (Denormalized) | Nominal | Booking.com, Walk-in |
| `rate_code_id` (FK)| รหัสประเภทราคา/โปรโมชัน | Nominal | RT_RACK, RT_PROMO |
| `gross_room_revenue`| รายได้รวมก่อนหักค่าใช้จ่าย (บาท) | Ratio (Continuous) | 2500, 8000 |
| `Commission Rate` | อัตราค่าคอมมิชชันที่หัก (%) | Ratio (Continuous) | 0.15, 0.18, 0 |
| `commission_amount` | จำนวนเงินค่าคอมมิชชัน (บาท) | Ratio (Continuous) | 450, 1440, 0 |
| `net_room_revenue` | รายได้สุทธิ (บาท) | Ratio (Continuous) | 2050, 6560 |
| `status` | สถานะการจอง | Nominal | Checked-Out, Cancelled, Confirmed |

### Table 2: `dim_channels` (ตารางข้อมูลช่องทางการจัดจำหน่าย)
| Attribute | Description | Data Type | Valid Range/Example |
| :--- | :--- | :--- | :--- |
| `channel_id` (PK) | รหัสอ้างอิงช่องทาง | Nominal | CH_01, CH_02 |
| `channel_name` | ชื่อแพลตฟอร์มการจอง | Nominal | Booking.com, Direct Web |
| `channel_type` | ประเภทของช่องทาง | Nominal | OTA, Direct, Wholesale |
| `commission_model` | รูปแบบการคิดค่าธรรมเนียม | Nominal | Percentage, Flat Fee, Merchant |
| `default_commission_rate`| อัตราค่าธรรมเนียมมาตรฐาน | Ratio (Continuous) | 0.15, 500, 0 |
| `contract_owner` | พนักงานขายที่รับผิดชอบสัญญา | Nominal | Alice, Bob, Charlie |

### Table 3: `dim_rate_codes` (ตารางข้อมูลแพ็กเกจราคา)
| Attribute | Description | Data Type | Valid Range/Example |
| :--- | :--- | :--- | :--- |
| `rate_code_id` (PK) | รหัสประเภทแพ็กเกจราคา | Nominal | RT_PROMO, RT_CORP |
| `rate_name` | ชื่อแพ็กเกจราคา | Nominal | Promotional, Corporate |
| `is_commissionable` | เงื่อนไขว่าแพ็กเกจนี้คิดคอมมิชชันหรือไม่ | Nominal (Boolean) | True / False |

### Table 4: `fact_marketing_spend` (ตารางข้อมูลค่าใช้จ่ายการตลาด)
| Attribute | Description | Data Type | Valid Range/Example |
| :--- | :--- | :--- | :--- |
| `spend_id` (PK) | รหัสรายการลงโฆษณา | Nominal | SP_001, SP_002 |
| `spend_date` | วันที่บันทึกค่าใช้จ่าย | Interval (Date) | 2023-01-01, 2023-02-01 |
| `channel_id` (FK) | รหัสช่องทาง (มักเชื่อมกับ Direct) | Nominal | CH_03 |
| `platform` | แพลตฟอร์มที่ลงโฆษณา | Nominal | Google Ads, Facebook |
| `cost_amount` | ค่าใช้จ่ายทางการตลาด (บาท) | Ratio (Continuous) | 6428, 13135 |
| `clicks` | จำนวนคลิกที่ได้รับจากโฆษณา | Ratio (Continuous) | 1752, 556 |

---

## Data Preparation & Cleansing 
- **Data Integration:** เชื่อมต่อข้อมูล (Merge) ตาราง `fact_bookings` กับ `dim_channels` และ `dim_rate_codes` แบบ Star Schema / Galaxy Schema
- **Remove Financial Noise:** ตั้งค่า `calculated_net_revenue` ของบุ๊กกิ้งที่มีสถานะ **Cancelled ให้เป็น 0** เพื่อป้องกันตัวเลขรายได้พองโตเกินจริง (Overstated Revenue)
- **Opportunity Cost Calculation:** สร้างคอลัมน์เก็บมูลค่าความเสียหายจากห้องที่ถูกยกเลิก เพื่อวัดขนาดของ Revenue Leakage
- **Feature Engineering:**
  - สกัดข้อมูลวันที่ (Datetime) เพื่อสร้างตัวแปร `is_weekend`
  - แบ่งประเภทวันเข้าพักเป็น **Weekday (Mon-Fri)** และ **Weekend (Sat-Sun)**
- **True Net Profit Logic:** กระจายต้นทุนค่าการตลาด (Marketing Spend) เข้าไปในทุกบุ๊กกิ้งของ Direct Web เพื่อหา **True Net ADR** ที่หักต้นทุนแฝงครบทุกมิติ

---

## การสำรวจและวิเคราะห์ข้อมูล (Exploratory Data Analysis - EDA)

เพื่อตอบสมมติฐานและทำความเข้าใจพฤติกรรมข้อมูล ได้ทำการวิเคราะห์ผ่าน Data Visualization ดังนี้:

### 1. Gross Revenue Breakdown: Net Revenue vs Commission by Channel
* **เป้าหมาย:** วิเคราะห์สัดส่วนรายได้สุทธิ (Net Revenue) เทียบกับต้นทุนค่าคอมมิชชัน (Commission) จากยอดรายได้รวม (Gross Revenue) ในแต่ละช่องทาง
* **ผลการวิเคราะห์:** ทำให้เห็นภาพชัดเจนว่าแม้ช่องทางกลุ่ม OTA จะสามารถสร้างรายได้รวม (ก้อนกราฟทั้งหมด) ได้สูงมาก แต่ก็ถูกหักต้นทุนค่าคอมมิชชันออกไปในสัดส่วนที่สูงตามไปด้วยเช่นกัน
* **การนำเสนอ:** ใช้ Stacked Bar Chart เพื่อให้เห็นองค์ประกอบของรายได้รวม ว่าถูกแบ่งเป็นกำไรที่โรงแรมได้จริง และต้นทุนที่ต้องจ่ายออกไปอย่างละเท่าไหร่
<img width="984" height="584" alt="image" src="https://github.com/user-attachments/assets/bdeca9c0-22cf-4791-a77e-cdfe7dc7e3ab" />

### 2. True Cost of Acquisition (COA %) by Channel
* **เป้าหมาย:** หาต้นทุนการได้ลูกค้าที่แท้จริง (True COA) โดยนำ "ค่าใช้จ่ายการตลาด (Marketing Spend)" มารวมกับ "ค่าคอมมิชชัน" เพื่อพิสูจน์ความคุ้มค่าของแต่ละช่องทาง
* **ผลการวิเคราะห์:** พบ Insight สำคัญว่า Direct Web ที่ดูเหมือนไม่ต้องเสียค่า Commission เลย กลับมีต้นทุน COA พุ่งสูงลิ่วเมื่อรวมกับค่ายิง Ads สะท้อนให้เห็นว่าการทำการตลาดทำได้ไม่คุ้มค่าเท่าที่ควร
* **การนำเสนอ:** ใช้ Bar Chart เปรียบเทียบสัดส่วนเปอร์เซ็นต์ COA เพื่อให้เห็นต้นทุนรวมในการได้ลูกค้า 1 คนของแต่ละช่องทาง
<img width="984" height="584" alt="image" src="https://github.com/user-attachments/assets/f753ac54-d8cc-4a00-9bd0-563f573eb8e0" />

### 3. Hidden Costs: Opportunity Cost from Cancellations by Channel
* **เป้าหมาย:** ประเมินมูลค่าความเสียหายทางธุรกิจ (Opportunity Cost) หรือ "ต้นทุนแฝง" ที่เกิดจากการยกเลิกห้องพัก (Cancellations) แยกตามช่องทาง
* **ผลการวิเคราะห์:** พบว่าช่องทาง OTA สร้างมูลค่าการสูญเสียรายได้ (Revenue Leakage) จากการยกเลิกห้องพักมหาศาลที่สุด ซึ่งเป็นความเสี่ยงที่โรงแรมต้องแบกรับไว้เมื่อเทียบกับช่องทางอื่นๆ
* **การนำเสนอ:** ใช้ Bar Chart เพื่อเปรียบเทียบมูลค่าความเสียหายที่เป็นตัวเงิน (บาท) ให้ผู้บริหารเห็นภาพขนาดของรายได้ที่รั่วไหลได้อย่างชัดเจน
<img width="984" height="584" alt="image" src="https://github.com/user-attachments/assets/cf2b36fb-ab05-4b65-88dd-4391b9875d8a" />

### 4. Final Net Profit by Channel (After ALL Costs)
* **เป้าหมาย:** วิเคราะห์หาผลกำไรสุทธิที่แท้จริง (Final Net Profit) ของแต่ละช่องทาง หลังจากนำรายได้มาหักลบด้วยต้นทุนแฝงทั้งหมด (Commission + Marketing) แล้ว
* **ผลวิเคราะห์เบื้องต้น:** ระบุช่องทางที่ได้กำไรมากที่สุด พบว่า Booking.com ทำกำไรสูงสุดให้โรงแรมที่ 651,325 บาท ตามมาด้วย Expedia (545,880 บาท) และ Direct Web (485,577 บาท)
* **การนำเสนอ:** ใช้ Bar Chart แบบเรียงลำดับ (Sorted) เพื่อชี้เป้าช่องทางที่ทำกำไรได้ดีที่สุดไปจนถึงน้อยที่สุด ช่วยให้ผู้บริหารตัดสินใจจัดสรรงบประมาณได้อย่างแม่นยำ
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

### Insight 4: Commission vs. Marketing Cost Per Checked-Out Booking (ค่าใช้จ่ายของ Marketing และ Commission ต่อการเข้าพัก)
* **ปัญหา:** ต้องการดูคุณภาพของการทำ Marketing เมื่อเทียบกับที่ต้องเสียค่า Commission ให้กับ OTA
* **Recommendation:** ควรวิเคราะห์ฐานลูกค้าหลัก เพื่อทำโฆษณาแบบเจาะจงเป้าหมาย แทนการยิงแอดแบบกว้างๆ เพื่อเพิ่มประสิทธิภาพ Content และลดงบการตลาดที่สูญเปล่าช่วยเพิ่มอัตราการคลิกและลดงบประมาณการตลาดที่ไม่จำเป็นลง
* **Impact:** ลดต้นทุนการจัดจำหน่าย ดันยอดจองจากช่องทางที่คุ้มค่าส่งผลให้กำไรสุทธิเติบโตขึ้น
<img width="1873" height="805" alt="image" src="https://github.com/user-attachments/assets/c859c368-5fd0-44d5-9b4d-7552d56a6eba" />
