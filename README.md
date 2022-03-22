# Плата умного домофона, версия с modbus

## Монтаж в разрыв линии 
Рекомендуется подключать в базе трубки, разъединив линию в квартиру.  Ввод линии подключается к Line -/+ на плате ESPDomofon, где + обычно красный. Полярность обычно можно подсмотреть на плате домофона. Intercom -/+ подключается соответственно к трубке.
Некорректная полярность не приведет к повреждениям, но работать не будет, 
На уличной панели при вызове будет отображаться error, в целом работа домофонной системы не будет нарушена. 
 
## Возможности и режимы
•	Управление как физической кнопкой на корпусе, так и через интеграцию с умным домом
•	Режим автоматического открытия двери (одиночный и постоянный)
•	Режим автоматического отклонения вызова
•	Режим "без звука" постоянный или на один звонок
•	Регулировка яркости светодиода

## Подключение
1. Аккуратно вскройте домофон и определите, где кабель, входящий из подъезда, подсоединён к плате. Запомните, какой из проводов кабеля плюс, какой минус, в дальнейшем это поможет избежать ошибок подключения (впрочем, если вы их перепутаете, ничего страшного не случится, ни плата, ни домофон не пострадают, просто не будут работать, пока не подключите правильно).
2. Проверьте, нет ли на входящем из подъезда кабеле винтовых соединений или скруток. В большинстве случаев этот кабель не припаян непосредственно к плате домофона, а подсоединён к соединительным проводам. Возможно, имеет смысл поставить плату именно в этот разрыв.
3. Подключите входящий из подъезда кабель к разъёмам Line+ и Line- платы (обратите внимание на подписи [на обратной стороне платы], подключите идущие к базе трубки домофона провода к разъёмам Intercom+ и Intercom- платы.
4. Подключите питание, и линии RS-485. Как вариант можно использовать провод типа витой пары. Подать можно от 5 до 24 вольт.
## Конфигурация
Платы уже приходят прошитыми.
Параметры порта по умолчанию: скорость 9600 бод, 8 битов, без битов четности, два стоп бита.
Адрес Slave ID – 137.
При использовании совместно с Wiren Board, загружаем конфиг файл config-wb-domofon-mb.json.
В веб интерфейсе появляются данные платы. Параметры порта UART можно настроить для совместимости с другими устройствами на линии.
Остальные параметры позволяют настроить корректную работу с вызывной панелью домофона.
Если с выключенной платой домофон работает как обычно, а с включенной не определяется входящий звонок, необходимо увеличить значение Длительность звонка, так же этим параметром можно изменить время гудка вызывной панели перед автоматическим открытием.
Если входящий звонок определяется, но не работает открытие двери через плату, необходимо увеличить значения Время снятой трубки или Время импульса открытия. 
В очень редких случаях не стабильного открывания двери, можно увеличить параметр Пауза перед снятием трубки.
Данные параметры настраиваются однократно, и в процессе дальнейшей эксплуатации не требуют изменения.
  

## Использование в системах умного дома
В системы умного дома управляющие элементы прокидываются посредством MQTT. Есть индикатор входящего вызова, кнопки открытия двери и отклонения вызова. Переключатели режимов платы. Регулятор яркости светодиода.
Для SprutHub настройки в файле domofon.json
Все режимы работают автономно на плате. При пропадании связи с системой умного дома, включенный режим продолжит работу. Настройки параметров сохраняются в энергонезависимой памяти.



[Чат в Telegram](https://t.me/domofon_esp)


Таблица регистров Modbus:
Адрес	Параметры регистра	Описание	Значения
	Тип	Доступ	Формат		
1	Coil	RO	bit	Входящий вызов	0, 1
4	Coil	RW	bit	Переключатель открытия домофона	0, 1
5	Coil	RW	bit	Переключатель отклонения вызова	0, 1
6	Coil	RW	bit	Переключатель автооткрытия однократно	0, 1
7	Coil	RW	bit	Переключатель автооткрытия постоянно	0, 1
8	Coil	RW	bit	Переключатель автоотбоя	0, 1
9	Coil	RW	bit	Переключатель беззвучного режима однократно	0, 1
10	Coil	RW	bit	Переключатель беззвучного режима	0, 1
					
2	Input	RO	u16	Режим домофона	0 – обычный,
1 – автооткрытие однократно,
2 – автооткрытие
3 – автоотбой,

3	Input	RO	u16	Беззвучный режим	0 – звук включён,
1 – звук отключён однократно,
2 – звук отключён
4	Holding	RW	u16	Пауза перед снятием трубки	х1, мс
5	Holding	RW	u16	Время снятой трубки	х1, мс
6	Holding	RW	u16	Время импульса открытия	х1, мс
7	Holding	RW	u16	Задержка входящего вызова	х1, мс
9	Holding	RW	u16	Длительность звонка	х1, тики
11	Holding	RW	u16	Яркость светодиода	х1,%
104-105	Input	RO	u32	Время работы с момента загрузки	х1, сек
110	Holding	RW	u16	Скорость порта RS-485	х100, боды
12 – 1200 бит/с
24 – 2400 бит/с
48 – 4800 бит/с
96 – 9600 бит/с
192 – 19200 бит/с
384 – 38400 бит/с
576 – 57600 бит/с
1152 – 115200 бит/с
111	Holding	RW	u16	Настройка бита чётности порта RS-485	0 – нет бита чётности (none)
1 – нечётный (odd)
2 – чётный (even) 
112	Holding	RW	u16	Количество стоп-битов порта RS-485	1, 2
120	Holding	RW	u16	Регистр перезагрузки платы	0, 1
121 	Input	RO	u16	Текущее напряжение питания	х1, мВ
128 	Holding	RW	u16	Modbus-адрес устройства	по умолчанию 137
200-205	Input	RO	string	Модель устройства	EDMFv1

