## Decouple applications
Container แต่ละอันควรจัดการปัญหาในเรื่องๆเดียว ไม่ควรทำมากกว่านั้น
ถ้ามากกว่านั้น ให้สร้าง container แยกใหม่จะง่ายกว่า
เช่น web application ประกอบด้วย 3 containers จัดการ web, databse และ in-memory cache

การจำกัด container ให้เป็น 1 process เป็นสิ่งที่ควรทำ แต่ก็ไม่ได้บังคับหนัก เช่น container อันนึงก็สามารถเกิด process แรกเริ่มได้, บางโปรแกรมอาจจะเกิด process เพิ่มขึ้นเอง เช่น Apache สร้าง 1 process ต่อ การร้องข้อครั้งนึง

เลือกให้ดีเพื่อทำให้ container เรียบง่ายและแยกส่วนๆให้มากที่สุด ถ้า container ต้องพึ่งอันอื่นๆ ท่านก็สามารถใช้ Docker container networks เพื่อทำให้มั่นว่าจะสามารถสื่อสารกันได้