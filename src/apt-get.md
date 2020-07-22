### **apt-get**

`apt-get` อาจเป็นคำสั่งที่ถูกเรียกใช้งานบ่อยครั้งในการใช้งานคำสั่ง `RUN` ในการติดตั้ง package ต่าง ๆ ด้วยคำสั่ง `RUN apt-get`

หลีกเลี่ยงการใช้งานคำสั่ง `RUN apt-get upgrade` และ `dist-upgrade` เนื่องจาก essential package จำนวนมากจาก image หลัก ไม่สามารถทำการ Update ภายใต้ container ที่ไม่ได้รับสิทธิพิเศษ (unprivileged container) ถ้า pakage ที่อยู่ใน image หลักมีความล่าสมัยให้ทำการติดต่อกับผู้บำรุงรักษาโดยตรง แต่ก็ยังมี pakage เฉพาะ (particular package) `foo` ที่ต้องทำการ Update ก็ให้ใช้คำสั่ง `apt-get install -y foo` ในการ update ได้อย่างอัตโนมัติ

ในการใช้งาน ให้ทำการรวมคำสั่ง `RUN apt-get upgrade` กับคำสั่ง `apt-get install` ในคราวเดียวกัน หรือใน RUN statement เดียวกัน ยกตัวอย่างเช่น

```
RUN apt-get update && apt-get install -y \
    package-bar \
    package-baz \
    package-foo
```
การใช้งานคำสั่ง `apt-get update` เฉย ๆ ต่อจากคำสั่ง `RUN` ทำให้เกิดเกิดปัญหา caching และจะเกี่ยวเนื่องให้การใช้งานคำสั่ง `apt-get install` ในลำดับถัดไปเกิดความผิดพลาด ยกตัวอย่าง หากเรามี Dockerfile   ที่มีการเขียนลักษณะดังนี้

```
FROM ubuntu:18.04
RUN apt-get update
RUN apt-get install -y curl
```

หลังจากการสร้าง image จะทำให้ layer ทั้งหมดอยู่ใน Docker cache
สมมติว่าเราทำการปรับแต่งเพิ่มเติมด้วยคำสั่ง `apt-get install` เพื่อเพิ่มเติม package พิเศษอื่น ๆ ดังนี้

```
FROM ubuntu:18.04
RUN apt-get update
RUN apt-get install -y curl nginx
```

Docker จะเห็นคำสั่งการทำงานในตอนแรกกับตอนปรับแต่งเหมือนกัน หรือเป็นตัวเดียวกัน ก็จะทำ cache เก่ามาใช้งาน เลยส่งผลให้ `apt-get update` จะไม่ถูกดำเนินการในขั้นตอนนี้ เพราะการสร้างจะมีการใช้งาน cache สุดท้ายก็จะส่งผลให้ `curl` และ `nginx` ไม่ถูก update เป็น version ล่าสุด

ให้ทำการใช้คำสั่ง `RUN apt-get update && apt-get install –y` เพื่อให้ทำการติดตั้ง packer ที่เป็น version ล่าสุดอย่างถูกต้อง ซึ่งเราจะเรียกเทคนิคนี้ว่า “cache busting” โดยเรายังสามารถใช้เทคนิคนี้ในการติดตั้งแบบระบุ version ได้อีกด้วย เรียกว่า version pinning ดังตัวอย่าง

```
RUN apt-get update && apt-get install -y \
    package-bar \
    package-baz \
    package-foo=1.3.*
```

การใช้งานคำสั่งดังตัวอย่างข้างต้น จะเป็นการเรียกใช้เป็น version เฉพาะโดยจะไม่มีการคำนึงถึง   cache ที่เคยมีอยู่ ก็จะเป็นการลดความผิดพลาดที่อาจเกิดขึ้นได้ หากใช้คำสั่งในลักษณะอื่น
ตัวอย่างต่อไปนี้เป็นตัวอย่างการใช้งาน `RUN` ในลักษณะ หรือรูปแบบที่เหมาะสม

```
RUN apt-get update && apt-get install -y \
    aufs-tools \
    automake \
    build-essential \
    curl \
    dpkg-sig \
    libcap-dev \
    libsqlite3-dev \
    mercurial \
    reprepro \
    ruby1.9.1 \
    ruby1.9.1-dev \
    s3cmd=1.1.* \
 && rm -rf /var/lib/apt/lists/*
```

อธิบายเพิ่มเติม คือเมื่อเราทำการล้าง apt cache ด้วยการลบ `/var/lib/apt/lists` ก็จะเป็นการลดขนาดของ image ไปด้วย เพราะ apt cache จะไม่ถูกเก็บไว้ใน Layer 

```
“Debian และ Ubuntu จะมีการใช้งานคำสั่ง apt-get-clean   โดยอัตโนมัติอยู่แล้ว ดังนั้นจึงไม่ต้องการเรียกใช้งาน หรือประกาศคำสั่งนี้”
```
