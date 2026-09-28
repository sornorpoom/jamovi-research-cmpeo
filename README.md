# ระบบนิเทศ ติดตามการวิจัยในชั้นเรียน (Jamovi for Classroom Research)
### กลุ่มนิเทศ ติดตามและประเมินผล สำนักงานศึกษาธิการจังหวัดเชียงใหม่
**ผู้ประสานงานและพัฒนานวัตกรรม:** ศน.รัชภูมิ สมสมัย (ศึกษานิเทศก์ ศธจ.เชียงใหม่)

---

## 📖 ความเป็นมาและวัตถุประสงค์ (Background & Objectives)
โครงการอบรมเชิงปฏิบัติการเรื่อง **"การประยุกต์ใช้โปรแกรมคอมพิวเตอร์สำเร็จรูป (Jamovi) สำหรับการวิจัยในชั้นเรียน"** มีเป้าหมายเพื่อยกระดับสมรรถนะครูผู้สอนในการพัฒนาผลสัมฤทธิ์ทางการเรียนของผู้เรียนโดยใช้กระบวนการวิจัยในชั้นเรียน (Classroom Action Research: CAR) เป็นฐาน และเสริมสร้างทักษะการประยุกต์ใช้โปรแกรมสถิติโอเพนซอร์ส **Jamovi (Version 2.7)** ในการวิเคราะห์ข้อมูลทางการศึกษา

เว็บแอปพลิเคชันนี้ทำหน้าที่เป็น **"ระบบนิเทศ ติดตามผล และคลินิกวิจัยหลังการอบรม (Post-Training Mentoring & Supervision Portal)"** เพื่อให้การสนับสนุน ส่งเสริม และนิเทศเชิงวิชาการแก่คุณครูผู้เข้าร่วมโครงการอย่างต่อเนื่องและมีประสิทธิภาพ

---

## ✨ คุณลักษณะเด่นของระบบ (Key Features & UX/UI)

1. **Academic KPI Dashboard:** แถบแสดงสถานะผลงานแบบเรียลไทม์ (ภารกิจที่มอบหมาย, จำนวนผลงานที่ส่งแล้ว, งานที่ผ่านการนิเทศ, งานที่รอปรับแก้, และกระทู้คำถามในคลินิกวิจัย)
2. **Jamovi Statistical Quick Guide:** มีคู่มือสรุปการเลือกใช้สถิติทางการศึกษาใน Jamovi ในตัวระบบ (Paired Samples t-test, Independent Samples t-test, One-Way ANOVA, Cronbach's Alpha)
3. **Multi-Role Experience:**
   - **ครูผู้สอน (Teacher):** ดูภารกิจ, ส่งไฟล์งาน `.omv` / `.pdf`, ติดตามผลการตรวจ, ส่งงานฉบับปรับปรุงแก้ไข (Resubmit), และตั้งคำถามในคลินิกวิจัย
   - **ศึกษานิเทศก์/ผู้ดูแลระบบ (Supervisor/Admin):** มอบหมายภารกิจใหม่, ตรวจผลงานและบันทึกข้อเสนอแนะเชิงวิชาการ, อัปเดตคลังสื่อ, และตอบคำถามในคลินิกวิจัย
4. **Automated Two-way Email Notification:** ส่งอีเมลแจ้งเตือนอัตโนมัติ 4 ทิศทาง (เมื่อมอบหมายภารกิจใหม่, เมื่อนิเทศคืนผลงาน, เมื่อครูตั้งคำถาม, และเมื่อผู้นิเทศตอบคำถาม)
5. **Dual-Engine Architecture:** สามารถรันได้ทั้งบน Google Apps Script Web App โดยตรง และ Deploy ขึ้น Vercel / GitHub Pages เพื่อความสะดวกรวดเร็วและเสถียรภาพสูงสุด

---

## 🏛️ สถาปัตยกรรมระบบ (System Architecture)

```
[ Frontend: Single Page App (HTML5 + Tailwind CSS + FontAwesome + SweetAlert2) ]
                      │
                      ├──> (รันบน Vercel / GitHub Pages) ──[ Fetch REST API (JSON) ]──┐
                      │                                                               │
                      └──> (รันบน Google Apps Script) ───[ google.script.run ]────────┤
                                                                                      ▼
                                                             [ Backend: Google Apps Script (Code.gs) ]
                                                                                      │
                                                                   ┌──────────────────┴──────────────────┐
                                                                   ▼                                     ▼
                                                    [ Google Sheets (Database) ]          [ Google Drive (Storage) ]
                                                    - Users                               - Folder งานวิจัยของครู
                                                    - Assignments                         - Folder บันทึกนิเทศ
                                                    - Submissions
                                                    - Resources
                                                    - QA_Board
```

---

## 🗄️ โครงสร้างฐานข้อมูล (Database Schema)

* **Google Spreadsheet ID:** `1Aw6biBPPJ-JYfU-wNRRsVW8JS8Hop_BmdywBXqSCw-Q`
* **Submissions Folder ID:** `1NhLS9JWt1WkZT699ijvFDGYB043qbp7b`
* **Supervisor Comments Folder ID:** `1nkZOBjX-xAw8InFr0sExABuOUM3slrcP`

### รายละเอียดแท็บข้อมูล (5 แท็บหลัก)
1. **`Users`**: `username`, `password`, `role`, `full_name`, `school_name`, `subject_group`, `email`, `status`
2. **`Assignments`**: `assignment_id`, `title`, `description`, `due_date`, `template_link`, `status`
3. **`Submissions`**: `submission_id`, `timestamp`, `username`, `teacher_name`, `school_name`, `assignment_id`, `research_title`, `file_link`, `teacher_reflection`, `status`, `supervisor_comment`, `feedback_date`
4. **`Resources`**: `resource_id`, `category`, `title`, `description`, `file_link`, `thumbnail_url`, `updated_date`
5. **`QA_Board`**: `post_id`, `timestamp`, `username`, `teacher_name`, `school_name`, `topic`, `detail`, `answer`, `answered_by`, `status`

---

## 🚀 แนวทางการติดตั้งและเผยแพร่ (Deployment Guide)

### 1. ติดตั้งบน Google Apps Script (GAS)
1. เปิด [Google Apps Script](https://script.google.com/) แล้วสร้างโปรเจกต์ใหม่
2. คัดลอกโค้ดจากไฟล์ `Code.gs` ไปวางในแท็บ `Code.gs`
3. สร้างไฟล์ HTML ชื่อ `Index` และคัดลอกโค้ดจากไฟล์ `index.html` ไปวาง
4. กด **Deploy > New Deployment** เลือก Type เป็น **Web App**
   - **Execute as:** `Me (เจ้าของโปรเจกต์)`
   - **Who has access:** `Anyone (ทุกคน)`
5. คัดลอก **Web App URL** ที่ได้เพื่อใช้งาน หรือนำมาใส่ในค่า `GAS_API_URL` ของ `index.html`

### 2. ติดตั้งบน Vercel ผ่าน GitHub
1. อัปโหลดไฟล์โปรเจกต์ทั้งหมดขึ้น GitHub Repository
2. เข้าสู่ระบบ [Vercel](https://vercel.com/) และกด **Add New > Project**
3. Import Repository ที่สร้างไว้
4. กด **Deploy** ระบบจะทำงานทันทีโดยไม่ต้องตั้งค่า Build Command เพิ่มเติม

---

## 🔐 บัญชีสำหรับทดสอบระบบ (Test Accounts)

| ประเภทผู้ใช้งาน | Username | Password ตั้งต้น | สิทธิ์ (Role) | สังกัด |
| :--- | :--- | :--- | :--- | :--- |
| **ศึกษานิเทศก์** | `supervisor01` | `1234` | `supervisor` | กลุ่มนิเทศฯ ศธจ.เชียงใหม่ |
| **ผู้ดูแลระบบ** | `admin` | `1234` | `admin` | ศธจ.เชียงใหม่ |
| **ครูผู้สอน (ตัวอย่าง 1)** | `T01` | `1234` | `teacher` | โรงเรียนวิมานทิพย์ |
| **ครูผู้สอน (ตัวอย่าง 2)** | `T03` | `1234` | `teacher` | โรงเรียนดาราวิทยาลัย |
| **ครูผู้สอน (ตัวอย่าง 3)** | `T16` | `1234` | `teacher` | โรงเรียนวารีเชียงใหม่ |

---

## 📞 ติดต่อสอบถามและสนับสนุนเชิงวิชาการ
* **กลุ่มนิเทศ ติดตามและประเมินผล สำนักงานศึกษาธิการจังหวัดเชียงใหม่**
* ศน.รัชภูมิ สมสมัย (อีเมล: sornorpoom@gmail.com)
