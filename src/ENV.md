### **ENV**

เพื่อให้ง่ายต่อการเรียกใช้งาน software เราสามารถใช้งาน `ENV` เพื่อทำการ update path ของ environment variable สำหรับ software ที่ติดตั้งอยู่ใน container ได้ ยกตัวอย่างเช่น

```
ENV PATH /usr/local/nginx/bin:$PATH
```

คำสั่งนี้สามารถจัดหา environment variables ที่จำเป็นในแต่ละ service และยังสามารถตั้งค่าให้ใช้งาน version ที่เป็นที่นิยมใช้กันโดยทั่วไป เพื่อให้ง่ายต่อการบำรุงรักษา มีการใช้งานดังตัวอย่างต่อไปนี้

```
ENV PG_MAJOR 9.3
ENV PG_VERSION 9.3.4
RUN curl -SL http://example.com/postgres-$PG_VERSION.tar.xz | tar -xJC /usr/src/postgress && …
ENV PATH /usr/local/postgres-$PG_MAJOR/bin:$PATH
```

แต่ละคำสั่งของ `ENV` จะมีการสร้าง intermediate layer เหมือนกับคำสั่ง `RUN` หมายความว่า ถึงแม้เราจะไม่ได้ตั้งค่า environment variable ใน layer ถัดไปแต่มันจะยังมีอยู่ใน layer นี้และไม่สามารถเปลี่ยนแปลงค่าได้อีกด้วย สามารถทดลองได้โดยเขียน Dockerfile ดังนี้ 

```
FROM alpine
ENV ADMIN_USER="mark"
RUN echo $ADMIN_USER > ./mark
RUN unset ADMIN_USER
```

ต่อด้วย

```
$ docker run --rm test sh -c 'echo $ADMIN_USER'
```

จะได้ผลลัพธ์ `mark`

ซึ่งเราสามารถหลีกเลี่ยง เพื่อที่จะไม่ตั้งค่า environment variable จริง ๆ ด้วยคำสั่ง `RUN` ด้วย shell commands ในการตั้งค่า, ใช้งาน และคืนค่า variable ทุกตัวใน single layer สามารถแยกชุดคำสั่งออกจากกันด้วย `;` หรือ `&&` ซึ่งถ้าเลือก `&&` ในการใช้งานหากมีคำสั่งที่ทำงานผิดพลาด `docker build` ก็จะไม่สามารถดำเนินการได้ด้วย ซึ่งเป็นสิ่งที่ดี และควรมีการใช้ `\` เพื่อให้ง่ายต่อการอ่าน ยกตัวอย่างเช่น

```
FROM alpine
RUN export ADMIN_USER="mark" \
    && echo $ADMIN_USER > ./mark \
    && unset ADMIN_USER
CMD sh
```
