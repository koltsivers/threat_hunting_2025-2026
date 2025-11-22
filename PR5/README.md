# Практическая работа №5
koltsivers@yandex.ru

## Цель работы

1.  Получить знания о методах исследования радиоэлектронной обстановки.
2.  Составить представление о механизмах работы Wi-Fi сетей на канальном
    и сетевом уровне модели OSI.
3.  Зекрепить практические навыки использования языка программирования R
    для обработки данных
4.  Закрепить знания основных функций обработки данных экосистемы
    tidyverse языка R

## Исходные данные

1.  Программное обеспечение Microsoft Windows 11 Pro
2.  RStudio Desktop
3.  Интерпретатор языка R 4.5.2

## План

1.  Импортируйте данные –
    https://storage.yandexcloud.net/dataset.ctfsec/P2_wifi_data.csv
    Данные были собраны с помощью анализатора беспроводного трафика
    airodump-ng
2.  Привести датасеты в вид “аккуратных данных”, преобразовать типы
    столбцов в соответствии с типом данных
3.  Просмотрите общую структуру данных с помощью функции glimpse()
4.  Произвести анализ данных.
5.  Определить небезопасные точки доступа (без шифрования – OPN)
6.  Определить производителя для каждого обнаруженного устройства
7.  Выявить устройства, использующие последнюю версию протокола
    шифрования WPA3, и названия точек доступа, реализованных на этих
    устройствах
8.  Отсортировать точки доступа по интервалу времени, в течение которого
    они находились на связи, по убыванию.
9.  Обнаружить топ-10 самых быстрых точек доступа.
10. Отсортировать точки доступа по частоте отправки запросов (beacons) в
    единицу времени по их убыванию. 11.Определить производителя для
    каждого обнаруженного устройства (пользоваться базой данных
    производителей из состава Wireshark или онлайн сервисами OUI lookup)
11. Обнаружить устройства, которые НЕ рандомизируют свой MAC адрес
12. Кластеризовать запросы от устройств к точкам доступа по их именам.
    Определить время появления устройства в зоне радиовидимости и время
    выхода его из нее.
13. Оценить стабильность уровня сигнала внури кластера во времени.
    Выявить наиболее стабильный кластер.

## Шаги:

    > install.packages("readr")
    WARNING: Rtools is required to build R packages but is not currently installed. Please download and install the appropriate version of Rtools before proceeding:

    https://cran.rstudio.com/bin/windows/Rtools/
    Устанавливаю пакет в ‘C:/Users/Daniil/AppData/Local/R/win-library/4.5’
    (потому что ‘lib’ не определено)
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/readr_2.1.6.zip'
    Content type 'application/zip' length 1237991 bytes (1.2 MB)
    downloaded 1.2 MB

    пакет ‘readr’ успешно распакован, MD5-суммы проверены

    Скачанные бинарные пакеты находятся в
        C:\Users\Daniil\AppData\Local\Temp\Rtmp4aF99p\downloaded_packages
    > install.packages("dplyr")
    WARNING: Rtools is required to build R packages but is not currently installed. Please download and install the appropriate version of Rtools before proceeding:

    https://cran.rstudio.com/bin/windows/Rtools/
    Устанавливаю пакет в ‘C:/Users/Daniil/AppData/Local/R/win-library/4.5’
    (потому что ‘lib’ не определено)
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/dplyr_1.1.4.zip'
    Content type 'application/zip' length 1593482 bytes (1.5 MB)
    downloaded 1.5 MB

    пакет ‘dplyr’ успешно распакован, MD5-суммы проверены

    Скачанные бинарные пакеты находятся в
        C:\Users\Daniil\AppData\Local\Temp\Rtmp4aF99p\downloaded_packages
    > install.packages("tidyr") 
    WARNING: Rtools is required to build R packages but is not currently installed. Please download and install the appropriate version of Rtools before proceeding:

    https://cran.rstudio.com/bin/windows/Rtools/
    Устанавливаю пакет в ‘C:/Users/Daniil/AppData/Local/R/win-library/4.5’
    (потому что ‘lib’ не определено)
    устанавливаю также зависимость ‘purrr’
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/purrr_1.2.0.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/tidyr_1.3.1.zip'
    пакет ‘purrr’ успешно распакован, MD5-суммы проверены
    пакет ‘tidyr’ успешно распакован, MD5-суммы проверены

    Скачанные бинарные пакеты находятся в
        C:\Users\Daniil\AppData\Local\Temp\Rtmp4aF99p\downloaded_packages
    > install.packages("stringr") 
    WARNING: Rtools is required to build R packages but is not currently installed. Please download and install the appropriate version of Rtools before proceeding:

    https://cran.rstudio.com/bin/windows/Rtools/
    Устанавливаю пакет в ‘C:/Users/Daniil/AppData/Local/R/win-library/4.5’
    (потому что ‘lib’ не определено)
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/stringr_1.6.0.zip'
    Content type 'application/zip' length 349425 bytes (341 KB)
    downloaded 341 KB

    пакет ‘stringr’ успешно распакован, MD5-суммы проверены

    Скачанные бинарные пакеты находятся в
        C:\Users\Daniil\AppData\Local\Temp\Rtmp4aF99p\downloaded_packages
    > install.packages("lubridate") 
    WARNING: Rtools is required to build R packages but is not currently installed. Please download and install the appropriate version of Rtools before proceeding:

    https://cran.rstudio.com/bin/windows/Rtools/
    Устанавливаю пакет в ‘C:/Users/Daniil/AppData/Local/R/win-library/4.5’
    (потому что ‘lib’ не определено)
    устанавливаю также зависимость ‘timechange’
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/timechange_0.3.0.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/lubridate_1.9.4.zip'
    пакет ‘timechange’ успешно распакован, MD5-суммы проверены
    пакет ‘lubridate’ успешно распакован, MD5-суммы проверены

    Скачанные бинарные пакеты находятся в
        C:\Users\Daniil\AppData\Local\Temp\Rtmp4aF99p\downloaded_packages
    > install.packages("janitor") 
    WARNING: Rtools is required to build R packages but is not currently installed. Please download and install the appropriate version of Rtools before proceeding:

    https://cran.rstudio.com/bin/windows/Rtools/
    Устанавливаю пакет в ‘C:/Users/Daniil/AppData/Local/R/win-library/4.5’
    (потому что ‘lib’ не определено)
    устанавливаю также зависимость ‘snakecase’
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/snakecase_0.11.1.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/janitor_2.2.1.zip'
    пакет ‘snakecase’ успешно распакован, MD5-суммы проверены
    пакет ‘janitor’ успешно распакован, MD5-суммы проверены

    Скачанные бинарные пакеты находятся в
        C:\Users\Daniil\AppData\Local\Temp\Rtmp4aF99p\downloaded_packages
    > install.packages("R.utils") 
    WARNING: Rtools is required to build R packages but is not currently installed. Please download and install the appropriate version of Rtools before proceeding:

    https://cran.rstudio.com/bin/windows/Rtools/
    Устанавливаю пакет в ‘C:/Users/Daniil/AppData/Local/R/win-library/4.5’
    (потому что ‘lib’ не определено)
    устанавливаю также зависимости ‘R.oo’, ‘R.methodsS3’
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/R.oo_1.27.1.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/R.methodsS3_1.8.2.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/R.utils_2.13.0.zip'
    пакет ‘R.oo’ успешно распакован, MD5-суммы проверены
    пакет ‘R.methodsS3’ успешно распакован, MD5-суммы проверены
    пакет ‘R.utils’ успешно распакован, MD5-суммы проверены

    Скачанные бинарные пакеты находятся в
        C:\Users\Daniil\AppData\Local\Temp\Rtmp4aF99p\downloaded_packages
    > install.packages("jsonlite") 
    WARNING: Rtools is required to build R packages but is not currently installed. Please download and install the appropriate version of Rtools before proceeding:

    https://cran.rstudio.com/bin/windows/Rtools/
    Устанавливаю пакет в ‘C:/Users/Daniil/AppData/Local/R/win-library/4.5’
    (потому что ‘lib’ не определено)
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/jsonlite_2.0.0.zip'
    Content type 'application/zip' length 1110706 bytes (1.1 MB)
    downloaded 1.1 MB

    пакет ‘jsonlite’ успешно распакован, MD5-суммы проверены

    Скачанные бинарные пакеты находятся в
        C:\Users\Daniil\AppData\Local\Temp\Rtmp4aF99p\downloaded_packages
    > install.packages("httr") 
    WARNING: Rtools is required to build R packages but is not currently installed. Please download and install the appropriate version of Rtools before proceeding:

    https://cran.rstudio.com/bin/windows/Rtools/
    Устанавливаю пакет в ‘C:/Users/Daniil/AppData/Local/R/win-library/4.5’
    (потому что ‘lib’ не определено)
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/httr_1.4.7.zip'
    Content type 'application/zip' length 502674 bytes (490 KB)
    downloaded 490 KB

    пакет ‘httr’ успешно распакован, MD5-суммы проверены

    Скачанные бинарные пакеты находятся в
        C:\Users\Daniil\AppData\Local\Temp\Rtmp4aF99p\downloaded_packages
    > install.packages("V8") 
    WARNING: Rtools is required to build R packages but is not currently installed. Please download and install the appropriate version of Rtools before proceeding:

    https://cran.rstudio.com/bin/windows/Rtools/
    Устанавливаю пакет в ‘C:/Users/Daniil/AppData/Local/R/win-library/4.5’
    (потому что ‘lib’ не определено)
    устанавливаю также зависимость ‘Rcpp’
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/Rcpp_1.1.0.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/V8_8.0.1.zip'
    пакет ‘Rcpp’ успешно распакован, MD5-суммы проверены
    пакет ‘V8’ успешно распакован, MD5-суммы проверены

    Скачанные бинарные пакеты находятся в
        C:\Users\Daniil\AppData\Local\Temp\Rtmp4aF99p\downloaded_packages
    > install.packages("igraph") 
    WARNING: Rtools is required to build R packages but is not currently installed. Please download and install the appropriate version of Rtools before proceeding:

    https://cran.rstudio.com/bin/windows/Rtools/
    Устанавливаю пакет в ‘C:/Users/Daniil/AppData/Local/R/win-library/4.5’
    (потому что ‘lib’ не определено)
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/igraph_2.2.1.zip'
    Content type 'application/zip' length 7372238 bytes (7.0 MB)
    downloaded 7.0 MB

    пакет ‘igraph’ успешно распакован, MD5-суммы проверены

    Скачанные бинарные пакеты находятся в
        C:\Users\Daniil\AppData\Local\Temp\Rtmp4aF99p\downloaded_packages
    > install.packages("fpc") 
    WARNING: Rtools is required to build R packages but is not currently installed. Please download and install the appropriate version of Rtools before proceeding:

    https://cran.rstudio.com/bin/windows/Rtools/
    Устанавливаю пакет в ‘C:/Users/Daniil/AppData/Local/R/win-library/4.5’
    (потому что ‘lib’ не определено)
    устанавливаю также зависимости ‘modeltools’, ‘DEoptimR’, ‘mclust’, ‘flexmix’, ‘prabclus’, ‘diptest’, ‘robustbase’, ‘kernlab’
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/modeltools_0.2-24.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/DEoptimR_1.1-4.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/mclust_6.1.2.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/flexmix_2.3-20.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/prabclus_2.3-4.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/diptest_0.77-2.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/robustbase_0.99-6.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/kernlab_0.9-33.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/fpc_2.2-13.zip'
    пакет ‘modeltools’ успешно распакован, MD5-суммы проверены
    пакет ‘DEoptimR’ успешно распакован, MD5-суммы проверены
    пакет ‘mclust’ успешно распакован, MD5-суммы проверены
    пакет ‘flexmix’ успешно распакован, MD5-суммы проверены
    пакет ‘prabclus’ успешно распакован, MD5-суммы проверены
    пакет ‘diptest’ успешно распакован, MD5-суммы проверены
    пакет ‘robustbase’ успешно распакован, MD5-суммы проверены
    пакет ‘kernlab’ успешно распакован, MD5-суммы проверены
    пакет ‘fpc’ успешно распакован, MD5-суммы проверены

    Скачанные бинарные пакеты находятся в
        C:\Users\Daniil\AppData\Local\Temp\Rtmp4aF99p\downloaded_packages
    > install.packages("mclust")
    WARNING: Rtools is required to build R packages but is not currently installed. Please download and install the appropriate version of Rtools before proceeding:

    https://cran.rstudio.com/bin/windows/Rtools/
    Устанавливаю пакет в ‘C:/Users/Daniil/AppData/Local/R/win-library/4.5’
    (потому что ‘lib’ не определено)
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/mclust_6.1.2.zip'
    Content type 'application/zip' length 4168227 bytes (4.0 MB)
    downloaded 4.0 MB

    пакет ‘mclust’ успешно распакован, MD5-суммы проверены

    Скачанные бинарные пакеты находятся в
        C:\Users\Daniil\AppData\Local\Temp\Rtmp4aF99p\downloaded_packages

``` r
library("readr")
library("dplyr")
```


    Присоединяю пакет: 'dplyr'

    Следующие объекты скрыты от 'package:stats':

        filter, lag

    Следующие объекты скрыты от 'package:base':

        intersect, setdiff, setequal, union

``` r
library("tidyr") 
library("stringr") 
library("lubridate") 
```


    Присоединяю пакет: 'lubridate'

    Следующие объекты скрыты от 'package:base':

        date, intersect, setdiff, union

``` r
library("janitor") 
```


    Присоединяю пакет: 'janitor'

    Следующие объекты скрыты от 'package:stats':

        chisq.test, fisher.test

``` r
library("R.utils") 
```

    Загрузка требуемого пакета: R.oo

    Загрузка требуемого пакета: R.methodsS3

    R.methodsS3 v1.8.2 (2022-06-13 22:00:14 UTC) successfully loaded. See ?R.methodsS3 for help.

    R.oo v1.27.1 (2025-05-02 21:00:05 UTC) successfully loaded. See ?R.oo for help.


    Присоединяю пакет: 'R.oo'

    Следующий объект скрыт от 'package:R.methodsS3':

        throw

    Следующие объекты скрыты от 'package:methods':

        getClasses, getMethods

    Следующие объекты скрыты от 'package:base':

        attach, detach, load, save

    R.utils v2.13.0 (2025-02-24 21:20:02 UTC) successfully loaded. See ?R.utils for help.


    Присоединяю пакет: 'R.utils'

    Следующий объект скрыт от 'package:tidyr':

        extract

    Следующий объект скрыт от 'package:utils':

        timestamp

    Следующие объекты скрыты от 'package:base':

        cat, commandArgs, getOption, isOpen, nullfile, parse, use, warnings

``` r
library("jsonlite") 
```


    Присоединяю пакет: 'jsonlite'

    Следующий объект скрыт от 'package:R.utils':

        validate

``` r
library("httr") 
library("V8") 
```

    Using V8 engine 11.9.169.6

``` r
library("igraph") 
```


    Присоединяю пакет: 'igraph'

    Следующий объект скрыт от 'package:R.oo':

        hierarchy

    Следующие объекты скрыты от 'package:lubridate':

        %--%, union

    Следующий объект скрыт от 'package:tidyr':

        crossing

    Следующие объекты скрыты от 'package:dplyr':

        as_data_frame, groups, union

    Следующие объекты скрыты от 'package:stats':

        decompose, spectrum

    Следующий объект скрыт от 'package:base':

        union

``` r
library("fpc") 
library("mclust")
```

    Package 'mclust' version 6.1.2
    Type 'citation("mclust")' for citing this R package in publications.


    Присоединяю пакет: 'mclust'

    Следующий объект скрыт от 'package:dplyr':

        count

###Задание 1: Импортируйте данные –
https://storage.yandexcloud.net/dataset.ctfsec/P2_wifi_data.csv Данные
были собраны с помощью анализатора беспроводного трафика airodump-ng

``` r
filename <- "P2_wifi_data.csv"
url <- "https://storage.yandexcloud.net/dataset.ctfsec/P2_wifi_data.csv"
if (!file.exists(filename)) {
  download.file(url, destfile = filename, mode = "wb", quiet = TRUE)
}
raw_text <- read_lines(filename, progress = FALSE)
station_header <- "Station MAC, First time seen, Last time seen, Power, # packets, BSSID, Probed ESSIDs"
header_station_idx <- str_which(raw_text, fixed(station_header))

if (length(header_station_idx) == 0) {
  header_station_idx <- str_which(raw_text, "^Station MAC,")
  if (length(header_station_idx) == 0) {
    stop("Заголовок станции не найден")
  }
}

empty_lines_before <- str_which(raw_text[1:(header_station_idx - 1)], "^\\s*$")
if (length(empty_lines_before) == 0) {
  stop("Разделитель не найден")
}
separator_idx <- empty_lines_before[length(empty_lines_before)]
ap_lines_count <- separator_idx - 3

if (ap_lines_count > 0) {
  ap_col_spec <- cols_only(
    "BSSID" = col_character(),
    "First time seen" = col_character(),
    "Last time seen" = col_character(),
    "channel" = col_number(),
    "Speed" = col_number(),
    "Privacy" = col_character(),
    "Cipher" = col_character(),
    "Authentication" = col_character(),
    "Power" = col_number(),
    "# beacons" = col_number(),
    "# IV" = col_number(),
    "LAN IP" = col_character(),
    "ID-length" = col_number(),
    "ESSID" = col_character(),
    "Key" = col_character()
  )
  wifi_ap_data <- read_csv(filename, n_max = ap_lines_count,
                           col_types = ap_col_spec,
                           show_col_types = FALSE,
                           progress = FALSE)
} else {
    stop("Нет данных точек доступа")
}

skip_station_lines <- header_station_idx - 1
station_col_spec <- cols_only(
  "Station MAC" = col_character(),
  "First time seen" = col_character(),
  "Last time seen" = col_character(),
  "Power" = col_number(),
  "# packets" = col_number(),
  "BSSID" = col_character(),
  "Probed ESSIDs" = col_character()
)
wifi_station_data <- read_csv(filename, skip = skip_station_lines,
                              col_types = station_col_spec,
                              show_col_types = FALSE,
                              progress = FALSE)
```

    Warning: One or more parsing issues, call `problems()` on your data frame for details,
    e.g.:
      dat <- vroom(...)
      problems(dat)

``` r
manuf_filename <- "wireshark_manuf.txt"
manuf_url <- "https://www.wireshark.org/download/automated/data/manuf"

if (!file.exists(manuf_filename)) {
  tryCatch({
    download.file(url = manuf_url, destfile = manuf_filename, mode = "wb")
  }, error = function(e){})
}

parse_manuf_data <- function(file_path) {
  lines <- readLines(file_path, encoding = "UTF-8")
  lines <- lines[trimws(lines) != ""]
  oui_vector <- character(0)
  manufacturer_vector <- character(0)
  
  for (line in lines) {
    parts <- str_split(line, "\t", n = 2)[[1]]
    if (length(parts) < 2) next
    
    raw_oui <- parts[1]
    manufacturer_name <- parts[2]
    clean_oui <- toupper(gsub("[^0-9A-F]", "", raw_oui))
    
    if (nchar(clean_oui) == 6) {
      oui_vector <- c(oui_vector, clean_oui)
      manufacturer_vector <- c(manufacturer_vector, manufacturer_name)
    }
  }
  
  result_df <- data.frame(
    oui = oui_vector,
    manufacturer = manufacturer_vector,
    stringsAsFactors = FALSE
  )

  unique_indices <- !duplicated(result_df$oui)
  result_df[unique_indices, ]
}

oui_db <- parse_manuf_data(manuf_filename)
```

###Задание 2-3: Привести датасеты в вид “аккуратных данных”,
преобразовать типы столбцов в соответствии с типом данных. Просмотрите
общую структуру данных с помощью функции glimpse(). Произвести анализ
данных.

``` r
wifi_ap_data <- wifi_ap_data %>%
  clean_names() %>%
  mutate(
    first_time_seen = ymd_hms(first_time_seen, tz = "UTC"),
    last_time_seen = ymd_hms(last_time_seen, tz = "UTC")
  ) %>%
  mutate(across(where(is.character), trimws))

wifi_station_data <- wifi_station_data %>%
  clean_names() %>%
  mutate(
    first_time_seen = ymd_hms(first_time_seen, tz = "UTC"),
    last_time_seen = ymd_hms(last_time_seen, tz = "UTC")
  ) %>%
  mutate(across(where(is.character), trimws))

cat("\n--- Структура данных точек доступа ---\n")
```


    --- Структура данных точек доступа ---

``` r
glimpse(wifi_ap_data)
```

    Rows: 167
    Columns: 15
    $ bssid           <chr> "BE:F1:71:D5:17:8B", "6E:C7:EC:16:DA:1A", "9A:75:A8:B9…
    $ first_time_seen <dttm> 2023-07-28 09:13:03, 2023-07-28 09:13:03, 2023-07-28 …
    $ last_time_seen  <dttm> 2023-07-28 11:50:50, 2023-07-28 11:55:12, 2023-07-28 …
    $ channel         <dbl> 1, 1, 1, 7, 6, 6, 11, 11, 11, 1, 6, 14, 11, 11, 6, 6, …
    $ speed           <dbl> 195, 130, 360, 360, 130, 130, 195, 130, 130, 195, 180,…
    $ privacy         <chr> "WPA2", "WPA2", "WPA2", "WPA2", "WPA2", "OPN", "WPA2",…
    $ cipher          <chr> "CCMP", "CCMP", "CCMP", "CCMP", "CCMP", NA, "CCMP", "C…
    $ authentication  <chr> "PSK", "PSK", "PSK", "PSK", "PSK", NA, "PSK", "PSK", "…
    $ power           <dbl> -30, -30, -68, -37, -57, -63, -27, -38, -38, -66, -42,…
    $ number_beacons  <dbl> 846, 750, 694, 510, 647, 251, 1647, 1251, 704, 617, 13…
    $ number_iv       <dbl> 504, 116, 26, 21, 6, 3430, 80, 11, 0, 0, 86, 0, 0, 0, …
    $ lan_ip          <chr> "0.  0.  0.  0", "0.  0.  0.  0", "0.  0.  0.  0", "0.…
    $ id_length       <dbl> 12, 4, 2, 14, 25, 13, 12, 13, 24, 12, 10, 0, 24, 24, 1…
    $ essid           <chr> "C322U13 3965", "Cnet", "KC", "POCO X5 Pro 5G", NA, "M…
    $ key             <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…

``` r
cat("\n--- Структура данных станций ---\n")
```


    --- Структура данных станций ---

``` r
glimpse(wifi_station_data)
```

    Rows: 12,081
    Columns: 7
    $ station_mac     <chr> "CA:66:3B:8F:56:DD", "96:35:2D:3D:85:E6", "5C:3A:45:9E…
    $ first_time_seen <dttm> 2023-07-28 09:13:03, 2023-07-28 09:13:03, 2023-07-28 …
    $ last_time_seen  <dttm> 2023-07-28 10:59:44, 2023-07-28 09:13:03, 2023-07-28 …
    $ power           <dbl> -33, -65, -39, -61, -53, -43, -31, -71, -74, -65, -45,…
    $ number_packets  <dbl> 858, 4, 432, 958, 1, 344, 163, 3, 115, 437, 265, 77, 7…
    $ bssid           <chr> "BE:F1:71:D5:17:8B", "(not associated)", "BE:F1:71:D6:…
    $ probed_essi_ds  <chr> "C322U13 3965", "IT2 Wireless", "C322U21 0566", "C322U…

###Задание 5: Определить небезопасные точки доступа (без шифрования –
OPN)

``` r
open_networks <- wifi_ap_data %>%
  filter(privacy == "OPN")
print(open_networks)
```

    # A tibble: 42 × 15
       bssid    first_time_seen     last_time_seen      channel speed privacy cipher
       <chr>    <dttm>              <dttm>                <dbl> <dbl> <chr>   <chr> 
     1 E8:28:C… 2023-07-28 09:13:03 2023-07-28 11:55:38       6   130 OPN     <NA>  
     2 E8:28:C… 2023-07-28 09:13:06 2023-07-28 11:55:12       6   130 OPN     <NA>  
     3 E8:28:C… 2023-07-28 09:13:06 2023-07-28 11:55:11       6   130 OPN     <NA>  
     4 E8:28:C… 2023-07-28 09:13:06 2023-07-28 11:55:10       6    -1 OPN     <NA>  
     5 00:25:0… 2023-07-28 09:13:06 2023-07-28 11:56:21      44    -1 OPN     <NA>  
     6 E8:28:C… 2023-07-28 09:13:09 2023-07-28 11:56:05      11   130 OPN     <NA>  
     7 E8:28:C… 2023-07-28 09:13:13 2023-07-28 10:27:06       6   130 OPN     <NA>  
     8 E8:28:C… 2023-07-28 09:13:13 2023-07-28 10:39:43       6   130 OPN     <NA>  
     9 E8:28:C… 2023-07-28 09:13:17 2023-07-28 11:52:32       1   130 OPN     <NA>  
    10 E8:28:C… 2023-07-28 09:13:50 2023-07-28 11:43:39      11   130 OPN     <NA>  
    # ℹ 32 more rows
    # ℹ 8 more variables: authentication <chr>, power <dbl>, number_beacons <dbl>,
    #   number_iv <dbl>, lan_ip <chr>, id_length <dbl>, essid <chr>, key <chr>

###Задание 6: Определить производителя для каждого обнаруженного
устройства

``` r
find_manufacturer <- function(mac_address, oui_database) {
  if (is.na(mac_address) || mac_address == "") {
    return(NA_character_)
  }
  clean_mac <- toupper(gsub("[^0-9A-F]", "", mac_address))
  if (nchar(clean_mac) >= 6) {
    oui_code <- substr(clean_mac, 1, 6)
  } else {
    return(NA_character_)
  }
  match_index <- which(oui_database$oui == oui_code)
  if (length(match_index) > 0) {
    return(oui_database$manufacturer[match_index[1]])
  } else {
    return(NA_character_)
  }
}

ap_macs <- wifi_ap_data$bssid
clean_ap_macs <- ap_macs[!is.na(ap_macs) & ap_macs != ""]
unique_ouis <- unique(substr(toupper(gsub("[^0-9A-F]", "", clean_ap_macs)), 1, 6))
unique_ouis <- unique_ouis[nchar(unique_ouis) == 6]

manufacturer_map <- setNames(
  sapply(unique_ouis, find_manufacturer, oui_db),
  unique_ouis
)

manufacturer_df <- data.frame(
  oui = names(manufacturer_map),
  manufacturer = manufacturer_map,
  stringsAsFactors = FALSE
) %>% distinct(oui, .keep_all = TRUE)

wifi_ap_data <- wifi_ap_data %>%
  mutate(
    oui_code = ifelse(
      is.na(bssid) | bssid == "", 
      NA_character_, 
      substr(toupper(gsub("[^0-9A-F]", "", bssid)), 1, 6)
    ),
    oui_code = ifelse(nchar(oui_code) == 6, oui_code, NA_character_)
  ) %>%
  left_join(manufacturer_df, by = c("oui_code" = "oui")) %>%
  relocate(manufacturer, .after = bssid) %>%
  select(-oui_code)
print(wifi_ap_data)
```

    # A tibble: 167 × 16
       bssid      manufacturer first_time_seen     last_time_seen      channel speed
       <chr>      <chr>        <dttm>              <dttm>                <dbl> <dbl>
     1 BE:F1:71:…  <NA>        2023-07-28 09:13:03 2023-07-28 11:50:50       1   195
     2 6E:C7:EC:…  <NA>        2023-07-28 09:13:03 2023-07-28 11:55:12       1   130
     3 9A:75:A8:…  <NA>        2023-07-28 09:13:03 2023-07-28 11:53:31       1   360
     4 4A:EC:1E:…  <NA>        2023-07-28 09:13:03 2023-07-28 11:04:01       7   360
     5 D2:6D:52:…  <NA>        2023-07-28 09:13:03 2023-07-28 10:30:19       6   130
     6 E8:28:C1:… "EltexEnter… 2023-07-28 09:13:03 2023-07-28 11:55:38       6   130
     7 BE:F1:71:…  <NA>        2023-07-28 09:13:03 2023-07-28 11:50:44      11   195
     8 0A:C5:E1:…  <NA>        2023-07-28 09:13:03 2023-07-28 11:36:31      11   130
     9 38:1A:52:… "SeikoEpson… 2023-07-28 09:13:03 2023-07-28 10:25:02      11   130
    10 BE:F1:71:…  <NA>        2023-07-28 09:13:03 2023-07-28 10:29:21       1   195
    # ℹ 157 more rows
    # ℹ 10 more variables: privacy <chr>, cipher <chr>, authentication <chr>,
    #   power <dbl>, number_beacons <dbl>, number_iv <dbl>, lan_ip <chr>,
    #   id_length <dbl>, essid <chr>, key <chr>

###Задание 7: Выявить устройства, использующие последнюю версию
протокола шифрования WPA3, и названия точек доступа, реализованных на
этих устройствах

``` r
wpa3_devices <- wifi_ap_data %>%
  filter(str_detect(authentication, regex("WPA3", ignore_case = TRUE)) | 
         str_detect(privacy, regex("WPA3", ignore_case = TRUE)))
print(wpa3_devices %>% select(bssid, privacy, essid))
```

    # A tibble: 8 × 3
      bssid             privacy   essid                                         
      <chr>             <chr>     <chr>                                         
    1 26:20:53:0C:98:E8 WPA3 WPA2  <NA>                                         
    2 A2:FE:FF:B8:9B:C9 WPA3 WPA2 "Christie’s"                                  
    3 96:FF:FC:91:EF:64 WPA3 WPA2  <NA>                                         
    4 CE:48:E7:86:4E:33 WPA3 WPA2 "iPhone (Анастасия)"                          
    5 8E:1F:94:96:DA:FD WPA3 WPA2 "iPhone (Анастасия)"                          
    6 BE:FD:EF:18:92:44 WPA3 WPA2 "Димасик"                                     
    7 3A:DA:00:F9:0C:02 WPA3 WPA2 "iPhone XS Max \U0001f98a\U0001f431\U0001f98a"
    8 76:C5:A0:70:08:96 WPA3 WPA2  <NA>                                         

###Задание 8: Отсортировать точки доступа по интервалу времени, в
течение которого они находились на связи, по убыванию.

``` r
calculate_session_duration <- function(ap_sessions, gap_threshold = 2700) {
  ap_sessions <- ap_sessions %>% 
    filter(!is.na(first_time_seen) & !is.na(last_time_seen))
  if (nrow(ap_sessions) == 0) {
    return(tibble(bssid = character(), total_duration = numeric()))
  }
  
  sorted_sessions <- ap_sessions %>% arrange(first_time_seen)
  session_starts <- sorted_sessions$first_time_seen[1]
  session_ends <- sorted_sessions$last_time_seen[1]
  
  for (i in 2:nrow(sorted_sessions)) {
    current_start <- sorted_sessions$first_time_seen[i]
    current_end <- sorted_sessions$last_time_seen[i]
    last_session_end <- session_ends[length(session_ends)]
    
    time_gap <- as.numeric(current_start - last_session_end, units = "secs")
    if (is.na(time_gap) || time_gap > gap_threshold) {
      session_starts <- c(session_starts, current_start)
      session_ends <- c(session_ends, current_end)
    } else {
      session_ends[length(session_ends)] <- max(session_ends[length(session_ends)], current_end)
    }
  }
  
  durations <- as.numeric(session_ends - session_starts, units = "secs")
  tibble(
    bssid = rep(sorted_sessions$bssid[1], length(durations)),
    total_duration = durations
  )
}

ap_duration_analysis <- wifi_ap_data %>%
  filter(!is.na(first_time_seen) & !is.na(last_time_seen)) %>%
  group_by(bssid) %>%
  summarise(
    session_info = list(calculate_session_duration(cur_data())),
    .groups = 'drop'
  ) %>%
  unnest(session_info) %>%
  group_by(bssid) %>%
  summarise(
    total_connection_time = sum(total_duration, na.rm = TRUE),
    .groups = 'drop'
  ) %>%
  arrange(desc(total_connection_time))
```

    Warning: There were 168 warnings in `summarise()`.
    The first warning was:
    ℹ In argument: `session_info = list(calculate_session_duration(cur_data()))`.
    ℹ In group 1: `bssid = "00:00:00:00:00:00"`.
    Caused by warning:
    ! `cur_data()` was deprecated in dplyr 1.1.0.
    ℹ Please use `pick()` instead.
    ℹ Run `dplyr::last_dplyr_warnings()` to see the 167 remaining warnings.

``` r
print(ap_duration_analysis)
```

    # A tibble: 167 × 2
       bssid             total_connection_time
       <chr>                             <dbl>
     1 00:25:00:FF:94:73                 19590
     2 E8:28:C1:DD:04:52                 19552
     3 E8:28:C1:DC:B2:52                 19510
     4 08:3A:2F:56:35:FE                 19492
     5 6E:C7:EC:16:DA:1A                 19458
     6 E8:28:C1:DC:B2:50                 19452
     7 48:5B:39:F9:7A:48                 19450
     8 E8:28:C1:DC:B2:51                 19450
     9 E8:28:C1:DC:FF:F2                 19448
    10 8E:55:4A:85:5B:01                 19446
    # ℹ 157 more rows

###Задание 9: Обнаружить топ-10 самых быстрых точек доступа.

``` r
fastest_aps <- wifi_ap_data %>%
  filter(!is.na(speed)) %>%
  arrange(desc(speed)) %>%
  slice(1:10)
print(select(fastest_aps, bssid, essid, speed, manufacturer))
```

    # A tibble: 10 × 4
       bssid             essid              speed manufacturer                      
       <chr>             <chr>              <dbl> <chr>                             
     1 26:20:53:0C:98:E8 <NA>                 866  <NA>                             
     2 96:FF:FC:91:EF:64 <NA>                 866  <NA>                             
     3 CE:48:E7:86:4E:33 iPhone (Анастасия)   866  <NA>                             
     4 8E:1F:94:96:DA:FD iPhone (Анастасия)   866  <NA>                             
     5 9A:75:A8:B9:04:1E KC                   360  <NA>                             
     6 4A:EC:1E:DB:BF:95 POCO X5 Pro 5G       360  <NA>                             
     7 56:C5:2B:9F:84:90 OnePlus 6T           360  <NA>                             
     8 E8:28:C1:DC:B2:41 MIREA_GUESTS         360 "EltexEnterpr\tEltex Enterprise L…
     9 E8:28:C1:DC:B2:40 MIREA_HOTSPOT        360 "EltexEnterpr\tEltex Enterprise L…
    10 E8:28:C1:DC:B2:42 <NA>                 360 "EltexEnterpr\tEltex Enterprise L…

###Задание 10: Отсортировать точки доступа по частоте отправки запросов
(beacons) в единицу времени по их убыванию.

``` r
ap_beacon_analysis <- wifi_ap_data %>%
  mutate(
    time_span = as.numeric(difftime(last_time_seen, first_time_seen, units = "secs")),
    beacon_rate = ifelse(
      is.na(time_span) | time_span == 0,
      NA_real_,
      number_beacons / time_span
    )
  ) %>%
  filter(!is.na(beacon_rate)) %>%
  arrange(desc(beacon_rate))
print(select(ap_beacon_analysis, bssid, essid, number_beacons, time_span, beacon_rate))
```

    # A tibble: 124 × 5
       bssid             essid                  number_beacons time_span beacon_rate
       <chr>             <chr>                           <dbl>     <dbl>       <dbl>
     1 F2:30:AB:E9:03:ED "iPhone (Uliana)"                   6         7       0.857
     2 B2:CF:C0:00:4A:60 "Михаил's Galaxy M32"               4         5       0.8  
     3 3A:DA:00:F9:0C:02 "iPhone XS Max \U0001…              5         9       0.556
     4 02:BC:15:7E:D5:DC "MT_FREE"                           1         2       0.5  
     5 00:3E:1A:5D:14:45 "MT_FREE"                           1         2       0.5  
     6 76:C5:A0:70:08:96  <NA>                               1         2       0.5  
     7 D2:25:91:F6:6C:D8 "Саня"                              5        13       0.385
     8 BE:F1:71:D6:10:D7 "C322U21 0566"                   1647      9461       0.174
     9 00:03:7A:1A:03:56 "MT_FREE"                           1         6       0.167
    10 38:1A:52:0D:84:D7 "EBFCD57F-EE81fI_DL_1…            704      4319       0.163
    # ℹ 114 more rows

###Задание 11: Определить производителя для каждого обнаруженного
устройства (пользоваться базой данных производителей из состава
Wireshark или онлайн сервисами OUI lookup)

``` r
wifi_station_data <- wifi_station_data %>%
  mutate(
    oui = ifelse(is.na(station_mac) | station_mac == "", 
                 NA_character_, 
                 substr(toupper(gsub("[^0-9A-Fa-f]", "", station_mac)), 1, 6)),
    oui = ifelse(nchar(oui) == 6, oui, NA_character_),
    manufacturer = sapply(oui, function(x) {
      if (is.na(x)) return(NA_character_)
      result <- oui_db$manufacturer[oui_db$oui == x]
      if (length(result) > 0 && !is.na(result[1])) result[1] else NA_character_
    })
  ) %>%
  select(-oui) %>%
  relocate(manufacturer, .after = station_mac)
print(wifi_station_data)
```

    # A tibble: 12,081 × 8
       station_mac       manufacturer  first_time_seen     last_time_seen      power
       <chr>             <chr>         <dttm>              <dttm>              <dbl>
     1 CA:66:3B:8F:56:DD  <NA>         2023-07-28 09:13:03 2023-07-28 10:59:44   -33
     2 96:35:2D:3D:85:E6  <NA>         2023-07-28 09:13:03 2023-07-28 09:13:03   -65
     3 5C:3A:45:9E:1A:7B "ChongqingFu… 2023-07-28 09:13:03 2023-07-28 11:51:54   -39
     4 C0:E4:34:D8:E7:E5 "AzureWaveTe… 2023-07-28 09:13:03 2023-07-28 11:53:16   -61
     5 5E:8E:A6:5E:34:81  <NA>         2023-07-28 09:13:04 2023-07-28 09:13:04   -53
     6 10:51:07:CB:33:E7 "Intel      … 2023-07-28 09:13:05 2023-07-28 11:56:06   -43
     7 68:54:5A:40:35:9E "Intel      … 2023-07-28 09:13:06 2023-07-28 11:50:50   -31
     8 74:4C:A1:70:CE:F7 "LiteonTechn… 2023-07-28 09:13:06 2023-07-28 09:20:01   -71
     9 8A:A3:5A:33:76:57  <NA>         2023-07-28 09:13:06 2023-07-28 10:20:27   -74
    10 CA:54:C4:8B:B5:3A  <NA>         2023-07-28 09:13:06 2023-07-28 11:55:04   -65
    # ℹ 12,071 more rows
    # ℹ 3 more variables: number_packets <dbl>, bssid <chr>, probed_essi_ds <chr>

###Задание 12: Обнаружить устройства, которые НЕ рандомизируют свой MAC
адрес

``` r
check_mac_randomization <- function(mac_addresses) {
  is_invalid <- is.na(mac_addresses) | (mac_addresses == "")
  result <- rep(NA, length(mac_addresses))
  valid_indices <- !is_invalid
  valid_macs <- mac_addresses[valid_indices]

  if (length(valid_macs) > 0) {
    first_byte <- substr(valid_macs, 1, 2)
    first_byte_dec <- strtoi(first_byte, base = 16)
    is_randomized <- (first_byte_dec & 0x02) != 0
    result[valid_indices] <- !is_randomized
  }

  return(result)
}

wifi_station_data <- wifi_station_data %>%
  mutate(
    non_randomized_mac = check_mac_randomization(station_mac)
  )

non_randomized_devices <- wifi_station_data %>%
  filter(non_randomized_mac == TRUE)
print(non_randomized_devices)
```

    # A tibble: 10 × 9
       station_mac       manufacturer  first_time_seen     last_time_seen      power
       <chr>             <chr>         <dttm>              <dttm>              <dbl>
     1 00:95:69:E7:7F:35 "LSDSciencea… 2023-07-28 09:13:11 2023-07-28 11:56:07   -69
     2 00:95:69:E7:7C:ED "LSDSciencea… 2023-07-28 09:13:11 2023-07-28 11:56:13   -55
     3 00:95:69:E7:7D:21 "LSDSciencea… 2023-07-28 09:13:15 2023-07-28 11:56:17   -33
     4 00:90:4C:E6:54:54 "Epigram    … 2023-07-28 09:16:59 2023-07-28 10:21:15   -65
     5 00:04:35:22:4F:75 "InfiNet    … 2023-07-28 09:46:33 2023-07-28 11:15:49   -83
     6 00:E9:3A:67:93:E9 "AzureWaveTe… 2023-07-28 10:15:18 2023-07-28 11:55:11   -73
     7 00:E9:3A:F8:10:C7 "AzureWaveTe… 2023-07-28 10:20:19 2023-07-28 10:20:19   -73
     8 00:0C:E7:A8:D6:73 "MediaTek   … 2023-07-28 10:22:07 2023-07-28 10:22:08   -67
     9 00:98:8C:CE:8E:45  <NA>         2023-07-28 10:34:53 2023-07-28 10:35:13   -65
    10 00:F4:8D:F7:C5:19 "LiteonTechn… 2023-07-28 10:45:04 2023-07-28 11:43:26   -73
    # ℹ 4 more variables: number_packets <dbl>, bssid <chr>, probed_essi_ds <chr>,
    #   non_randomized_mac <lgl>

###Задание 13: Кластеризовать запросы от устройств к точкам доступа по
их именам. Определить время появления устройства в зоне радиовидимости и
время выхода его из нее.

``` r
station_clusters <- wifi_station_data %>%
  filter(bssid != "(not associated)") %>%
  group_by(bssid) %>%
  summarise(
    first_appearance = min(first_time_seen, na.rm = TRUE),
    last_appearance = max(last_time_seen, na.rm = TRUE),
    client_count = n_distinct(station_mac),
    observation_count = n(),
    .groups = 'drop'
  ) %>%
  arrange(first_appearance)
print(station_clusters)
```

    # A tibble: 74 × 5
       bssid  first_appearance    last_appearance     client_count observation_count
       <chr>  <dttm>              <dttm>                     <int>             <int>
     1 BE:F1… 2023-07-28 09:13:03 2023-07-28 11:53:16            2                 2
     2 BE:F1… 2023-07-28 09:13:03 2023-07-28 11:51:54            1                 1
     3 00:25… 2023-07-28 09:13:06 2023-07-28 11:56:21           45                45
     4 00:26… 2023-07-28 09:13:06 2023-07-28 11:55:30            8                 8
     5 1E:93… 2023-07-28 09:13:06 2023-07-28 11:50:50            3                 3
     6 E8:28… 2023-07-28 09:13:06 2023-07-28 11:55:10            3                 3
     7 0C:80… 2023-07-28 09:13:08 2023-07-28 11:53:36            2                 2
     8 0A:C5… 2023-07-28 09:13:09 2023-07-28 11:34:42            1                 1
     9 E8:28… 2023-07-28 09:13:09 2023-07-28 11:55:51            4                 4
    10 9A:75… 2023-07-28 09:13:14 2023-07-28 11:51:50            1                 1
    # ℹ 64 more rows

###Задание 14: Оценить стабильность уровня сигнала внури кластера во
времени. Выявить наиболее стабильный кластер.

``` r
signal_stability <- wifi_station_data %>%
  filter(bssid != "(not associated)", !is.na(power)) %>%
  group_by(bssid) %>%
  summarise(
    average_power = mean(power, na.rm = TRUE),
    power_variation = sd(power, na.rm = TRUE),
    observations = n(),
    .groups = 'drop'
  ) %>%
  arrange(power_variation) %>%
  mutate(
    stability_score = 1 / (1 + power_variation)
  ) %>%
  select(bssid, everything())
print(signal_stability)
```

    # A tibble: 74 × 5
       bssid             average_power power_variation observations stability_score
       <chr>                     <dbl>           <dbl>        <int>           <dbl>
     1 86:DF:BF:E4:2F:23         -71              0               2           1    
     2 E8:28:C1:DC:C8:32          -1              0               2           1    
     3 E8:28:C1:DC:FF:F2         -73              2               3           0.333
     4 CE:B3:FF:84:45:FC         -85              2.83            2           0.261
     5 E8:28:C1:DD:04:40         -61              2.83            2           0.261
     6 8E:55:4A:85:5B:01         -50.3            4.13            6           0.195
     7 00:26:99:F2:7A:E2         -64.2            4.40            8           0.185
     8 E8:28:C1:DC:B2:50         -59.8            5.22            5           0.161
     9 E8:28:C1:DC:F0:90         -63.7            6.11            3           0.141
    10 00:25:00:FF:94:73         -71.2            6.51           45           0.133
    # ℹ 64 more rows

## Оценка результата

В рамках практческой работы была исследована радиоэлектронная обстановка
и составлено представление о механизмах работы Wi-Fi сетей на канальном
и сетевом уровне модели OSI.

## Вывод

В практической работе мы использовали навыки написания кода на языке
программирования R для обработки данных и закрепили знания основных
функций обработки данных экосистемы tidyverse языка R.
