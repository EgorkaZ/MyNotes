# Trivial File Transfer Protocol

Нужен для пересылки больших данных поверх [[UDP]]

Схема:
* Отправить первую пачку
* Получить ack
* Отправить следующую пачку

Беды:
* Большие задержки
* Что делать, если теряются пакеты?
	* Наверное, ждать таймаут

При этом никто не следит за общим состоянием. Прикол: если первый пакет заблудился, успел затаймаутиться и мы отправили снова первый пакет, после чего дошли оба, то на *i*-й пакет мы просто попросим *i+1*-й, а на подтверждение *i*-го отправим *i+1*-й. В итоге отправим файл дважды. Называется Sorcerer's Apprentice Syndrome

#rfc 1350