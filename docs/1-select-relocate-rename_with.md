# Функции select(), rename_with() и relocate()

## Описание
Первый урок курса посвящён таким операциям, как продвинутый выбор столбцов, их переименование и изменения порядка столбцов таблицы.

В этом видео уроке мы познакомимся с такими функциями как: `select()`, `rename_with()`, `relocate()`, `any_of()`, `all_of()`.

В основе урока лежит статья ["dplyr 1.0.0: select, rename, relocate"](https://www.tidyverse.org/blog/2020/03/dplyr-1-0-0-select-rename-relocate/).

## Видео
<iframe width="560" height="315" src="https://www.youtube.com/embed/k4v5cfLuUAQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Код

```r
#devtools::install_github("tidyverse/dplyr")
library(dplyr, warn.conflicts = FALSE)

# rename
# Переименовать столбцы для устранение дублирования их имён
df1 <- tibble(a = 1:5, a = 5:1, .name_repair = "minimal")
df1

df1 %>% rename(b = 2)

# select
# обращение к столбцам по типу
df2 <- tibble(x1 = 1, x2 = "a", x3 = 2, y1 = "b", y2 = 3, y3 = "c", y4 = 4)

# числовые столбцы
df2 %>% select(is.numeric)
# НЕ текстовые столбцы
df2 %>% select(!is.character)

# смешанный тип обращения
# числовые стобцы, название которых начинается на X
df2 %>% select(starts_with("x") & is.numeric)


# выбор полей с помощью функций any_of и all_of
vars <- c("x1", "x2", "y1", "z")
df2 %>% select(any_of(vars))

df2 %>% select(all_of(vars))


# функция rename_with
df2 %>% rename_with(toupper)

df2 %>% rename_with(toupper, starts_with("x"))

df2 %>% rename_with(toupper, is.numeric)


# relocate для изменения порядка стобцов
df3 <- tibble(w = 0, x = 1, y = "a", z = "b")
# переместить столбцы y, z в начало
df3 %>% relocate(y, z)
# переместить текстовые столбцы вначало
df3 %>% relocate(is.character)

# поместить столбец w после y
df3 %>% relocate(w, .after = y)
# поместить столбец w перед y
df3 %>% relocate(w, .before = y)
# переместить w в конец
df3 %>% relocate(w, .after = last_col())
```

## Упражнения
Для примера мы будем использовать встроенный набор данных `iris`, выполните следующие действия:

1. Выберите все столбцы, которые заканчиваются на `Width`.
2. Переместите с помощью функции `relocate()` единственный текстовый столбец в левую часть таблицы.
3. Замените с помощью функции `rename_with()` в названии столбцов точку на нижнее подчёркивание, и преобразуйте имена в нижний регистр.
