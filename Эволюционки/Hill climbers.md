# Hill climbers
Дискретные [[Градиентный спуск|градиентные спуски]]

## Задача
* Есть строка из upper-case латиницы (алфавитом назовём $\Sigma$) длины $N$
* Найти строку с максимальной суммой символов (т.е. _ZZZ...Z_)
* Начинаем с _AAA...A_
* Имеем набор операций _M_ вида "поставить первой буквой _А_", "поставить первой буквой _B_", ...,"поставить первой буквой _Z_", "поставить второй буквой _A_", ...
	* Всего $|\Sigma|N$ операций

#### Примечание
Метафорически, сумма символов это "мера приспособленности" нашей "особи", операции — [[Мутации|мутации]]

### First ascent hill climber

Применяем первое улучшающее действие

Таким образом, для каждой из $N$ букв мы применим $\Sigma$ операций, перебирая, как максимум, все $|\Sigma|N$. Итого, сложность $O(\Sigma^2N^2)$

```rust
fn Next(a: String, M: Actions) -> String
{
	for act in M {
		let b = act.apply(&a);
		if measure(&b) > measure(&a) {
			return b
		}
	}
	return a
}
```

### Steepest ascent HC

На каждом шаге выберем такое действие, которое делает максимальный шаг по улучшению ситуации.

Всё ещё перебираем $N\Sigma$ операций, для каждого из $N$ символов, но теперь по одному разу. Сложность $O(N^2\Sigma)$

```rust
fn Next(a: String, M: Actions) -> String
{
	let mut best = a;
	for act in M {
		let b = act.apply(&a);
		if measure(&best) > measure(&b) {
			best = b;
		}
	}
	return best
}
```

### Random mutation HC

Теперь будем рандомно тасовать [[Мутации|мутации]] 

Мысль: чтобы выбить вероятность $\Large \frac{1}{n}$, у нас матожидается $n$ попыток.

Если у нас приспособленность $x$, то вероятность [[Мутации|мутаций]], которые улучшат нам положение, будет $\Large \frac{N\Sigma - x}{N\Sigma}$, значит, понадобится $\Large \frac{N\Sigma}{N\Sigma - x}$ мутаций. Примем, что можем улучшиться не более, чем на 1, просуммируем по всем $x$, что-то поколдуем... 

Сложность $O(N\Sigma \log(N\Sigma)$

```rust
fn Next(a: String, M: Actions) -> String
{
	for act in M.shuffled() {
		let b = act.apply(&a);
		if measure(&b) > measure(&a) {
			return b
		}
	}
	return a
}
```

### Random Local Search
Упрощённый [[Hill climbers#Random mutation HC|RMHC]]

* Работает похуже, потому что может повторять [[Мутации|мутации]]
* Не нужно помнить всё, что пробовали, легко реализовать

```rust
fn Next(a: String, M: Actions) -> String
{
	let act = M.shuffled().next();
	let b = act.apply(&a);
	when! {
		measure(&b) >= measure(&a) -> b,
		_ => a,
	}
}
```