# Поиск с запретами

Мудрость: за границей не надо питаться дважды в одном месте

```rust
struct Self {
	tabu: BiMap<String, Ordered<Time>>,
	limit: usize,
	last_time: Time
}

fn Next(&mut self, a: String) -> String
{
	let curr = random_mutation().apply(&a);
	
	if measure(&curr) >= measure(&a) {
		self.last_time += 1;
		self.tabu.insert(curr, self.last_time);
		if self.tabu.len() > self.limit {
			let oldest = self.tabu.right().min();
			self.tabu.erase(oldest);
		}
		
		return curr
	} else {
		return a
	}
}
```


* В общем случае (например, в непрерывном),  проверять, что `curr` вне $\large \varepsilon$-окрестности от `tabu`

* Ещё можно хранить в `tabu` не особей, а [[Мутации|мутации]], при этом сохранять обратную мутацию, т.е. такую, которая откатит изменения только что применённой