# Форматы путей к файлам и каталогам

Colorer Library поддерживает несколько форматов и механизмов для указания путей к файлам и каталогам. Это абсолютные и относительные пути, ссылки на файлы внутри ZIP-архивов и использование переменных окружения. Данные форматы применимы при указании местоположения файлов в конфигурационных XML-файлах (например, в catalog.xml), а также внутри HRC- и HRD-файлов (например, для импорта других схем или указания на внешние ресурсы).

## Абсолютный (полный) путь
   
Это стандартный полный путь к файлу или каталогу в файловой системе операционной системы. Например:

=== "Windows"
    ```
    c:\user\test\1.xml
    ```
=== "Linux"
    ```
    /home/user/1.xml
    ```
    
## Длинные пути (Windows)
   
Для операционной системы Windows Colorer Library поддерживает специальный префикс `\\?\` для работы с путями, превышающими стандартное ограничение MAX_PATH. Это позволяет обращаться к файлам и каталогам с очень длинными именами или глубокой вложенностью. Пример: `\\?\C:\Very long path\Another very long path segment\...\file.txt`

## Относительный путь

Относительный путь указывает местоположение файла или каталога относительно другого, "базового" пути. В контексте Colorer Library базовым путем обычно является местоположение файла, в котором данный относительный путь указан (например, catalog.xml или HRC-файл).

Путь считается относительным, если он не является абсолютным (т.е. не начинается с корневого каталога или буквы диска в Windows) и может содержать специальные обозначения, такие как `.` (текущий каталог) и `..` (родительский каталог).

**Пример:** Если в файле `C:\projects\my_project\config\catalog.xml` указан относительный путь `../schemas/main.hrc`, то он будет разрешен в абсолютный путь `C:\projects\my_project\schemas\main.hrc`.

## Файл внутри ZIP-архива

Colorer Library позволяет обращаться к файлам, находящимся внутри **ZIP-архивов**. Это особенно удобно для распространения библиотек схем, которые могут содержать большое количество HRC и HRD файлов.

Синтаксис пути к файлу внутри архива следующий:

```jar:путь_до_архива!путь_до_файла_в_архиве```

Где:

* `jar:`– специальный префикс, указывающий на использование ZIP-архива. (Примечание: префикс `jar` является историческим наследием, так как JAR-файлы в Java являются ZIP-архивами).
* `<путь_к_ZIP_архиву>` – путь к самому ZIP-файлу. Этот путь может быть абсолютным, относительным или использовать переменные окружения (см. ниже).
* `!` – разделитель между путем к архиву и путем внутри архива.
* `<путь_к_файлу_внутри_архива>` – путь к целевому файлу или каталогу внутри структуры ZIP-архива.
* 
**Пример:** `jar:schemes.zip!languages/cplusplus.hrc`

Этот путь указывает на файл `languages/cplusplus.hrc`, который находится внутри архива `schemes.zip`. Если `schemes.zip` указан без абсолютного пути, он будет искаться относительно базового пути (например, каталога, где находится `catalog.xml`).

Сам `<путь_к_ZIP_архиву>` может быть задан с использованием любого из форматов, описанных в этом разделе (абсолютный, относительный, с переменными окружения).

## Использование переменных окружения

Colorer Library поддерживает подстановку значений переменных окружения в путях к файлам и каталогам. Синтаксис зависит от операционной системы:

* **Windows**: Переменные указываются в формате `%VARIABLE_NAME%` (например, `%COLORER_HOME%\catalog\catalog.xml`).
* **Linux/macOS**: Переменные указываются в формате `$VARIABLE_NAME` или `${VARIABLE_NAME}` (например, `$COLORER_HOME\catalog\catalog.xml` или `${COLORER_HOME}\catalog\catalog.xml`).

## Особенности указания путей в XML External Entities

При определении внешних XML-сущностей (External Entities) в декларации `DOCTYPE` XML-файла (например, в catalog.xml), строка, указанная в атрибуте `SYSTEM`, также интерпретируется как путь к файлу. Например:

`<!ENTITY my_include SYSTEM "includes/common_settings.xml">`

Эти пути в целом следуют ранее описанным правилам (абсолютные, относительные, ссылки на архивы), но имеют **важные отличия при использовании переменных окружения**:

1. **Формат переменных окружения:** Независимо от операционной системы, переменные окружения внутри SYSTEM-идентификатора всегда указываются в формате `$VARIABLE_NAME` (без `{}`)
2. **Префикс `env:` для путей с переменными окружения (не в архивах):** Если путь содержит переменную окружения и не является путем к файлу внутри архива (т.е. не начинается с `jar:`), он **должен** начинаться со специального префикса `env:`. Этот префикс указывает парсеру Colorer, что необходимо произвести подстановку переменных окружения.


Примеры:

=== "Относительный путь (стандартно)"
    ```xml
    <!ENTITY catalog-console SYSTEM "hrd/catalog-console.xml">
    ``` 
    Разрешается относительно текущего XML-файла.

=== "Файл в архиве (стандартно)"
    ```xml
    <!ENTITY catalog-console SYSTEM "jar:data/common-allpacked.zip!hrd/catalog-console.xml">
    ```
    Стандартный синтаксис `jar:`, путь к архиву `data/common-allpacked.zip` разрешается относительно текущего XML-файла.

=== "Переменная окружения (с префиксом env:)"
    ```xml
    <!ENTITY catalog-console SYSTEM "env:$FARHOME/hrd/catalog-console.xml">
    ```
    `$FARHOME` будет заменено значением переменной окружения. Префикс `env:` обязателен, так как путь не указывает на архив.

=== "Переменная окружения и архив (без префикса env:)"
    ```xml
    <!ENTITY catalog-console SYSTEM "jar:$FARHOME/data/common-allpacked.zip!hrd/catalog-console.xml">
    ```
    `$FARHOME` будет заменено значением переменной окружения. Префикс `env:` не нужен, так как путь уже начинается с `jar:`, что является достаточным указанием для специальной обработки.
