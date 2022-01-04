# Решения заданий {-}

## Задания к первому уроку {-}

1. Выберите все столбцы, которые заканчиваются на `Width`.

```r
library(dplyr)
#> 
#> Присоединяю пакет: 'dplyr'
#> Следующие объекты скрыты от 'package:stats':
#> 
#>     filter, lag
#> Следующие объекты скрыты от 'package:base':
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
