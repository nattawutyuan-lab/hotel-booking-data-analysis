
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


นี่คือเนื้อหาไฟล์ README.md ที่นำอีโมจิออกทั้งหมดแล้วครับ

The Azure Stay Dilemma: Hotel Booking Data Analysis
การวิเคราะห์ข้อมูลเพื่อปรับสมดุลช่องทางการจัดจำหน่ายและเพิ่มกำไรสุทธิให้กับโรงแรม Azure Stay

Section 1: Strategic Alignment
Background & Pain Points (ความท้าทายของโรงแรม)
ปัจจุบันโรงแรม Azure Stay กำลังเผชิญกับปัญหา ต้นทุนการจัดจำหน่ายที่สูงเกินไป (High Distribution Costs) แม้โรงแรมจะสามารถขายห้องพักได้และมียอดจอง (Volume) เข้ามาอย่างต่อเนื่อง แต่กลับต้องสูญเสียรายได้ไปกับค่าคอมมิชชันจำนวนมหาศาลให้กับตัวแทนขายออนไลน์ (OTAs เช่น Expedia หรือ Booking.com) ส่งผลให้เกิดภาวะ "รายได้สูงแต่กำไรสุทธิต่ำ" (Profit Margin Erosion) โปรเจกต์นี้จึงเกิดขึ้นเพื่อวิเคราะห์เจาะลึกและระบุอย่างชัดเจนว่า ช่องทางใดคือช่องทางที่สร้าง "กำไรสุทธิ (Net Revenue)" ให้กับธุรกิจอย่างแท้จริง เพื่ออุดรอยรั่วไหลของกำไร

SMART Objectives
S (Specific): ปรับสมดุลสัดส่วนช่องทางการจัดจำหน่าย (Channel Mix Optimization) โดยลดการพึ่งพายอดจองจาก OTAs ที่มีค่าคอมมิชชันสูง และเพิ่มสัดส่วนยอดจองผ่านช่องทางตรง (Direct Channel) หรือช่องทางที่มีต้นทุนต่ำกว่า

M (Measurable): เพิ่มรายได้สุทธิต่อห้องพักที่เปิดขาย (Net RevPAR) ให้สูงขึ้น 10% และลดสัดส่วนต้นทุนค่าคอมมิชชันโดยรวมลง 5%

A (Achievable): วิเคราะห์ข้อมูลเพื่อหา Profitable Channels และใช้กลยุทธ์จำกัดโควตาห้องพัก (Inventory Control) ในช่องทางที่กำไรน้อย เพื่อนำโควตานั้นไปจัดโปรโมชันเพิ่มแรงจูงใจในช่องทางที่กำไรสูง

R (Relevant): สอดคล้องกับเป้าหมายสูงสุดของ Azure Stay ในการเพิ่มประสิทธิภาพการทำกำไรสูงสุด (Maximize Net Revenue) และควบคุมต้นทุนการได้มาซึ่งลูกค้า (Cost of Acquisition)

T (Time-bound): เริ่มดำเนินการและวัดผลสำเร็จของการปรับกลยุทธ์ช่องทางการขายได้ภายใน 1 ไตรมาส (3 เดือน)

Section 2: Analytical Design
Hypothesis & Method (สมมติฐานและวิธีการคำนวณ)
1. ช่องทาง Direct (เว็บโรงแรม) ทำกำไรสุทธิต่อห้อง (Net ADR) ได้สูงกว่าช่องทาง OTA อย่างชัดเจน

Net ADR (OTA): Sum(net_room_revenue) / Count(booking_id) (เฉพาะ channel_type = 'OTA')

Net ADR (Direct): Sum(net_room_revenue) / Count(booking_id) (เฉพาะ channel_type = 'Direct')

จุดตัดสินใจ: หากส่วนต่างเป็นบวกและคุ้มค่า ควรเพิ่มสัดส่วนโควตาห้องพักให้กับฝั่ง Direct

2. ช่องทางที่คิดค่าคอมมิชชันแบบ 'จ่ายเหมา (Flat Fee)' คุ้มค่ากว่าแบบ 'หักเปอร์เซ็นต์ (Percentage)'

Cost % (Flat Fee): [ Sum(commission_amount) / Sum(gross_room_revenue) ] * 100 (เฉพาะ commission_model = 'Flat Fee')

Cost % (Percentage): [ Sum(commission_amount) / Sum(gross_room_revenue) ] * 100 (เฉพาะ commission_model = 'Percentage')

จุดตัดสินใจ: เปรียบเทียบสัดส่วน % ต้นทุน ช่องทางใดต่ำกว่าควรผลักดันการจองผ่านช่องทางนั้น

3. ช่องทางที่มี Cancellation Rate สูง ทำให้โรงแรมเสียโอกาสขายห้องให้ลูกค้าที่จ่ายแพงกว่า

Cancellation Rate by Channel: COUNT(booking_id WHERE status = 'Cancelled') / COUNT(Total booking_id) (Group by channel_id)

GitHub Documentation: hotel-booking-data-analysis/data at main · nattawutyuan-lab/hotel-booking-data-analysis

Section 3: Data Execution & EDA
ข้อมูลจำลองที่ใช้ในการวิเคราะห์ถูกออกแบบมาให้มีความสอดคล้องกัน ประกอบด้วย 4 ตารางหลัก ดังนี้:

Table 1: fact_bookings (The Transaction Data)
booking_id (PK): รหัสการจอง

booking_date / check_in_date: วันที่จอง และ วันที่เข้าพัก

channel_id (FK) / rate_code_id (FK): อ้างอิงช่องทางและรหัสราคา

gross_room_revenue: รายได้รวมที่ลูกค้าจ่าย (ก่อนหักคอมมิชชัน)

commission_amount: ต้นทุนค่าคอมมิชชันสำหรับการจองนี้

net_room_revenue: gross_room_revenue - commission_amount (รายได้สุทธิ)

status: Confirmed, Cancelled, Checked-Out

Table 2: dim_channels (The Cost Definitions)
channel_id (PK) / channel_name / channel_type: รหัส, ชื่อ, และประเภทช่องทาง (เช่น OTA, Direct)

commission_model: ประเภทการคิดค่าใช้จ่าย (Percentage, Flat Fee, Net Rate)

default_commission_rate: อัตราค่าธรรมเนียมมาตรฐาน

contract_owner: ผู้จัดการฝ่ายขายที่ดูแลช่องทางนั้นๆ

Table 3: dim_rate_codes
rate_code_id (PK) / rate_name: รหัสและชื่อประเภทราคา (เช่น RT_CORP)

is_commissionable: ระบุว่าราคานี้มีการหักคอมมิชชันหรือไม่ (True/False)

Table 4: fact_marketing_spend (The True Cost of Direct Channels)
spend_id (PK) / spend_date: รหัสและวันที่ใช้งบการตลาด

channel_id (FK): อ้างอิงช่องทาง (เน้นไปที่ Direct Website)

platform: แพลตฟอร์มที่ลงโฆษณา (Google Ads, Facebook)

cost_amount: จำนวนเงินที่จ่ายไป

clicks: จำนวนคลิกที่ได้รับจากโฆษณา

Section 4: Insights & Impact
จากการสำรวจข้อมูล (EDA) และเชื่อมโยงข้อมูลรายได้ ต้นทุน และช่องทาง พบข้อมูลเชิงลึก (Insights) และข้อเสนอแนะดังนี้:

Insight 1: ต้นทุนแฝงที่กลืนกินกำไร (The Commission Trap)
แม้ช่องทางประเภท OTA จะสร้างยอดการจอง (Volume) ได้สูงที่สุด แต่กลับพบว่ากำไรสุทธิหายไปอย่างมีนัยสำคัญเนื่องจาก Commission Rate ที่สูงถึง 15-20% ต่างจากช่องทาง Direct Web ที่ไม่มีค่าคอมมิชชัน (0%) และมีอัตราการยกเลิกต่ำ ซึ่งสะท้อนถึงลูกค้าที่มี "ความตั้งใจเข้าพักจริง" (High Booking Quality)

Recommendation: ปรับกลยุทธ์งบการตลาดและกระตุ้นยอด Direct Booking

Action: โยกย้ายงบที่เสียไปกับคอมมิชชัน OTA มาทำแคมเปญกระตุ้นการจองตรง (Direct Web) เช่น โค้ดส่วนลด 5-10% หรือให้สิทธิพิเศษ (Free Breakfast) พร้อมทำ A/B Testing ปรับปรุงหน้าเว็บ (UI/UX) ให้จองง่ายขึ้น

Impact: ลดต้นทุนค่าคอมมิชชันในระยะยาว และเก็บ First-party data ของลูกค้าไว้ทำการตลาดแบบ Personalized ต่อไป

Insight 2: ปัญหาการยกเลิกการจองจากราคาโปรโมชัน (The Promotional Cancellation Trap)
เมื่อวิเคราะห์ "ประเภทราคา" และ "สถานะการจอง" พบว่าราคาประเภท "Promotional" มีอัตราการยกเลิกสูงผิดปกติถึง 24.5% สะท้อนพฤติกรรม "จองกั๊ก" ของกลุ่ม Serial Cancellers ทำให้โรงแรมเสียโอกาสขายห้องให้ลูกค้าจริง (Opportunity Cost)

Recommendation: ปรับเปลี่ยนนโยบายการยกเลิกให้รัดกุมขึ้น

Action: ปรับเงื่อนไขราคา Promotional เป็นแบบ "Non-refundable" (ไม่สามารถขอคืนเงินได้) หรือบังคับชำระมัดจำล่วงหน้า 30-50% ทันทีเมื่อทำรายการ

Impact: สกัดกั้นกลุ่มลูกค้าจองกั๊ก คัดกรองเฉพาะผู้ที่ตั้งใจเข้าพักจริง ทำให้การพยากรณ์รายได้ (Revenue Forecasting) แม่นยำขึ้น และลดปัญหาห้องว่างกะทันหัน

Insight 3: คุณภาพของการจองแยกตามช่องทาง (Booking Quality by Channel)
กลุ่มลูกค้า Corporate Rate และ B2B (Wholesalers) มีอัตราการยกเลิกที่ ต่ำมาก เมื่อเทียบกับลูกค้ารายย่อย (Leisure) และมีแนวโน้มความต้องการเข้าพักที่สม่ำเสมอตลอดทั้งปี

Recommendation: สร้างแคมเปญร่วมกับกลุ่มลูกค้าองค์กร (B2B/Corporate)

Action: ให้ทีมขาย (Contract Owners) โฟกัสการทำสัญญากับบริษัท หรือทำ Tiered Pricing (ยอดจองถึงเป้า ได้ส่วนลดพิเศษ) ผ่านช่องทาง GDS หรือ Hotelbeds

Impact: สร้างกระแสเงินสดที่มั่นคง (Stable Cash Flow) ช่วยอุดรอยรั่วและรักษารายได้ในช่วง Low Season ที่ลูกค้ารายย่อยมักจะมีจำนวนลดลง
