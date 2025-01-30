# Daikin-EKHHE
try to receive some information from Daikin EKHHE

to receive some data, i use an Wavesahre RS232/485 ETH Converter. I connect the converter to the CN23 in parallel of the Displaypanel from the Device

i openend the website of the RS485 Converter and use these setting to establish a conncetion

<img src="pics/connect.jpg" alt="connection" height="400px"> <img src="pics/waveshare.png" alt="waveshare webui" height="200px">

the important part is to use 9600 baud, as TCP Server i use my home automation system named symcon. 

This unit sents data with Modbus Ascii, this have fixed length/values. This is good for us.

some examples:

This string contains some values, we start counting our array by zero 

`DD 01 00 00 00 00 37 37 06 00 0F 05 0A 4E 00 00 00 01 1E 00 00 07 00 00 00 7F 05 CD 7F 7F 7F 7F 7F 7F 7F 7F 7F 7F 7F 0E 62`


Name   | Parametertype (manual) | description
-------- | ---------------------------|------------
Array no: 0 | start always with Hex DD | is our starting block to get temperature information
Array no: 6 | B | value in HEX, a value of 37 means 55 degress
Array no: 7 | A | same as above
Array no: 8 | C  | same as above
Array no: 10 | D  | same as above
Array no: 11 | E  | same as above
Array no: 12 | F | same as above
Array no: 13 | G  | same as above
Array no: 14 | H  | same as above
Array no: 18 | I | lowercase i 
Array no: 21 | Digital In 1-3 |  this contains the stati in reversed bit order*1 


*1 off means, that the Digital Input is openend anf on means, its closed. 
Digital Input 3 | Digital Input 2  |  Digital Input 1 | Array 21 result
----------------| -----------------|------------------|----------------
1 (off) |         1 (off) |        1 (off) |        7
1 (off) |         1 (off) |        0 (on) |       6
1 (off) |         0 (on) |       1 (off) |        5
1 (off) |         0 (on) |       0 (on) |        4
0 (on) |         1 (off) |       1 (off) |         3
0 (on) |         1 (off) |        0 (on) |       2
0 (on) |          0 (on) |      1 (off) |        1 
0 (on) |         0 (on) |        0 (on) |       0 

<img src="https://github.com/user-attachments/assets/85f3a40f-3937-4740-8774-e49f22c1c5ea" alt="small teaser of received data" height="250px">

The information for the target temperature can be found in this HEX Value started with D2

' D2 01 14 00 1E 3C 0A 00 07 11 1E FB 03 37 37 37 37 32 3E 3E 07 04 41 07 02 02 00 00 00 03 00 01 01 0A 28 0A 8C 3E 4B 1E 0F 09 19 58 04 FF 00 00 02 02 01 00 00 00 00 00 0C 34 20 2B F9 19 0C 5A 32 00 01 7F 7F 7F C2 `

Name   | Parametertype (manual) | description
-------- | ---------------------------|------------
Arrayno: 0 | start always with Hex D2 | is our starting block to get more information
Arrayno: 1 | power | 0 = standby, 1 = on
Arrayno: 3 | mode | 0 = auto, 1 = eco, 2 = boost, 3 = electric, 4 = fan, 5 = holiday 
Arrayno: 13 | target temperature |  value in HEX, a value of 37 means 55 degress

https://github.com/lorbetzki/Daikin-EKHHE/discussions/
