# User Datagram Protocol
Протокол [[Слои протоколов#Transport|L4]]

#rfc 768

```c
s = socket()
bind(s, 53)
sockaddr{IP, port}
sendto(s, data, dst)
recvfrom(s, *data, *src)
```

### UDP datagram

![[Pasted image 20220406202813.png]]

Лежит внутри [[IP packet|IP пакета]]

Держит **source** и **destination** [[Port|порты]]

**length** есть как и в IP пакете, потому что в общем-то, ходить поверх [[IP]] мы не обязаны. Хранит в себе размер и хедера, и данных

**checksum** держит в себе не только хедер, но и данные

