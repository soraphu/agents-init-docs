### รายละเอียดและหน้าที่ของทั้ง 4 ไฟล์:

1. **`AGENTS.md` (หรือ `.cursorrules` / `CLINERULES`)**
   - **คืออะไร:** คำแนะนำและกฎเหล็กสำหรับ **AI Agent / Coding Assistant** (เช่น Cursor, Windsurf, Claude Dev)
   - **สิ่งที่ควรใส่:**
     - Tech Stack และไลบรารีบังคับใช้
     - โครงสร้างของ Project Code (เช่น โฟลเดอร์ไหนเก็บอะไร)
     - กฎการเขียน Code Style (เช่น Formatting, Naming Conventions, Strict TypeScript)
     - สิ่งที่ AI "ห้ามทำ" (เช่น ห้ามลบ Comment เก่า, ห้ามปรับ DB Schema โดยไม่ได้รับอนุญาต)

2. **`PRD.md` (Product Requirement Document)**
   - **คืออะไร:** เอกสารสรุปความต้องการทางผลิตภัณฑ์และธุรกิจ (ใช้แทน `SRS.md` ในทีมสาย Agile)
   - **สิ่งที่ควรใส่:**
     - Problem Statement (ปัญหาที่ต้องการแก้ไข)
     - Target Users (กลุ่มผู้ใช้งาน)
     - Features & Acceptance Criteria (ฟีเจอร์ที่ต้องมีและเงื่อนไขการตรวจรับงาน)
     - Non-functional Requirements (ความเร็ว, Security, Scalability)

3. **`ARCHITECTURE.md`**
   - **คืออะไร:** ภาพรวมทางเทคนิคและสถาปัตยกรรมของระบบทั้งหมด (ย้ายไฟล์สรุปสถาปัตยกรรมมาไว้ไฟล์นี้ได้เลย)
   - **สิ่งที่ควรใส่:**
     - High-level Design Diagram (Frontend <-> Backend <-> Database <-> External APIs)
     - เหตุผลในการเลือกใช้ Tech Stack ต่างๆ (ADR - Architecture Decision Record)
     - Data Flow & Security Layer

4. **`LOG.md` (หรือ `CHANGELOG.md` / `DECISION_LOG.md`)**
   - **คืออะไร:** บันทึกประวัติและการเปลี่ยนแปลงที่สำคัญของโปรเจกต์
   - **สิ่งที่ควรใส่:**
     - **Version Release Notes:** แต่ละ Version อัปเดตฟีเจอร์อะไร แก้ Bug ตัวไหน
     - **Architectural Decisions:** บันทึกการตัดสินใจทางเทคนิคที่สำคัญ (เช่น เปลี่ยนจากการใช้ REST เป็น GraphQL วันไหน และเพราะอะไร) เพื่อให้คนที่มาดูทีหลังเข้าใจบริบท

---
