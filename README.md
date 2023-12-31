# Основные команды Git
> Автор: Vadim Shavrov (Weeller)


## Навигация
- `pwd` (от англ. _**p**rint **w**orking **d**irectory_, «показать рабочую папку») — покажи, в какой я папке;
- `ls` (от англ. _**l**i**s**t directory contents_, «отобразить содержимое директории») — покажи файлы и папки в текущей папке;
- `ls -a` — покажи также скрытые файлы и папки, названия которых начинаются с символа `.`;
- `cd first-project` (от англ. _**c**hange **d**irectory_, «сменить директорию») — перейди в папку `first-project`;
- `cd first-project/html` — перейди в папку `html`, которая находится в папке `first-project`;
- `cd ..` — перейди на уровень выше, в родительскую папку;
- `cd ~` — перейди в домашнюю директорию (`/Users/Username`);
- `cd /` — перейди в корневую директорию.


## Работа с файлами и папками

#### Создание
- `touch index.html` (англ. _touch_, «коснуться») — создай файл `index.html` в текущей папке;
- `touch index.html style.css script.js` — если нужно создать сразу несколько файлов, можно напечатать их имена в одну строку через пробел;
- `mkdir second-project` (от англ. _**m**a**k**e **dir**ectory_, «создать директорию») — создай папку с именем `second-project` в текущей папке.

#### Копирование и перемещение
- `cp file.txt ~/my-dir` (от англ. _**c**o**p**y_, «копировать») — скопируй файл в другое место;
- `mv file.txt ~/my-dir` (от англ. _**m**o**v**e_, «переместить») — перемести файл или папку в другое место.

#### Чтение
- `cat file.txt` (от англ. _con**cat**enate and print_, «объединить и распечатать») — распечатай содержимое текстового файла `file.txt`.

#### Удаление
- `rm about.html` (от англ. _**r**e**m**ove_, «удалить») — удали файл `about.html`;
- `rmdir images` (от англ. _**r**e**m**ove **dir**ectory_, «удалить директорию») — удали папку `images`;
- `rm -r second-project` (от англ. _**r**e**m**ove_, «удалить» + _**r**ecursive_, «рекурсивный») — удали папку `second-project` и всё, что она содержит.


## Полезные возможности

- Команды необязательно печатать и выполнять по очереди. Можно указать их списком — разделить двумя амперсандами (`&&`).
- У консоли есть собственная память — буфер с несколькими последними командами. По ним можно перемещаться с помощью клавиш со стрелками вверх (`↑`) и вниз (`↓`).
- Чтобы не вводить название файла или папки полностью, можно набрать первые символы имени и дважды нажать `Tab`. Если файл или папка есть в текущей директории, командная строка допишет путь сама.
Например, вы находитесь в папке `dev`. Начните вводить `cd first` и дважды нажмите `Tab`. Если папка `first-project` есть внутри `dev`, командная строка автоматически подставит её имя. Останется только нажать `Enter`.


## Хеш — идентификатор коммита

- Git преобразует информацию о коммитах с помощью алгоритма SHA-1 и для каждого из них рассчитывает уникальный идентификатор — хеш.
- Хеш — основной идентификатор коммита и позволяет узнать его автора, дату и содержимое закоммиченных файлов.
- Все хеши, а также таблицу соответствий `хеш → информация` о коммите Git хранит в папке `.git`.
- Можно вызвать не только полный лог, но и сокращённый — это делается командой `git log --oneline`.
- В сокращённом логе выводятся сокращённые хеши — их можно использовать точно так же, как и полные, поскольку команда самостоятельно подберёт оптимальное количество символов.


## Файл HEAD

- В числе прочих файлов в папке `.git` есть служебный файл `HEAD`. Он указывает на самый свежий коммит.
- Вместо хеша последнего коммита можно написать слово `HEAD` — Git вас поймёт.


## Статусы файлов в Git

- Статусом `untracked` помечается файл, о существовании которого Git знает, но не следит за изменениями в нём. Этот статус — противоположность `tracked`, в который попадают все файлы, отслеживаемые Git.
- Файл переходит в статус `staged` после выполнения git add.
- Статус `modified` означает, что файл был изменён.
- Большинство файлов в проектах «шагает» по следующему циклу: «изменён» → «добавлен в список на коммит» → «закоммичен» → «изменён» → и так далее.

#### Типичный жизненный цикл файла в Git представлен на схеме Mermaid ниже

```mermaid
graph LR;
  untracked 	-- 	"git add" 		--> staged;
  staged    	-- 	"git commit"     	--> tracked;
  modified	--	"git add"		--> staged;
  staged	--	"changes"		--> modified;
  tracked	--	"changes"		--> modified;
```

- Команда `git status` всегда подскажет, что происходит с файлом: например, он добавлен в список «на коммит» или ещё вообще не отслеживается, или изменён.
- `git status` показывает явно следующие состояния файлов: `untracked`, `staged` и `modified`.
- `git status` подсказывает, какие команды можно выполнить, чтобы поменять состояние файла.


## Стили оформления сообщений к коммитам

#### Корпоративный

- Во многих компаниях применяется Jira — система для организации проектов и задач. У каждой задачи в Jira есть идентификатор из нескольких заглавных латинских букв и номера. Например, `LGS-239` значит, что это 239-я задача в проекте **LGS** (сокращение от англ. _**log**istics_ — «логистика»).

```bash
$ git commit -m "LGS-239: Дополнить список пасхалок новыми числами"
```

#### Conventional Commits

- Стандарт **Conventional Commits** (англ. «соглашение о коммитах») отличается качественной документацией и подробной проработкой. Он подходит для репозиториев с исходным кодом программ.
- Conventional Commits предлагает такой формат коммита: `<type>: <сообщение>`. Первая часть `type` — это тип изменений. Таких типов достаточно много. Основные их них: `feat` (сокращение от англ. _feature_) — для новой функциональности;
`fix` (от англ. «исправить», «устранить») — для исправленных ошибок.

```bash
git commit -m "feat: добавить подсчёт суммы заказов за неделю"
```

#### GitHub-стиль

- GitHub можно использовать не только для хранения файлов проекта, но и для ведения списка задач (англ. _issue_) этого проекта. Если коммит «закрывает» или «решает» какую-то задачу, то в его сообщении удобно указывать ссылку на неё. Для этого в любом месте сообщения нужно указать `#<номер задачи>`. В таком случае GitHub свяжет коммит и задачу.

```bash
$ git commit -m "Исправить #334, добавить график температуры"
```

#### Примечание: инфинитив и императив

- Для сообщений на русском языке часто рекомендуют использовать инфинитивы. Например: Добавить тесты для `PipkaService`, `Исправить ошибку #123` и так далее.
- Для сообщений на английском рекомендуется использовать повелительное наклонение (англ. _imperative_). Например: `Use library mega_lib_300`, `Fix exit button` и так далее.
- Эти рекомендации сложились исторически, и им следуют многие проекты.


###### Источник: [Яндекс.Практикум](https://practicum.yandex.ru/)
###### Язык разметки Markdown: [шпаргалка по синтаксису с примерами](https://skillbox.ru/media/code/yazyk-razmetki-markdown-shpargalka-po-sintaksisu-s-primerami/)
###### Соглашение о коммитах: [Conventional Commits 1.0.0-beta.4](https://www.conventionalcommits.org/ru/v1.0.0-beta.4/)
###### Формат описания схем: [Mermaid](https://github.blog/2022-02-14-include-diagrams-markdown-files-mermaid/)
