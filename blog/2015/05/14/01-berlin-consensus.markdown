---
last\_modified: 2015-05-14 04:15:55
tags: CPAN
author: perlnews
title: Берлинский консенсус
---
На днях был опубликован [Берлинский
консенсус](https://github.com/Perl-Toolchain-Gang/toolchain-site/blob/master/berlin-consensus.md)
— финальная договорённость по результатам прошедшего [Берлинского
QA-хакатона](http://perlnews.ru/blog/2015/04/21/02-qa-hackaton-berlin-2015.html).
Данный документ описывает новые соглашения и идеи для репозитория CPAN и
тулчейна Perl, а также новые рекомендации для CPAN-авторов. Рассмотрим более
подробно концепции и тезисы Берлинского консенсуса.

---

## Поддержание CPAN модулей в целостном и рабочем состоянии

Приводится аналогия CPAN как реки. Модули, от которых не зависит ни один модуль
на CPAN, находятся около устья реки. Если от модуля начинают зависеть другие
модули, он поднимается вверх по течению реки. Чем больше таких обратных
зависимостей у модуля, тем выше он поднимается вверх по реке. Здесь учитывается
не только явные обратные зависимости, а также обратные зависимости, которые
есть у зависимых модулей. Рассчитав полное число таких зависимостей, можно
выяснить насколько высоко модуль поднимается выше по течению реки.

Если модуль ломается (сборка, тесты, обратная совместимость, и т.д.), то он
ломает и все зависимые модули, т.е. визуально загрязняет реку вниз по течению.
Поэтому, чем больше модуль используется, тем ответственней требуется подходить
к его обновлению и выпуску новых версий.

На сегодняшний день получена примерно следующая статистика зависимостей:

* ~50 дистрибутивов с 10'000 и больше зависимых модулей,
* ~200 дистрибутивов с 1'000 — 9'999 зависимых модулей,
* ~2'000 дистрибутивов с 10 — 999 зависимых модулей,
* ~8'000 дистрибутивов с 1 — 9 зависимых модулей,
* ~16'000 дистрибутивов без известных зависимых модулей.

Выделено три группы и соответствующие рекомендации для авторов модулей,
входящих в заданные группы:

* Устье реки — ни одного или до 3-4 десятков зависимых модулей. Такие модули
  должны удовлетворять следующим правилам:
    * Соответствовать базовым правилам качества
      [Kwalitee](http://cpants.cpanauthors.org/kwalitee).
    * Документация (в идеале с проверкой на грамматические ошибки).
    * Тесты в папке `t`.
    * Лицензия.
    * Файл Changes с описанием изменений.
    * Файлы `META.json` и `META.yml` в соответствии со спецификацией
      [CPAN::Meta::Spec](https://metacpan.org/pod/CPAN::Meta::Spec).
    * Секция документации `SEE ALSO`, если модуль реализует функционал, сходный
      с существующими модулями, с описанием аналогов и отличий данного модуля
      от них.
    * При сборке не делать лишних подключений в интернет («позвонить домой»);
      не делать изменений в файловой системе за пределами каталога дистрибутива
      или каталога для временных файлов; не отправлять почту.
    * Не переопределять другие модули, перезаписывая `.pm` файлы существующих
      модулей  или другим образом нейтрализуя модули из других дистрибутивов.
    * Не запускать «авторских тестов» на системах пользователя.
    * Не быть злонамеренным.
    * Не быть сделанными кое-как.
* Середина реки — от 50 до 2-3 тысяч зависимых модулей. Такие модули должны
  удовлетворять всем правилам предыдущего уровня и дополнительно должны:
    * Поддержка стабильного API. Как можно реже делать изменения, ломающие
      обратную совместимость. Проводить процедуру постепенного устаревания
      функций.
    * Поддержка переносимости, по возможности, на всех **основных**
      архитектурах и операционных системах. Должны быть предприняты шаги по
      поддержке старых версий Perl 5.8 и 5.10 и должны иметь явно указанную
      минимальную версию Perl в метаданных.
    * Дистрибутив должен иметь публичный репозиторий.
    * Дистрибутив должен быть лицензирован с теми же условиями как и сам Perl,
      либо иметь [OSI-совместимую](http://opensource.org/licenses/) лицензию.
      Следует заметить, что GPL-only лицензии могут ограничивать насколько
      высоко дистрибутив может подняться вверх по течению реки. Кроме того,
      следует учитывать, что не во всех законодательствах действует принцип
      отказа от всех прав и передачи в общественное достояние, проще выбрать
      лицензию с максимальными передаваемыми правами, такую как CC0.
    * Автор должен реагировать на сообщения в баг-трекере.
    * Дистрибутив должен иметь второго майнтейнера, зарегистрированного в PAUSE
      или какой-то другой запасной план по передаче управления в случае, если
      основной автор не может продолжать работу над модулем.
    * Дистрибутив должен иметь качественную систему тестов с хорошим покрытием.
    * Перед релизом стабильных версий должны выпускаться релизы для
      разработчиков, не индексируемые на CPAN.
    * Проверяться результаты сборок в [матрице](http://matrix.cpantesters.org/)
      CPAN testers.
    * С осторожностью добавлять или изменять зависимости.
* Исток реки — свыше 3-4 тысяч зависимых модулей. Такие модули должны
  удовлетворять всем правилам предыдущих уровней и дополнительно должны:
    * Проводить ревью кода перед стабильными релизами.
    * Иметь публичный форум для обсуждения. Все заметные изменения должны
      обсуждаться перед реализацией.
    * По возможности должны проектироваться совместимыми со старшими версиями.
    * C и XS модули должны быть совместимы со стандартом C89, как и сам Perl.
    * Выполнять тестирование производительности перед релизом больших
      изменений.
    * Тестироваться с bleadperl.
    * Тестировать некоторые зависимые модули с новой версией, перед релизом.
    * Релиз стабильных версий должен проводиться в такое время суток и дня
      недели, чтобы автор мог быстро отреагировать (откат или фикс) на
      возможные последствия.

## Ответственность за форк

Иногда возникает необходимость в создании форка модуля или альтернативной
реализации. Перед выполнением подобных действий, желательно провести публичное
обсуждение с автором модуля, возможно проблему удастся разрешить без форка.
Если выполнение форка не избежать, то следует публично уведомить автора, в
документации модуля привести рациональные объяснения (стараясь избегать
эмоциональных замечаний или персональной неприязни). Авторам рекомендуется не
«сжигать мосты» и пытаться найти компромиссное решение. После выполнения форка
важно оценить, насколько могут быть затронуты модули вниз по реке, если они
изменят зависимости. Возможно форк привнесёт новые проблемы, которых не было в
оригинальном модуле.

## Инструменты сборки

К тулчейну, или инструментам сборки, относятся модули, которые требуются для
загрузки, сборки, тестирования и установки большой группы модулей на CPAN. По
определению, модули входящие в состав базового Perl, а также параллельно
развиваемые на CPAN (например, `ExtUtils::MakeMaker`) относятся к категории
истока реки. Модули, которые не входят в состав Perl, но часто используются в
сборке относятся к категории «тулчейна» для модулей, которые их используют.

Консенсус определяет несколько практик для тулчейна:

* Инструменты сборки должны поддерживать высокий уровень обратной
  совместимости, поддерживая Perl, начиная с 5.8.1.
* Должно быть более одного «главного» майнтейнера (независимо от прав на
  PAUSE).
* Функциональные изменения требуют согласования по крайне мере одного другого
  майнтейнера.
* Значительные изменения, или изменения, приводящие к поломке обратной
  совместимости должны обсуждаться публично, например, в списке рассылки
  [cpan-workers](http://lists.perl.org/list/cpan-workers.html).
* Если дискуссия над развитием тулчейна не может достигнуть компромисса, автор
  должен быть согласен на тай-брейк, когда авторитетный специалист скажет
  последнее слово в споре. По умолчанию таким человеком является текущий
  майнтейнер Perl (pumpkin).
* Стабильная версия не может быть выпущена, пока её стабильность не проверена
  (smoke-тестирование). Исключение только в случае экстренных исправлений,
  которые должны выполняться без промедления.
* Авторы тулчейна должны быть согласны, что если основной майнтейнер отходит от
  дел, авторами выбирается новый основной майнтейнер. Администраторы PAUSE
  выделяют необходимые права по результатам обсуждения.

## Спецификация META

Обсуждались изменения в спецификации [CPAN::Meta::Spec](https://metacpan.org/pod/CPAN::Meta::Spec).

* Необходимости в выпуске 3-й версии спецификации нет.
* Секция `prereqs` может содержать поля, которые не могут быть разрешены как
  зависимости. Например, `perl`, `Config`, `Errno`. Особенно важно, что ни один
  CPAN-клиент не пытался установить perl как зависимость.
* Уточнение спецификации: `recommends` — это зависимости, которые опциональны,
  но устанавливаются по умолчанию, а `suggests` — это зависимости, которые
  также опциональны, но не устанавливаются по умолчанию.
* Ключ `x_breaks` теперь должен обрабатываться всеми CPAN-клиентами, который
  указывает какие версии каких модулей окажутся сломанными из-за установки
  данного модуля. CPAN-клиент должен предпринять принудительно обновление
  указанных модулей до установки модуля, если они установлены в системе.
* Иногда два модуля могут указывать друг друга в `recommends`, что приводит к
  циклической зависимости. Планируется добавить новый ключ в META, который бы
  инструктировал устанавливать зависимость после установки самого модуля.

## Только Perl

Планируется выделить специальную переменную окружения и реализовать её
использование в `ExtUtils::CBuilder`, которая бы указывала для модулей,
проверяющие наличие компилятора в системе, что компилятора нет. Это позволит
собирать модули, поддерживающие несколько вариантов сборки, только в варианте с
чистым Perl.

## CPAN тестирование

По историческим причинам отчёты CPAN Testers, в случае если не удалось
установить нужные зависимости не формируются и не рассылаются. Предложено, что
подобные отчёты будут формироваться, но их статус будет отличен от `FAIL`.

## PAUSE

Исторически сложилось, что любой желающий мог получить управление заброшенным
модулем, отправив сообщение администраторам PAUSE с просьбой получения прав.
Теперь, в свете концепции реки CPAN, модули с большим числом зависимых модулей
уже довольно опасно отдавать первому встречному. Поэтому принято решение, что
теперь для таких модулей не будет процедуры передачи прав. Дистрибутив, который
лишился майнтейнера, погибает. Всем кто не хочет, чтобы дистрибутив оказался
похоронен на CPAN рекомендуется обзавестись помощником, который бы мог
продолжить сопровождение модуля.

По результатам хакатона было принято решение, что PAUSE не требуется
переписывать с нуля, достаточно того, что PAUSE теперь может запущен под Plack,
что облегчит его тестирование и дальнейшее развитие.

PAUSE начнёт рассылать предупреждения авторам, которые отправляют модули без
указания лицензии.

Также PAUSE должен начать обязательную индексацию META-файлов (META.json или
META.yml) в дистрибутивах.

Будет ускорено удаление старых тарболлов со CPAN (сейчас они хранятся 3 дня).

## Планы развития Test-Simple

Текущие проблемы `Test::Builder`:

* Проблемы с поддержкой асинхронных/параллельных процессов тестирования.
* Отсутствия точки расширения, привело к тому, что многие тестовые модули лезут
  во внутренности `Test::Builder` замещая его методы. Такие хаки приводят к
  большой хрупкости этих модулей и их зависимостей.
* Тестирование модулей тестирование основано на их TAP-выводе, что значительно
  усложняет весь процесс. Поэтому сами модули страдают от плохого тестирования.
* Привязка к формату TAP ограничивает расширение возможностей тестовой
  библиотеки.
* Текущий `Test::Builder` тяжёлый и медленный, содержащий множество
  повторяющихся фрагментов кода.

С учётом готовящейся замены в виде модуля `Test::Stream` и, принимая во
внимание риски, были приняты решения:

* Выпустить релиз для разработчиков Test-Simple.
* Подготовить документ со списком всех известных проблем.
* Пригласить людей тестировать последние сборки для разработчиков и давать свои
  отзывы.
* Обновить CPAN-дельта смокеры для тестирования модулей как на стабильной, так
  и на разрабатываемой версии Test-Simple.
* Ревью кода `$Test::Builder::Level` в плане обратной совместимости.
* Бенчмарки производительности.

