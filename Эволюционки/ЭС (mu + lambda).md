# Эволюционная стратегия (mu + lambda)

Похоже на то, что мы видели в [[ЭА (1 + lambda)]] и [[ЭА (1, lambda)]], но теперь у нас вместо $1$ будет $\mu$ родителей, что будет нашим размером популяции

```rust
const prnt_cnt: usize; // mu
const children_cnt: usize; // lambda

let mut population: Set<Unit> = repeat_with(random_solution)
	.take(prnt_cnt);
let mut best = parents.max_by(measure);

while have_time() {
	let mut children: Set<Unit> = (1..children_cnt)
		.map(|_| mutate(choose(&parent)));
	let population = children
		.chain(parents)
		.sorted_by(measure)
		.take(prnt_cnt);
	best = population.chain(best).max_by(measure);
}

```

Сходится в локальном оптимуме быстрее, чем [[ЭС (mu, lambda)]], но охотнее застревает в них