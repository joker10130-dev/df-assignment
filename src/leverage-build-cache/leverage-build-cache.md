## Leverage build cache

### ใช้ cache ช่วยในการ build image

เวลามีการ build image เกิดขึ้น Docker จะทำตามคำสั่งต่างๆ ที่อยู่ภายใน `Dockerfile` เมื่อมีการสร้าง image Docker จะหา image ใน cache ว่าเคยมีอยู่ก่อนหรือไม่เพื่อทำการ reuse image 
ซึ่งจะช่วยให้การสร้าง image นั้นเร็วขึ้นไม่ต้องเสียเวลาสร้างใหม่อีก แต่ถ้าไม่ต้องการใช้ cache สามารถใช้คำสั่ง `--no-cache=true` ใน `docker build` ได้ ซึ่งการทำงานของ cache หลักๆก็มีดังนี้

* เริ่มด้วยการเทียบคำสั่งใน parent image ซึ่งอยู่ใน cache กับ child image ว่ามีคำสั่งเหมือนกันไหม ถ้าเหมือนกันจะมีการ `Using cache` เกิดขึ้น
<br>

<p align="center">
  <img src="https://raw.githubusercontent.com/krittin1/df-g4/master/src/leverage-build-cache/27.png" />
</p>



###### T. Dumrongthawatchai. (2019, September 10). Docker EP. 6: ลองใช้ Docker กับโปรเจคของเรากันเถอะ. Retrived from https://dekcrack.com/docker-ep-6-ลองใช้-docker-กับโปรเจคของเรากันเถอะ

* สำหรับคำสั่ง `ADD` และ `COPY` ที่อยู่ใน `Dockerfile` จะมีการตรวจสอบความถูกต้องจากค่าคำนวณ checksum โดยจะมีการเปรียนเทียบกับค่า checksum ของ image ที่อยู่ใน cache มาก่อนถ้ามีการเปลี่ยนแปลง
ต่างๆ เกิดขึ้นจะไม่มีการใช้ cache เข้ามาช่วย

* แต่คำสั่ง `ADD` และ `COPY` จะไม่มีการตรวจสอบไฟล์ที่อยู่ภายใน Docker Container หากมีการ hit เกิดขึ้นใน cache 
