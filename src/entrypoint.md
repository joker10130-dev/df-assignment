# ENTRYPOINT

The best use for ENTRYPOINT is to set the image’s main command, allowing that image to be run as though it was that command (and then use CMD as the default flags).

`ENTRYPOINT` ใช้ในการเรียกใช้ชุดคำสั่ง(command)ของ image ว่าเมื่อรัน image แล้ว จะให้รันคำสั่งอะไรบ้าง เช่นเดียวกับ `CMD` โดยสามารถใช้งานร่วมกันกับ `CMD` โดยใช้ CMD กำหนด Default flags ให้กับ Dockerfile

ตัวอย่างของ image สำหรับชุดคำสั่ง s3cmd :

```
ENTRYPOINT ["s3cmd"]
CMD ["--help"]

```

สามารถ run ให้แสดงแสดงชุดของคำสั่ง help โดยพิมพ์คำสั่ง :

```
\$ docker run s3cmd

```

หรือเพิ่ม parameters เพื่อ execute คำสั่ง :

```
\$ docker run s3cmd ls s3://mybucket

```

คำสั่ง `ENTRYPOINT` สามารถใช้ร่วมกับ helper script ซึ่งสามารถทำงานในลักษณะเดียวกันกับคำสั่งด้านบน

ตัวอย่าง Postgres Official Image โดยใช้ script ด้านล่างนี้เป็น `ENTRYPOINT`:

```
#!/bin/bash
set -e

if [ "$1" = 'postgres' ]; then
chown -R postgres "\$PGDATA"

    if [ -z "$(ls -A "$PGDATA")" ]; then
        gosu postgres initdb
    fi

    exec gosu postgres "$@"

fi

exec "\$@"

```

helper script จะถูกคัดลอกลงใน container และรันผ่าน `ENTRYPOINT` เมื่อ start container :

```
COPY ./docker-entrypoint.sh /
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["postgres"]

```

ตัวอย่างการใช้งาน Postgres

start Postgres:

```
\$ docker run postgres

```

การรัน Postgres และส่ง parameters ไปยัง server:

```
\$ docker run postgres postgres --help

```

ตอนรันสามารถเลือกรันบน tool อื่น ๆ เช่น Bash

```
\$ docker run --rm -it postgres bash

```
