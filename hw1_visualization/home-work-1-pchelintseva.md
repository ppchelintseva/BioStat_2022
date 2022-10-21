Homework 1
================
Polina Pchelintseva
2022-10-18


    ## R Markdown

    This is an R Markdown document. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details on using R Markdown see <http://rmarkdown.rstudio.com>.


    1 Задание. Загружаем данные для анализа. 

    ```r
    dat <- read.csv("./insurance_cost.csv")

  2 Задание. Строим гистограммы для всех нумерических переменных

``` r
library(dplyr)
```

    ## Warning: пакет 'dplyr' был собран под R версии 4.1.3

    ## 
    ## Присоединяю пакет: 'dplyr'

    ## Следующие объекты скрыты от 'package:stats':
    ## 
    ##     filter, lag

    ## Следующие объекты скрыты от 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
dat_n <- select_if(dat, is.numeric) 
library(Hmisc)
```

    ## Warning: пакет 'Hmisc' был собран под R версии 4.1.3

    ## Загрузка требуемого пакета: lattice

    ## Загрузка требуемого пакета: survival

    ## Warning: пакет 'survival' был собран под R версии 4.1.3

    ## Загрузка требуемого пакета: Formula

    ## Загрузка требуемого пакета: ggplot2

    ## Warning: пакет 'ggplot2' был собран под R версии 4.1.3

    ## 
    ## Присоединяю пакет: 'Hmisc'

    ## Следующие объекты скрыты от 'package:dplyr':
    ## 
    ##     src, summarize

    ## Следующие объекты скрыты от 'package:base':
    ## 
    ##     format.pval, units

``` r
hist.data.frame(dat_n)
```

![](home-work1-pchelintseva_files/figure-gfmunnamed-chunk-3-1.png)<!-- -->
  Задание 3. Нарисуйте график плотности по колонке charges. Отметьте
вертикальные линии средней и медианы на графике. Раскрасьте текст и
линии средней и медианы разными цветами. Добавьте текстовые пояснения
значения средней и медианы. Подберите тему для графика. Назовите оси.

``` r
library(ggplot2)
charges_mean <- round(mean(dat$charges),1)
charges_median <- round(median(dat$charges), 1)

d_plot <- ggplot(data = dat, aes(x = charges)) + geom_density() + geom_vline(aes(xintercept = charges_mean), color = "red")  + annotate("text", x= charges_mean+7000, y = 0.0001, label=paste0("Mean=", charges_mean), color = "blue")+ labs(x = "Charges", y = "Probability") + theme_bw() + geom_vline(aes(xintercept = charges_median), color = "#66CC99")+ annotate("text", x = charges_median - 5000, y = 0.00007, label = paste0("Median=", charges_median), color="666666" )
d_plot
```

![](home-work1-pchelintseva_files/figure-gfmunnamed-chunk-4-1.png)<!-- -->
  Задание 4. Сделайте три box_plot по отношению переменных charges и (1)
sex (2) smoker (3) region. Подберите тему для графика. Назовите оси.

``` r
library(ggpubr)
```

    ## Warning: пакет 'ggpubr' был собран под R версии 4.1.3

``` r
charges_sex <- ggplot() + geom_boxplot(data = dat, aes(x = sex, y = charges)) +labs(x ="Пол", y = "Траты")+theme_bw()  
charges_smoker <- ggplot() + geom_boxplot(data = dat, aes(x = smoker, y = charges))+labs(x="Курит ли человек", y="Траты")+theme_bw() 
charges_region <- ggplot() + geom_boxplot(data = dat, aes(x = region, y = charges))+labs(x="Регион", y = 'Траты')+theme_bw()+theme(axis.text.x = element_text(angle=45)) 
combine_plot <- ggarrange(charges_sex, charges_smoker, charges_region, ncol = 3, nrow = 1)
combine_plot
```

![](home-work1-pchelintseva_files/figure-gfmunnamed-chunk-5-1.png)<!-- -->
  Задание 5. Объедините графики из заданий 3 и 4 в один так, чтобы
сверху шёл один график из задания 3, а под ним 3 графика из задания 4.
Сделайте общее название для графика.

``` r
fourplots <- ggpubr::ggarrange(d_plot,combine_plot, ncol = 1, nrow = 2) 
fourplots <- fourplots %>% annotate_figure(fourplots, top = text_grob("Charge variable characteristics", 
                face = "bold", size = 14))
fourplots
```

![](home-work1-pchelintseva_files/figure-gfmunnamed-chunk-6-1.png)<!-- -->

  6. Сделайте фасет графика из задания 3 по колонке region.

``` r
d_plot <- ggplot(data = dat, aes(x = charges)) + geom_density() + geom_vline(aes(xintercept = charges_mean), color = "red")  + annotate("text", x= charges_mean+12000, y = 0.0001, label=paste0("Mean=", charges_mean), color = "blue", size = 2)+ labs(x = "Charges", y = "Probability") + theme_bw() + geom_vline(aes(xintercept = charges_median), color = "#66CC99")+ annotate("text", x = charges_median - 1000, y = 0.00007, label = paste0("Median=", charges_median), color="666666", size = 2) + facet_grid(.~region)
d_plot
```

![](home-work1-pchelintseva_files/figure-gfmunnamed-chunk-7-1.png)<!-- -->
  7. Постройте scatter plot отношения переменных age и charges. Добавьте
названия осей, название графика и тему. Сделайте так, чтобы числа по оси
Х отображались 14 шрифтом.

``` r
ggplot() + geom_point(data = dat, aes(x = age, y = charges)) + ggtitle("Зависимоть трат от возраста") + labs(x = 'Возраст', y = 'Траты') + theme(axis.text = element_text(size = 14)) + theme_classic()
```

![](home-work1-pchelintseva_files/figure-gfmunnamed-chunk-8-1.png)<!-- -->

  8. Проведите линию тренда для предыдущего графика.

``` r
ggplot(data = dat, aes(x = age, y = charges)) + geom_point()+geom_smooth(method=lm, color="red", fullrange = T, fill="#69b3a2", se=TRUE) + ggtitle("Зависимоть трат от возраста") + labs(x = 'Возраст', y = 'Траты') + theme(axis.text = element_text(size = 14)) +theme_classic() 
```

    ## `geom_smooth()` using formula 'y ~ x'

![](home-work1-pchelintseva_files/figure-gfmunnamed-chunk-9-1.png)<!-- -->
  9. Сделайте разбивку предыдущего графика по колонке smokers (у вас
должно получится две линии тренда для курящих и нет).

``` r
ggplot(data = dat, aes(x = age, y = charges, color=smoker, group = smoker)) + geom_point()+geom_smooth(method=lm, color="red", fullrange = T, fill="#69b3a2", se=TRUE) + ggtitle("Зависимоть трат от возраста и того, курит ли человек") + labs(x = 'Возраст', y = 'Траты') + theme(axis.text = element_text(size = 14)) +theme_classic() 
```

    ## `geom_smooth()` using formula 'y ~ x'

![](home-work1-pchelintseva_files/figure-gfmunnamed-chunk-10-1.png)<!-- -->
  10. Сделайте график из заданий 7-9, но вместо переменной age
используйте переменную bmi.

``` r
ggplot(data = dat, aes(x = bmi, y = charges, color = smoker, group = smoker)) + geom_point()+geom_smooth(method=lm, color="red", fullrange = T, fill="#69b3a2", se=TRUE) + ggtitle("Зависимоть трат от возраста и того, курит ли человек") + labs(x = 'Индекс массы тела', y = 'Траты') + theme(axis.text = element_text(size = 14)) +theme_classic()
```

    ## `geom_smooth()` using formula 'y ~ x'

![](home-work1-pchelintseva_files/figure-gfmunnamed-chunk-11-1.png)<!-- -->
  11. Самостоятельно задайте вопрос No1 к данным (вопрос должен быть про
какую-то подвыборку данных). Ответьте на него построив график на
подвыборке данных. График должен содержать все основные элементы
оформления (название, подписи осей, тему и проч.). Аргументируйте выбор
типа графика.

Распределение курящих из данных по возрасту и индексу массы тела

``` r
dat_12 <- dat %>% 
  mutate(age_group = case_when(
      age < 35 ~ "age: 21-34",
      age >= 35 & age < 50 ~ "age: 35-49",
      age >= 50 ~ "age: 50+")) %>% filter(smoker == 'yes')
     
ggplot()+geom_point(data = dat_12, aes(x = age, y = bmi), color = 'violet')+ggtitle('Распределение индекса массы тела у курильщиков разного возраста')+ labs(x = 'возраст', y = 'индекс массы тела')+theme_minimal()
```

![](home-work1-pchelintseva_files/figure-gfmunnamed-chunk-12-1.png)<!-- -->
Вывод: среди курильщиуов индекс массы тела распределен равномерно во
всех возрастах

  12. Сравнить распределение индекса массы тела у курилищиков по
регионам

``` r
ggplot()+geom_boxplot(data = dat_12, aes(x =region, y=bmi, fill=region))+labs(x='регион', y='индекс массы тела')+ggtitle('Распределение индекса массы тела среди курильщиков по регионам')+theme_bw()
```

![](home-work1-pchelintseva_files/figure-gfmunnamed-chunk-13-1.png)<!-- -->
Индекс массы тела среди курильщиков значимо не отличается по разным
регионам.

  13. Сравнить распределение индекса массы тела у мужчин и женщин старше
50

``` r
dat_13 <- dat %>% 
  mutate(age_group = case_when(
      age < 35 ~ "age: 21-34",
      age >= 35 & age < 50 ~ "age: 35-49",
      age >= 50 ~ "age: 50+")) %>% filter(age_group == "age: 50+")
ggplot()+geom_boxplot(data = dat_13, aes(x = sex, y = bmi, fill = sex))+ggtitle('Распределение индекса массы тела у мужчин и женщин страше 50 лет')+labs(x='пол', y='индекс массы тела')+theme_minimal()
```

![](home-work1-pchelintseva_files/figure-gfmunnamed-chunk-14-1.png)<!-- -->

Индекс массы тела среди женщин и мужчин значимо не отличается.

  14. (это задание засчитывается за два) Приблизительно повторите
график:

``` r
dat <- dat %>% 
  mutate(age_group = case_when(
      age < 35 ~ "age: 21-34",
      age >= 35 & age < 50 ~ "age: 35-49",
      age >= 50 ~ "age: 50+"
    ))
ggplot(data = dat, aes(x = bmi, y = log(charges),color = age_group, group = age_group))+geom_point(aes(alpha = children),color = "mediumorchid4") + geom_smooth(method=lm, fullrange = T, se=TRUE) + facet_grid(.~age_group)+ggtitle("Отношение индекса массы тела к логарифму трат по возрастным группам")+ guides(alpha ="none")+theme_minimal()+theme(legend.position="bottom")
```

    ## `geom_smooth()` using formula 'y ~ x'

![](home-work1-pchelintseva_files/figure-gfmunnamed-chunk-15-1.png)<!-- -->
