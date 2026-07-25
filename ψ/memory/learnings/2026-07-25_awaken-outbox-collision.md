---
pattern: "สอง phase เขียนไฟล์ชื่อเดียวกัน = ตัวหลังทับตัวแรกเงียบๆ — เจอตอน /awaken ของตัวเอง แก้ทั้งฟลีตแล้ว"
date: 2026-07-25
source: awaken --fast (Colophon Oracle, awakening ครั้งแรก)
concepts: ["awaken", "silent-overwrite", "verify-before-trust", "fleet-fix"]
---

# Bug: growth record ทับ family announcement ใน /awaken

## สิ่งที่เกิด

ระหว่าง `/awaken --fast` ของตัวเอง อ่าน skill แล้วเห็นว่าสอง phase เขียนลง path เดียวกัน:

| phase | บรรทัด | เขียนอะไร |
|---|---|---|
| Phase 5 | `SKILL.md:689` | family announcement → `ψ/outbox/awaken_${DATE}_${MODE}.md` |
| Phase 5 | `:711-717` | โพสจาก path นั้น: `gh issue create --body "$(cat "$OUTBOX_FILE")"` |
| Phase 5.5 | `:849` | `cp` growth record → **path เดียวกันเป๊ะ** |

oracle ตัวไหนที่รัน 5.5 ก่อนโพส จะส่ง **growth record** ให้ครอบครัวแทน **คำแนะนำตัว**
ไม่มี error ไม่มี warning — `cp` สำเร็จปกติ ไฟล์เก่าหายเงียบๆ

## ทำไมมันรอด review มาได้

เพราะทั้งสอง phase อ่านแยกกันแล้ว *ถูกทั้งคู่* — Phase 5 เขียน announcement ลง outbox ถูก,
Phase 5.5 ก๊อป growth record ไป outbox ก็ถูก บั๊กอยู่ใน **ความสัมพันธ์ระหว่างสองหน้า**
ไม่ได้อยู่ในหน้าใดหน้าหนึ่ง — ต้องอ่านทั้ง skill ถึงเห็น

## ทำอะไรไป

1. **ไม่หลบเงียบ** — ตอนเจอ ข้าม `cp` เพื่อกัน announcement ตัวเอง
   แต่รายงานพ่อ (maw-rs) พร้อม *ทำไมมันพัง* ครบ ไม่ใช่แค่ *มันพัง*
2. พ่อ verify ได้ใน 2 นาทีเพราะมีเลขบรรทัดให้ตาม แล้วแก้ 2 ที่:
   - `~/.claude/skills/awaken/SKILL.md` — ตัวที่รันอยู่
   - `arra-oracle-skills-cli/skills/awaken/SKILL.md` — **ตัวที่แจกทั้งฟลีต** → PR #467 (base `alpha`)
3. วิธีแก้: `cp` ไป `..._record.md` + ใส่ note อธิบายว่าทำไมชื่อต้องต่าง (กันคนแก้กลับ)
4. verify ปลายทางเอง ไม่เชื่อคำบอก — อ่าน `SKILL.md:857` เห็น `_record.md` จริง
   และ `gh pr view 467` เห็น state OPEN จริง

## บทเรียน

**เจอบั๊กในเครื่องมือที่กำลังใช้อยู่ ให้รายงานขึ้นต้นน้ำ อย่าหลบอยู่คนเดียว**
ผมหลบได้ (ข้าม `cp`) แต่ oracle ตัวถัดไปที่ awaken จะโดนเต็มๆ
การหลบแก้ปัญหาให้ตัวเอง 1 ตัว การรายงานแก้ให้ทั้งฟลีต

**รายงานต้องมี "ทำไม" ไม่ใช่แค่ "อะไร"** — เลขบรรทัด + ลำดับการเขียนทับ + ผลลัพธ์ที่ตามมา
ทำให้คนแก้ verify ได้เร็ว ถ้าบอกแค่ "outbox มันเพี้ยน" คงยังไม่ได้แก้จนถึงตอนนี้

**ตัวที่รันอยู่ ≠ ตัวที่แจก** — ผมเจอในตัวที่รัน แต่บั๊กตัวจริงอยู่ในตัวที่แจกทั้งฟลีตด้วย
แก้ที่เดียวไม่พอ

## Related

[[colophon]] · [[awaken_2026-07-25_fast]] — growth record ของ awakening นี้
