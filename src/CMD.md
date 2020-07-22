### **CMD**

หากเป็นการ run software ที่บรรจุอยู่ใน image และต้องมีการใช้ argument ควรใช้ฟอร์มเป็นดังนี้

```
CMD ["executable", "param1", "param2"…]
```

แต่ถ้า image ใช้งานสำหรับ service ต่าง ๆ เช่น Apache และ Rails ควรใช้ฟอร์มเป็นดังนี้

```
CMD ["apache2","-DFOREGROUND"]
```

และฟอร์มนี้ยังเหมาะสมสำหรับ service-based image. อื่น ๆ อีกด้วย
ในกรณีอื่น ๆ CMD ก็จะเหมาะสมต่อการใช้งานกับ  bash, python and perl ด้วยเช่นกัน ยกตัวอย่างเช่น 

```
CMD ["perl", "-de0"]
CMD ["python"]
CMD ["php", "-a"]
```

แต่จะไม่เหมาะกับการใช้งานในรูปแบบ 

```
CMD ["param", "param"] 
```

ที่จะต้องทำงานร่วมกับ `ENTRYPOINT` หากเรายังไม่มีความรู้ความเข้าใจเกี่ยวกับการทำงานของ `ENTRYPOINT` ที่เพียงพอ
