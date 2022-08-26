--- 
title: "Введение в dplyr 1.0.0"
author: "Алексей Селезнёв"
date: "2022-08-26"
site: bookdown::bookdown_site
documentclass: book
url: https://github.com/selesnow/dplyr_1_0_0_course
cover-image: img/cover.png
description: |
  Серия видео уроков посвящённая обновлению dplyr 1.0.0..
biblio-style: apalike
csl: chicago-fullnote-bibliography.csl
---

# Введение {-}

------

## О курсе {-}
<a href="https://selesnow.github.io"><img src="img/cover.png" align="right" alt="Cover image" class="cover" width="230" height="366" /></a>`dplyr` - один из наиболее популярных пакетов, который реализует грамматику манипуляции данными в языке R. Официально первый релиз `dplyr` был выпещен в 2014 году, но пакет развивался, и первая стабильная версия с устоявшимся синтаксисом, под номером 1.0.0 была выпущена весной 2020 года. Этому релизу предшествовала серия статей, в которых Хедли Викхем описывал все нововведения в `dplyr`. Этот мини курс появился из 5 видео уроков снятых по этим статьям.

В основе видео уроков лежат следующие статьи:

* [dplyr 1.0.0: select, rename, relocate](https://www.tidyverse.org/blog/2020/03/dplyr-1-0-0-select-rename-relocate/)
* [dplyr 1.0.0: working across columns](https://www.tidyverse.org/blog/2020/04/dplyr-1-0-0-colwise/)
* [dplyr 1.0.0: working within rows](https://www.tidyverse.org/blog/2020/04/dplyr-1-0-0-rowwise/)
* [dplyr 1.0.0: new summarise() features](https://www.tidyverse.org/blog/2020/03/dplyr-1-0-0-summarise/)
* [dplyr 1.0.0: last minute additions](https://www.tidyverse.org/blog/2020/05/dplyr-1-0-0-last-minute-additions/)

## Для кого этот курс {-}
Для прохождения курса вы уже должны иметь навыки работы с инфраструктурой `tidyverse`. Приступать к прохождению курса "Введение в dplyr 1.0.0" я советую тем, кто уже имеет базовые навыки работы в R. Т.е. изначально я рекомендую вам пройти курс ["Язык R для пользователей Excel"](https://selesnow.github.io/r4excel_users/), и потом приступать к прохождению данного курса.

## Рекомендации по прохождению курса {-}
Данный курс состоит из 5 видео уроков общей длительность 1 час 2 минуты. К каждому уроку есть рассмотренный в видео код, это сделано для вашего удобства, скопируйте его и выполняйте по мере просмотра видео лекции. 

К каждому уроке есть упражнения, их выполнение не является обязательным, но поможет вам понять усвоили ли вы материал урока. Все упражнения достаточно простые, и зачастую не заберут у вас более 5 - 10 минут. Решение каждого упражнения можно найти в разделе ["Решение заданий"](решения-заданий.html).

## Об авторе {-}
Меня зовут Алексей Селезнёв, с 2008 года я являюсь практикующим аналитиком. На данный момент основной моей деятельностью является развитие отдела аналитики в агентстве интернет-маркетинга Netpeak.
<a href="https://selesnow.github.io"><img src="img/author.png" width="200" height="200" align="left" alt="Алексей Селезнёв" hspace="20" vspace="7" /></a>

Мною были разработаны такие R пакеты как: `ryandexdirect`, `rfacebookstat`, `timeperiodsR`, `rvkstat` и некоторые другие. На данный момент написанные мной пакеты только с CRAN были установленны более 130 000 раз.

Также я являюсь автором курса ["Язык R для интернет-маркетинга"](https://needfordata.ru/r).

Веду свой авторский [Telegram](https://t.me/R4marketing) и [YouTube](https://www.youtube.com/R4marketing/?sub_confirmation=1) канал R4marketing. Буду рад видеть вас в рядах подписчиков.

Периодически публикую статью на различных интернет медиа, зачастую это [Хабр](https://habr.com/ru/users/selesnow/) и [Netpeak Journal](https://netpeak.net/ru/blog/user/publication/826/).

Неоднократно выступал на профильных конференциях по аналитике и интернет маркетингу, среди которых Матемаркетинг, GoAnalytics, Analyze, eCommerce, 8P и прочие.

Начиная с 2016 года всячески стараюсь популяризировать язык R среди русскоязычных аналитиков и маркетологов. Этот курс также был создан с этой целью.

## Программа курса {-}

1. [Функции select(), rename_with() и relocate()](функции-select-rename_with-и-relocate.html)
2. [Функция across()](функция-across.html)
3. [Перебор строк функцией rowwise()](перебор-строк-функцией-rowwise.html)
4. [Обновлённая функция summarise()](обновлённая-функция-summarise.html)
5. [Добавление, изменение и удаление строк дата фрейма через rows_*()](добавление-изменение-и-удаление-строк-дата-фрейма-через-rows_.html)

## Благодарности {-}
Курс, и все сопутствующие материалы предоставляются бесплатно, но если у вас есть желание отблагодарить автора за этот видео курс вы можете перечислить любую произвольную сумму на [этой странице](https://secure.wayforpay.com/payment/r4excel_users).

Либо с помощью кнопки:
<center>
<script type="text/javascript" id="widget-wfp-script" src="https://secure.wayforpay.com/server/pay-widget.js?ref=button"></script> <script type="text/javascript">function runWfpWdgt(url){var wayforpay=new Wayforpay();wayforpay.invoice(url);}</script> <button type="button" onclick="runWfpWdgt('https://secure.wayforpay.com/button/b9c8a14345975');" style="display:inline-block!important;background:#2B3160 url('https://s3.eu-central-1.amazonaws.com/w4p-merch/button/bg2x2.png') no-repeat center right;background-size:cover;width: 256px!important;height:54px!important;border:none!important;border-radius:14px!important;padding:18px!important;box-shadow:3px 2px 8px rgba(71,66,66,0.22)!important;text-align:left!important;box-sizing:border-box!important;" onmouseover="this.style.opacity='0.8';" onmouseout="this.style.opacity='1';"><span style="font-family:Verdana,Arial,sans-serif!important;font-weight:bold!important;font-size:14px!important;color:#ffffff!important;line-height:18px!important;vertical-align:middle!important;">Оплатить</span></button>
</center>
