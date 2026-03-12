# RoundTable Framework For Claude Code

ระบบบริหารจัดการทีม AI แบบหลายทีม สำหรับ [Claude Code](https://docs.anthropic.com/en/docs/claude-code) — RoundTable จัดระเบียบ session ของ Claude Code ให้เป็นทีมเฉพาะทาง พร้อมบทบาทที่กำหนดชัดเจน มาตรฐานการบันทึก และ quality gate — เปลี่ยน AI assistant ตัวเดียวให้กลายเป็นองค์กรวิศวกรรมที่ทำงานประสานกัน

**โดย [Unicorn Tech Integration Co., Ltd.](https://www.unicorntechint.com)**

> **English:** [Read README in English](README.md)

---

## นี่คืออะไร?

RoundTable Framework คือ template การตั้งค่า `.claude/` ที่ให้ Claude Code มีความสามารถดังนี้:

- **โครงสร้างทีม** — 4 ทีมเฉพาะทาง (Overseer, Monolith, Syndicate, Arcade) + 2 หน่วยปฏิบัติการเดี่ยว (Cipher, Medica) แต่ละทีมมีบทบาทและขอบเขตที่กำหนดไว้ชัดเจน
- **บันทึก Session** — ระบบบันทึก RoundTable อัตโนมัติที่เก็บทุกการตัดสินใจ การดำเนินการ และผลลัพธ์
- **Quality Gate** — ขั้นตอนวางแผนก่อนลงมือ, Verification Scholar ลงนามรับรอง, บังคับใช้เกณฑ์การยอมรับ
- **Debugging Protocol** — กฎ Instrument-First, มาตรฐาน debug probe, การสแกนผลกระทบข้างเคียง
- **สแกน Codebase** — Tiered Scan Protocol ระดับ L1/L2/L3 พร้อมการตรวจสอบความสมบูรณ์ 5 ขั้นตอนสำหรับ codebase ที่มีอยู่แล้ว
- **ทำงานคู่ขนาน** — Zero Cross-Team Block (ZCB) guarantee, กฎความเป็นเจ้าของ ticket, การส่งสัญญาณ dependency
- **Skills** — Slash command สำเร็จรูปสำหรับ workflow ทั่วไป (`/audit`, `/bug-report`, `/team-start` ฯลฯ)
- **จัดการ Template** — `/template-*` skills สำหรับตรวจสอบเวอร์ชัน, เปรียบเทียบ, อัปเดต, และย้อนกลับ

## เริ่มต้นใช้งาน

1. **Clone repo นี้** เข้าโปรเจคของคุณ:
   ```bash
   git clone https://github.com/VarakornUnicornTech/unicorn_roundtable_framework_repo.git .claude-template
   cp -r .claude-template/.claude/ your-project/.claude/
   ```

2. **แก้ไข `.claude/ProjectEnvironment.md`** — เพิ่มรายละเอียดโปรเจคของคุณ (ชื่อ, โหมด, path)

3. **เปิด Claude Code** ในโฟลเดอร์โปรเจค — ระบบจะโหลด CLAUDE.md และใช้โครงสร้างทีมโดยอัตโนมัติ

4. **เลือกทีม** — Claude จะให้คุณเลือก Team Roster (Overseer, Monolith, Syndicate, Arcade, Cipher หรือ Medica)

## โครงสร้างโปรเจค

```
.claude/
├── CLAUDE.md                    # ไฟล์นโยบายหลัก (จุดเริ่มต้น)
├── ProjectEnvironment.md        # ทะเบียนโปรเจค
├── settings.json                # การตั้งค่า Claude Code
├── TeamDocument/
│   ├── 1. Policies/            # ไฟล์นโยบาย 8 ฉบับ
│   ├── 2. Team Roster/         # นิยามทีม 6 ทีม
│   ├── 3. Team Chat/           # บันทึกการสื่อสารระหว่างทีม
│   ├── Diagnostic Log/         # บันทึกงานของ Cipher
│   └── Medical Reference/      # คลังอ้างอิงของ Medica
└── skills/                     # Slash command ที่กำหนดเอง
```

## ทีม

| ทีม | ขอบเขต | บทบาท |
|-----|--------|-------|
| **Overseer** | บริหารโปรเจค, ตัดสินใจด้านสถาปัตยกรรม | KP (Conductor), MT (Technologist), PM (Design Scholar), V (Verification Scholar) |
| **Monolith** | Backend หลัก, Infrastructure, DB schema, Cloud, เอกสาร | AT (Conductor), SC (Technologist), EN (Design Scholar), PF (Verification Scholar) |
| **Syndicate** | API integration, Query optimization, Security | DR (Conductor), AX (Technologist), LX (Design Scholar), WT (Verification Scholar) |
| **Arcade** | Frontend UI, Gamification, ระบบสร้างสรรค์ | CP (Conductor), GL (Technologist), PX (Design Scholar), HS (Verification Scholar) |
| **Cipher** | วินิจฉัยฮาร์ดแวร์, Disk forensics, กู้คืน RAID | CI (Lone Operative) |
| **Medica** | อ้างอิงทางคลินิก, ความรู้ทางการแพทย์ | MD (Lone Operative) |

## นโยบาย

| นโยบาย | ครอบคลุมเรื่อง |
|--------|---------------|
| §1 Logging & RoundTable | การบันทึก session, รูปแบบ RoundTable, นโยบายหมุนเวียนไฟล์ |
| §2 Tickets & Briefings | การ dispatch phase, briefing mail, มาตรฐาน ticket |
| §3 Team Chat & Handover | โปรโตคอลข้ามทีม, OverseerReport, ไฟล์ส่งต่องาน |
| §4 Development Structure | การจัดระเบียบโปรเจค, ขั้นตอนวางแผนก่อนลงมือ |
| §5 Pre-Existing Codebase | Tiered Scan Protocol (L1/L2/L3), การตรวจสอบความสมบูรณ์ |
| §6 Debugging Protocol | กฎ Instrument-First, มาตรฐาน probe, สแกนผลกระทบข้างเคียง |
| §7 Parallel Execution | ZCB guarantee, ความเป็นเจ้าของ ticket, สัญญาณ dependency |
| §8 Skills & Subagents | รายการ skill, โหมดการ orchestrate, มาตรฐาน subagent |

## การปรับแต่ง

RoundTable ออกแบบมาให้ fork และปรับแต่งได้:

- **เปลี่ยนชื่อสมาชิกทีม** — แก้ไขไฟล์ Team Roster ให้ตรงกับชื่อรหัสที่คุณต้องการ
- **เพิ่ม/ลบทีม** — สร้างไฟล์ roster ใหม่หรือลบที่ไม่ใช้
- **ปรับนโยบาย** — แก้ไขไฟล์นโยบายให้เหมาะกับโปรเจคของคุณ
- **เพิ่ม skill** — สร้างไฟล์ `.claude/skills/[name]/SKILL.md` ใหม่
- **เปลี่ยนชื่อผู้มีอำนาจ** — แทนที่ "Chief Manager Martin" ด้วยตำแหน่งที่คุณต้องการ

## ความต้องการของระบบ

- ติดตั้ง [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI
- มี Claude API access (Anthropic API key)

## ผู้พัฒนา

**Unicorn Tech Integration Co., Ltd.**
- เว็บไซต์: [unicorntechint.com](https://www.unicorntechint.com)
- GitHub: [@VarakornUnicornTech](https://github.com/VarakornUnicornTech)
- สถานที่: กรุงเทพมหานคร, ประเทศไทย

## สัญญาอนุญาต

MIT License — ดูรายละเอียดที่ [LICENSE](LICENSE)

---

*RoundTable Framework v1.0.0 — สร้างโดย Unicorn Tech Integration Co., Ltd.*
