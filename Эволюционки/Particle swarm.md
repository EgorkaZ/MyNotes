# Оптимизация роем частиц

* Моделирует поведение роя/стаи/стада животных
	* Не особо умные, но умеют коммуницировать
* В явном виде отбора нет, но [[Мутации|мутации]] направленные
* "Частица" содержит решение и ещё...
	* $\Large \vec{x}$: положение — собственно, решение
	* $\Large \vec{v}$: скорость — изменение решения на предыдущем шаге
	* $\Large \vec{x}^*$: лучшее решение, когда-либо найденное _данной_ частицей
	* $\Large \vec{x}^+$: лучшее решение, когда-либо найденное _информантами_ данной частицы
	* $\Large \vec{x}^!$: лушчее решение, когда-либо найденное кем-либо

* [[Мутации|Мутация]]:
	* $\Large \vec{v}' \leftarrow \alpha\vec{v} + \beta(\vec{x}^* - \vec{x}) + \gamma(\vec{x}^+ - \vec{x}) + \delta(\vec{x}^! - \vec{x})$
	* $\Large \vec{x}' \leftarrow \vec{x} + \vec{v}'$