# Эволюционный алгоритм (1, lambda)

В точности [[ЭА (1 + lambda)]], но теперь родитель не участвует в отборе

* Выбираем лучшего из $\lambda$ потомков
* Лучший потомок **всегда** заменяет родителя

```rust
const lambda: usize;

fn Next(a: String, M: Actions) -> String
{
	let mut shuffled = M.shuffled();
	let mut best_new = shuffled.next().apply(&a);
	
	for act in shuffled {
		let curr_new = curr_act.apply(&a);
		if measure(&curr_new) > measure(&best_new) {
			best_new = curr_new
		}		
	}

	best_new
}
```
