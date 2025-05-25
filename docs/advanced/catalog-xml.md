# catalog.xml

В **catalog.xml** описываются пути до hrc и hrd файлов.

Формат файла catalog.xml имеет фиксированную структуру, описанную в [catalog.xsd](https://colorer.github.io/schema/v1/catalog.xsd).

Простой пример содержимого файла выглядит представлен ниже на вкладке Simple.

В базовой библиотеке схем широко применяются возможности синтаксиса xml по вставке одного файла в другой (external entity). Поэтому пример файла, поставляемого в базовой библиотеке схем, выглядит немного сложнее, вкладка Hard.


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

## Подстановки файлов (external entity)

**external entity** - вставка внешнего файла в структуру текущего.

Запись вида
```
<!ENTITY catalog-console SYSTEM "hrd/catalog-console.xml">
```
в начале xml файла говорит о том, что вместо `&catalog-console;` ниже в файле будет подставляться содержимое файла `hrd/catalog-console.xml`. При указании относительного пути, путь рассчитывается от текущего файла. Работа с путями описана в разделе [Форматы путей файлов](file-paths.md).

## catalog

Основной блок xml-файла. Содержит в себе hrc-sets и hrd-sets.

## hrc-sets

В блоке `hrc-sets` задаются пути до hrc файлов. Это может быть путь как до конкретного файла, так и до папки. В случае указания пути до папки в ней обрабатываются все файлы с расширением `.hrc`, исключая `.ent.hrc`. Но только на первом уровне, во вложенных папках поиск файлов не производится.

## hrd-sets

В блоке `hrd-sets` задаются пути до файлов цветовых стилей.
