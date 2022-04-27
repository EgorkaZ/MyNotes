# Consistency Availability Partition tolearnce теорема

## Предисловие
[[Paxos]] и [[RAFT]]:
* Могут пережить отказ какой-то ноды
* Очень страдают на ненадёжных каналах связи

![[Pasted image 20220425173046.png]]

## Теорема

Выделим три свойства:

### Consistency
Все клиенты видят одинаковые данные

### Availability
Система работает, несмотря на сбои узлов (запрос к живому узлу должен получить ответ)

### Partition tolerance
Система работает несмотря на обрыв связи разными частями (partition)

### Формулировка
В одной системе может быть только *2 из 3х*

![[Pasted image 20220425173400.png]]

## Деление по CAP

* Consistency + Availability = Easy ( #todo будет запись про это? )
* Consistency + Partition tolerance = [[Paxos]]/[[RAFT]]
* Availability + Partition tolerance = [[Gossip]]