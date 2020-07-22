# ONBUILD

- คำสั่ง `ONBUILD` จะทำงานหลังจากสร้าง `Dockerfile` เสร็จสิ้น โดย `ONBUILD` จะ execute ใน child image ที่ได้กำหนดที่ `FROM` ของ image ที่สร้าง
- บทความได้เปรียบเทียบว่า คำสั่ง `ONBUILD` เป็นคำสั่งของ parent `Dockerfile` ที่ให้กับ child `Dockerfile`
- Docker build จะทำการเรียกใช้งานคำสั่ง `ONBUILD` ก่อนคำสั่งอื่น ๆ ใน child `Dockerfile`
- `ONBUILD` เหมาะสำหรับ images ที่จะถูกสร้าง จาก `FROM` ของอีก image

ตัวอย่างเช่น : หากต้องการใช้ `ONBUILD` สำหรับ language stack ของ image ที่สร้างขึ้นมาเอง สามารถเขียนภาษานั้นใน `Dockerfile`เช่น ตัวแปร `Ruby’s ONBUILD`

- Images ที่สร้างด้วย `ONBUILD` ควรมี tag แยกระบุชัดเจน ให้เข้าใจได้ง่าย เพื่อให้ผู้ใช้ทำการเลือกใช้งานได้ต่อไป

ตัวอย่างเช่น : `ruby:1.9-onbuild` or `ruby:2.0-onbuild`

- ควรระวังการใช้ `ADD` หรือ `COPY` ใน `ONBUILD` การที่ “onbuild” image fails ถ้าการ build ที่เกิดขึ้นจากมีบางส่วนที่ขาดหายไป
