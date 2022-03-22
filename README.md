# Плата умного домофона, версия с modbus

## Монтаж в разрыв линии 
Рекомендуется подключать в базе трубки, разъединив линию в квартиру.  Ввод линии подключается к Line -/+ на плате ESPDomofon, где + обычно красный. Полярность обычно можно подсмотреть на плате домофона. Intercom -/+ подключается соответственно к трубке.
Некорректная полярность не приведет к повреждениям, но работать не будет, 
На уличной панели при вызове будет отображаться error, в целом работа домофонной системы не будет нарушена. 
![схема подключения](https://github.com/Ge1mer/Domofon_modbus/blob/main/%D0%A1%D1%85%D0%B5%D0%BC%D0%B0%20%D0%BF%D0%BE%D0%B4%D0%BA%D0%BB%D1%8E%D1%87%D0%B5%D0%BD%D0%B8%D1%8F1.png)
 
## Возможности и режимы
*	Управление как физической кнопкой на корпусе, так и через интеграцию с умным домом
*	Режим автоматического открытия двери (одиночный и постоянный)
*	Режим автоматического отклонения вызова
*	Режим "без звука" постоянный или на один звонок
*	Регулировка яркости светодиода

## Подключение
1. Аккуратно вскройте домофон и определите, где кабель, входящий из подъезда, подсоединён к плате. Запомните, какой из проводов кабеля плюс, какой минус, в дальнейшем это поможет избежать ошибок подключения (впрочем, если вы их перепутаете, ничего страшного не случится, ни плата, ни домофон не пострадают, просто не будут работать, пока не подключите правильно).
2. Проверьте, нет ли на входящем из подъезда кабеле винтовых соединений или скруток. В большинстве случаев этот кабель не припаян непосредственно к плате домофона, а подсоединён к соединительным проводам. Возможно, имеет смысл поставить плату именно в этот разрыв.
3. Подключите входящий из подъезда кабель к разъёмам Line+ и Line- платы (обратите внимание на подписи [на обратной стороне платы], подключите идущие к базе трубки домофона провода к разъёмам Intercom+ и Intercom- платы.
4. Подключите питание, и линии RS-485. Как вариант можно использовать провод типа витой пары. Подать можно от 5 до 24 вольт.
## Конфигурация
Платы уже приходят прошитыми.
Параметры порта по умолчанию: скорость 9600 бод, 8 битов, без битов четности, два стоп бита.
Адрес Slave ID – 137.
При использовании совместно с Wiren Board, загружаем конфиг файл [config-wb-domofon-mb.json](https://github.com/Ge1mer/Domofon_modbus/blob/main/config-wb-domofon-mb.json).
В веб интерфейсе появляются данные платы. Параметры порта UART можно настроить для совместимости с другими устройствами на линии.
![веб интерфейс](table.png)![веб2](https://github.com/Ge1mer/Domofon_modbus/blob/eea6ae64c5e195300b1280aba232ee066f219485/table%20config.png)

Остальные параметры позволяют настроить корректную работу с вызывной панелью домофона.
Если с выключенной платой домофон работает как обычно, а с включенной не определяется входящий звонок, необходимо увеличить значение Длительность звонка, так же этим параметром можно изменить время гудка вызывной панели перед автоматическим открытием.
Если входящий звонок определяется, но не работает открытие двери через плату, необходимо увеличить значения Время снятой трубки или Время импульса открытия. 
В очень редких случаях не стабильного открывания двери, можно увеличить параметр Пауза перед снятием трубки.
Данные параметры настраиваются однократно, и в процессе дальнейшей эксплуатации не требуют изменения.
  

## Использование в системах умного дома
В системы умного дома управляющие элементы прокидываются посредством MQTT. Есть индикатор входящего вызова, кнопки открытия двери и отклонения вызова. Переключатели режимов платы. Регулятор яркости светодиода.

Для SprutHub настройки в файле [domofon.json](https://github.com/Ge1mer/Domofon_modbus/blob/main/domofon.json)

Все режимы работают автономно на плате. При пропадании связи с системой умного дома, включенный режим продолжит работу. Настройки параметров сохраняются в энергонезависимой памяти.



[Чат в Telegram](https://t.me/domofon_esp)

![Регистры модбас](https://github.com/Ge1mer/Domofon_modbus/blob/main/Registers.png)
