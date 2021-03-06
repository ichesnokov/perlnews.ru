---
tags: Perl
author: perlnews
title: Выпущен Perl 5.24.0
---

9 мая 2016 года была выпущена новая стабильная версия языка программирования
Perl 5.24.0. Разработка велась примерно 11 месяцев, начиная с Perl 5.22.0, и
содержит примерно 360 000 изменённых строк среди 1800 файлов от 77 авторов.

---

Основные изменения, которые появились в новом релизе:

* Постфиксное разыменование больше не является экспериментальным. Удалена
  поддержка авторазыменования.
* Поддержка стандарта Юникод 8.0
* Убрана поддержка лексической переменной `my $_`
* Вложенное объявление переменных теперь запрещено, например, запись `my ($x,
  my($y));` вызовет фатальную ошибку
* Оптимизирована процедура входа и выхода в/из область видимости, что ускоряет
  вызов функций, циклов и блоков кода
* Прирост в скорости в регулярных выражениях с фиксированной строкой поиска за
  счёт использования соответствующей аппаратной поддержки
* Ускорены операции сложения, вычитания и умножения
* Снижено пиковое потребление памяти при компиляции регулярных выражений
* Исправлено множество ошибок

О других изменениях можно узнать в документе
[perldelta](https://metacpan.org/pod/release/RJBS/perl-5.24.0/pod/perldelta.pod).
