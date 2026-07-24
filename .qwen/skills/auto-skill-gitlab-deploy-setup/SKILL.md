---
name: gitlab-deploy-setup
description: สร้าง remote repository บน GitLab, ตั้งค่า remote origin, และ push โค้ดเพื่อให้ CI/CD ทำ deploy ไปยัง GitLab Pages
source: auto-skill
extracted_at: '2026-07-24T17:28:23.204Z'
---

# ขั้นตอนการตั้งค่า GitLab Deploy สำหรับโปรเจ็ค Police Portal

1. **สร้าง Repository บน GitLab**
   - หากยังไม่มี ให้สร้างใหม่จาก GitLab UI หรือใช้ GitLab API (ต้องมี Personal Access Token ที่มี `api` และ `write_repository` scope)
   - จำ URL ของ repository (เช่น `https://gitlab.com/username/police-portal.git`)

2. **ตั้งค่า remote origin**
   ```bash
   cd /home/boy/police
   git remote add origin <REPO_URL>
   ```
   *แทน `<REPO_URL>` ด้วย URL ที่ได้จากขั้นตอน 1*

3. **Push โค้ดไปยัง Remote**
   ```bash
   git push -u origin main
   ```
   - ครั้งแรกอาจต้องตั้งชื่อ branch เป็น `main` (หรือ `master` ตามที่ GitLab กำหนด) หากยังไม่มีให้ทำ:
   ```bash
   git branch -M main
   ```

4. **ยืนยันว่า GitLab CI/CD ทำงาน**
   - หลังจาก `push` GitLab จะรัน pipeline ที่กำหนดใน `.gitlab-ci.yml`
   - ตรวจสอบสถานะ pipeline ที่ **CI/CD > Pipelines** ของ repository
   - หาก pipeline สำเร็จ หน้าเว็บจะถูกเผยแพร่ที่ `https://<username>.gitlab.io/<project-name>/`

5. **อัปเดต README (ถ้าต้องการ)**
   - เพิ่มลิงก์ไปยัง GitLab Pages และคำแนะนำการ deploy ให้ผู้ร่วมพัฒนารู้

**เคล็ดลับ**
- หากต้องการสร้าง repository ผ่าน API สามารถใช้คำสั่ง `curl` ตัวอย่าง:
  ```bash
  curl --header "PRIVATE-TOKEN: <YOUR_ACCESS_TOKEN>" \
       --data "name=police-portal&visibility=public" \
       "https://gitlab.com/api/v4/projects"
  ```
- อย่าลืมตั้งค่า **Protected Branch** หากต้องการให้เฉพาะผู้ที่ได้รับอนุญาตเท่านั้นที่สามารถ push ได้

---
*Skill นี้จัดทำโดย Qwen Code เพื่อให้ขั้นตอนการตั้งค่า GitLab Deploy เป็นมาตรฐานและทำซ้ำได้ง่าย*