# Hotel Booking Data Analysis 

## สมาชิกกลุ่ม
* 6610201013 ณัฐวุฒิ เคลือบทอง
* 66102010142 ธนัท กันหารี
* 66102010571 อนันตสิน ลาภวิสุทธิสาโรจน์

## เกี่ยวกับโปรเจกต์
โปรเจกต์นี้เป็นการวิเคราะห์ข้อมูลการจองห้องพักของโรงแรม (Hotel Booking) โดยใช้ชุดข้อมูลที่สร้างขึ้นผ่าน AI (AI-generated dataset) ในรูปแบบ Star Schema เพื่อจำลองสถานการณ์จริงของธุรกิจโรงแรม เช่น การเปรียบเทียบช่องทางการจอง (OTA vs Direct), ค่าคอมมิชชั่น, และงบการตลาด

## สมมติฐานการวิเคราะห์ (3 Hypotheses)
โปรเจกต์นี้ได้ตั้งสมมติฐานและทำการทดสอบ 3 ข้อหลัก ได้แก่:
1. Direct Channel Profitability Advantage

Hypothesis:
ช่องทาง Direct (เว็บไซต์โรงแรม) ทำกำไรสุทธิต่อห้อง (Net ADR) ได้สูงกว่าช่องทาง OTA อย่างชัดเจน

Method:
ใช้คอลัมน์ net_room_revenue และ channel_type ในการคำนวณค่าเฉลี่ยรายได้สุทธิต่อการจอง

Net ADR (OTA)
= Sum(net_room_revenue) / Count(booking_id)
(เฉพาะ channel_type = 'OTA')
Net ADR (Direct)
= Sum(net_room_revenue) / Count(booking_id)
(เฉพาะ channel_type = 'Direct')

Decision Rule:
นำ Net ADR (Direct) − Net ADR (OTA)

ถ้าผลลัพธ์เป็นบวกอย่างมีนัยสำคัญ → ควรเพิ่มสัดส่วนการขายผ่าน Direct
เพราะสร้างกำไรสุทธิต่อห้องได้มากกว่า
2. Commission Model Efficiency

Hypothesis:
ช่องทางที่คิดค่าคอมมิชชั่นแบบ Flat Fee คุ้มค่ากว่าแบบ Percentage

Method:
ใช้คอลัมน์ commission_amount, gross_room_revenue, และ commission_model

Cost % (Flat Fee)
= [ Sum(commission_amount) / Sum(gross_room_revenue) ] × 100
(เฉพาะ commission_model = 'Flat Fee')
Cost % (Percentage)
= [ Sum(commission_amount) / Sum(gross_room_revenue) ] × 100
(เฉพาะ commission_model = 'Percentage')

Decision Rule:

เปรียบเทียบค่า Cost %
ช่องทางที่มี % ต่ำกว่า = ต้นทุนต่ำกว่า = ควรส่งเสริมมากกว่า
3. Weekend Commission Waste

Hypothesis:
ในช่วงวันหยุดสุดสัปดาห์ (ศุกร์–เสาร์) โรงแรมจ่ายค่าคอมมิชชั่นให้ OTA มากเกินความจำเป็น ทั้งที่ Demand สูงอยู่แล้ว

Method:

(1) คำนวณ Weekend Occupancy %

ใช้ check_in_date, booking_id, และ status

Weekend Occupancy %
= [ Count(booking_id) / Total Rooms Available ] × 100
(เฉพาะวันศุกร์–เสาร์ และ status = 'Checked-Out')

(2) คำนวณ Commission Waste

ใช้ commission_amount และ channel_type

Commission Waste
= Sum(commission_amount)
(เฉพาะ channel_type = 'OTA' และวันศุกร์–เสาร์)

Decision Rule:

ถ้า Weekend Occupancy > 85%
→ แปลว่าห้องเต็มอยู่แล้ว (High Demand)
ค่า Commission Waste
→ ถือเป็น “ต้นทุนที่สามารถประหยัดได้”

Insight เชิงธุรกิจ:
โรงแรมสามารถ:

ปิด OTA บางส่วนในช่วง weekend
หรือจำกัดจำนวนห้อง (Quota Control)
→ เพื่อเพิ่มกำไรโดยไม่กระทบยอดขาย

## โครงสร้างของ Repository
* `data/` : โฟลเดอร์จัดเก็บชุดข้อมูลดิบ (CSV) เช่น fact_bookings, fact_marketing_spend, dim_channels
* `notebooks/` : โฟลเดอร์จัดเก็บไฟล์ Jupyter Notebook (`.ipynb`) ที่บรรจุโค้ด Python, กระบวนการ Data Cleaning และการสร้าง Data Visualization

## วิธีการใช้งาน (How to run)
1. Clone repository นี้ลงเครื่อง หรือดาวน์โหลดเป็นไฟล์ ZIP
2. เปิดไฟล์ในโฟลเดอร์ `notebooks/` ผ่าน Google Colab หรือ Jupyter Notebook
3. ตรวจสอบให้แน่ใจว่าไฟล์ข้อมูลในโฟลเดอร์ `data/` ถูกเรียกใช้อย่างถูกต้องก่อนกด Run All
