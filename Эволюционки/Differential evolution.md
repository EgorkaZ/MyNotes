# Дифференциальная эволюция

* Предназначена для задач на $\large \mathbb{R}^n$, но возможны варианты
* [[Мутации|Мутация]]: линейная комбинация нескольких решений
* [[Скрещивание]]: как правило, однородное

## Современные версии
* SHaDE, DISH
	* Крутые
* Особенности: настройка параметров в процессе работы (как в [[Evolution strategies|эволюционных стратегиях]])

## Нотация DE/x/y/z
* $x \in \{rand, best\}$: базовая особь для [[Мутации|мутации]]
* $y \in \mathbb{Z}^+$: число используемых векторов переноса
* $z \in {bin, ...}$: оператор [[Скрещивание|скрещивания]]


### DE/rand/1/* псевдокод

```rust
const mut_power: f64; // обычно в [0.5, 1.0]
const pop_cnt: usize;

let mut pop: Set<_> = repeat_with(random_solution).take(pop_cnt);
let best = pop.max_by(measure);

while have_time() {
	let mut children = Set::new();
	
	for i in (0..n) {
		let a = rand(0, n, except![i]);
		let b = rand(0, n, except![i, a]);
		let c = rand(0, n, except![i, a, b]);
		let (a, b, c) = (&pop[a], &pop[b], &pop[c]);
		let d = a + mut_power * (b - c);
		
		let curr = crossover(&d, &pop[i]);
		children.insert(max_by(measure)(curr, pop[i]));
	}
	pop = children;
	best = pop.chain(best).max_by(measure);
}
```

В адаптивных версиях `mut_power` как-то шевелится. Если `mut_power > 1`, то скорее всего, не сойдётся

В чём прикол: [[Мутации|мутация]] будет примерно пропорциональна "диаметру" популяции (см. строку `let d = ...`). То есть, побольше в начале, поменьше в конце (вспомним [[Evolution strategies#Пример простой правило одной пятой|правило "одной пятой"]]). И это хорошо
