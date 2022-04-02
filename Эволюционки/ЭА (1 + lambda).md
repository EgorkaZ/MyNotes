# Эволюционный алгоритм (1 + lambda)

В отличие от [[ЭА (1 + 1)|схемы (1 + 1)]], теперь в отборе (кроме родителя) участвует не один, а $\lambda$ детей

Ускорение движения по "градиенту"
* Создавать $\lambda$ потомков, выбирать лучшего
* Лучший потомок заменяет родителя, **если он не хуже**

```rust
const lambda: usize;

fn Next(a: String, M: Actions) -> String
{
	let mut shuffled = M.shuffled();
	let mut best_new = shuffled.next().apply(&a);
	
	for act in shuffled.take(lambda - 1) {
		let curr_new = shuffled.apply(&a);
		if measure(&curr_new) > measure(&best_new) {
			best_new = curr_new;
		}
	}
	
	when! {
		measure(&best_new) >= measure(&a) => best_new,
		_ => a
	}
}

```