# Log Replication
![[Pasted image 20220404233524.png]]
* **Commited** - подтверждено большинством
* **Leader** регулярно шлёт всем **AppendEntries**
	* Там есть `leader_id` и пачка записей (`log_index`, `term`, `data`)
	* Там есть предыдущий (`log_index`, `term`)
	* **Follower** применяет к своему журналу, только если совпало

### Возможные расхождения
![[Pasted image 20220404233543.png]]
* *a-b* - отсутствие записей
	* Скажет об этом **Leader**'у
	* Рано или поздно, заполнит
* *c-d* - неподтверждённые записи
	* Есть только у меньшинства серверов
	* Значит, что *d* был лидером 7-го [[RAFT Leader Election#Термы|терма]], но не успел их разослать
		* *с* был лидером в 6-м терме
* *e-f* - всё сразу
	* *f* был лидером 2-го терма, но не смог их никому разослать

### Согласование
* Храним `next_index` для каждого **Follower**'а в **Leader**'е
	* Пытаемся засылать запись с этого индекса
* Если что-то не так, уменьшаем его на один
	* Лучше не оптимизировать, и так норм
* **Commited** записи не должны перезаписываться
	* [[RAFT Leader Election#Election restriction|Election Restriction]]
* Не **Commited** записи перетрутся
* Бтв, индекс это номер записи в журнале

### Проблема commitment
![[Pasted image 20220404233621.png]]

a. S1 сделал запись 2, частично реплицировал и упал
b. S5 принял запрос и тоже упал
c. S1 ожил, продолжил считать себя лидером, продолжает реплицировать, шлёт запись в S3
варианты:
	d. S1 упал окончательно, S5 ожил и закончил репликацию
	e. S1 продолжил репликацию
	
Итого, запись 2 в пункте _c_ как бы **Commited**, но всё ещё может перезаписаться. Потому запись 2 не будет считаться **Commited**, т.к. не будет записью текущего **Leader**'а

#### Правило Commited
*Только записи из текущего term leader'а* считаются **Commited** при записи большинство (тот же [[Кворум]], что в [[RAFT Leader Election|Leader Election]]) серверов

### Смена конфигурации
Чё делать, если сервер больше никогда не вернётся в семью 
![[Pasted image 20220404233651.png]]

* Сделать запись о смене конфигурации в журнал
	* $C_old \rightarrow C_{old,new} \rightarrow C_new$
* Сначала новые сервера включаются после без права голоса, ждём, пока на него реплицируется журнал
* При коммите $C_new$ старый лидер должен уйти

### Сжатие журналов
* Независимо на каждом сервере делается snapshot
* **Leader** иногда вынужден посылать snapshot