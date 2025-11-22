# Практическая работа №3
koltsivers@yandex.ru

## Цель работы

1.  Развить практические навыки использования языка программирования R
    для обработки данных
2.  Закрепить знания базовых типов данных языка R
3.  Развить практические навыки использования функций обработки данных
    пакета `dplyr` – функции
    `select(), filter(), mutate(), arrange(), group_by()`

## Исходные данные

1.  Программное обеспечение Microsoft Windows 11 Pro
2.  RStudio Desktop
3.  Интерпретатор языка R 4.5.2

## Задание

Используя программный пакет dplyr, освоить базовые операции в языке
программирования R.

## Ход работы

1.  Проанализировать встроенные в пакет nycflights13 наборы данных с
    помощью языка R и ответить на вопросы:
    -   Сколько встроенных в пакет nycflights13 датафреймов?
    -   Сколько строк в каждом датафрейме?
    -   Сколько столбцов в каждом датафрейме?
    -   Как просмотреть примерный вид датафрейма?
    -   Сколько компаний-перевозчиков (carrier) учитывают эти наборы
        данных (представлено в наборах данных)?
    -   Сколько рейсов принял аэропорт John F Kennedy Intl в мае?
    -   Какой самый северный аэропорт?
    -   Какой аэропорт самый высокогорный (находится выше всех над
        уровнем моря)?
    -   Какие бортовые номера у самых старых самолетов?
    -   Какая средняя температура воздуха была в сентябре в аэропорту
        John FKennedy Intl (в градусах Цельсия).
    -   Самолеты какой авиакомпании совершили больше всего вылетов в
        июне?
    -   Самолеты какой авиакомпании задерживались чаще других в 2013
        году?
2.  Оценить результаты и сделать вывод

## Шаги:

### Шаг 1

#### Установим пакет nycflights13 с помощью install.packages(‘nycflights13’).

    > install.packages("nycflights13")
    WARNING: Rtools is required to build R packages but is not currently installed. Please download and install the appropriate version of Rtools before proceeding:

    https://cran.rstudio.com/bin/windows/Rtools/
    Устанавливаю пакет в ‘C:/Users/Daniil/AppData/Local/R/win-library/4.5’
    (потому что ‘lib’ не определено)
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/nycflights13_1.0.2.zip'
    Content type 'application/zip' length 4511548 bytes (4.3 MB)
    downloaded 4.3 MB

    пакет ‘nycflights13’ успешно распакован, MD5-суммы проверены

    Скачанные бинарные пакеты находятся в
      C:\Users\Daniil\AppData\Local\Temp\RtmpmsiLms\downloaded_packages

#### Подключим необходимые библиотеки

``` r
library(nycflights13)
library(dplyr)
```


    Присоединяю пакет: 'dplyr'

    Следующие объекты скрыты от 'package:stats':

        filter, lag

    Следующие объекты скрыты от 'package:base':

        intersect, setdiff, setequal, union

#### Сколько встроенных в пакет nycflights13 датафреймов?

``` r
length(data(package = "nycflights13")$results[, "Item"])
```

    [1] 5

#### Сколько строк в каждом датафрейме?

``` r
nrow(airlines)
```

    [1] 16

``` r
nrow(airports) 
```

    [1] 1458

``` r
nrow(flights)
```

    [1] 336776

``` r
nrow(planes)
```

    [1] 3322

``` r
nrow(weather)
```

    [1] 26115

#### Сколько столбцов в каждом датафрейме?

``` r
ncol(airlines)
```

    [1] 2

``` r
ncol(airports)
```

    [1] 8

``` r
ncol(flights) 
```

    [1] 19

``` r
ncol(planes)
```

    [1] 9

``` r
ncol(weather)
```

    [1] 15

#### Как просмотреть примерный вид датафрейма?

``` r
head(airlines)
```

    # A tibble: 6 × 2
      carrier name                    
      <chr>   <chr>                   
    1 9E      Endeavor Air Inc.       
    2 AA      American Airlines Inc.  
    3 AS      Alaska Airlines Inc.    
    4 B6      JetBlue Airways         
    5 DL      Delta Air Lines Inc.    
    6 EV      ExpressJet Airlines Inc.

#### Сколько компаний-перевозчиков (carrier) учитывают эти наборы данных (представлено в наборах данных)?

``` r
unique_airlines <- unique(airlines$carrier)
length(unique_airlines)
```

    [1] 16

#### Сколько рейсов принял аэропорт John F Kennedy Intl в мае?

``` r
flights %>%
  filter(dest == "JFK", month == 5) %>%
  nrow()
```

    [1] 0

#### Какой самый северный аэропорт?

``` r
airports %>%
  arrange(desc(lat)) %>%
  head(1) %>%
  select(name, lat)
```

    # A tibble: 1 × 2
      name                      lat
      <chr>                   <dbl>
    1 Dillant Hopkins Airport  72.3

#### Какой аэропорт самый высокогорный (находится выше всех над уровнем моря)?

``` r
airports %>%
  arrange(desc(alt)) %>%
  head(1) %>%
  select(name, alt)
```

    # A tibble: 1 × 2
      name        alt
      <chr>     <dbl>
    1 Telluride  9078

#### Какие бортовые номера у самых старых самолетов?

``` r
planes %>%
  filter(!is.na(year)) %>%
  arrange(year) %>%
  head(10) %>%
  select(year, tailnum)
```

    # A tibble: 10 × 2
        year tailnum
       <int> <chr>  
     1  1956 N381AA 
     2  1959 N201AA 
     3  1959 N567AA 
     4  1963 N378AA 
     5  1963 N575AA 
     6  1965 N14629 
     7  1967 N615AA 
     8  1968 N425AA 
     9  1972 N383AA 
    10  1973 N364AA 

#### Какая средняя температура воздуха была в сентябре в аэропорту John FKennedy Intl (в градусах Цельсия).

``` r
weather %>%
  filter(origin == "JFK", month == 9) %>%
  summarise(mean_temp_c = (mean(temp, na.rm = TRUE) - 32)* 5/9) %>%
  pull(mean_temp_c)
```

    [1] 19.38764

#### Самолеты какой авиакомпании совершили больше всего вылетов в июне?

``` r
flights %>%
  filter(month == 6) %>%
  count(carrier, sort = TRUE) %>%
  head(1) %>%
  left_join(airlines, by = "carrier") %>%
  select(name, n)
```

    # A tibble: 1 × 2
      name                      n
      <chr>                 <int>
    1 United Air Lines Inc.  4975

#### Самолеты какой авиакомпании задерживались чаще других в 2013 году?

``` r
flights %>%
  filter(!is.na(dep_delay) & !is.na(arr_delay)) %>%
  mutate(delayed = dep_delay > 0 | arr_delay > 0) %>%
  group_by(carrier) %>%
  summarise(delayed_flights = sum(delayed),
            total_flights = n(),
            delay_ratio = delayed_flights / total_flights) %>%
  arrange(desc(delay_ratio)) %>%
  left_join(airlines, by = "carrier")
```

    # A tibble: 16 × 5
       carrier delayed_flights total_flights delay_ratio name                       
       <chr>             <int>         <int>       <dbl> <chr>                      
     1 F9                  476           681       0.699 Frontier Airlines Inc.     
     2 FL                 2156          3175       0.679 AirTran Airways Corporation
     3 WN                 7546         12044       0.627 Southwest Airlines Co.     
     4 UA                32741         57782       0.567 United Air Lines Inc.      
     5 EV                28277         51108       0.553 ExpressJet Airlines Inc.   
     6 YV                  292           544       0.537 Mesa Airlines Inc.         
     7 VX                 2745          5116       0.537 Virgin America             
     8 B6                28545         54049       0.528 JetBlue Airways            
     9 MQ                12715         25037       0.508 Envoy Air                  
    10 9E                 8562         17294       0.495 Endeavor Air Inc.          
    11 DL                21473         47658       0.451 Delta Air Lines Inc.       
    12 AA                14143         31947       0.443 American Airlines Inc.     
    13 US                 8346         19831       0.421 US Airways Inc.            
    14 AS                  289           709       0.408 Alaska Airlines Inc.       
    15 OO                   11            29       0.379 SkyWest Airlines Inc.      
    16 HA                  129           342       0.377 Hawaiian Airlines Inc.     

### Шаг 2

#### Оценка результатов

В результате выполнения данной практической работы мы развили навыки
использования языка программирования R для обработки данных

#### Вывод

Мы научились использовать функции обработки данных пакета dplyr –
функции select(), filter(), mutate(), arrange(), group_by()
