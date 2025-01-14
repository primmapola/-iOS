В Objective-C и системе управления памятью Cocoa, введение "side tables" (или таблиц вспомогательных данных) является ответом на необходимость более эффективного управления памятью и поддержки многопоточности. Ранее, управление счётчиком ссылок осуществлялось напрямую в объектах, что ограничивало возможности и эффективность управления памятью в условиях современных многопоточных приложений.

### Прежний подход

Исходно в Objective-C каждый объект содержал свой счётчик ссылок (retain count) внутри самого объекта. Это означало, что каждый раз, когда объект получал сообщение `retain` или `release`, его внутренний счётчик изменялся. Хотя этот метод был простым и прямым, он сталкивался с несколькими проблемами:

1. **Проблемы с многопоточностью**: При изменении счётчика ссылок из разных потоков возникали условия гонки, что могло привести к некорректному управлению памятью.
2. **Неэффективное использование памяти**: Каждый объект должен был содержать дополнительное пространство для счётчика ссылок, даже если объект никогда не участвовал в операциях подсчёта ссылок.

### Введение side tables

Для решения этих и других проблем были введены "side tables". Эти таблицы представляют собой отдельные структуры данных, которые хранят информацию о счётчиках ссылок объектов вне самих объектов. Использование side tables имеет несколько преимуществ:

1. **Улучшенная поддержка многопоточности**: Теперь изменения счётчиков ссылок могут быть синхронизированы на уровне side table, что значительно снижает вероятность возникновения условий гонки и делает управление памятью более надёжным в многопоточной среде.
2. **Оптимизация использования памяти**: Объекты, которые не участвуют в операциях подсчёта ссылок, не нуждаются в дополнительном пространстве для счётчика ссылок, так как все данные о ссылках теперь хранятся в side table. Это делает объекты меньше по размеру и уменьшает общее потребление памяти.
3. **Гибкость управления**: Система может более эффективно управлять памятью, оптимизируя работу с side tables в зависимости от текущего использования объектов и условий выполнения программы.

Введение side tables — это часть общей стратегии улучшения производительности и надёжности Objective-C и Cocoa, особенно в условиях современного многопоточного и многозадачного программирования. Эти изменения позволяют разработчикам создавать более стабильные и эффективные приложения, работающие на разнообразных устройствах Apple.


### Суть 
Раньше в объектах был счетчик ссылок (retain count) и при каждом retain/release он изменялся, что приводило к проблемам особенно в многопоточном приложении - писать в этот счетчик было довольно сложно))