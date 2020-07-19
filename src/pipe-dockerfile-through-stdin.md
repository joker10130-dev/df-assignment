# Pipe Dockerfile through stdin
Docker สามารถสร้าง Image ได้โดยการ Piping ตัว Dockerfile ผ่าน stdin ได้ที่ Terminal เลย ทำให้ลดขั้นตอนการสร้าง Dockerfile ไว้ในเครื่องได้ โดยสามารถใช้ได้ 2 วิธี ดังตัวอย่างที่ 1
### ตัวอย่างที่ 1
```bash
echo -e 'FROM busybox\nRUN echo "hello world"' | docker build -
```

หรือ

```bash
docker build -<<EOF
FROM busybox
RUN echo "hello world"
EOF
```

## BUILD AN IMAGE USING A DOCKERFILE FROM STDIN, WITHOUT SENDING BUILD CONTEXT
### รูปแบบการใช้งาน

```bash
docker build [OPTIONS] -
```

ดังที่ได้กล่าวข้างต้น การใช้ Piping ทำให้ไม่จำเป็นต้องมี Dockerfile อยู่ในเครื่องจริง ๆ ก็ได้ ซึ่งเหมาะกับการสร้าง Image ที่ไม่จำเป็นต้องมีการดึงไฟล์เข้าไปใช้ใน Image
**หากพยายามใช้คำสั่ง ADD หรือ COPY ในวิธีการ Piping จะทำให้การสร้าง Image นั้น Fail ได้ ดังตัวอย่างที่ 2**
### ตัวอย่างที่ 2

```bash
docker build -t myimage:latest -<<EOF
FROM busybox
COPY somefile.txt .
RUN cat /somefile.txt
EOF
```

จะเห็นได้ว่ามีการพยายาม COPY ไฟล์ somefile.txt เข้าไปในการสร้าง Image ผลที่ได้จะเป็นดังนี้

```bash
...
Step 2/3 : COPY somefile.txt .
COPY failed: stat /var/lib/docker/tmp/docker-builder249218248/somefile.txt: no such file or directory
```

## BUILD FROM A LOCAL BUILD CONTEXT, USING A DOCKERFILE FROM STDIN
### รูปแบบการใช้งาน

```bash
docker build [OPTIONS] -f- PATH
```

หากต้องการจะนำไฟล์ใด ๆ เข้าไปในกระบวนการสร้าง Image ด้วยจะต้องกำหนดที่อยู่ของไฟล์เข้าไปด้วย ดังตัวอย่างที่ 3
### ตัวอย่างที่ 3

```bash
docker build -t myimage:latest -<<EOF
FROM busybox
COPY somefile.txt .
RUN cat /somefile.txt
EOF
```

ด้วยวิธีการนี้จะทำให้การ ADD หรือ COPY ไฟล์เข้าไปในการสร้าง Image ได้

## BUILD FROM A REMOTE BUILD CONTEXT, USING A DOCKERFILE FROM STDIN
### รูปแบบการใช้งาน

```bash
docker build [OPTIONS] -f- PATH
```

มีรูปแบบการใช้งานเช่นเดียวกับในหัวข้อที่ผ่านมา แต่การกำหนด PATH นั้นสามารถอ้างอิงจาก Git repository ได้ ดังตัวอย่างที่ 4
### ตัวอย่างที่ 4

```bash
docker build -t myimage:latest -f- https://github.com/docker-library/hello-world.git <<EOF
FROM busybox
COPY hello.c .
EOF
```

**เบื้องหลังของกลไกนี้คือ Docker จะทำการ Clone จาก Git repository ที่กำหนดมาไว้บนเครื่องก่อน แล้วส่งไฟล์เหล่านั้นเข้าไปในกระบวนการสร้าง Image ต่อไป**