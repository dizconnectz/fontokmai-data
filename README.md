# fontokmai-data: สำเนาข้อมูลของ fontokmai (ฝนตกไหม)

> **ไม่ใช่ประกาศทางการ** ประกาศเตือนภัยให้ติดตามจาก[กรมอุตุนิยมวิทยา](https://www.tmd.go.th/)ควบคู่เสมอ
>
> fontokmai ไม่ได้เกี่ยวข้องหรือได้รับการสนับสนุนจากกรมอุตุนิยมวิทยา
>
> ไฟล์ข้อมูลอยู่ใน branch [`gh-pages`](https://github.com/dizconnectz/fontokmai-data/tree/gh-pages) ส่วน branch นี้มีแค่เอกสาร

## นี่คืออะไร

repo นี้เก็บสำเนาข้อมูลชุดล่าสุดที่ระบบ fontokmai รวบรวมจากหน่วยงานและผู้ให้บริการข้อมูล รวมกับค่าที่ระบบคำนวณเอง
ข้อมูลชุดนี้ใช้กับเว็บ [ฝนตกไหม](https://dizconnectz.github.io/fontokmai/) ซึ่งเป็นแผนที่ฝน น้ำ และน้ำท่วมของไทย
ส่วนโค้ด เอกสาร และวิธีคำนวณอยู่ใน repo หลัก [dizconnectz/fontokmai](https://github.com/dizconnectz/fontokmai)

- เซิร์ฟเวอร์ของโปรเจกต์อัปโหลดข้อมูลชุดใหม่ราวทุก 15 นาที โดยแทนที่ branch `gh-pages` ทั้งหมดด้วย commit เดียว
  **repo นี้จึงไม่มีประวัติย้อนหลัง** และไม่ควรอ้างถึง commit ใด commit หนึ่งเป็นการถาวร
- อย่าแก้ไฟล์หรือเปิด PR ใน `gh-pages` เพราะรอบถัดไปจะเขียนทับ ถ้าพบข้อมูลผิด แจ้งได้ที่ Issues ของ repo หลัก (ดูหัวข้อสุดท้าย)

## ที่อยู่ไฟล์

ให้เริ่มที่ **<https://dizconnectz.github.io/fontokmai-data/data/v1/manifest.json>**

- `manifest.json` บอกรุ่นของชุดข้อมูล (`generation_id`) เวลาที่สร้าง (`generated_at` เป็นเวลา UTC) เวลาที่ควรมีรอบถัดไป (`next_due_at`)
  สถานะของแหล่งหลัก (`source_status`) และรายชื่อไฟล์พร้อมขนาดและ SHA-256 (`files`)
- ไฟล์อื่นอยู่ใต้ `data/v1/` ตาม `files[].path` เช่น `alerts.json`, `radar.json`, `forecast/`, `water/`, `bkk/`, `floods/`, `live/`, `summary/`, `ref/`
- host อนุญาตให้ทุกเว็บอ่านได้ (`Access-Control-Allow-Origin: *`) แต่เก็บ cache 10 นาที (`Cache-Control: max-age=600`)
  ตอนโหลด `manifest.json` จึงควรต่อท้ายด้วย `?t=<เวลา>` แล้วโหลดไฟล์อื่นตามรายการของรุ่นเดียวกัน อย่ารวมไฟล์ต่างรุ่นกัน
- แต่ละไฟล์มีเวลาของข้อมูลเอง เช่น `fetched_at`, `observed_at`, `report_date` และบางไฟล์อัปเดตตามรอบของแหล่ง ไม่ใช่ทุก 15 นาที
  เช่น พยากรณ์ฝนทุก 6 ชม. แนวโน้มแม่น้ำวันละครั้ง และข้อมูล กทม. เป็นครั้งคราว ควรดูเวลาก่อนใช้ทุกครั้ง และอย่าถือว่าข้อมูลที่ขาดหมายความว่าปลอดภัย
- รูปแบบไฟล์ (JSON Schema, TypeScript types และตัวอย่าง) อยู่ที่ [`contracts/v1/`](https://github.com/dizconnectz/fontokmai/tree/main/contracts/v1) ใน repo หลัก
  ส่วนลำดับการโหลดและการตรวจว่าข้อมูลยังใหม่อยู่ใน [`contracts/v1/README.md`](https://github.com/dizconnectz/fontokmai/blob/main/contracts/v1/README.md)
- ที่อยู่นี้เป็น host ช่วงพัฒนาและอาจย้ายได้ในอนาคต ถ้าจะดึงข้อมูลอัตโนมัติ ไม่ควรดึงถี่กว่ารอบละ 15 นาที

## ไม่ใช่ประกาศทางการ

- `alerts.json` เป็นไฟล์เดียวที่มีประกาศเตือนภัยของกรมอุตุนิยมวิทยา (รูปแบบ CAP) ตามต้นฉบับ
- ค่าที่ระบบคำนวณเอง **ไม่ใช่ประกาศของหน่วยงาน** และต้องแสดงแยกจากประกาศเสมอ ได้แก่
  จุดที่ต้องระวัง (`summary/overview.json`), คลองที่อาจล้น (`summary/canals.json` ซึ่งใช้เกณฑ์ทดลอง),
  แนวโน้มฝน 14 วัน (`forecast/outlook.json` ทดลอง) และแนวโน้มน้ำในแม่น้ำจากแบบจำลอง (`forecast/rivers.json`)
- ห้ามใช้ข้อมูลชุดนี้เป็นแหล่งเตือนภัยเพียงแหล่งเดียว ให้ติดตามประกาศของกรมอุตุนิยมวิทยาและหน่วยงานในพื้นที่ควบคู่
- ข้อมูลที่ไม่ได้ลงนามรับรองจากกรมอุตุนิยมวิทยาใช้ประกอบนิติกรรมไม่ได้

## เครดิตแหล่งข้อมูล

ตารางนี้สรุปจาก [docs/sources.md](https://github.com/dizconnectz/fontokmai/blob/main/docs/sources.md) และช่อง `credit_th` ในไฟล์ข้อมูล (ตรวจเมื่อ 2026-10-17)
เครดิตฉบับเต็มอยู่ที่หน้า[แหล่งข้อมูลและเครดิต](https://dizconnectz.github.io/fontokmai/sources/)

| แหล่ง | ใช้ทำอะไร (ไฟล์) | เงื่อนไขโดยย่อ | ลิงก์ |
|---|---|---|---|
| กรมอุตุนิยมวิทยา | ประกาศเตือนภัย CAP (`alerts.json`) · ภาพเรดาร์รวม (`radar.json`, `radar/*.png`) · อากาศวันนี้รายสถานี (`weather/today.json` ผ่านระบบ DXS ของ กทม.) | CAP เป็น public domain และให้เครดิตกรมฯ · ข้อมูลอื่นเผยแพร่ได้แบบไม่แสวงหาประโยชน์ทางการค้า โดยต้องระบุ “กรมอุตุนิยมวิทยา” ทุกครั้ง | [tmd.go.th](https://www.tmd.go.th/) |
| Open-Meteo | พยากรณ์ฝน 72 ชม. และ 7 วัน (`forecast/rain.json` จากแบบจำลองของ ECMWF, NOAA, DWD และอื่นๆ) · แนวโน้มฝน 14 วันแบบ ensemble (`forecast/outlook.json` จาก ECMWF และ NOAA) | CC BY 4.0 | [open-meteo.com](https://open-meteo.com/) |
| GloFAS (Copernicus Emergency Management Service) ผ่าน Open-Meteo | แนวโน้มน้ำในแม่น้ำจากแบบจำลอง (`forecast/rivers.json`) | CC BY 4.0 | [Flood API](https://open-meteo.com/en/docs/flood-api) |
| iTIC และ Longdo Traffic | รายงานน้ำท่วมตอนนี้ (`live/floods.json`) · เหตุน้ำท่วมถนนย้อนหลัง (ส่วนหนึ่งของ `ref/road_flood_history.json`) | CC BY 4.0 | [traffic.longdo.com](https://traffic.longdo.com/) |
| สำนักการระบายน้ำ กรุงเทพมหานคร | ระดับน้ำคลอง ฝน ถนนท่วม และสรุปสถานการณ์ (`bkk/*.json` ผ่านระบบ DXS) · สถิติน้ำท่วมขังถนนสายหลัก (ส่วนหนึ่งของ `ref/road_flood_history.json`) | DXS อนุมัติให้นำไปเผยแพร่ต่อได้ โดยให้เครดิตเป็นข้อความ ห้ามใช้โลโก้ · สถิติเป็น CC BY | [dds.bangkok.go.th](https://dds.bangkok.go.th/), [data.bangkok.go.th](https://data.bangkok.go.th/dataset/frd_dds) |
| กรมชลประทาน | เขื่อนใหญ่ 35 แห่ง (`water/dams.json`) · น้ำไหลและสถานะของสถานีจากรายงานสถานการณ์น้ำประจำวันและผังน้ำเจ้าพระยาตอนล่าง รวมตัวเลขสำรองบางสถานีจากศูนย์อุทกวิทยาชลประทานภาคกลาง (`water/flows.json`) | ข้อมูลเขื่อนเป็น Creative Commons Attribution (data.go.th) · จากรายงานเผยแพร่เฉพาะตัวเลขและสถานะ พร้อมเครดิต เวลา และลิงก์ ไม่เผยแพร่ไฟล์ PDF หรือภาพ | [app.rid.go.th/reservoir](https://app.rid.go.th/reservoir), [รายงานประจำวัน](https://water.rid.go.th/flood/flood/daily.pdf) |
| GISTDA | พื้นที่น้ำท่วมจากภาพดาวเทียมรายอำเภอ (`floods/satellite.json`) · พื้นที่น้ำท่วมซ้ำซากรายตำบล (`ref/flood_freq.json`) | ข้อมูลเปิดภาครัฐ ให้เครดิต GISTDA | [disaster.gistda.or.th](https://disaster.gistda.or.th/) |
| กรมการปกครอง | ชื่อและจุดที่ตั้งของตำบล อำเภอ และจังหวัด (`ref/places.json`) | CC BY | [data.go.th](https://data.go.th/dataset/5370f16a-b553-45ca-929d-7db9a6fc5043) |
| กรมแผนที่ทหาร / OCHA (COD-AB ผ่าน geoBoundaries) | ขอบเขตจังหวัดและอำเภอที่ปรับให้เรียบแล้ว (`ref/boundaries.json`) | CC BY 3.0 IGO | [HDX](https://data.humdata.org/dataset/cod-ab-tha) |
| OpenStreetMap | เส้นแม่น้ำ (`ref/river_lines.json`) · เส้นคลอง (`ref/canals.json`) · ตำแหน่งเขื่อนและหัวคลอง (ใน `water/dams.json` และ `water/flows.json`) | © OpenStreetMap contributors · ODbL 1.0 | [openstreetmap.org/copyright](https://www.openstreetmap.org/copyright) |
| HydroRIVERS v1.0 (HydroSHEDS, WWF) | เส้นทางน้ำท้ายเขื่อน (ช่อง `downstream_th` ใน `water/dams.json`) | HydroSHEDS v1 License: ต้องแสดงข้อความในหัวข้อถัดไป | [hydrosheds.org](https://www.hydrosheds.org/products/hydrorivers) |
| กล้อง CCTV ของกรมทรัพยากรน้ำ, กฟผ. และ กทม. | ทะเบียนกล้อง (`ref/cctv.json`) มีชื่อ ตำแหน่ง และลิงก์ไปหน้ากล้องของเจ้าของ ไม่มีภาพในชุดนี้ | ลิงก์ไปหน้าของเจ้าของเท่านั้น | [telemetry.dwr.go.th](https://telemetry.dwr.go.th/reportCctv), [egatwater.egat.co.th](https://egatwater.egat.co.th/) |

## ใบอนุญาต

- โค้ดของ fontokmai อยู่ใน repo หลัก ภายใต้ PolyForm Noncommercial 1.0.0
  ดู [LICENSE](https://github.com/dizconnectz/fontokmai/blob/main/LICENSE) และ [NOTICE](https://github.com/dizconnectz/fontokmai/blob/main/NOTICE)
- เนื้อหาและข้อมูลที่ fontokmai สร้างเอง (ข้อความและผลประเมิน เช่น `summary/overview.json`, `summary/canals.json`) ใช้ CC BY-NC 4.0 ตาม NOTICE
  คือให้เครดิตตามที่ NOTICE ระบุพร้อมลิงก์ และห้ามใช้เชิงพาณิชย์
- ข้อมูลของหน่วยงานและผู้ให้บริการอื่น **ยังอยู่ภายใต้เงื่อนไขของเจ้าของข้อมูลนั้น** (ดูตารางด้านบนและ docs/sources.md)
  การนำไฟล์มาไว้ใน repo นี้ไม่ได้เปลี่ยนเงื่อนไขเหล่านั้น เช่น ส่วนที่ดัดแปลงจาก OpenStreetMap ยังเป็น ODbL 1.0
- ชื่อ “fontokmai” “ฝนตกไหม” และโลโก้ ไม่รวมอยู่ในสิทธิ์ตามใบอนุญาต

### ข้อความที่ข้อมูลบางชุดกำหนดให้แสดง (Third-party notices)

- **ขอบเขตจังหวัดและอำเภอ** (`ref/boundaries.json`) ปรับให้เรียบจาก Thailand Subnational Administrative Boundaries (COD-AB)
  ของกรมแผนที่ทหาร / OCHA ผ่าน geoBoundaries (Runfola, D. et al. (2020) geoBoundaries: A global database of political administrative
  boundaries. PLoS ONE 15(4): e0231866) ภายใต้ [CC BY 3.0 IGO](https://creativecommons.org/licenses/by/3.0/igo/)
  fontokmai ปรับเส้นให้เรียบและเพิ่มรหัสของกรมการปกครอง
- **OpenStreetMap** (`ref/river_lines.json`, `ref/canals.json` และตำแหน่งใน `water/dams.json`, `water/flows.json`)
  © OpenStreetMap contributors เผยแพร่ภายใต้ [Open Database License 1.0](https://opendatacommons.org/licenses/odbl/1-0/)
- **HydroSHEDS** (เส้นทางน้ำท้ายเขื่อนใน `water/dams.json`): This product (fontokmai, ฝนตกไหม) incorporates data from the HydroSHEDS version 1
  database which is © World Wildlife Fund, Inc. (2006-2022) and has been used herein under license. WWF has not evaluated the data as altered
  and incorporated within fontokmai, and therefore gives no warranty regarding its accuracy, completeness, currency or suitability for any
  particular purpose. Portions of the HydroSHEDS v1 database incorporate data which are the intellectual property rights of © USGS (2006-2008),
  NASA (2000-2005), ESRI (1992-1998), CIAT (2004-2006), UNEP-WCMC (1993), WWF (2004), Commonwealth of Australia (2007), and Her Royal Majesty
  and the British Crown and are used under license. The HydroSHEDS v1 database and more information are available at https://www.hydrosheds.org.

## แจ้งปัญหาหรือติดต่อ

แจ้งข้อมูลผิด ลิงก์เสีย หรือถามเรื่องการนำข้อมูลไปใช้ได้ที่ [Issues ของ repo หลัก](https://github.com/dizconnectz/fontokmai/issues)
repo นี้ไม่รับ PR ที่แก้ไฟล์ข้อมูล

## English (for developers)

- A public, latest-only snapshot of the data behind [fontokmai](https://dizconnectz.github.io/fontokmai/), a rain, water and flood map of Thailand.
  Code and documentation: [dizconnectz/fontokmai](https://github.com/dizconnectz/fontokmai).
- Entry point: <https://dizconnectz.github.io/fontokmai-data/data/v1/manifest.json>, refreshed about every 15 minutes.
  It lists every file of the current generation with its size and SHA-256. Schemas, TypeScript types and examples are in
  [`contracts/v1/`](https://github.com/dizconnectz/fontokmai/tree/main/contracts/v1) of the main repository.
- The `gh-pages` branch is replaced by a single commit on every round, so there is no history here. Do not open pull requests against it.
- This is not an official warning service. Only `alerts.json` carries official alerts (Thai Meteorological Department, CAP).
  Summaries, canal outlooks and trends are computed by fontokmai. fontokmai is not affiliated with or endorsed by the TMD.
- Licence: code under PolyForm Noncommercial 1.0.0; content and data made by fontokmai under CC BY-NC 4.0 (see NOTICE);
  third-party data stays under its owners' terms (table above and `docs/sources.md`).
- Issues: <https://github.com/dizconnectz/fontokmai/issues>
