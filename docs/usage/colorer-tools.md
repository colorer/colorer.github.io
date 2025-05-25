# Colorertools

Colorertools – это набор консольных утилит, которые предоставляют доступ к основным функциям Colorer Library непосредственно из командной строки. Это позволяет использовать библиотеку для пакетной обработки файлов, тестирования HRC-схем, генерации HTML-разметки с подсветкой и других задач.

Актуальные сборки Colorertools можно загрузить со [страницы релизов основного проекта Colorer Library](https://github.com/colorer/Colorer-library/releases).

Приведенное ниже описание команд и параметров актуально для версий, начиная с 1.5.0.
(далее блок Usage)

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

### Поведение и значения по умолчанию

Библиотека Colorer и приложения на ее основе (если иное не переопределено в самом приложении) придерживаются следующих соглашений и значений по умолчанию:

* **Путь к catalog.xml:** Если путь к файлу catalog.xml не указан явно через параметры, библиотека пытается определить его из переменной окружения **COLORER_CATALOG**. Если переменная также не установлена или пуста, инициализация библиотеки завершится с ошибкой.
* **Класс стиля для HTML/RGB вывода:** При генерации цветного вывода (например, в HTML), по умолчанию используется HRD-класс стилей "rgb" (из секции `<hrd-class name="rgb">` в HRD-файле).
* **Класс стиля для текстового вывода:** При генерации текстового представления разбора (например, для отладки или специфических форматов), по умолчанию используется HRD-класс стилей "text" (из секции `<hrd-class name="text">`).
* **Имя HRD-стиля:** Если имя используемого HRD-стиля не указано явно, проверяется переменная окружения **COLORER_HRD**. Если переменная не задана, используется стиль с именем "default" (из файла default.hrd или другого, определённого в catalog.xml как стиль по умолчанию).
* **Путь к hrcsettings.xml:** Если путь к файлу hrcsettings.xml (для пользовательских настроек прототипов) не указан явно, проверяется переменная окружения **COLORER_HRC_SETTINGS**. Если она не установлена, работа продолжается без загрузки дополнительных пользовательских настроек из этого файла.
  
### Переменные окружения для конфигурации

Colorer Library может использовать следующие переменные окружения для определения путей к конфигурационным файлам и выбора настроек по умолчанию. Они обычно проверяются, если соответствующие параметры не были переданы приложению или утилите явно:

* **COLORER_CATALOG**: Полный путь к файлу catalog.xml. Используется, если путь не задан через параметры командной строки или API.
* **COLORER_HRD**: Имя используемого HRD-стиля раскраски (например, "default", "eclipse"). Используется, если стиль не задан явно.
* **COLORER_HRC_SETTINGS**: Полный путь к файлу hrcsettings.xml. Используется, если путь не задан явно.
