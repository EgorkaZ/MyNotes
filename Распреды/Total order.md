# Общий порядок

Может нарушиться только в случае [[Массовая рассылка сообщений|массовой рассылки сообщений]]. Ортогонален [[Упорядочивание сообщений#Порядок unicast сообщений|порядку unicast сообщений]] 

$$\nexists m,n \in M; p,q \in P: rcv_p(m) < rcv_p(n) \&	rcv_q(n) < rcv_q(m)$$

Тривиально выполняется для всех unicast сообщений

![[Pasted image 20220221181627.png]]
Пример нарушения общего порядка 

Восстанавливаем через [[Алгоритм восстановления общего порядка]]
