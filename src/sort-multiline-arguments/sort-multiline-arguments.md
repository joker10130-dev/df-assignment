## Sort multi-line arguments 

### เรียงลำดับ argument 


 ถ้าเป็นไปได้ ควรที่จะเรียงลำดับ argument ตาม alphanumeric เพื่อให้ง่ายต่อการเปลี่ยนแปลงในภายหลัง  ซึ่งการทำแบบนี้จะช่วยเลี่ยงการเกิด package ที่ซ้ำซ้อนกันได้
 แล้วยังช่วยให้เราสามารถที่จะ update list ได้ง่ายขึ้นด้วย นอกจากนี้เวลาเรา Pull requests ไปยังทีมทำให้คนในทีมอ่านและรีวิวง่าย โดยจะเขียน argument แยกแต่
 ละบรรทัดกันแลัวขั้นด้วย 1 space ก่อน backslash `\`

```dockerfile

RUN apt-get update && apt-get install -y \
  bzr \
  cvs \
  git \
  mercurial \
  subversion
  ```

ดูตัวอย่างประกอบจาก image [`buildpack-deps`](https://github.com/docker-library/buildpack-deps)
