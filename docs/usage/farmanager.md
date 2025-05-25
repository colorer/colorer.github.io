# FarColorer

FarColorer – это плагин, обеспечивающий подсветку синтаксиса в текстовом редакторе файлового менеджера [Far Manager](https://farmanager.com/).

Плагин включен в официальный дистрибутив Far Manager, начиная с версии v3.0.2948.

* **Исходный код:** [https://github.com/colorer/FarColorer](https://github.com/colorer/FarColorer)
* **Сборки (релизы):** [https://github.com/colorer/FarColorer/releases](https://github.com/colorer/FarColorer/releases)
* **Обсуждение на форуме Far Manager:** [Colorer — гибкая раскраска синтаксиса в редакторе и др.](https://forum.farmanager.com/viewtopic.php?p=179185)

## Варианты сборок FarColorer

Плагин FarColorer поставляется в двух вариантах сборки, различающихся механизмом обработки строк:

* **legacy**: Использует для обработки строк внутреннюю реализацию, применявшуюся в Colorer с ранних версий. Этот вариант обеспечивает совместимость со старыми операционными системами, такими как Windows XP.
* **modern**: Использует библиотеку [ICU (International Components for Unicode)](https://icu.unicode.org/) для обработки строк, что обеспечивает улучшенную поддержку Unicode и интернационализации.

В официальную поставку Far Manager включена **modern**-версия плагина.

Оба варианта сборок доступны для загрузки на [странице релизов](https://github.com/colorer/FarColorer/releases).

