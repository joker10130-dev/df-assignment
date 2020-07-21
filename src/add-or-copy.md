# ADD or COPY

`ADD` และ `COPY` มีลักษณะการทำงานที่คล้ายกัน คือนำข้อมูลเข้าไปยัง Docker

COPY จะนำข้อมูลจาก local file ไปยัง Docker container

`ADD` ทำงานเหมือน `COPY`จะมีความสามารถที่เพิ่มขึ้นมา คือแยกไฟล์ .tar อัตโนมัติจาก local ไปยัง Docker image เช่น

```
ADD rootfs.tar.xz /

```

## COPY

ในกรณีที่มีหลาย Dockerfile ที่ต่างกัน ให้ `COPY` ทีละไฟล์ แทนที่จะทำพร้อมกัน วิธีนี้จะช่วยจัดการ cache จากการ build ในแต่ละไฟล์

ตัวอย่างเช่น :

```
COPY requirements.txt /tmp/
RUN pip install --requirement /tmp/requirements.txt
COPY . /tmp/

```

ผลลัพธ์คือมี cache อยู่บ้างจากขั้นตอนการ `RUN` ถ้าใส่ `COPY . /tmp` ก่อนหน้าขั้นตอนการ RUN

## ADD

การใช้ `ADD` ในการ fetch packages จาก remote URLs และใช้ `curl` หรือ `wget` ช่วยในการจัดการการทำงานพร้อม ๆ กัน เช่น การแตกไฟล์และลบไฟล์ที่ไม่ต้องการที่จะเพิ่มเข้าไปยัง Docker image

ตัวอย่างเช่น :

```
ADD http://example.com/big.tar.xz /usr/src/things/
RUN tar -xJf /usr/src/things/big.tar.xz -C /usr/src/things
RUN make -C /usr/src/things all

```

```
RUN mkdir -p /usr/src/things \
    && curl -SL http://example.com/big.tar.xz \
    | tar -xJC /usr/src/things \
    && make -C /usr/src/things all

```

สำหรับหรับ files หรือ directories อื่น ๆ ที่ไม่ต้องการใช้ `ADD` ในการจักการการแตกไฟล์ (tar auto-extraction) ควรใช้ `COPY`
