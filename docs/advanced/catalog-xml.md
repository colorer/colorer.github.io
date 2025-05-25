# catalog.xml

Файл **catalog.xml** является центральным конфигурационным файлом для Colorer Library. Его основная задача – индексировать и указывать пути к файлам HRC-схем (определяющих синтаксис типов файлов) и HRD-стилей (определяющих правила их раскраски).
Структура файла catalog.xml строго определена и соответствует XML-схеме, доступной по адресу: [catalog.xsd](https://colorer.github.io/schema/v1/catalog.xsd).
Ниже представлены два примера структуры файла catalog.xml:

1. **Простой пример (вкладка "Simple"):** Демонстрирует базовую структуру с прямым указанием файлов.
2. **Сложный пример (вкладка "Hard"):** Иллюстрирует использование XML External Entities для включения содержимого из других файлов. Этот подход активно применяется в базовой библиотеке схем Colorer для лучшей организации и модульности.

=== "Simple"

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <catalog xmlns="http://colorer.github.io/schema/v1/catalog" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://colorer.github.io/schema/v1/catalog https://colorer.github.io/schema/v1/catalog.xsd">
        <hrc-sets>
            <location link="hrc/proto.hrc"/>
            <location link="hrc/auto"/>
        </hrc-sets>
        <hrd-sets>
            <hrd class="console" name="default" description="Aqua on blue">
                <location link="hrd/console/default.hrd"/>
            </hrd>
            <hrd class="rgb" name="default" description="White (crimsoned)">
                <location link="hrd/rgb/white.hrd"/>
            </hrd>
            <hrd class="text" name="tags" description="HTML italic, underline indention">
                <location link="hrd/text/tags.hrd"/>
            </hrd>
        </hrd-sets>
    </catalog>
    ```

=== "Hard"

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE catalog [
            <!ENTITY hrd "hrd">
            <!ENTITY catalog-console SYSTEM "hrd/catalog-console.xml">
            <!ENTITY catalog-rgb SYSTEM "hrd/catalog-rgb.xml">
            <!ENTITY catalog-text SYSTEM "hrd/catalog-text.xml">
            ]>
    <catalog xmlns="http://colorer.github.io/schema/v1/catalog" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://colorer.github.io/schema/v1/catalog https://colorer.github.io/schema/v1/catalog.xsd">
        <hrc-sets>
            <location link="hrc/proto.hrc"/>
            <location link="hrc/auto"/>
        </hrc-sets>
        <hrd-sets>
            &catalog-console;
            &catalog-rgb;
            &catalog-text;
        </hrd-sets>
    </catalog>
    ```

## Включение файлов с помощью XML External Entities

**XML External Entity (внешняя сущность)** – это стандартный механизм языка XML, который позволяет включать содержимое одного XML-файла (или другого внешнего ресурса) в структуру текущего XML-документа.

Объявление внешней сущности, например:
```
<!ENTITY catalog-console SYSTEM "hrd/catalog-console.xml">
```
размещается в секции DOCTYPE в начале XML-файла (как показано в примере "Hard"). Это объявление связывает имя сущности (`catalog-console`) с внешним файлом (`hrd/catalog-console.xml`).
Впоследствии, когда XML-парсер встречает в теле документа ссылку на эту сущность (например, `&catalog-console;`), он заменяет её содержимым указанного внешнего файла.
Относительные пути в атрибуте SYSTEM (как `hrd/catalog-console.xml`) разрешаются относительно местоположения текущего файла catalog.xml. Подробнее о форматах путей см. в разделе [Форматы путей файлов](file-paths.md).

## Элемент `<catalog>`

Это корневой элемент XML-документа catalog.xml. Он служит контейнером для двух основных секций: `<hrc-sets>` и `<hrd-sets>`.

## Элемент `<hrc-sets>`

Секция `<hrc-sets>` предназначена для регистрации HRC-схем в библиотеке. Внутри этого элемента используются дочерние элементы `<location link="..."/>` для указания путей:

* **К конкретному HRC-файлу:** Например, `<location link="hrc/myparser.hrc"/>`.
* **К каталогу с HRC-файлами:** Например, `<location link="hrc/languages/"/>`. В этом случае Colorer Library автоматически загрузит все файлы с расширением `.hrc` из указанного каталога.
    * **Исключение:** Файлы с расширением `.ent.hrc` (обычно используемые как фрагменты для XML External Entities) игнорируются при сканировании каталога.
    * **Нерекурсивный поиск:** Сканирование каталога происходит только на первом уровне вложенности; файлы в подкаталогах не загружаются автоматически через указание родительского каталога.

## Элемент `<hrd-sets>`

Секция `<hrd-sets>` служит для регистрации доступных HRD-стилей раскраски. Каждый стиль определяется элементом `<hrd>`, который имеет следующие атрибуты и дочерние элементы:

* **Атрибут `class`**: Определяет тип вывода, для которого предназначен стиль (например, "console" для консольного вывода, "rgb" для HTML/графического вывода с RGB-цветами, "text" для простого текстового вывода).
* **Атрибут `name`**: Уникальное имя стиля в рамках его класса (например, "default", "eclipse", "dark_blue"). Это имя используется для выбора стиля в приложениях.
* **Атрибут `description`**: Краткое описание стиля (например, "Aqua on blue", "White (crimsoned)").
* **Элемент `<location link="..."/>`**: Указывает путь к файлу `.hrd`, содержащему определение данного стиля.

Пример:

```xml
<hrd class="rgb" name="default" description="White (crimsoned)">
    <location link="hrd/rgb/white.hrd"/>
</hrd>
```

Это определяет стиль с именем "default" для класса "rgb", описание которого "White (crimsoned)", а само определение стиля находится в файле hrd/rgb/white.hrd.
