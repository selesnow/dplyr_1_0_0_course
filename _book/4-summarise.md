# Обновлённая функция summarise()

## Описание
В этом уроке мы рассмотрим новые возможности функции `summarise()`.

Урок основан на статье ["dplyr 1.0.0: new summarise() features"](https://www.tidyverse.org/blog/2020/03/dplyr-1-0-0-summarise/).

## Видео
<iframe width="560" height="315" src="https://www.youtube.com/embed/4RNBDDui6Yw" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Код

```r
#devtools::install_github("tidyverse/dplyr")
library(dplyr)

# Основные изменения
## теперь sunnarise может вернуть
### вектор любой длинны
### дата фрейм любой размерности

# #######################################################
# тестовые данные
# #######################################################
df <- tibble(
  grp = rep(1:2, each = 5), 
  x = c(rnorm(5, -0.25, 1), rnorm(5, 0, 1.5)),
  y = c(rnorm(5, 0.25, 1), rnorm(5, 0, 0.5)),
)

df

# получим минимальные и максимальные значения для каждой группы
# и поместим эти значения в строки
df %>% 
  group_by(grp) %>% 
  summarise(rng = range(x))

## функция range возвращает вектор длинны 2
range(df$x)
## но функция summarise разворачивает его, 
## приводя каждое из возвращаемых значений в новую строку

# тоже самое, но для столбцов
df %>% 
  group_by(grp) %>% 
  summarise(tibble(min = min(x), mean = mean(x)))

# #######################################################
# Расчёт квантилей
# #######################################################
df %>% 
  group_by(grp) %>% 
  summarise(x = quantile(x, c(0.25, 0.5, 0.75)), q = c(0.25, 0.5, 0.75))

# можем избежать дублирования кода и написать функцию для вычисления квантиля
quibble <- function(x, q = c(0.25, 0.5, 0.75)) {
  tibble(x = quantile(x, q), q = q)
}
# используем собственную функцию в summarise
df %>% 
  group_by(grp) %>% 
  summarise(quibble(x, c(0.25, 0.5, 0.75)))

# доработаем функцию таким образом 
# что бы названия столбца подтягивались из аргумена
quibble2 <- function(x, q = c(0.25, 0.5, 0.75)) {
  tibble("{{ x }}" := quantile(x, q), "{{ x }}_q" := q)
}

df %>% 
  group_by(grp) %>% 
  summarise(quibble2(x, c(0.25, 0.5, 0.75)))


# мы не присваивали имена новых столбцов внутри summarise
# потому что если функция возвращает объект сложной стурктуры
# мы получим вложенные дата фреймы
out <- df %>% 
  group_by(grp) %>% 
  summarise(quantile = quibble2(y, c(0.25, 0.75)))

str(out)

# обращаемся к вложенному фрейму
out$y

# или к его столбцам
# по смыслу такая конструкция напоминает объяденённые имена стобцов в электронных таблицах
out$quantile$y_q

# summarise + rowwise
# эта комбинация функций теперь может заменить purrr и apply
tibble(path = dir(pattern = "\\.csv$")) %>% 
  rowwise(path) %>% 
  summarise(readr::read_csv(path))

# #######################################################
# Предыдущие подходы
# #######################################################
# вычисляем квантили
df %>% 
  group_by(grp) %>% 
  summarise(y = list(quibble(y, c(0.25, 0.75)))) %>% 
  tidyr::unnest(y)

df %>% 
  group_by(grp) %>% 
  do(quibble(.$y, c(0.25, 0.75)))
```