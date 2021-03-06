# Routing Information Protocol

Протокол для [[Routing#Distance vector|Distance vector routing]]'а

* Узлы, которые могут за N шагов доставлять пакеты в некую подсеть S (для примера, 1.0.0.0/24), сообщают своим соседям об этом
* Соседи:
	* Запоминают:
		* Через кого
		* В какую подсеть
		* За сколько шагов
			* #NB 4 бита под расстояние, потому живёт при расстоянии не больше 16
	* Если есть предложение доступаться за меньше шагов, запоминаем узел за меньше
	* Говорят соседям о том, что могут доступиться до S через N+1 шаг

## Проблемы
* Приходится доверять соседям: все, у кого N=0, конфиг проставлен вручную
* Если есть больше, чем один путь до подсети S, при этом источник этого пути отвалился, оставшиеся узлы верят, что есть какой-то путь длиной в N до подсети S, о чём друг другу сообщают
	* Не сообщаем узлам, от которых услышали о доступе, о своём доступе
		* Вообще говоря, цикл может случиться

#rfc 2453 #rfc 2080