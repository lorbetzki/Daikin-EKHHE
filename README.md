# Daikin-EKHHE
try to receive some information from Daikin EKHHE

to receive some data, i use an Waveshare RS232/485 ETH Converter. I connect the converter to the CN23 in parallel of the Displaypanel from the Device

i openend the website of the RS485 Converter and use these setting to establish a conncetion

<img src="pics/connect.jpg" alt="connection" height="400px"> <img src="pics/waveshare.png" alt="waveshare webui" height="200px">

the important part is to use 9600 baud, as TCP Server i use my home automation system named symcon. 

This unit sents data with Modbus Ascii, this have fixed length/values. This is good for us. But before...

*** DISCLAIMER ***
Warning, this information has been compiled by reverse engineering. I assume no liability for damages, errors or other problems. If you are not sure, don't do it!

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

The information for the target temperature can be found in this HEX Value started with CC

'CC 01 15 00 1E 3C 0A 00 07 11 1E FB 03 33 32 37 37 32 3E 3E 07 04 41 07 02 02 00 00 00 03 00 01 01 0A 28 0A 8C 3E 4B 1E 0F 09 19 58 04 FF 00 00 02 02 01 00 00 00 00 00 00 15 08 20 2B F9 19 0C 5A 32 14 00 01 7F A7`

Name   | Parametertype (manual) | description
-------- | ---------------------------|------------
Arrayno: 0 | start always with Hex CC | is our starting block to get more information
Arrayno: 1 | Multiparameter | 0 = standby, 1 = on and more Parameter *2
Arrayno: 2 | Multiparameter | *3 
Arrayno: 3 | mode | *4 0 = auto, 1 = eco, 2 = boost, 3 = electric, 4 = fan, 5 = holiday 
Arrayno: 4 | (P4) | Antilegionella duration 
Arrayno: 5 | (P7) | Delay between two consecutive defrosting cylce 
Arrayno: 6 | (P10) | Maximum defrosting duration 
Arrayno: 7 | (P2) | Electrical heater switching-on delay"
Arrayno: 8 | (P8) | Temperature threshold for defrosting start
Arrayno: 9 | (P29) | Antilegionella starting hour
Arrayno: 10 | | 
Arrayno: 11 | (P8) | Temperature threshold for defrosting start  
Arrayno: 12 | (P9) | Temperature threshold for defrosting stop  
Arrayno: 13 | target temperature |  value in HEX, a value of 37 means 55 degress
Arrayno: 14 | | 
Arrayno: 15 | | 
Arrayno: 16 | | 
Arrayno: 17 | | 
Arrayno: 18 | | 
Arrayno: 19 | | 
Arrayno: 20 | (P1) | Hysteresis on lower water probe for heat-pump working
Arrayno: 21 | (P32) | Temperature threshold for electrical heater usage in AUTO mode 
Arrayno: 22 | (P3) | Antilegionella setpoint temperature
Arrayno: 23 | (P30) | Hysteresis on upper water probe for electrical heater working  
Arrayno: 24 | (P25) | Offset value on upper water temp probe 
Arrayno: 25 | (P26) | Offset value on lower water temp probe  
Arrayno: 26 | (P27) | Offset value on air-inlet temp probe 
Arrayno: 27 | (P28) | Offset value on defrosting temp probe  
Arrayno: 28 | (P12) | External pump usage mode  
Arrayno: 29 | (P14) | Type of evaporator fan  
Arrayno: 30 | (P24) | Off-peak working mode 
Arrayno: 31 | (P16) | Solar mode integration
Arrayno: 32 | (P23) | Photovoltaic mode integration
Arrayno: 33 | (P17) | Heat-pump starting delay after DIG1 opening 
Arrayno: 34 | (P18) | Lower water probe temperature value to stop the heat-pump in solar mode integration
Arrayno: 35 | (P19) | Hysteresis on lower water probe to start the pump in solar mode integration
Arrayno: 36 | (P20) | Temperature threshold for solar drain valve / solar collector roll-up shutter action in solar mode integration  
Arrayno: 37 | (P21) | Lower water probe temperature value to stop the heat-pump in photovoltaic mode integration  
Arrayno: 38 | (P22) | Upper water probe temperature value to stop the electrical heater in photovoltaic mode integration  
Arrayno: 39 | (P34) | Superheating calculation period for EEV automatic control mode
Arrayno: 40 | (P37) | EEV step opening during defrosting mode (x10) 
Arrayno: 41 | (P38) | Minimum EEV step opening with automatic control mode (x10)
Arrayno: 42 | (P40) | Initial EEV step opening with automatic control mode / EEV step opening with manual control mode (x10)
Arrayno: 43 | (P36) | Desuperheating setpoint for EEV automatic control mode 
Arrayno: 44 | (P35) | Superheating setpoint for EEV automatic control mode
Arrayno: 45 | (P41) | AKP1 temperature threshold for EEV KP1 gain 
Arrayno: 46 | (P42) | AKP2 temperature threshold for EEV KP2 gain  
Arrayno: 47 | (P43) | AKP3 temperature threshold for EEV KP3 gain  
Arrayno: 48 | (P44) | EEV KP1 gain 
Arrayno: 49 | (P45) | EEV KP2 gain  
Arrayno: 50 | (P46) | EEV KP3 gain  
Arrayno: 51 | | 
Arrayno: 52 | | 
Arrayno: 53 | | 
Arrayno: 54 | | 
Arrayno: 55 | | 
Arrayno: 56 | | 
Arrayno: 57 | HOUR | Systemtime hour
Arrayno: 58 | MINUTE | Systemtime min
Arrayno: 59 | | 
Arrayno: 60 | (P47) | Maximum allowed inlet temperature for heat-pump working 
Arrayno: 61 | (P48) | Minimum allowed inlet temperature for heat-pump working  
Arrayno: 62 | (P49) | Threshold on inlet temperature for evaporator EC or AC with double speed blower speed setting 
Arrayno: 63 | (P50) | Antifreeze lower water temperature setpoint 
Arrayno: 64 | (P51) | Evaporator EC blower higher speed setpoint  
Arrayno: 65 | (P52) | Evaporator EC blower lower speed setpoint
Arrayno: 66 | |
Arrayno: 67 | |
Arrayno: 68 | |
Arrayno: 69 | (P54) | Low pressure switch bypass time 
Arrayno: 70 | Checksum | *5


*2 Array 1 is an multidimension parameter, there are more combinations.... but its the beginning
0 = aus, 1 = an, 4 = EEV-Steuerungsmodus manuell 10 = Warmwasser-Umwälzpumpe immer an, 
Hex | Binary | Parameter | Value
---|-----|----|------
0 |0b00000 | off | 0
1 | 0b00001| on | 1
4 |0b00100 | P39 | 1
10 | 0b10000|  P13 | 1

*3 Array 2 is a multidimension parameter, there are more combinations.... but its the beginning
Hex | Binary | Parameter | Value
---|-----|----|------
11 |  0b10001 | P5 | 0
1D | 0b11101 | P6 | 1			
14 | 0b10100 | P11 | 0
15 | 0b10101 | P11 | 1
17 | 0b10111 | P15 | 1
5 | 0b00101 | P33 | 0

*4 Array 3 is for the mode
HEX | Value
---|----
0 | auto
1 | eco
2 | boost
3 | electric
4 | fan 
5 | holiday

To write minus you need to count backward
HEX | Value
---|----
FF | -1 
FE | -2
FD|-3
FC|-4
FB|-5
FA|-6
...|and so on

*5 *** DISCLAIMER ***
Warning, this information has been compiled by reverse engineering. I assume no liability for damages, errors or other problems. If you are not sure, don't do it!

if you want to write to the Heatpump you need to replace the first CC with CD, change one Parameter and recalc the CRC Sum. You need to send the complete string like CC received. You can also

code example in PHP  
```
function CalculateChecksum(string $hex)
		{
			$bufLen = strlen($hex);
			$sum_dec = 170; // Hex AA
			for ($i=0; $i < $bufLen; $i+=2)
			{
				//echo substr($hex, $i, 2);
				$sum_dec += hexdec(substr($hex, $i, 2));
			}

			$result = strtoupper(dechex(($sum_dec % 256)));
			$this->SendDebug(__FUNCTION__, 'Calculate checksum for HEX: '.$hex." with result ". $result, 0);

			return str_pad($result,2,"0", STR_PAD_LEFT);
		}
```

Example usage: 
`CalculateChecksum('CD0115001E3C0A0007111EFB0333323737323E3E070441070202000000030001010A280A8C3E4B1E0F09195804FF00000202010000000000001508202BF9190C5A321400017F');
`

Result: A8 
you need to send 

`CD0115001E3C0A0007111EFB0333323737323E3E070441070202000000030001010A280A8C3E4B1E0F09195804FF00000202010000000000001508202BF9190C5A321400017FA8`

I write a litte module for symcon home automation -> https://github.com/lorbetzki/net.lorbetzki.daikin-ekhhe.git

Faster informations -> https://github.com/lorbetzki/Daikin-EKHHE/discussions/
