# Transmission Control Protocol

Существует поверх [[IP]]

#### Server
```c
s = socket();
bind(s, 80);
s2 = accept(s);
read(s2);
write(s2);
```

#### Client
```c
s = socket();
connect(s, addr);
read(s);
write(s);
```

![[Pasted image 20220406235049.png]]

* Sequence number - номер отрезка среди всех данных
* Data offset - размер хедера/с какого оффсета начинаются данные
* Как и в [[UDP]], используем [[Port|порты]]
* Флаги
	* CWR, ECE - см. [[IP packet|здесь]]
	* ACK - проставляется при отправке ack'а
	* SYN - проставляется в [[TCP#3-way handshake|3-way handshake]]
	* FIN - я больше не буду слать данные
	* URG - на самом деле, у вас два канала в TCP соединении, один из них - для всякой служебной важной информации
	* PSH - отдай приложению накопленное сейчас
* Window Size - размер отсылаемого окна

#rfc 793, 1122, 2018, 5681, 7323

## Потеря пакетов

* В ответ на пакеты клиент отправляет ack'и
* В ack'е он говорит, какое количество байт от всех данных он получил

### Fast Retransmit

Избегаем повторной отправки пакетов, если теряется пакет посередине

* Если потерялся один пакет между двумя другими, клиент повторно отправляет ack повторно предыдущий ack
* Сервер отправляет следующий пакет за ack'нутым
* Клиент восполняет картину данных, шлёт ack на самый дальний из пришедших подряд

### Selective acknowledgements

Если клиент с сервером заранее договорятся при подключении, может проставлять в поле Options номера пакетов, которые на самом деле не дошли, хотя ack проставлен правее

## Congestion control

Нужно как-то эмпирически выяснить, сколько пакетов за раз можно засылать, чтобы они не терялись. См. [[Congestion control|здесь]]

## Flow control

Если приложение задыхается и не может принять столько данных, шлём в ack'е серверу Window Size = 0 (то бишь, просим его не слать нисколько данных)

## 3-way handshake

Проблема: нужно (сравнительно ?) много ресурсов, чтобы создать TCP соединение

Что делаем:
* Шлём SYN сообщение серверу
* Сервер шлёт обратно сообщение, в котором говорит, с какого числа будет нумеровать данные
	* Так мы проверяем базовую вменяемость клиента: он действительно слушает соединения на этом [[IP address|адресе]]
* После этого уже шлём ещё один ACK, где пишем сообщённое сервером число
	* Тут мы убедились в адекватности клиента
	* Только после этого выделяем какие-то ресурсы

![[Pasted image 20220407010831.png]]

### Fast open

* Выдаём клиенту некий SYN Cookie - чиселка-хешик от чего-то там
* Если в следующий раз при установке соединения клиент пошлёт эту чиселку, поверим ему сразу
	* В этом же SYN'е могут быть данные

### Бонус

Большой [[DFA|автомат]] установки соединения
![[Pasted image 20220407013331.png]]