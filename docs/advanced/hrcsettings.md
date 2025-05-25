# hrcsettings.xml

hrcsettings - это конфигурационный файл, предназначенный для расширения настроек прототипов, определённых в базовой библиотеке HRC-схем, а также для хранения пользовательских модификаций этих настроек.

## Расширение настроек

Прототипы, описанные в базовой библиотеке HRC-схем, содержат параметры, влияющие на их поведение или используемые ядром Colorer. Однако, приложениям, построенным на основе Colorer-library (например, FarColorer), могут требоваться дополнительные параметры для прототипов, отсутствующие в базовой библиотеке. Также, может возникнуть необходимость переопределить значения по умолчанию для существующих параметров.
Для этих целей используется файл hrcsettings.xml, формат которого аналогичен описанию прототипов в стандартных HRC-файлах, что позволяет централизованно управлять такими расширениями.

=== "Формат"

    ```xml
    <?xml version="1.0" encoding='UTF-8'?>
    <hrc-settings>
        <prototype name="">
            <parameters>
                <param name="" value="" description=""/>
            </parameters>
            <param name="" value="" description=""/>
        </prototype>
    </hrc-settings>
    ```

=== "Пример"

    ```xml
    <?xml version="1.0" encoding='UTF-8'?>
    <hrc-settings>
        <prototype name="default">
            <param name="show-cross" value="none"  description="Visibility of the cross (horizontal, vertical, both, none)"/>
            <param name="cross-zorder" value="bottom" description="Position of the cross, which points out cursor position"/>
            <param name="maxlinelength" value="5000" description="Maximum parsed length of line of the text"/>
            <param name="backparse" value="6000" description="Number of lines, after which parser stops continous analysis. Infinite, if zero."/>
            <param name="fullback" value="yes" description="If yes, draws background in inlined languages till end of the screen"/>
            <param name="default-fore" value="" description="User-defined foreground color for this particular type"/>
            <param name="default-back" value="" description="User-defined foreground color for this particular type"/>
            <param name="hotkey" value="" description="Key is assigned in the menu select the file type, for this type"/>
            <param name="favorite" value="false" description="If true then the prototype is displayed in a group of favorites"/>
            <param name="maxblocksize" value="300" description="Maximum length of regexp block in text"/>
            <param name="disabled" value="false" description="Disable coloring for the prototype. See 'use-default'."/>
            <param name="use-default" value="false" description="Use the default type if this prototype is disabled"/>
        </prototype>
    </hrc-settings>
    ```

Элементы `<param>` могут располагаться как непосредственно внутри элемента `<prototype>`, так и в дочернем блоке `<parameters>`. Рекомендуется использовать вложенный блок `<parameters>`, следуя структуре стандартного описания прототипа (например, в основных HRC-файлах). Прямое вложение `<param>` в `<prototype>` поддерживается для обратной совместимости.

Формат файла позволяет определять и другие элементы конфигурации прототипа (например, условия сопоставления файлов), о чем будет рассказано в следующем разделе. Однако, если основная задача — только расширение набора параметров или изменение их значений по умолчанию, эти дополнительные элементы обычно не используются.

## Хранение пользовательских настроек

Для приложений, не имеющих собственной системы хранения конфигурации (например, consoletools), файл hrcsettings.xml может служить местом для сохранения пользовательских настроек прототипов. Это позволяет пользователям модифицировать и сохранять не только значения параметров прототипа (элементы `<param>`), но и условия его выбора для конкретных файлов (элементы `<filename>` и `<firstline>`).

=== "Формат"

    ```xml
    <?xml version="1.0" encoding='UTF-8'?>
    <hrc-settings>
        <prototype name="">
            <filename weight=""></filename>
            <firstline weight=""></firstline>
            <parameters>
                <param name="" value="" description=""/>
            </parameters>
            <param name="" value="" description=""/>
        </prototype>
    </hrc-settings>
    ```


## Порядок применения настроек из hrcsettings.xml

При загрузке файла hrcsettings.xml, для каждого прототипа, упомянутого в нем, происходит следующее:

- **Добавление новых параметров:** Если параметр указан в hrcsettings.xml, но отсутствует в ранее загруженных описаниях прототипа (например, из базовой HRC-библиотеки), он добавляется к прототипу.
- **Обновление существующих параметров:** Если параметр уже существует, его значение по умолчанию (атрибут value) и описание (атрибут description) обновляются согласно значениям из hrcsettings.xml.
- **Замена условий выбора прототипа:** Условия сопоставления прототипа с файлами (определяемые элементами `<filename>` и `<firstline>`) полностью заменяются теми, что указаны в hrcsettings.xml. Предыдущие условия выбора для данного прототипа, если они были, игнорируются.

