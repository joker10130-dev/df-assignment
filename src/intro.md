# Intro

Dockerfile มีไว้เพื่อกำหนดการทำงานล่วงหน้าในการจัดการคำสั่งสำหรับสร้าง images เพื่อให้เป็นไปโดยอัตโนมัติและไม่เสียเวลา
ตัวอย่าง

```
FROM ubuntu:18.04
COPY . /app
RUN make /app
CMD python /app/app.py
```

FROM บอกที่มา image
COPY เพิ่มไฟล์ล่าสุดจาก Docker
RUN  สร้าง application
CMD  คำสั่งพิเศษไว้ใช้กับสิ่งของข้างใน container