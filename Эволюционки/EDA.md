# Estimation-of-Distribution Algorithms
( a.k.a. Алгоритмы оценки распределений)

В отличие от обычных эволюционных алгоритмов, не имеем нормальной популяции с нормальным отбором, вместо этого создаём модели, как-то их оцениваем

## Общая схема

```rust
let mut models = vec![model_0]; // модель, представляющая распределение "по умолчанию"
let len = ...; // размер выборки

for t in (0..) {
	let samples = Vec::with_capacity(len);
	let fitness = Vec::with_capacity(len);
	for i in (0..len) {
		samples.push(sample_from(&models[t])); // генерация решения
		fitness.push(check_fitness(&samples.last())); // вычисление приспособленности
	}
	let new_model = Model::create(&models[t], samples, fitness);
	models.push(new_model);
}
```

Что можно менять:
* Устройство модели
* Размер выборки
* Правило обновления

## Алгоритмы

* [[Univariate EDA]]
* [[Multivariate EDA]]
* [[Байесовская оптимизация]]
* [[Ant Colony Optimization]]