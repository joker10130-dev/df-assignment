## FROM

คำสั่ง `FROM` เป็นคำสั่งที่อยู่ภายใน `Dockerfile` ซึ่งจะเป็นการบอกที่มาของ image นั้น โดยจะ initialize การ set image ให้ทำตามคำสั่งต่างๆ ถัดไป
ใน Dockerfile มักเริ่มต้นคำสั่งด้วย คำสั่ง FROM จะมีได้หลายคำสั่งในไฟล์เดียวก็ได้ สำหรับสร้างหลาย image เราสามารถตั้งชื่อให้กับ stage ได้โดยเพิ่ม
`AS name` ใน `FROM` 

**ตัวอย่างที่ 1: คำสั่ง FROM**

```dockerfile
    FROM [--platform=<platform>] <image> [AS <name>]
```
หรือ


```dockerfile
    FROM [--platform=<platform>] <image>[:<tag>] [AS <name>]
```

หรือ

```dockerfile
    FROM [--platform=<platform>] <image>[@<digest>] [AS <name>]

```
การใช้ `tag` หรือ `digest` จะมีหรือไม่มีก็ได้ หากไม่มีการเลือกตัวใดตัวหนึ่ง builder จะใช้ `tag` ตัวล่าสุดเป็นค่า default builder จะคืนค่า 
error ถ้าไม่สามารถหา `tag` เจอ
สามารถใช้ `--platform` flag ในการระบุ platform ของ image ในกรณีที่ `FROM` อ้างอิงถึงหลาย platform

### ทำความเข้าใจระหว่าง ARG และ FROM

คำสั่ง `FROM` จะสนับสนุนกับตัวแปรที่ถูกประกาศกับคำสั่ง `ARG` ที่อยู่ก่อนคำสั่ง `FROM`

**ตัวอย่างที่ 2: คำสั่ง FROM และ ARG**

```dockerfile
ARG  CODE_VERSION=latest
FROM base:${CODE_VERSION}
CMD  /code/run-app

FROM extras:${CODE_VERSION}
CMD  /code/run-extras
```

```dockerfile

ARG VERSION=latest
FROM busybox:$VERSION
ARG VERSION
RUN echo $VERSION > image_version
```

คำสั่ง `ARG` จะใช้ไม่ได้กับคำสั่งหลังจาก `FROM` เพราะอยู่นอก build stage
ถ้าเป็นไปได้ อาจจะเริ่มต้นใช้ image แบบ official image ได้โดยทำการ pull image ได้จาก registry อย่าง Docker Hub ซึ่งในบทความแนะนำ
ตัว  [`Alpine image`](https://hub.docker.com/_/alpine/) เพราะใช้พื้นที่น้อยและทำงานกับ Linux
