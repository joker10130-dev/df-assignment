# Use multi-stage builds
การทำ Multi-stage builds ใน Dockerfile สามารถช่วยในการลดขนาด Image ได้ โดยจะใช้ Image ที่ชื่อ scratch มาช่วยในการทำให้ Image ที่สร้างนั้นเป็น Single layer image นั่นเอง

## ขั้นตอนในการทำ Multi-stage builds
1. ติดตั้งเครื่องมือทีั่จำเป็นในการสร้าง Application
2. ติดตั้ง หรืออัพเดท Library dependencies
3. สร้าง Application ของเรา

### ตัวอย่างการสร้าง Go application

```bash
FROM golang:1.11-alpine AS build

# ติดตั้งเครื่องมือที่จำเป็นใน Project
# หากใช้คำสั่ง `docker build --no-cache .` ในการสร้าง Image จะเป็นการอัพเดท Dependencies
RUN apk add --no-cache git
RUN go get github.com/golang/dep/cmd/dep

# กำหนดให้ใช้ dependencies สำหรับ Project นี้จากไฟล์ Gopkg.toml และ Gopkg.lock โดยนำไปไว้ใน /go/src/project/ และตั้ง Work directory เป็น /go/src/project/
# ใน Layer นี้จะถูกสร้างใหม่อีกครั้งเมื่อไฟล์ Gopkg แต่ละไฟล์ถูกอัพเดทเท่านั้น
COPY Gopkg.lock Gopkg.toml /go/src/project/
WORKDIR /go/src/project/
# ลง library dependencies
RUN dep ensure -vendor-only

# สำเนาไฟล์ทั้งหมดใน Project และ Build เป็น Application
# ใน Layer นี้จะถูกสร้างใหม่อีกครั้งเมื่อไฟล์ใน Project directory มีการเปลี่ยนแปลง
COPY . /go/src/project/
RUN go build -o /bin/project

# สร้าง Single layer image ด้วย scratch และ Application ที่ Build จากขั้นตอนก่อนหน้า
FROM scratch
COPY --from=build /bin/project /bin/project
ENTRYPOINT ["/bin/project"]
CMD ["--help"]
```