## Understand build context (เข้าใจคำว่า build)
เมื่อใช้คำสั่ง build ตำแหน่งแฟ้มที่ทำงานอยู่จะเรียกว่า build context ซึ่ง Dockerfile จะอยู่ที่นี่ แต่เราก็สามารถกำหนดเองได้ว่าไว้ที่ไหนด้วย -f
แต่ไม่ว่าจะอยู่ที่ไหน ก็ต้องส่งไปที่ Docker daemon แบบ build context

การใส่ไฟล์ที่ไม่สำคัญเมื่อสร้าง image จะทำให้ build context และ image ใหญ่ขึ้น เวลาในการสร้าง การ push, pull และ runtime ก็จะนานเช่นกัน ถ้าต้องการทราบขนาด build context ก็ให้หาข้อความนี้ตอนสร้าง Dockerfile

```
Sending build context to Docker daemon 187.8MB
```