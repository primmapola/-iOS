`capture list` (список захвата) - процедура обработки «захваченных» ссылок. Если вы не используете `capture list`, то по умолчанию все выражения будут иметь `strong reference`. Для объявления `capture lists` используйте ключевое слово `weak` или `unowned`.

1. `weak` захваченные выражения являются `Optional` и не удерживаются замыканием, таким образом, они могут быть освобождены и установлены в nil.
2. Используем `unowned` если значение не будет `.none` до освобождения.