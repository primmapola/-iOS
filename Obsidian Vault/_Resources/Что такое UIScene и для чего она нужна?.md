`UIScene` — это концепция, введенная в iOS 13, которая изменяет традиционный подход к управлению жизненным циклом приложения и его пользовательским интерфейсом. Она позволяет приложению управлять несколькими экземплярами пользовательского интерфейса одновременно, что особенно актуально для устройств, поддерживающих многозадачность, таких как iPad, где можно открыть несколько окон одного и того же приложения.

Вот основные моменты, касающиеся `UIScene`:

1. **Множественные экземпляры:** Ранее приложение iOS имело один экземпляр `UIWindow`, теперь с помощью `UIScene` можно иметь несколько экземпляров, каждый из которых управляет своим собственным пользовательским интерфейсом.

2. **Независимый жизненный цикл:** Каждая `UIScene` имеет свой собственный жизненный цикл, независимый от жизненного цикла приложения и других сцен. Это облегчает управление различными состояниями активности в приложении.

3. **Управление сессиями:** Каждая `UIScene` связана с экземпляром `UISceneSession`, который содержит информацию о текущей конфигурации сцены и позволяет системе восстанавливать сцены при необходимости.

4. **Delegate:** Как и приложение, у `UIScene` есть свой делегат (`UISceneDelegate`), который отвечает за отслеживание изменений состояния сцены и управление поведением сцены при таких изменениях.

5. **Адаптация к многозадачности:** `UIScene` упрощает разработку приложений, поддерживающих многозадачность, особенно на iPad, где пользователи могут взаимодействовать с несколькими экземплярами приложения одновременно в Split View или Slide Over.

Введение `UIScene` улучшает структуризацию кода и упрощает создание многозадачных и адаптивных приложений на iOS, позволяя разработчикам более гибко управлять пользовательскими интерфейсами и их состояниями.