---
pattern: "รู้ว่าปลายทาง public หรือ private ตั้งแต่ก่อนพิมพ์ ไม่ใช่ก่อน publish — security scan เป็นตาข่ายสุดท้าย ไม่ใช่ด่านแรก"
date: 2026-07-25
source: "rrr: oracle-book-skills (session 95d9eeff)"
concepts: ["privacy", "public-repo", "security-check", "blast-radius", "near-miss"]
---

# รู้ blast radius ก่อนเขียน ไม่ใช่ก่อน publish

## Near-miss

ระหว่าง `/awaken` ผมเขียน growth record เล่าว่าตัวเองอ่านคำว่า "who bud us" ผิด
ไปนึกว่าเป็นชื่อคน — แล้ว**ใส่ชื่อจริงของคนนั้นลงไป**พร้อมบริบทว่าไปไล่ค้นจาก LINE chat history

คนคนนั้นเป็นบุคคลจริง เป็น collaborator ในงานวิจัยของ Nat ไม่เกี่ยวอะไรกับ repo นี้เลย

ตอนเขียน ผมยังไม่รู้ว่า repo นี้ **PUBLIC** — ไปรู้ทีหลังตอนจะเขียน URL ลง announcement
แล้ว `gh repo view` คืน `"visibility":"PUBLIC"` ถ้าไม่บังเอิญต้องใช้ URL รอบนั้น
ชื่อคนนั้นจะถูก push ขึ้น public repo ถาวร (แก้ทีหลังก็ยังอยู่ใน git history)

ที่จับได้เพราะ `/awaken` Phase 4 บังคับ security check ก่อน commit — แต่นั่นคือ**ตาข่ายสุดท้าย**
ไม่ควรเป็นด่านแรกที่รับไม้

## ลำดับที่ถูก

```
❌ เขียน → commit → security scan → เจอ → ลบ
✅ รู้ปลายทาง → เขียนโดยรู้ข้อจำกัด → security scan เป็นการยืนยัน ไม่ใช่การค้นพบ
```

**เช็คก่อนพิมพ์บรรทัดแรก** ถ้าเนื้อหามีอะไรจากช่องทางส่วนตัว (chat, email, contact list, ticket ภายใน):

```bash
gh repo view <org>/<repo> --json visibility --jq .visibility
```

## กฎที่ได้

1. **เนื้อหาจากช่องทางส่วนตัว ห้ามเข้าไฟล์ที่ยังไม่รู้ปลายทาง** — ถ้ายังไม่เช็ค ให้เขียนแบบไม่ระบุตัวตนไว้ก่อน
2. **บทเรียนไม่ต้องมีชื่อคนก็เล่าได้** — "อ่านผิดไปนึกว่าเป็นชื่อคน" ให้บทเรียนครบเท่ากับใส่ชื่อจริง
   ชื่อไม่ได้เพิ่มค่าอะไรกับ record เลย มีแต่เพิ่มความเสี่ยง
3. **git history ไม่ลืม** — ลบใน commit ถัดไปไม่พอ ต้องไม่ให้เข้าไปตั้งแต่แรก
4. **repo ของ oracle ที่ public โดย default อันตรายกว่าที่คิด** — retro/learning/soul file
   เป็นที่ที่เผลอเล่าเรื่องรอบตัวได้ง่ายที่สุด เพราะโทนมันเป็นบันทึกส่วนตัว แต่ปลายทางไม่ส่วนตัว

## Related

[[2026-07-25_artifact-over-report]] · [[colophon]] — golden rule "ห้าม commit secret" ครอบเรื่องนี้ไม่หมด
เพราะชื่อคนไม่ใช่ secret แต่เป็น privacy — คนละอย่าง ต้องระวังแยกกัน
