# Решения заданий {-}

## Задания к первому уроку {-}

1. Выберите все столбцы, которые заканчиваются на `Width`.

```r
library(dplyr)
#> 
#> Attaching package: 'dplyr'
#> The following objects are masked from 'package:stats':
#> 
#>     filter, lag
#> The following objects are masked from 'package:base':
#> 
#>     intersect, setdiff, setequal, union

select(iris, ends_with('Width')) %>% 
  tibble()
#> # A tibble: 150 x 2
#>    Sepal.Width Petal.Width
#>          <dbl>       <dbl>
#>  1         3.5         0.2
#>  2         3           0.2
#>  3         3.2         0.2
#>  4         3.1         0.2
#>  5         3.6         0.2
#>  6         3.9         0.4
#>  7         3.4         0.3
#>  8         3.4         0.2
#>  9         2.9         0.2
#> 10         3.1         0.1
#> # ... with 140 more rows
#> # i Use `print(n = ...)` to see more rows
```

2. Переместите с помощью функции `relocate()` единственный текстовый столбец в левую часть таблицы.

```r
relocate(iris, where(is.factor)) %>% 
  tibble()
#> # A tibble: 150 x 5
#>    Species Sepal.Length Sepal.Width Petal.Length Petal.Width
#>    <fct>          <dbl>       <dbl>        <dbl>       <dbl>
#>  1 setosa           5.1         3.5          1.4         0.2
#>  2 setosa           4.9         3            1.4         0.2
#>  3 setosa           4.7         3.2          1.3         0.2
#>  4 setosa           4.6         3.1          1.5         0.2
#>  5 setosa           5           3.6          1.4         0.2
#>  6 setosa           5.4         3.9          1.7         0.4
#>  7 setosa           4.6         3.4          1.4         0.3
#>  8 setosa           5           3.4          1.5         0.2
#>  9 setosa           4.4         2.9          1.4         0.2
#> 10 setosa           4.9         3.1          1.5         0.1
#> # ... with 140 more rows
#> # i Use `print(n = ...)` to see more rows
```

3. Замените с помощью функции `rename_with()` в названии столбцов точку на нижнее подчёркивание, и преобразуйте имена в нижний регистр.

```r
renamer <- function(x) gsub('\\.', '\\_', x) %>% tolower()
rename_with(iris, renamer) %>% 
  tibble()
#> # A tibble: 150 x 5
#>    sepal_length sepal_width petal_length petal_width species
#>           <dbl>       <dbl>        <dbl>       <dbl> <fct>  
#>  1          5.1         3.5          1.4         0.2 setosa 
#>  2          4.9         3            1.4         0.2 setosa 
#>  3          4.7         3.2          1.3         0.2 setosa 
#>  4          4.6         3.1          1.5         0.2 setosa 
#>  5          5           3.6          1.4         0.2 setosa 
#>  6          5.4         3.9          1.7         0.4 setosa 
#>  7          4.6         3.4          1.4         0.3 setosa 
#>  8          5           3.4          1.5         0.2 setosa 
#>  9          4.4         2.9          1.4         0.2 setosa 
#> 10          4.9         3.1          1.5         0.1 setosa 
#> # ... with 140 more rows
#> # i Use `print(n = ...)` to see more rows
```

## Задания ко второму уроку {-}

1. Используйте функцию `across()`, и разделите значения полей имена которых заканчиваются на `Length` на среднее значение по этим же столбцам.

```r
library(dplyr)

mutate(iris, across(ends_with('Length'), ~ . / mean(.))) %>% 
  tibble()
#> # A tibble: 150 x 5
#>    Sepal.Length Sepal.Width Petal.Length Petal.Width Species
#>           <dbl>       <dbl>        <dbl>       <dbl> <fct>  
#>  1        0.873         3.5        0.373         0.2 setosa 
#>  2        0.839         3          0.373         0.2 setosa 
#>  3        0.804         3.2        0.346         0.2 setosa 
#>  4        0.787         3.1        0.399         0.2 setosa 
#>  5        0.856         3.6        0.373         0.2 setosa 
#>  6        0.924         3.9        0.452         0.4 setosa 
#>  7        0.787         3.4        0.373         0.3 setosa 
#>  8        0.856         3.4        0.399         0.2 setosa 
#>  9        0.753         2.9        0.373         0.2 setosa 
#> 10        0.839         3.1        0.399         0.1 setosa 
#> # ... with 140 more rows
#> # i Use `print(n = ...)` to see more rows
```

2. Посчитайте среднее значение столбцов, имена которых начинаются на `Sepal` сгруппировав данные по столбцу `Species`.

```r
group_by(iris, Species) %>% 
  summarise(across(starts_with('Sepal'), mean))
#> # A tibble: 3 x 3
#>   Species    Sepal.Length Sepal.Width
#>   <fct>             <dbl>       <dbl>
#> 1 setosa             5.01        3.43
#> 2 versicolor         5.94        2.77
#> 3 virginica          6.59        2.97
```

## Заданиe к третьему уроку {-}

1. Ваша задача не переворачивая таблицу, добавить в неё 4 столбца:

* winter_avg_sales - средний объём продаж за зимные месяца;
* spring_avg_sales - средний объём продаж за весенние месяца;
* summer_avg_sales - средний объём продаж за летние месяца;
* autumn_avg_sales - средний объём продаж за осенние месяца;

И оставить из исходной таблицы только столбец с обозначением года, и рассчитанные на предыдущем шаге столбцы.



Решение:

```r
library(dplyr)

rowwise(sales) %>% 
  mutate(
    winter_avg_sales = mean(Dec, Jan, Feb),
    spring_avg_sales = mean(c_across(Mar:May)),
    summer_avg_sales = mean(c_across(Jun:Aug)),
    autumn_avg_sales = mean(c_across(Sep:Nov))
  ) %>% 
  select(year, matches('avg'))
#> # A tibble: 6 x 5
#> # Rowwise: 
#>    year winter_avg_sales spring_avg_sales summer_a~1 autum~2
#>   <int>            <dbl>            <dbl>      <dbl>   <dbl>
#> 1  2000              297             215.       243     276 
#> 2  2001              263             248.       272     225.
#> 3  2002              187             241.       189     289 
#> 4  2003              234             309        305.    206.
#> 5  2004              183             220        301.    290.
#> 6  2005              273             273        275.    252.
#> # ... with abbreviated variable names 1: summer_avg_sales,
#> #   2: autumn_avg_sales
```

## Заданиe к четвёртому уроку {-}

1. Сгенерируйте согласно этим параметрам таблицу содержащую в столбце `sim` номер строки таблицы параметров, а в столбце `val` сами значения случайных распределений. Для воспроизводимости результатов установите счётчик генерации случайных чисел в позиции 400 (`set.seed(400)`). Тогда итоговый результат будет иметь следующий вид:



Решение:

```r
library(dplyr)
set.seed(400)

params %>%
   rowwise(sim) %>%
   summarise(val = rnorm(n, mean, sd))
#> `summarise()` has grouped output by 'sim'. You can override
#> using the `.groups` argument.
#> # A tibble: 21 x 2
#> # Groups:   sim [3]
#>      sim    val
#>    <dbl>  <dbl>
#>  1     1  -4.18
#>  2     1   4.08
#>  3     1   8.36
#>  4     1  -2.41
#>  5     2  -4.02
#>  6     2 -11.5 
#>  7     2  10.6 
#>  8     2   9.20
#>  9     2   3.08
#> 10     2  -3.75
#> # ... with 11 more rows
#> # i Use `print(n = ...)` to see more rows
```

## Заданиe к пятому уроку {-}

1. Ваша задача расчитать фактическую запрлату каждого сотрудника по формуле `total = rate * time_rate + bonus - penalty`.



Решение:

```r
library(dplyr)

rows_update(salary, bonus, by = 'employee_id') %>% 
  rows_update(penalty, by = 'employee_id') %>% 
  rows_insert(new, by = 'employee_id') %>% 
  left_join(time_rate, by = 'employee_id') %>% 
  mutate(total = rate * time_rate + bonus - penalty)
#> # A tibble: 6 x 6
#>   employee_id  rate bonus penalty time_rate total
#>         <int> <dbl> <dbl>   <dbl>     <dbl> <dbl>
#> 1           1  1000     0     150       1     850
#> 2           2  1200     0       0       1    1200
#> 3           3   700   100       0       1     800
#> 4           4  1500     0     320       0.8   880
#> 5           5  2000   500      80       1    2420
#> 6           6   500     0       0       0.5   250
```
