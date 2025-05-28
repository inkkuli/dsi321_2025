# EGAT Real-time Power Generation Scraper

สคริปต์ Python นี้ออกแบบมาเพื่อดึงข้อมูลการผลิตไฟฟ้าแบบเรียลไทม์จากเว็บไซต์ของการไฟฟ้าฝ่ายผลิตแห่งประเทศไทย (EGAT) ที่ URL https://www.sothailand.com/sysgen/egat/ โดยอาศัย Selenium เพื่อโต้ตอบกับหน้าเว็บและอ่านข้อมูลจาก Console Log ซึ่งเป็นแหล่งที่เว็บไซต์ใช้สำหรับอัปเดตข้อมูลแบบ dynamic ข้อมูลที่ได้จะถูกบันทึกลงในไฟล์ CSV อย่างต่อเนื่อง เพื่อเก็บข้อมูลใหม่ตามช่วงเวลาที่กำหนด


## โครงสร้างโปรเจค

```
egat-scraper-project/
├── src/
│   ├── __init__.py
│   ├── scraper/
│   │   ├── __init__.py
│   │   └── egat_scraper.py        # Contains EGATRealTimeScraper class
│   ├── config.py                  # Configuration settings
│   ├── tasks.py                   # Prefect task definitions
│   └── flows.py                   # Prefect flow definitions
├── notebooks/
│   └── run_scraper_and_save_to_lakefs.ipynb  # Your original notebook for reference
├── run_scheduled_flow.py          # Script to run/deploy the Prefect flow
├── requirements.txt               # Project dependencies
├── docker-compose.yml             # Existing docker-compose
└── .env                           # For environment variables (you'll create this)
```

## คุณสมบัติเด่น

* **ดึงข้อมูลแบบ Real-time:**  บันทึกค่ากำลังการผลิตและอุณหภูมิขณะนั้น
* **อ่านข้อมูลจาก Console Log:** ใช้เทคนิคเฉพาะในการอ่าน log ที่ JavaScript แสดงใน Console แทนการพาร์ส HTML โดยตรง
* **รองรับ Headless mode:** ใช้ Chrome ผ่าน Selenium ในโหมดไม่มีหน้าจอ เพื่อประสิทธิภาพและทำงานเบื้องหลัง
* **บริหารจัดการ ChromeDriver อัตโนมัติ:** ไม่ต้องติดตั้งไดรเวอร์เอง ด้วย webdriver-manager
* **จัดเก็บข้อมูลในรูป CSV:** ต่อข้อมูลใหม่ในไฟล์ CSV ที่ระบุ โดยมี timestamp กำกับ
* **รองรับการทำงานต่อเนื่อง:** ทำ scraping แบบวนซ้ำได้โดยตั้งช่วงเวลาไว้
* **มีระบบ Logging:** มีการบันทึกข้อความ log เพื่อใช้วิเคราะห์หรือแก้ปัญหาที่เกิดขึ้นระหว่างรัน


## หลักการทำงาน

เว็บไซต์ https://www.sothailand.com/sysgen/egat/ แสดงข้อมูลแบบเรียลไทม์ โดย JavaScript จะอัปเดตค่ากำลังผลิต (หน่วย MW) และอุณหภูมิ และแสดงข้อความใน Console Log ด้วย สคริปต์นี้จึงใช้ Console Log เป็นแหล่งดึงข้อมูล โดยมีขั้นตอนการทำงานหลักคือ:

1. **เริ่มต้น WebDriver:** เปิด Chrome ผ่าน Selenium ในโหมด Headless พร้อมตั้งค่าให้สามารถเก็บ Console Logs ได้
2. **เปิดหน้าเว็บไซต์เป้าหมาย:** เปิดหน้าเว็บ กฟผ. ที่ระบุ
3. **รอโหลดข้อมูล:** ให้เวลาเว็บโหลดและ JavaScript ทำงานจนเสร็จ
4. **ดึงข้อมูลจาก Console:** ค้นหาข้อความที่ขึ้นต้นด้วย `updateMessageArea:` จาก log 
5. **แยกและจัดรูปแบบข้อมูล:** ใช้ regex ดึงข้อมูลที่สำคัญ (timestamp, MW, temp ฯลฯ) ออกมา
6. **บันทึกลง CSV:** แปลงเป็น dictionary แล้วต่อท้ายข้อมูลลงไฟล์ CSV
7. **วนซ้ำ (ถ้าตั้งค่า):** ทำซ้ำทุก ๆ n วินาที ถ้าใช้ `scrape_continuously` 
8. **ปิด WebDriver อย่างเหมาะสม:** เมื่อสคริปต์จบการทำงานหรือหยุดกลางทาง


## สิ่งที่ต้องมีก่อนเริ่มใช้งาน

* Python 3.7 ขึ้นไป
* ติดตั้ง Google Chrome ไว้ในเครื่อง

## การติดตั้งและตั้งค่า

1. **Clone Repository (ถ้ามี) หรือบันทึกไฟล์สคริปต์:**
```bash
# ถ้าเป็น Repository
# git clone <repository_url>
# cd <repository_directory>
```

2. **สร้าง Virtual Environment:**
```bash
python -m venv venv
source venv/bin/activate  # สำหรับ Windows: venv\Scripts\activate
```

3. **ติดตั้ง Package ที่จำเป็น:**
```txt
pandas
selenium
webdriver-manager
```

จากนั้นรันคำสั่ง:
```bash
pip install -r requirements.txt
```