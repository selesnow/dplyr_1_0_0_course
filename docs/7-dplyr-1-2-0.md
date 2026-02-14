# dplyr 1.2.0: filter_out(), when_any(), recode_values(), replace_values()

## Описание
Этот урок посвящён релизу dplyr 1.2.0, в котором основной фокус сделан на удобную и выразительную работу с заменой и исключением значений.

В релизе появились следующие нововведения:

1. Добавлена функция `filter_out()`, которая является семантически более читаемой альтернативой `filter(!...)` и упрощает исключение строк из данных.
2. Добавлена функция `recode_values()`, позволяющая декларативно и безопасно перекодировать значения в векторах.
3. Добавлена функция `replace_values()`, предназначенная для массовой замены значений без изменения длины объекта.
4. Добавлена функция `replace_when()`, которая расширяет идею `case_when()` и делает условную замену значений более выразительной и читаемой.

В основе урока лежит статья ["dplyr 1.2.0"](https://tidyverse.org/blog/2026/02/dplyr-1-2-0/).

## Видео
<iframe width="560" height="315" src="https://www.youtube.com/embed/2GXYCZKKYDA?enablejsapi=1" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Код

``` r
library(dplyr)


# filter_out --------------------------------------------------------------
patients <- tibble(
  name = c("Anne", "Mark", "Sarah", "Davis", "Max", "Derek", "Tina"),
  deceased = c(FALSE, TRUE, NA, TRUE, NA, FALSE, TRUE),
  date = c(2005, 2010, NA, 2020, 2010, NA, NA)
)

patients

# Отфильтруйте строки, в которых пациент умер , и год смерти был до 2012 года.
patients |>
  filter(!(deceased & date < 2012))

# проверка
anti_join(
  patients,
  patients |> filter(!(deceased & date < 2012)),
  join_by(name, deceased, date)
)

# правильный вариант через filter
patients |>
  filter(
    !((deceased & !is.na(deceased)) &
        (date < 2012 & !is.na(date)))
  )

# вариант с filter out
patients |>
  filter_out(deceased, date < 2012)

# filter - позволяет указать какие строки надо сохранить
# filter_out - позволяет указать какие строки надо убрать из таблицы


# when_any() --------------------------------------------------------------

countries <- tibble(
  name = c("US", "CA", "PR", "RU", "US", NA, "CA", "PR"),
  score = c(200, 100, 150, NA, 50, 100, 300, 250)
)

countries

# Отфильтруйте строки, где значения «US» и «CA» находятся в диапазоне от 200 до 300, 
# или строки, где значения «PR» и «RU» находятся в диапазоне от 100 до 200.
 
# решение через фильтр filter()
countries |>
  filter(
    (name %in% c("US", "CA") & between(score, 200, 300)) |
      (name %in% c("PR", "RU") & between(score, 100, 200))
  )

countries |>
  filter(when_any(
    name %in% c("US", "CA") & between(score, 200, 300),
    name %in% c("PR", "RU") & between(score, 100, 200)
  ))


# recode_values()  --------------------------------------------------------

likert <- tibble(
  score = c(1, 2, 3, 4, 5, 2, 3, 1, 4)
)

# Задача реализовать оценки по шкале Ликерта
# решение через case_when()
likert |>
  mutate(
    category = case_when(
      score == 1 ~ "Strongly disagree",
      score == 2 ~ "Disagree",
      score == 3 ~ "Neutral",
      score == 4 ~ "Agree",
      score == 5 ~ "Strongly agree"
    )
  )

# решение через recode_values()
likert |>
  mutate(
    category = score |>
      recode_values(
        1 ~ "Strongly disagree",
        2 ~ "Disagree",
        3 ~ "Neutral",
        4 ~ "Agree",
        5 ~ "Strongly agree"
      )
  )

# решение через создание таблицы соответствий
lookup <- tribble(
  ~from , ~to             ,
  1 , "Strongly disagree" ,
  2 , "Disagree"          ,
  3 , "Neutral"           ,
  4 , "Agree"             ,
  5 , "Strongly agree"    ,
)

likert |>
  mutate(category = recode_values(score, from = lookup$from, to = lookup$to))

# обработка не описанных для перекодирования значений
# аргумент unmatched
likert <- tibble(
  score = c(0, 1, 2, 2, 4, 5, 2, 3, 1, 4)
)

likert |>
  mutate(
    score = score |>
      recode_values(
        from = lookup$from,
        to = lookup$to,
        unmatched = "default", default = 'UNK'
      )
  )


# replace_values()  -------------------------------------------------------

# Задача объединить некоторые, но не все, названия этих школ в общие категории
schools <- tibble(
  name = c(
    "UNC",
    "Chapel Hill",
    NA,
    "Duke",
    "Duke University",
    "UNC",
    "NC State",
    "ECU"
  )
)

# решение через case_when() или recode_values():
schools |>
  mutate(
    name = case_when(
      name %in% c("UNC", "Chapel Hill") ~ "UNC Chapel Hill",
      name %in% c("Duke", "Duke University") ~ "Duke",
      .default = name
    )
  )

schools |>
  mutate(
    name = recode_values(
      name,
      c("UNC", "Chapel Hill") ~ "UNC Chapel Hill",
      c("Duke", "Duke University") ~ "Duke",
      default = name
    )
  )

# решение через replace_values
schools |>
  mutate(
    name = name |>
      replace_values(
        c("UNC", "Chapel Hill") ~ "UNC Chapel Hill",
        c("Duke", "Duke University") ~ "Duke"
      )
  )

# решение через replace_values + таблица сопостовления
lookup <- tribble(
  ~from             , ~to               ,
  "UNC"             , "UNC Chapel Hill" ,
  "Chapel Hill"     , "UNC Chapel Hill" ,
  "Duke"            , "Duke"            ,
  "Duke University" , "Duke"            ,
)

schools |>
  mutate(name = replace_values(name, from = lookup$from, to = lookup$to))

# ещё примеры использования  replace_values()
state <- c("NC", "NY", "CA", NA, "NY", "Unknown", NA)

## замена пропущенных значений константой, аналог tidyr::replace_na
replace_values(state, NA ~ "Unknown")

## замена пропущенных значений данными из другого столбца
region <- c("South", "North", "West", "East", "North", "Unknown", "West")
replace_values(state, NA ~ region)

## Замена проблемных значений на NA
replace_values(state, "Unknown" ~ NA)

## Замена разных проблемных значений в одно действие
replace_values(state, c(NA, "Unknown", "-") ~ "<missing>")

# пример устаревщих реализаций
if_else(is.na(state), "Unknown", state)
case_when(is.na(state) ~ "Unknown", .default = state)

```

## Упражнения
1. **Проблема двусмысленности:** В чем заключается главная претензия авторов к функции `filter()` при попытке «отфильтровать вон» (drop) определенные строки?

2. **Обработка пропусков:** Каким образом `filter_out()` обрабатывает значения `NA` и почему это безопаснее, чем использовать `filter(!условие)`?

3. **Логика OR:** Какую синтаксическую проблему решает функция `when_any()` внутри `filter()`?

4. **Типизация:** В чем концептуальное различие между функциями `recode_values()` (Recoding) и `replace_values()` (Replacing) с точки зрения типа выходных данных?

5. **Защитное программирование:** Зачем в функциях перекодировки использовать аргумент `unmatched = "error"`?
