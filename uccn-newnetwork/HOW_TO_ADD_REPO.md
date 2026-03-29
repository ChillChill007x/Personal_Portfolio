# 📦 วิธีเพิ่ม GitHub Repository เข้า Portfolio

## UCCN New Network Project

Repository นี้อยู่บน GitHub และไม่สามารถ clone อัตโนมัติได้เนื่องจากข้อจำกัดของระบบ  
คุณสามารถเพิ่มเข้าไปใน Portfolio ด้วยตัวเองได้ 2 วิธี:

---

## 🔧 วิธีที่ 1: Clone Repository (แนะนำ)

### ขั้นตอน:

1. **เปิด Terminal/Command Prompt**

2. **Navigate ไปยัง folder ที่ต้องการ:**
   ```bash
   cd portfolio-github/uccn-newnetwork
   ```

3. **Clone repository:**
   ```bash
   git clone https://github.com/ChillChill007x/UCCN_Newnetwork.git .
   ```
   
   หรือถ้าต้องการเก็บใน subfolder:
   ```bash
   git clone https://github.com/ChillChill007x/UCCN_Newnetwork.git
   ```

4. **ตรวจสอบไฟล์:**
   ```bash
   ls -la
   ```

5. **เพิ่มเข้า Git (ถ้าต้องการ):**
   ```bash
   cd ..  # กลับไป portfolio-github
   git add uccn-newnetwork/
   git commit -m "Add UCCN New Network project"
   git push
   ```

---

## 📥 วิธีที่ 2: Download ZIP

### ขั้นตอน:

1. **เปิดเว็บเบราว์เซอร์** และไปที่:
   ```
   https://github.com/ChillChill007x/UCCN_Newnetwork
   ```

2. **คลิกปุ่ม "Code"** (สีเขียว)

3. **เลือก "Download ZIP"**

4. **แตกไฟล์ ZIP:**
   - Windows: คลิกขวา → "Extract All..."
   - Mac: Double-click ไฟล์ ZIP
   - Linux: `unzip UCCN_Newnetwork-main.zip`

5. **คัดลอกไฟล์:**
   - คัดลอกเนื้อหาทั้งหมดจาก folder ที่แตกออกมา
   - วางไว้ใน `portfolio-github/uccn-newnetwork/`

6. **ลบ README.md เดิม (optional):**
   - ถ้า repo มี README.md ของตัวเอง
   - คุณอาจต้องการลบ README.md ที่ผมสร้างไว้
   - หรือเปลี่ยนชื่อเป็น `README_INTRO.md`

---

## 📁 โครงสร้างที่ควรได้

หลังจากเพิ่ม repository แล้ว โครงสร้าง folder ควรเป็นแบบนี้:

```
portfolio-github/
└── uccn-newnetwork/
    ├── README.md                  ← README จาก GitHub repo
    ├── [ไฟล์อื่นๆ จาก repo]
    ├── configs/                   ← (ถ้ามี)
    ├── docs/                      ← (ถ้ามี)
    └── ...
```

---

## 🔗 อัพเดท README หลัก

README หลักของ Portfolio (`portfolio-github/README.md`) ได้ถูกอัพเดตให้รวม UCCN Project แล้ว:

```markdown
| **UCCN New Network** | Unified Campus Communication Network project | 
  [🔗 GitHub Repository](https://github.com/ChillChill007x/UCCN_Newnetwork.git) · 
  [📖 Details](./uccn-newnetwork/) |
```

---

## ✅ Checklist

- [ ] Clone หรือ download repository จาก GitHub
- [ ] วางไฟล์ใน `portfolio-github/uccn-newnetwork/`
- [ ] ตรวจสอบว่ามี README.md และไฟล์ที่จำเป็น
- [ ] Test ว่า links ใน README หลักทำงาน
- [ ] Commit และ push ไปยัง GitHub (ถ้าใช้ Git)

---

## 💡 Tips

### ถ้าต้องการแก้ไข README:

1. **เก็บ README เดิมไว้:**
   ```bash
   mv README.md README_INTRO.md
   ```

2. **ใช้ README จาก repo:**
   - README จาก UCCN repo จะเป็น documentation หลัก
   - README_INTRO.md จะเป็นข้อมูลเบื้องต้น

### ถ้าต้องการ sync กับ GitHub:

```bash
cd uccn-newnetwork
git remote add upstream https://github.com/ChillChill007x/UCCN_Newnetwork.git
git pull upstream main
```

---

## 🆘 Troubleshooting

### ปัญหา: "Permission denied"
```bash
# ลองใช้ HTTPS แทน SSH
git clone https://github.com/ChillChill007x/UCCN_Newnetwork.git
```

### ปัญหา: "Repository not found"
- ตรวจสอบว่า URL ถูกต้อง
- ตรวจสอบว่า repo เป็น public
- ลองเปิดใน browser ก่อน

### ปัญหา: "Folder not empty"
```bash
# ลบ folder และ clone ใหม่
rm -rf uccn-newnetwork
mkdir uccn-newnetwork
cd uccn-newnetwork
git clone https://github.com/ChillChill007x/UCCN_Newnetwork.git .
```

---

## 📧 ติดต่อ

หากมีปัญหาหรือคำถาม:
- **Email:** Suphakit.f@kkumail.com
- **GitHub Issues:** [สร้าง issue ใน repo](https://github.com/ChillChill007x/UCCN_Newnetwork/issues)

---

**สำเร็จแล้ว! 🎉** UCCN New Network Project จะถูกเพิ่มเข้า Portfolio ของคุณ
