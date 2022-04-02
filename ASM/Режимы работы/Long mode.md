# Long mode

Жизнь в [[x64]] мире

* Compatibility
	* Compatibility --> Protected
		* `EFER.LME = 1` 
			* long mode enable
		* `CR4.PAE = 1`
			* physical address extension
		* `CR0.PG = 1`
			* включение страничной адресации
	* Protected --> Compatibility
		* `CR0.PG = 0`
			* отключить сраничную адресацию
		* `EFER.LME = 0`
			* вообще, необязательно, но стоит
* 64-bit mode
	* 64-bit --> Compatibility
		* `CS.L = 0` имеется в виду поле long в _дескрипторе_ CS
	* Compatibility --> 64-bit
		* `CS.L = 1`

Переход подрежимов — просто, может делать userspace. Переход между режимами — тяжело, делает ядро. Перейти в real mode не выйдет

Compatibility, например, не имеет `sysenter`, `sysexit` (ну вы поняли)


### Амуде
От `CS` остались только права и битность. В `DS`, `ES`, `SS` — вообще ничего

В `FS`, `GS` осталась только база