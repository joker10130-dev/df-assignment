## LABEL

คำสั่ง `LABEL` จะเป็นการเพิ่ม metadata ลงใน image เราสามารถเพิ่มตัว label เข้าไปใน image เพื่อช่วยจัดการกับ project เราได้ แต่ละ label 
จะเริ่มต้นด้วยคำสั่ง `LABEL` และคู่ key-value pairs เป็นคำสั่งที่ใช้เพิ่ม label ต่างๆ ซึ่งประกาศได้หลายค่าใน `Dockerfile` label จะมีอยู่ใน
parent image ซึ่งได้รับมาจาก image ของเรา หากมี label อยู่แล้ว แต่มีค่าต่างกัน ค่าที่ใช้ล่าสุดจะแทนที่ค่าที่ตั้งไว้ก่อนหน้านี้

**ตัวอย่างที่ 1: การประกาศ label**
```dockerfile
    LABEL <key>=<value> <key>=<value> <key>=<value> ...
```

```dockerfile
    # Set one or more individual labels
    LABEL com.example.version="0.0.1-beta"
    LABEL vendor1="ACME Incorporated"
    LABEL vendor2=ZENITH\ Incorporated
    LABEL com.example.release-date="2015-02-12"
    LABEL com.example.version.is-production=""
```
ถ้า label เป็น string ที่มี space ควรใส่ภายใต้ `"` หรือใช้การ escape จะดีที่สุด 

```dockerfile
    # Set multiple labels on one line
    LABEL com.example.version="0.0.1-beta" com.example.release-date="2015-02-12"
```
ใน image สามารถมีได้หลาย label แต่เราสามารถรวม label ต่างๆ เป็น อันเดียวได้ 
```dockerfile
    # Set multiple labels at once, using line-continuation characters to break long lines
    LABEL vendor=ACME\ Incorporated \
    com.example.is-beta= \
    com.example.is-production="" \
    com.example.version="0.0.1-beta" \
    com.example.release-date="2015-02-12"
```
หากต้องการดู label ของ image ให้ใช้คำสั่ง `docker image inspect` ใน cli และสามารถใช้ตัวเลือก `--format` เพื่อแสดงเฉพาะแต่ label
```
    docker image inspect --format='' myimage
```
