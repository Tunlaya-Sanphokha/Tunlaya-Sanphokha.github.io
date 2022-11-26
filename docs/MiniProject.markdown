# MiNi Project - MQTT กับบอร์ด ESP8266
### Mini Project
Mini Project ที่กลุ่มพวกเราสนใจคือ พวกเราต้องการที่ทำการวัดอุณหภูมิ โดยใช้ sensor DHT11  เชื่อมต่อเข้ากับ MQTT และทำการควบคุมการเปิดปิดหลอดไฟจากที่ไหนก็ได้   โดยทดลองใช้ บอร์ด Raspberry Pi   โดยที่พวกเราสนใจที่จะทำ Project นี้ เพราะคิดว่าอาจจะเป็นประโยชน์ในการทำการเกษตรได้ในอนาคต สามารถทำการควบคุมการเปิดปิดของหลอดไฟ ในฟาร์ม เมื่อถึงอุณหภูมิ ที่ต้องการได้ 
โดย Project นี้ เราคิดว่าสามารถพัฒนา ให้เป็นการแจ้งเตือน อุณหภูมิ ผ่านโทรศัพท์มือถือ หรือ เปิดปิดไฟ จากมือถือได้เหมือนกัน
### อุปกรณ์ที่ใช้
##### Hardware
###### •	DHT11
###### •	บอร์ด ESP8266
###### •	บอร์ด Raspberry Pi 
##### Software
###### •	Arduino IDE 
###### •	Arduino IOT Cloud
###### •	hivemq.com
###### •	Thornny
###### •	Raspberry Pi OS

### Raspberry Pi Pin Configuration
แผนภาพด้านล่างจะแสดง Pin ของ Raspberry Pi  เราจะเริ่มเชื่อมต่อกับ Pin 7ซึ่งเป็น GPIO4 ของ Raspberry Pi การกำหนดค่า  Pin GPIO จะเหมือนกันสำหรับ Raspberry Pi ทั้งหมด

<p align="center">
  <img src="photo/4/1.PNG" alt="1" width="400" height="700"/>
</p>

### Connection Diagram
เราจะทำการเชื่อมต่อ Sensor อุณหภูมิ และความชื้น DHT11 กับ Raspberry Pi ตามภาพด้านล่าง

<p align="center">
  <img src="photo/4/2.PNG" alt="2" width="500" height="500"/>
</p>

### วัดอุณหภูมิจาก DHT11  โดยใช้ Raspberry Pi4
1. ก่อนอื่นเราจะทำการติดตั้ง library Adafruit DHT ให้เข้าไปที่ Raspberry pi Os แล้วทำการ clone ตาม Code ด้านล่าง

```C+
git clone https://github.com/adafruit/Adafruit_Python_DHT.git
```
เพื่อดาวโหลดไฟล์ที่จำเป็นภายใต้โฟลเดอร์ ที่ชื่อว่า Adafruit_Python_DHT 
<p align="center">
  <img src="photo/4/3.PNG" alt="3" width="700" height="200"/>
</p>

2.ls เพื่อแสดงไฟล์ Adafruit_Python_DHT แล้ว cd เข้า ไฟล์นั้น
<p align="center">
  <img src="photo/4/4.PNG" alt="4" width="700" height="100"/>
</p>

3.รัน command เพื่อ ติดตั้ง python Version2 และ Version3
```C+
sudo python setup.py install
sudo python3 setup.py install
pi@raspberrypi:~/Adafruit_Python_DHT $ sudo python setup.py install
pi@raspberrypi:~/Adafruit_Python_DHT $ sudo python3 setup.py install
```
<p align="center">
  <img src="photo/4/5.PNG" alt="5" width="700" height="800"/>
</p>

4. เข้าไปแก้ไข Code ตาม part ไฟล์ ดังนี้ 
```C+
/usr/local/lib/python3.9/dist-packages/Adafruit_DHT-1.4.0-py3.9-linux-aarch64.egg/Adafruit_DHT
```
แล้วแก้ไข Code  ตามนี้
```C+
elif match.group(1) == 'BCM2711':
#Pi 4b
return 3
```

<p align="center">
  <img src="photo/4/6.PNG" alt="6" width="700" height="600"/>
</p>

<p align="center">
  <img src="photo/4/7.PNG" alt="7" width="700" height="600"/>
</p>

5.เปิด ไฟล์ dht11.py เพื่อเขียนโค้ดดังนี้ 
โค้ดส่วนนี้จะทำหน้าที่ อ่าน ข้อมูลจาก sensor วัด อุณหภูมิ dth11  โดยให้แสดงผลผ่าน terminal และแสดงค่า อุณหภูมิ กับ ค่าความชื้นสัมพัทธ์
<p align="center">
  <img src="photo/4/8.PNG" alt="8" width="700" height="300"/>
</p>
ทำการเชื่อมต่อ บอร์ด Rpi4 กับ sensor วัดอุณหภูมิ dht11 ผ่านขา pin 
<p align="center">
  <img src="photo/4/9.PNG" alt="9" width="600" height="500"/>
</p>
<p align="center">
  <img src="photo/4/10.PNG" alt="10" width="600" height="500"/>
</p>
เมื่อกด run code จะแสดงผลลัพธ์ ผ่าน terminal ดังนี้
<p align="center">
  <img src="photo/4/11.PNG" alt="11" width="700" height="600"/>
</p>

### การใช้ บอร์ด ESP8266 กับ Sensor วัดอุณหภูมิ DHT11 ผ่าน MQTT Broker
โดยเมื่อลองใช้ Raspberry Pi แล้ว กลุ่มเราอยากจะลองเปลี่ยนไปใช้เป็น ESP8266 เนื่องจากมี wifi ในตัว  พวกเราได้ใช้ Code และ รันบนบอร์ด ESP8266 แต่ว่า Error 
<p align="center">
  <img src="photo/4/12.PNG" alt="12" width="700" height="500"/>
</p>
หลังจาก ที่ Code Error  เราได้ทำการติดตั้ง Adafruit_Sensor Library เพิ่มเติมตามคำแนะนำของอาจารย์  ทำให้ Code สามารถ Compile ได้ แต่ยังไม่สามารถแสดงค่า ของอุณหภูมิ ที่วัดออกมาได้
<p align="center">
  <img src="photo/4/13.PNG" alt="13" width="700" height="500"/>
</p>
หลังจากที่ code สามารถ Compile ได้แล้วเราจะมาเลือก Broker MQTT ที่จะใช้เพื่อเชื่อมต่ออุปกรณ์วัดอุณหภูมิ และ บอร์ด ESP8266  ทำการควบคุมการเปิดปิดไฟ จาก MQTT แสดงค่า อุณหภูมิผ่าน MQTT 

กลุ่มของเราได้เลือก Broker  hivemq.com  เป็น Broker  ที่เราจะใช้ในการเชื่อมต่อ อุปกรณ์วัดอุณหภูมิ DHT11 กับบอร์ด ESP8266 
##### เว็บที่ใช้ [hivemq](https://www.hivemq.com/public-mqtt-broker/)
<p align="center">
  <img src="photo/4/14.PNG" alt="14" width="700" height="500"/>
</p>
ภาพตัวอย่าง  Broker ที่เราเลือกใช้
โดยเมื่อเราได้กำหนดและทำการเชื่อม  wifi  ที่เราเชื่อมต่ออยู่ตามรูปนี้
<p align="center">
  <img src="photo/4/15.PNG" alt="15" width="600" height="200"/>
</p>
แล้วทำการ  Run code เพื่อเช็คว่าตัว code มี error ตรงไหนรึเปล่า
<p align="center">
  <img src="photo/4/16.PNG" alt="16" width="700" height="600"/>
</p>
จากนั้นให้เราไปที่  Broker  ที่เราได้เลือกไว้ เป็น Broker ที่ชื่อว่า  hivemq.com   
<p align="center">
  <img src="photo/4/17.PNG" alt="17" width="700" height="600"/>
</p>
จากนั้นให้กด   ปุ่ม Try MQTT Browser Client  เพื่อเข้าสู่  MQTT Browser Client
<p align="center">
  <img src="photo/4/18.PNG" alt="18" width="600" height="500"/>
</p>
รูปภาพแสดงหน้าตา เมื่อกด ปุ่ม  Try MQTT Browser Client   หลังจากนั้นทำการเลือก Port ที่เราใช้ คือ Port  8000  แล้วกดปุ่ม  Connect สีฟ้า   
<p align="center">
  <img src="photo/4/19.PNG" alt="19" width="700" height="400"/>
</p>
เมื่อกดปุ่ม Connect  แล้ว หลังจากนั้น เราจะใส่ Topic ที่เราได้กำหนด ในcode  เป็น  Topic  ที่ชื่อว่า  room/lamp  ทำการส่ง Message  ว่า “on”  เพื่อทำให้ไฟ ติด โดยถ้าต้องการให้ข้อมูลส่งได้ ครบถ้วนและไม่ต้องหล่น ให้ตั้ง QoS เป็น 2
<p align="center">
  <img src="photo/4/20.PNG" alt="20" width="400" height="400"/>
</p>
จากนั้นให้เราทำการ Add Topic Subscription เป็น  Topic ที่ชื่อว่า  room/temperature  และ  room/humidity  เพื่อบอก อุณหภูมิ และ ความชื้นสัมพัทธ์ 
<p align="center">
  <img src="photo/4/21.PNG" alt="21" width="600" height="400"/>
</p>
 เมื่อทำการ  add Topic Subscription  จะแสดงค่า อุณหภูมิ และ ความชื้นสัมพัทธ์ ที่ DHT11 วัดได้
 
 ###### ภาพรวมทั้งหมดจะแสดงดังนี้ 
 
 <p align="center">
  <img src="photo/4/22.PNG" alt="22" width="700" height="600"/>
</p>
ในส่วนของการควบคุมการเปิดปิดไฟนั้น  เมื่อเราได้กำหนด  Topic เป็น  room/lamp  และกำหนด Messages ที่จะส่งเป็น  “on”  เพื่อให้ไฟติด  ให้เรากดปุ่ม Publish  ไฟจะติด
 <p align="center">
  <img src="photo/4/23.PNG" alt="23" width="600" height="600"/>
</p>
และเมื่อเราทำการส่ง Message “off”  เพื่อทำการปิดไฟ ตามรูปด้านล่าง
 <p align="center">
  <img src="photo/4/24.PNG" alt="24" width="700" height="600"/>
</p>
เมื่อ Message ที่วิ่งผ่าน MQTT Broker ส่งมาถึงตัวบอร์ด ไฟจะดับ ตามรูปที่แสดงด้านล่าง
 <p align="center">
  <img src="photo/4/25.PNG" alt="25" width="600" height="600"/>
</p>

### การแสดงอุณหภูมิที่วัดได้ ผ่านโทรศัพท์มือถือ

###### Block diagram
 <p align="center">
  <img src="photo/4/26.PNG" alt="26" width="700" height="500"/>
</p>

### การแสดงอุณหภูมิที่วัดได้ ผ่านโทรศัพท์มือถือ

##### จากที่เราได้ลองใช้  DHT11 กับบอร์ด  ESP8266  แล้ว สามารถวัดอุณหภูมิโดยแสดงบน  Broker  ที่ชื่อว่า  hivemq.com  และ สามารถ ส่ง message  “on” หรือ “off” เพื่อควบคุมการเปิดปิดไฟได้แล้วนั้น เราได้เล็งเห็นว่า จากงานที่เราทำ สามารถนำไปพัฒนาต่อได้ โดยอาจจะใช้ DHT11 ในการวัดอุณหภูมิ แล้วแสดงบนโทรศัพมือถือ เพื่อบอกอุณหภูมิในห้องที่เราอาศัยอยู่ได้ 

##### โดยเราได้ลองใช้เป็น  Arduino  IOT Cloud ซึ่งเป็น Server ให้บริการสำหรับ IOT ลักษณะการทำงานคล้ายกับที่ Google Cloud Firebase , Amazon AWS ให้บริการ

##### เมื่อเข้าไปที่หน้า IOT Cloud แล้วทำการ Log in  อาจจะใช้ Google ในการ LogIN ก็ได้ หลังจากนั้น ให้เราทำการเพิ่ม Project  ในการสร้างอะไรสักอย่างไปที่ Create Thing  ทำการ Add Variables  สร้างตัวแปรขึ้นมา และ กำหนดของข้อมูล เราได้กำหนดเป็น  temperature  ประเภทของข้อมูลจะเป็น CloudTemperatureSensor temperature;  เมื่อเราได้ทำการ Add Variable ตามที่เราต้องการแล้วจะได้หน้าต่างตามนี้

 <p align="center">
  <img src="photo/4/27.PNG" alt="27" width="700" height="500"/>
</p>

##### การตั้งค่าอุปกรณ์โดยเลือกเป็นบอร์ด ESP8266  NodeMCU (ESP12E Module)

 <p align="center">
  <img src="photo/4/28.PNG" alt="28" width="500" height="500"/>
</p>

##### การตั้งค่า Variable โดยมี 2 ค่าคือ Temperature และ Humidity

 <p align="center">
  <img src="photo/4/29.PNG" alt="29" width="500" height="500"/>
</p>

##### ในหน้า Things จะแสดงชื่องานของเรา

 <p align="center">
  <img src="photo/4/30.PNG" alt="30" width="700" height="200"/>
</p>

##### ในหน้านี้ได้ตั้งชื่อ ว่า Temperature ในส่วนของ Setup ได้ลิงค์ข้อมูล อุณหภูมิกับความชื้นมาคือ humudity กับ teperature และ setup บอร์ด ESP8266 เป็น NodeMCU (ESP12E Module) ตัวเว็บจะให้เราตั้งค่าบอร์ดและเชื่อมต่อ Network

<p align="center">
  <img src="photo/4/31.PNG" alt="31" width="700" height="500"/>
</p>

##### ทำการตั้งค่า Configure network ใส่รหัส wifi Secret Key ที่จะได้รับตอนตั้งค่าบอร์ดในตอนแรก

<p align="center">
  <img src="photo/4/32.PNG" alt="32" width="400" height="300"/>
</p>

##### เมื่อเสร็จแล้ว Network จะขึ้นแบบนี้

<p align="center">
  <img src="photo/4/33.PNG" alt="33" width="300" height="300"/>
</p>

##### เมื่อเชื่อมต่อสำเร็จตัว Associated Device จะ online 

<p align="center">
  <img src="photo/4/34.PNG" alt="34" width="700" height="600"/>
</p>

##### ในส่วนของการ Upload code ลงบอร์ดจะต้อง Download Arduino Create Agent ก่อน

<p align="center">
  <img src="photo/4/35.PNG" alt="35" width="700" height="400"/>
</p>

##### ให้คลิก Dowload และติดตั้ง Arduino Create Agent ให้เรียบร้อย

<p align="center">
  <img src="photo/4/36.PNG" alt="36" width="400" height="300"/>
</p>

##### ในส่วนของ code ในหน้า Sketch  เราจะทำการเพิ่ม ฟังก์ชันที่ชื่อว่า dht_sensor_getdata() เพื่อส่งข้อมูลของอุณหภูมิและความชื้น โดยทำการกำหนด ไลบรารี ของ sensor DHT11 ซึ่งเมื่อเรา Search Libraries เราจะเห็นว่า Libraries ได้ติดตั้งไว้อยู่แล้ว  โดยฟังก์ชัน void loop()  เราจะทำการ เรียกใช้  ฟังก์ชัน dht_sensor_getdata()  ซึ่งในฟังก์ชันนี้เราจะมีการอ่านค่า อุณหภูมิ และ ความชื้น และเอามาเก็บไว้ที่ตัวแปรที่เราได้กำหนดไป  แล้วมาแสดงที่ หน้า Dashborad ไปยัง Widget ที่เราได้กำหนดไว้ 

<p align="center">
  <img src="photo/4/37.PNG" alt="37" width="700" height="600"/>
</p>

<p align="center">
  <img src="photo/4/38.PNG" alt="38" width="700" height="600"/>
</p>

##### ในส่วนนี้ Dashboards เราจะกำหนด  Widget ที่จะใช้แสดงสำหรับ Project ของเรา เมื่อเราได้เลือก  widget ที่จะใช้แสดงแล้วเราจะทำการลิ้งค์ว่า widget  ที่เราใช้จะแสดงเป็นข้อมูลประเภทไหน เมื่อเรากำหนดเสร็จแล้ว Widget เหล่านี้ที่เราเลืือก ก็จะแสดงบน แอปพลิเคชันโทรศัพท์ของเราด้วย 

<p align="center">
  <img src="photo/4/39.PNG" alt="39" width="800" height="200"/>
</p>

##### การตั้งค่า Dashboaeds เพื่อเชื่อมข้อมูลจาก Sensor มาแสดงเป็น GUI โดย Link จาก Variable

<p align="center">
  <img src="photo/4/40.PNG" alt="40" width="800" height="600"/>
</p>

##### การ Link variable 

<p align="center">
  <img src="photo/4/41.PNG" alt="41" width="700" height="300"/>
</p>

##### เมื่อ sensor DHT11 อ่านค่า temperature  กับ  humidity ได้แล้วจะแสดงให้เราเห็นดังนี้

<p align="center">
  <img src="photo/4/42.PNG" alt="42" width="800" height="600"/>
</p>
<p align="center">
  <img src="photo/4/43.PNG" alt="43" width="800" height="600"/>
</p>

##### ในโทรศัพท์จะมีแอพชื่อ Arduino IoT Cloud Remote

<p align="center">
  <img src="photo/4/44.PNG" alt="44" width="400" height="400"/>
</p>

##### ให้เราติดตั้งและ login ก็จะมีให้หน้า Dashboards ที่เราทำไว้ในเว็บมาเเสดง

<p align="center">
  <img src="photo/4/45.PNG" alt="45" width="400" height="400"/>
</p>

##### เมื่อกดเข้าไปก็จะแสดงหน้า Temperature และ Humidity ที่วัดได้บนมือถือของเรา เท่านี้เราก็จะสามารถดู Temperature และ  Humidity  บนมือถือของเราได้แล้ว

<p align="center">
  <img src="photo/4/46.PNG" alt="46" width="600" height="700"/>
</p>

### จัดทำโดย
##### นางสาวตุลยา สารโพคา 6201012620341
##### นายณัฐชนน วะลับ 6201012620074
##### นายปรเมศวร์ วรพัฒนชัย 6201012620147
### อาจารย์ที่ปรึกษา
### อาจารย์เรวัต ศิริโภคาภิรมย์
#### Project นี้เป็นส่วนหนึ่งของวิชา SOFTWARE DEVELOPMENT PRACTIC  
#### มหาวิทยาลัยเทคโนโลยีพระจอมเกล้าพระนครเหนือ

### [Home About Me](https://tunlaya-sanphokha.github.io/index.html)
