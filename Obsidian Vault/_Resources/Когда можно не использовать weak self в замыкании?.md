Можно не использовать, когда мы уверены, что не будет retain cycle [[Что такое Retain Cycle?]] Бывает ситуации когда retain cycle полезен - [[Всегда ли плох цикл сильных ссылок?]]
Примеры: 
	1. Когда мы используем замыкание в качестве какой то глобальной функции(потому что скоуп котором она лежит != класс)
	2. Когда подсказывает компилятор)
	3. 