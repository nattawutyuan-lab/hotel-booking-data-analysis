นี่คือเวอร์ชันที่จัดรูปแบบใหม่ให้ **อ่านง่าย ดูโปรขึ้น และเหมาะใส่ใน GitHub / รายงาน**:

---

# Hotel Booking Data Analysis

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

