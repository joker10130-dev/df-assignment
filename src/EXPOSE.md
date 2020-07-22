### **EXPOSE**

เนื่องจาก `EXPOSE` เป็นการจัดการเกี่ยวกับ port การเชื่อมต่อของ container เราควรใช้ common port หรือ traditional port ของแต่ละ Application เช่น 

- `EXPOSE 80` สำหรับ Apache web server
- `EXPOSE 27017` สำหรับ image ที่มีฐานข้อมูลเป็น MongoDB

ในการเข้าถึงจากภายนอกผู้ใช้งานสามารถใช้คำสั่ง docker run พร้อมด้วย flag indicating เพื่อระบุการเชื่อมต่อของ port ของผู้ใช้งาน 
และสำหรับการเชื่อมต่อกันระหว่าง container นั้น Docker ได้จัดเตรียม environment variables เพื่อให้เราจัดการ path ไว้แล้ว เช่น
``` 
MYSQL_PORT_3306_TCP
```