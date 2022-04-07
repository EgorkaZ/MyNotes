# Neighbour Discovery Protocol

Работает поверх [[IP]] с помощью [[ICMP]]

* Address resolution
	* Neighbour Solicitation
	* Neighbour Advertisement
	* В source [[Физическая сеть#MAC address|MAC]]  пишем свой адрес, в destination - Broadcast
* Router discovery - кому в сети отправить пакет, чтоб он отправился в Интернет
	* Router Solicitation
	* Router Advertisement
* Redirection - уметь говорить "теперь роутер он, а не я"
	* Redirect

#rfc 4861
