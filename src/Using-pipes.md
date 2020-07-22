### **Using pipes**

ตัวอักษร pipe (`|`) ใช้สำหรับการส่ง Output ของคำสั่งหนึ่งไปยังอีกคำสั่งหนึ่ง เช่น

```
RUN wget -O - https://some.site | wc -l > /number
```

เนื่องจาก Docker ประมวลคำสั่งนี้โดยใช้ /bin/sh -c interpreter  ซึ่งจะสามารถสร้าง image ได้สำเร็จตราบใดที่ `wc –l` ยังสามารถทำงานได้ถูกต้องถึงแม้จะมีการผิดพลาดอื่น ๆ ภายใน เว้นแต่ว่าคำสั่ง `wget` จะทำงานผิดพลาดเท่านั้น

ดังนั้นเราสามารถใช้  `set -o pipefail &&`  ในการช่วยตรวจสอบความผิดพลาดอื่น ๆ ที่อาจเกิดขึ้นได้ โดยมีการใช้งานดังนี้

```
RUN set -o pipefail && wget -O - https://some.site | wc -l > /number
```

แต่ไม่ใช่ทุก shell ที่จะสามารถใช้ `-o pipefail` เช่น เช่น `dash` ก็จะต้องใช้ exec form สำหรับการใช้งานคำสั่ง `RUN` ซึ่งมีตัวอย่างการใช้งานคำสั่งดังนี้

```
RUN ["/bin/bash", "-c", "set -o pipefail && wget -O - https://some.site | wc -l > /number"]
```
