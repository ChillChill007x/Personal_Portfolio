# 📦 วิธีการจัดไฟล์จาก Network.rar เข้า Portfolio

## ขั้นตอนที่ 1: แตกไฟล์ Network.rar

### สำหรับ Windows:
1. คลิกขวาที่ไฟล์ `Network.rar`
2. เลือก "Extract Here" หรือ "Extract to Network/"
3. หรือใช้โปรแกรม WinRAR, 7-Zip, หรือ Windows 11 (รองรับ RAR โดยตรง)

### สำหรับ macOS:
1. ติดตั้ง The Unarchiver จาก App Store (ฟรี)
2. Double-click ที่ `Network.rar`
3. หรือใช้ Terminal: `unar Network.rar`

### สำหรับ Linux:
```bash
# ติดตั้ง unar
sudo apt-get install unar   # Ubuntu/Debian
sudo yum install unar       # CentOS/RHEL

# แตกไฟล์
unar Network.rar
```

## ขั้นตอนที่ 2: จัดไฟล์เข้า Portfolio

หลังจากแตกไฟล์แล้ว ให้จัดเรียงไฟล์ตามประเภท:

### 📁 ไฟล์ที่เกี่ยวกับ Lab1
```
portfolio-github/
└── lab1/
    ├── README.md
    ├── [ไฟล์ PDF รายงาน Lab1]
    ├── images/
    │   └── [Screenshot Lab1]
    └── configs/
        └── [ไฟล์ config ถ้ามี]
```

### 📁 ไฟล์ที่เกี่ยวกับ Lab2
```
portfolio-github/
└── lab2/
    ├── README.md
    ├── [ไฟล์ Video หรือ Link]
    └── images/
        └── [Screenshot Lab2]
```

### 📁 ไฟล์ที่เกี่ยวกับ Lab3
```
portfolio-github/
└── lab3/
    ├── README.md
    ├── LAB3.pdf
    └── images/
```

### 📁 ไฟล์ที่เกี่ยวกับ Lab4
```
portfolio-github/
└── lab4/
    ├── README.md
    └── [ไฟล์เอกสาร Lab4]
```

### 📁 ไฟล์ Projects
```
portfolio-github/
├── new-network/
│   ├── README.md
│   └── [ไฟล์โปรเจค New Network]
├── sprint-alpha/
│   ├── README.md
│   └── [ไฟล์ Sprint Alpha]
└── project-network/
    ├── README.md
    └── [ไฟล์ Project Network]
```

## ขั้นตอนที่ 3: อัพเดท README ในแต่ละ Lab

หลังจากใส่ไฟล์แล้ว อย่าลืมอัพเดท README.md ในแต่ละ folder:

### ตัวอย่าง Lab1 README:
```markdown
# LAB 1 - Network Fundamentals

## 📋 Overview
[คำอธิบายสั้นๆ]

## 📄 Documents
- [Lab1_Report.pdf](./Lab1_Report.pdf)

## 📸 Screenshots
![Screenshot 1](./images/screenshot1.png)
![Screenshot 2](./images/screenshot2.png)

## ✅ Completion
Lab นี้เสร็จสมบูรณ์แล้ว
```

## ขั้นตอนที่ 4: อัพโหลดขึ้น GitHub

```bash
cd portfolio-github
git add .
git commit -m "Add all lab files and projects"
git push origin main
```

## 💡 Tips

1. **ตั้งชื่อไฟล์ให้เข้าใจง่าย**
   - ใช้ชื่อแบบ: `Lab1_Report.pdf`, `Lab2_Config.txt`
   - หลีกเลี่ยง: `งานส่ง.pdf`, `final_final_v2.pdf`

2. **รูปภาพ**
   - แปลงเป็น PNG หรือ JPG
   - ขนาดไม่เกิน 2MB ต่อไฟล์
   - ตั้งชื่อที่บ่งบอกเนื้อหา

3. **PDF**
   - ตรวจสอบว่าเปิดได้ก่อนอัพโหลด
   - ขนาดไฟล์ไม่ใหญ่เกินไป

4. **Links**
   - ถ้ามีวิดีโอ อัพโหลดไป YouTube หรือ Google Drive
   - ใส่ลิงก์ใน README

## ⚠️ ข้อควรระวัง

- **อย่าอัพโหลดไฟล์ Network.rar ขึ้น GitHub** (ขนาดใหญ่เกินไป)
- แตกไฟล์ก่อน แล้วอัพโหลดเฉพาะไฟล์ที่จำเป็น
- ตรวจสอบว่า repository เป็น **Public** เพื่อให้อาจารย์เข้าถึงได้

---

หากมีคำถาม สามารถแก้ไข README นี้ได้เลยครับ! 🚀
