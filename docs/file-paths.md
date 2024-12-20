# Форматы путей файлов

В Colorer Library для указания пути до файла или папки могут использоваться различные возможности: относительные пути, архивы, переменные окружения. В том числе такие пути поддерживаются в библиотеке схем (файлы xml, hrc, hrd).

## Полный путь
   
Обычный полный путь до файла на файловой системе. Например
=== "Windows"
    ```
    c:\user\test\1.xml
    ```
=== "Linux"
    ```
    /home/user/1.xml
    ```
    
## Длинный путь
   
В Windows поддерживается работа с [длинными путями](https://learn.microsoft.com/en-us/windows/win32/fileio/maximum-file-path-limitation?tabs=registry). Полный путь может быть указан через `\\?\`, например `\\?\c:\user\test\1.xml`

## Относительный путь

Путь считается относительным, если:

* не содержит указания на диск (Windows)
* не начинается с корня (Linux)
* в пути встречаются ссылки на текущую папку `./` или на родительскую `../`

Полный путь до файла рассчитывается относительно файла, в котором указан относительный путь. Т.е. если путь `..\1.xml` указан в файле `c:\users\test\base\catalog.xml`, то полный путь до файла равен `c:\users\test\1.xml`.

## Файл в архиве
   
Файлы библиотеки схем могут храниться в **zip** архиве. Это позволяет, например, упростить доставку схем, когда их десятки/сотни.

Путь, указывающий на файл в архиве, имеет следующий формат:

```
jar:путь_до_архива!путь_до_файла_в_архиве
```

Например `jar:common.zip!base/cpp.hrc`: файл common.zip в текущей папке и внутри архива base/cpp.hrc.

При этом путь до архива так же может быть указан любыми описанными тут вариантами.

!!! note

    jar - исторически оставшийся артефакт из мира Java. Там jar файлы это архивы.


## Переменные окружения

При указании путей поддерживается использование переменных окружения в формате, поддерживаемым текущей операционной системой. Например,
=== "Windows"
    ```
    %COLORER_HOME%\catalog\catalog.xml
    ```
=== "Linux"
    ```
    $COLORER_HOME/catalog/catalog.xml
    $COLORER_HOME$/catalog/catalog.xml
    ```

## Пути в xml external entity

Запись вида
```
<!ENTITY catalog-console SYSTEM "hrd/catalog-console.xml">
```
в начале xml файла, называется external entity. Строка, указанная после SYSTEM, это путь до файла, который будет в дальнейшем подставлен в текущий вместо `&catalog-console;`.

Данный путь так же поддерживает перечисленные выше форматы. Но для переменных окружения есть отличие:

* переменная окружения вне зависимости от платформы указывается в формате $NAME
* если путь не в формате архива (не используется `jar:`), то в начале пути необходимо указать `env:`

Примеры:

=== "Относительный путь"
    ```
    <!ENTITY catalog-console SYSTEM "hrd/catalog-console.xml">
    ```
=== "Архив"
    ```
    <!ENTITY catalog-console SYSTEM "jar:data/common-allpacked.zip!hrd/catalog-console.xml">
    ```
=== "Переменные окружения"
    ```
    <!ENTITY catalog-console SYSTEM "env:$FARHOME/hrd/catalog-console.xml">
    ```
=== "Переменные окружения и архив"
    ```
    <!ENTITY catalog-console SYSTEM "jar:$FARHOME/data/common-allpacked.zip!hrd/catalog-console.xml">
    ```
