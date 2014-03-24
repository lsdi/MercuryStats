Меркурий 200.02 <-> CAN(RS485)
==

Электросчетчик [Меркурий](http://www.incotexcom.ru/m200.htm) имеет CAN-интерфейс для подключения к внешним системам.  
Скрипт периодически опрашивает прибор и получает с него параметры электросети(напряжение,ток,мошность нагрузки),  
которые отсылает на Xively для построение графиков.  
https://xively.com/feeds/19249442

Протокол (реверсинг)
==
Для обращения к прибору необходимо знать его счетчика. Для модели 200.02 это последние 6 цифр серийного номера.  
В моем случае это `411486` в десятичном представлении либо `06 47 5E` в hex

### Формат фрейма запрос

Начало пакета | Адрес счетчика | Запрос | CRC16 (Modbus)
:---: | :---: | :---: | :---: 
*1 байт* | *3 байта* | *1 байт* | *2 байта*
`00` | `06 47 5E` 

### Формат фрейма ответа

Заголовок| Адрес счетчика | Запрос<br> (на который отвечаем) | Ответ | CRC16 (Modbus) 
 :---: | :---: | :---: | :---: | :---: 
*1 байт* | *3 байта* | *1 байт* | |*2 байта*
`00` | `06 47 5E` | 

### Запросы и Ответы

Запрос<br>(hex) |  Описание команды |  Ответ
 :------------: | :---------------- | ---------------
`00 06 47 5E 63 EC D4` | Текущие параметры электросети
`00 06 47 5E 21 6C E5` | Дата время текущее
`00 06 47 5E 24 AC E6` | Дата время
`00 06 47 5E 25 6D 26` | Дата время
`00 06 47 5E 27 EC E7` | Значение энергии | *[12 байт]*<br>[3] Тариф1<br>[3] Тариф2<br>[3] Тариф3<br>[3] Тариф4
`00 06 47 5E 2F ED 21` | служебная информация                                                      
`00 06 47 5E 65 6C D6` | служебная информация                                
`00 06 47 5E 2C AD 20` | служебная информация                                 
`00 06 47 5E 2B EC E2` | служебная информация                                 
`00 06 47 5E 87 EC 9F` | служебная информация                                 
`00 06 47 5E 29 6D 23` | напряжение батарейки | 1 байт вольт, 1 байт десятой части
`00 06 47 5E 66 2C D7` | Дата изготовления | байт адреса, байт месяца, байт года
`00 06 47 5E 2A 2D 22` | индикация на дисплее
`00 06 47 5E 67 ED 17` | индикация на дисплее                           
`00 06 47 5E 28 AC E3` | версия | байт версии, байт подверсии
`00 06 47 5E 31 00 E8` | тарифное расписание
`00 06 47 5E 23 ED 24` | лимит энергии | 9999 квт*ч
`00 06 47 5E 22 2C E4` | лимит мощности | 99990 Вт
`00 06 47 5E 26 2D 27` | чтение мощности                             
`00 06 47 5E 2D 6C E0` | функция испульсного выхода чтение                             
`00 06 47 5E 25 6D 26` | коррекция времени кнопками                           
`00 06 47 5E 24 AC E6` | флаг перехода зимнее.летнее

