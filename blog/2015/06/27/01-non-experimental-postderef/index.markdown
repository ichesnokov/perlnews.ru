---
tags: Perl
author: perlnews
title: Постфиксное разыменование больше не экспериментальное
---

Принят
[патч](http://perl5.git.perl.org/perl.git/commitdiff/2ad792cd4e9684519736fe03fd28a706b71ceda3),
который убирает статус экспериментальной у возможности постфиксного
разыменования. Напомним, что постфиксное разыменование впервые появилось в
Perl 5.20 и позволяет использовать новый синтаксис для разыменования:

    $array_ref->@*; # вместо @{ $array_ref }
    $hash_ref->%*;  # вместо %{ $hash_ref }
    ...

С выходом Perl 5.24, больше не потребуется использовать конструкцию

    no warnings "experimental::postderef";

чтобы подавлять предупреждение, это выражение станет пустой операцией. Для
активирования возможности можно по-прежнему использовать выражение

    use feature "postderef", "postderef_qq";

или просто

    use v5.24;
