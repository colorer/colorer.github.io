# Colorertools

Консольная программа для работы с возможностями библиотеки. 
Все версии можно скачать на [странице релизов](https://github.com/colorer/Colorer-library/releases).

Описание приведено для версий, начиная с 1.5.0.

```
Usage: colorer (command) (parameters) (logging parameters) [<filename>]
 Commands:
  -l         Lists all available languages
  -lt        Lists all available languages (HRC types)
  -ll        Lists and loads full HRC database
  -r         RE tests
  -h         Generates plain coloring from <filename> (uses 'rgb' hrd class)
  -ht        Generates plain coloring from <filename> using tokens output
  -v         Runs viewer on file <fname> (uses 'console' hrd class)
  -p<n>      Runs parser in profile mode (if <n> specified, makes <n> loops)
 Parameters:
  -c<path>   Uses specified 'catalog.xml' file
  -cs<path>  Uses specified 'hrcsettings.xml' file
  -cu<path>  Load user hrc-files from path
  -i<name>   Loads specified hrd rules from catalog
  -t<type>   Tries to use type <type> instead of type autodetection
  -ls<name>  Use file <name> as input linking data source for href generation
  -o<name>   Use file <name> as output stream
  -ln        Add line numbers into the colorized file
  -db        Disable BOM start symbol output in Unicode encodings
  -dc        Disable information header in generator's output
  -ds        Disable HTML symbol substitutions in generator's output
  -dh        Disable HTML header and footer output
 Logging parameters (default logging off):
  -eh<name>  Log file name prefix
  -ed<name>  Log file directory
  -el<name>  Log level (off, debug, info, warning, error). Default value is off.
```

### Значения по умолчанию

В работу библиотеки заложено следующее поведение по умолчанию, которое применимо и для программ, основанных на ней (если не расширено):

* Если не задан путь до catalog.xml, то производится поиск в переменной окружения **COLORER_CATALOG**. Если и там пусто, то работа библиотеки завершается с ошибкой.
* Класс стиля раскраски при преобразовании в реальные цвета (Style) по умолчанию "rgb".
* Класс стиля раскраски при преобразовании в текстовое представление (Text) по умолчанию "text".
* Если не задано имя стиля раскраски, то проверяется переменная окружения **COLORER_HRD**. Если и там пусто, то используется "default" стиль.
* Если не задан путь до hrcsettings.xml, то производится поиск в переменной окружения **COLORER_HRC_SETTINGS**. Если там пусто, то работа продолжается.

### Переменные окружения

* **COLORER_CATALOG** - путь до catalog.xml. Обрабатывается только в случае если не задан путь через параметры.
* **COLORER_HRD** - имя стиля раскраски. Обрабатывается только в случае если не задан через параметры.
* **COLORER_HRC_SETTINGS** - путь до hrcsettings.xml. Обрабатывается только в случае если не задан путь через параметры.