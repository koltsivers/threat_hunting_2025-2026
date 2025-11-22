# Практическая работа №4
koltsivers@yandex.ru

## Цель работы

1.  Закрепить практические навыки использования языка программирования R
    для обработки данных
2.  Закрепить знания основных функций обработки данных экосистемы
    tidyverse языка R
3.  Закрепить навыки исследования метаданных DNS трафика

## Исходные данные

1.  Программное обеспечение Microsoft Windows 11 Pro
2.  RStudio Desktop
3.  Интерпретатор языка R 4.5.2

## Задание

Используя программный пакет dplyr, освоить анализ DNS логов с помощью
языка программирования R.

## Ход работы

1.  Импортируйте данные DNS –
    https://storage.yandexcloud.net/dataset.ctfsec/dns.zip. Данные были
    собраны с помощью сетевого анализатора zeek
2.  Добавьте пропущенные данные о структуре данных (назначении столбцов)
3.  Преобразуйте данные в столбцах в нужный формат,просмотрите общую
    структуру данных с помощью функции glimpse()
4.  Сколько участников информационного обмена всети Доброй Организации?
5.  Какое соотношение участников обмена внутрисети и участников
    обращений к внешним ресурсам?
6.  Найдите топ-10 участников сети, проявляющих наибольшую сетевую
    активность.
7.  Найдите топ-10 доменов, к которым обращаются пользователи сети и
    соответственное количество обращений
8.  Определите базовые статистические характеристики (функция summary()
    ) интервала времени между последовательными обращениями к топ-10
    доменам.
9.  Часто вредоносное программное обеспечение использует DNS канал в
    качестве канала управления, периодически отправляя запросы на
    подконтрольный злоумышленникам DNS сервер. По периодическим запросам
    на один и тот же домен можно выявить скрытый DNS канал. Есть ли
    такие IP адреса в исследуемом датасете?
10. Определите местоположение (страну, город) и организацию-провайдера
    для топ-10 доменов. Для этого можно использовать сторонние
    сервисы,например http://ip-api.com (API-эндпоинт –
    http://ip-api.com/json).

## Шаги:

###Задание 1: Импортируйте данные DNS – https://storage.yandexcloud.net/dataset.ctfsec/dns.zip. Данные были собраны с помощью сетевого анализатора zeek

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
        C:\Users\Daniil\AppData\Local\Temp\RtmpyyNVaA\downloaded_packages
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
        C:\Users\Daniil\AppData\Local\Temp\RtmpyyNVaA\downloaded_packages
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
        C:\Users\Daniil\AppData\Local\Temp\RtmpyyNVaA\downloaded_packages
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
        C:\Users\Daniil\AppData\Local\Temp\RtmpyyNVaA\downloaded_packages
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
        C:\Users\Daniil\AppData\Local\Temp\RtmpyyNVaA\downloaded_packages

``` r
library(httr)
library(jsonlite)
library(readr)
library(dplyr)
```


    Присоединяю пакет: 'dplyr'

    Следующие объекты скрыты от 'package:stats':

        filter, lag

    Следующие объекты скрыты от 'package:base':

        intersect, setdiff, setequal, union

``` r
library(stringr)
library(knitr)
temp_dir <- tempdir()
download.file(
  url = "https://storage.yandexcloud.net/dataset.ctfsec/dns.zip",
  destfile = file.path(temp_dir, "dns.zip"),
  mode = "wb",
  quiet = TRUE
)
unzip(
  zipfile = file.path(temp_dir, "dns.zip"),
  exdir = temp_dir
)
log_files <- list.files(temp_dir, pattern = "\\.log$", full.names = TRUE)
```

###Задание 2: Добавьте пропущенные данные о структуре данных (назначении столбцов)

``` r
column_names <- c(
  "timestamp", "uid", "source_ip", "source_port", "destination_ip", 
  "destination_port", "protocol", "transaction_id", "query", "qclass", 
  "qclass_name", "qtype", "qtype_name", "rcode", "rcode_name", 
  "AA", "TC", "RD", "RA", "Z", "answers", "TTLS", "rejected"
)
dns_data <- read_delim(
  log_files[1],
  delim = "\t",
  col_names = column_names,
  comment = "#",
  na = c("", "NA", "-"),
  trim_ws = TRUE,
  show_col_types = FALSE,
  progress = FALSE
) %>% as_tibble()
print(dns_data, n = 10)
```

    # A tibble: 427,935 × 23
         timestamp uid         source_ip source_port destination_ip destination_port
             <dbl> <chr>       <chr>           <dbl> <chr>                     <dbl>
     1 1331901006. CWGtK431H9… 192.168.…       45658 192.168.27.203              137
     2 1331901015. C36a282Jlj… 192.168.…         137 192.168.202.2…              137
     3 1331901016. C36a282Jlj… 192.168.…         137 192.168.202.2…              137
     4 1331901017. C36a282Jlj… 192.168.…         137 192.168.202.2…              137
     5 1331901006. C36a282Jlj… 192.168.…         137 192.168.202.2…              137
     6 1331901007. C36a282Jlj… 192.168.…         137 192.168.202.2…              137
     7 1331901007. C36a282Jlj… 192.168.…         137 192.168.202.2…              137
     8 1331901006. ClEZCt3GLk… 192.168.…         137 192.168.202.2…              137
     9 1331901007. ClEZCt3GLk… 192.168.…         137 192.168.202.2…              137
    10 1331901008. ClEZCt3GLk… 192.168.…         137 192.168.202.2…              137
    # ℹ 427,925 more rows
    # ℹ 17 more variables: protocol <chr>, transaction_id <dbl>, query <chr>,
    #   qclass <dbl>, qclass_name <chr>, qtype <dbl>, qtype_name <chr>,
    #   rcode <dbl>, rcode_name <chr>, AA <lgl>, TC <lgl>, RD <lgl>, RA <lgl>,
    #   Z <dbl>, answers <chr>, TTLS <chr>, rejected <lgl>

###Задание 3: Преобразуйте данные в столбцах в нужный формат,просмотрите общую структуру данных с помощью функции glimpse()

``` r
dns_data_clean <- dns_data %>%
  mutate(
    timestamp = as.POSIXct(timestamp, origin = "1970-01-01"),
    across(c(source_port, destination_port, transaction_id, qclass, qtype, rcode), as.numeric)
  ) %>% 
  filter(!is.na(source_ip) & !is.na(destination_ip))
glimpse(dns_data_clean)
```

    Rows: 427,935
    Columns: 23
    $ timestamp        <dttm> 2012-03-16 16:30:05, 2012-03-16 16:30:15, 2012-03-16…
    $ uid              <chr> "CWGtK431H9XuaTN4fi", "C36a282Jljz7BsbGH", "C36a282Jl…
    $ source_ip        <chr> "192.168.202.100", "192.168.202.76", "192.168.202.76"…
    $ source_port      <dbl> 45658, 137, 137, 137, 137, 137, 137, 137, 137, 137, 1…
    $ destination_ip   <chr> "192.168.27.203", "192.168.202.255", "192.168.202.255…
    $ destination_port <dbl> 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137…
    $ protocol         <chr> "udp", "udp", "udp", "udp", "udp", "udp", "udp", "udp…
    $ transaction_id   <dbl> 33008, 57402, 57402, 57402, 57398, 57398, 57398, 6218…
    $ query            <chr> "*\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\…
    $ qclass           <dbl> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,…
    $ qclass_name      <chr> "C_INTERNET", "C_INTERNET", "C_INTERNET", "C_INTERNET…
    $ qtype            <dbl> 33, 32, 32, 32, 32, 32, 32, 32, 32, 32, 33, 33, 33, 1…
    $ qtype_name       <chr> "SRV", "NB", "NB", "NB", "NB", "NB", "NB", "NB", "NB"…
    $ rcode            <dbl> 0, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
    $ rcode_name       <chr> "NOERROR", NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
    $ AA               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
    $ TC               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
    $ RD               <lgl> FALSE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE…
    $ RA               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
    $ Z                <dbl> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 1, 1, 1,…
    $ answers          <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
    $ TTLS             <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
    $ rejected         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALS…

###Задание 4: Сколько участников информационного обмена всети Доброй Организации?

``` r
source_ips <- dns_data_clean %>% distinct(source_ip) %>% pull()
dest_ips <- dns_data_clean %>% distinct(destination_ip) %>% pull()
all_unique_ips <- union(source_ips, dest_ips)
length(all_unique_ips)
```

    [1] 1359

###Задание 5: Какое соотношение участников обмена внутрисети и участников обращений к внешним ресурсам?

``` r
internal_ips <- all_unique_ips[grepl("^(10\\.|192\\.168\\.|172\\.(1[6-9]|2[0-9]|3[0-1])\\.)", all_unique_ips)]
external_ips <- all_unique_ips[!grepl("^(10\\.|192\\.168\\.|172\\.(1[6-9]|2[0-9]|3[0-1])\\.)", all_unique_ips)]
length(internal_ips) / length(external_ips)
```

    [1] 13.77174

###Задание 6: Найдите топ-10 участников сети, проявляющих наибольшую сетевую активность.

``` r
top_active_sources <- dns_data_clean %>%
  count(source_ip, name = "request_count") %>%
  arrange(desc(request_count)) %>%
  slice(1:10)
print(top_active_sources)
```

    # A tibble: 10 × 2
       source_ip       request_count
       <chr>                   <int>
     1 10.10.117.210           75943
     2 192.168.202.93          26522
     3 192.168.202.103         18121
     4 192.168.202.76          16978
     5 192.168.202.97          16176
     6 192.168.202.141         14967
     7 10.10.117.209           14222
     8 192.168.202.110         13372
     9 192.168.203.63          12148
    10 192.168.202.106         10784

###Задание 7: Найдите топ-10 доменов, к которым обращаются пользователи сети и соответственное количество обращений

``` r
top_10_domains <- dns_data_clean %>%
  count(query, name = "access_count") %>%
  arrange(desc(access_count)) %>%
  slice_head(n = 10)
print(top_10_domains)
```

    # A tibble: 10 × 2
       query                                                            access_count
       <chr>                                                                   <int>
     1 "teredo.ipv6.microsoft.com"                                             39273
     2 "tools.google.com"                                                      14057
     3 "www.apple.com"                                                         13390
     4 "time.apple.com"                                                        13109
     5 "safebrowsing.clients.google.com"                                       11658
     6 "*\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\…        10401
     7 "WPAD"                                                                   9134
     8 "44.206.168.192.in-addr.arpa"                                            7248
     9 "HPE8AA67"                                                               6929
    10 "ISATAP"                                                                 6569

###Задание 8: Определите базовые статистические характеристики (функция summary() ) интервала времени между последовательными обращениями к топ-10 доменам.

``` r
time_interval_stats <- dns_data_clean %>%
  filter(query %in% top_10_domains$query) %>%
  arrange(query, timestamp) %>%
  group_by(query) %>%
  filter(n() > 1) %>%
  mutate(
    time_difference = as.numeric(timestamp - lag(timestamp), units = "secs")
  ) %>%
  summarise(
    min_interval = min(time_difference, na.rm = TRUE),
    q1_interval = quantile(time_difference, 0.25, na.rm = TRUE),
    median_interval = median(time_difference, na.rm = TRUE),
    mean_interval = mean(time_difference, na.rm = TRUE),
    q3_interval = quantile(time_difference, 0.75, na.rm = TRUE),
    max_interval = max(time_difference, na.rm = TRUE),
    .groups = 'drop'
  )
print(time_interval_stats)
```

    # A tibble: 10 × 7
       query      min_interval q1_interval median_interval mean_interval q3_interval
       <chr>             <dbl>       <dbl>           <dbl>         <dbl>       <dbl>
     1 "*\\x00\\…            0       0.150           0.5           11.2        1.5  
     2 "44.206.1…            0       2.09            4             16.0       20.1  
     3 "HPE8AA67"            0       0.75            0.75          16.6       25.5  
     4 "ISATAP"              0       0.75            0.760         17.5        1.05 
     5 "WPAD"                0       0.75            0.75          12.6        1.11 
     6 "safebrow…            0       0               1             10.0        2.01 
     7 "teredo.i…            0       0               0              2.94       0.510
     8 "time.app…            0       0.370           1.76           8.67       4.72 
     9 "tools.go…            0       0               0              8.19       1    
    10 "www.appl…            0       0               1              8.61       3.01 
    # ℹ 1 more variable: max_interval <dbl>

###Задание 9: Часто вредоносное программное обеспечение использует DNS канал в качестве канала управления, периодически отправляя запросы на подконтрольный злоумышленникам DNS сервер. По периодическим запросам на один и тот же домен можно выявить скрытый DNS канал. Есть ли такие IP адреса в исследуемом датасете?

``` r
detect_periodic_requests <- function(data, domains, min_requests = 5) {
  periodic_results <- list()
  for(domain_name in domains) {
    domain_records <- data %>% 
      filter(query == domain_name) %>%
      arrange(source_ip, timestamp)
    
    ip_analysis <- domain_records %>%
      group_by(source_ip) %>%
      filter(n() >= min_requests) %>%
      mutate(
        time_gap = as.numeric(timestamp - lag(timestamp), units = "secs")
      ) %>%
      summarise(
        total_requests = n(),
        average_interval = mean(time_gap, na.rm = TRUE),
        interval_std = sd(time_gap, na.rm = TRUE),
        is_regular = ifelse(average_interval > 0, interval_std / average_interval < 0.5, FALSE),
        .groups = 'drop'
      ) %>%
      filter(is_regular) %>%
      mutate(domain = domain_name)
    
    if(nrow(ip_analysis) > 0) {
      periodic_results[[domain_name]] <- ip_analysis
    }
  }
  bind_rows(periodic_results) %>%
    select(domain, source_ip, total_requests, average_interval, interval_std)
}
suspicious_activity <- detect_periodic_requests(dns_data_clean, top_10_domains$query)
print(suspicious_activity)
```

    # A tibble: 9 × 5
      domain                  source_ip total_requests average_interval interval_std
      <chr>                   <chr>              <int>            <dbl>        <dbl>
    1 "safebrowsing.clients.… 192.168.…              7           14.3        4.60   
    2 "safebrowsing.clients.… 192.168.…              8           16.2        0.165  
    3 "safebrowsing.clients.… 192.168.…              8           16.2        0.520  
    4 "*\\x00\\x00\\x00\\x00… 192.168.…              9            1.51       0.00641
    5 "WPAD"                  192.168.…             14            0.656      0.296  
    6 "ISATAP"                192.168.…            108            0.874      0.313  
    7 "ISATAP"                192.168.…              6            0.754      0.0270 
    8 "ISATAP"                192.168.…             33            0.862      0.147  
    9 "ISATAP"                192.168.…             90            0.767      0.121  

###Задание 10: Определите местоположение (страну, город) и организацию-провайдера для топ-10 доменов. Для этого можно использовать сторонние сервисы,например http://ip-api.com (API-эндпоинт – http://ip-api.com/json).

``` r
get_geo_info <- function(ip) {
  if (is.na(ip) || ip == "") {
     return(tibble(
      ip_address = NA_character_,
      country = "IP не определён",
      city = "IP не определён",
      isp = "IP не определён"
    ))
  }
  if (grepl("^(10\\.|192\\.168\\.|172\\.(1[6-9]|2[0-9]|3[0-1])\\.)", ip)) {
    return(tibble(
      ip_address = ip,
      country = "Частный IP",
      city = "Частный IP",
      isp = "Частный IP"
    ))
  }
  url <- paste0("http://ip-api.com/json/", ip)
  response <- GET(url)
  if (status_code(response) == 200) {
    data <- fromJSON(content(response, "text"))
    if (data$status == "success") {
      return(tibble(
        ip_address = ip,
        country = data$country,
        city = data$city,
        isp = data$isp
      ))
    } else {
      return(tibble(
        ip_address = ip,
        country = paste("API ошибка:", data$status),
        city = paste("API ошибка:", data$status),
        isp = paste("API ошибка:", data$status)
      ))
    }
  } else {
    return(tibble(
      ip_address = ip,
      country = "Ошибка API",
      city = "Ошибка API",
      isp = "Ошибка API"
    ))
  }
}
dns_with_dest_ip <- dns_data_clean %>%
  filter(!is.na(destination_ip)) %>%
  select(query, destination_ip) %>%
  distinct()
relevant_dns <- dns_with_dest_ip %>%
  filter(query %in% top_10_domains$query)
geo_results_df <- tibble(
  ip_address = character(),
  country = character(),
  city = character(),
  isp = character()
)
unique_ips_to_check <- unique(relevant_dns$destination_ip)
for (ip in unique_ips_to_check) {
  geo_info_row <- get_geo_info(ip)
  geo_results_df <- bind_rows(geo_results_df, geo_info_row)
}
domain_geo_info_final <- relevant_dns %>%
  left_join(geo_results_df, by = c("destination_ip" = "ip_address")) %>%
  rename(ip_address = destination_ip) %>%
  select(domain = query, ip_address, country, city, isp)
domain_order_factor <- factor(domain_geo_info_final$domain, levels = top_10_domains$query)
domain_geo_info_final_sorted <- domain_geo_info_final %>%
  mutate(domain_order = domain_order_factor) %>%
  arrange(domain_order) %>%
  select(-domain_order)
print(domain_geo_info_final_sorted)
```

    # A tibble: 1,213 × 5
       domain                    ip_address       country       city       isp      
       <chr>                     <chr>            <chr>         <chr>      <chr>    
     1 teredo.ipv6.microsoft.com fec0:0:0:ffff::2 Switzerland   Morat      Internet…
     2 teredo.ipv6.microsoft.com fec0:0:0:ffff::1 Switzerland   Morat      Internet…
     3 teredo.ipv6.microsoft.com fec0:0:0:ffff::3 Switzerland   Morat      Internet…
     4 teredo.ipv6.microsoft.com 192.168.207.4    Частный IP    Частный IP Частный …
     5 teredo.ipv6.microsoft.com 192.168.0.1      Частный IP    Частный IP Частный …
     6 tools.google.com          192.168.207.4    Частный IP    Частный IP Частный …
     7 tools.google.com          192.168.206.44   Частный IP    Частный IP Частный …
     8 tools.google.com          156.154.70.22    United States New York   Neustar …
     9 tools.google.com          8.26.56.26       United States Clifton    Flexenti…
    10 tools.google.com          68.87.75.198     United States Pittsburgh Comcast …
    # ℹ 1,203 more rows

## Оценка результатов

В результате выполнения данной практической работы мы исследовали
подозрительную сетевую активность во внутренней сети Доброй Организации.
Были восстановлены недостающие метаданные и подготовлены ответы на
вопросы.

## Вывод

Мы закрепили практические навыки использования языка программирования R
для обработки данных, знания основных функций обработки данных
экосистемы tidyverse языка R и навыки исследования метаданных DNS
трафика
