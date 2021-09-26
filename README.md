# TP_Highload
Домашнее задание по курсу технопарка Highload

# Tumblr

# MVP
1. Авторизация/регистрация
2. Создание блога/постов
3. Просмотр ленты рекомендаций

# Целевая аудитория
* 330 млн посещений в месяц
* 7,2 млн новых блогов создаются ежемесячно
* приблизительно 9,7 млн. пользователей, посещающих платформу ежедневно
* 22 млн постов каждый день
* среднее время, проведенное на платформе 9 минут
* платформа используется по всему миру
  * 50% трафика - Северная Америка (в основном США)
  * 30% - Европа
  * 12% - Азия
  * 3% - Австралия

# Расчет нагрузки
### Продуктовые метрики
1. Месячная аудитория составляет около 330 млн. уникальных посещений
2. Дневная аудитория - 9,7 млн пользователей
3. Если взять общее количество аккаунтов 440 млн и всего опубликованных постов ~180 млрд, то получаем 
```
180000 / 440 = 408 постов на один блог
```
Вес одного поста при загрузке изображения или gif достигает 3мб, видео - до 100мб. При хранении размер картинок ужимается до ~30Кб, gif до 5Кб, видео до 5Мб в среднем.
Большей частью постов являются изображения - ~85%, gif - ~10%, видео - ~5%.
```
0.85 * 408 * 30 + 0.1 * 408 * 5 + 0.05 * 408 * 5 * 1024 = 108Мб примерно хранилище одного пользователя
```
4. Среднее время просмотра ленты составляет 9 минут, если считать, что показывается по 4 поста в течение 5 секунд, то за 9 минут подгружается
```
4 * 9 * 60 / 5 = 432 поста  
```
За один раз подгружается 40 постов => за 9 минут на одного пользователя ~ 11 запросов. 
```
10 млн * 11 = 110 млн запросов в день
```
5. Каждый день создается около 50Гб новых постов и 2.7Тб обновлений списков последователей
6. Ежемесячно 330 млн уникальных посещений, это пирблизительно 10 млн посещений в день из них около 30% авторизаций => примерно 3 млн запросов на авторизацию.
   Годовой прирост аудитории ~ 30 млн пользователей:
```
30 млн / 365 = 82 тыс запросов на регистрацию в день
```

### Технические метрики
1. Общее количество постов ~180 млрд, из них 85% - изображения, 10% - gif, 5% - видео.
```
(0.85 * 180млрд * 30 + 0.1 * 180млрд * 5 + 0.05 * 180млрд * 5 * 1024) / (1024 * 1024 * 1024) = 4727Пб 
```
2. Сетевой трафик
  * При создании: 22 млн постов каждый день, средний размер поста уже в сжатом состоянии 40Кб.
 ```
 22млн / (24 * 60 * 60) * 40 / 1024 = 10 Мб/с 
 ```
  * При загрузке ленты: 110 млн запросов на выгрузку постов в день. В каждом запрос по 40 постов
  ```
  110млн * 40 / (24 * 60 * 60) * 40 / 1024 = 1900Мб/с - пиковая нагрузка
  ```
 3. RPS: 
|  Запрос         |      RPS      | Доп. информация при расчетах       |
| --------------- | ------------- | ---------------------------------- |
| Регистрации     | 1             | 82 тыс. запросов в день            |
| Авторизации     | 35            |  3 млн запросов в день             |
| Создание блога  | 3             |  7,2 млн созданий ежемесячно       |
| Создание постов | 1427          | 1427 постов создается в секунду[2] |
| Просмотр ленты  | 1273          | 110 млн запросов в день            |


# Используемые источники
1. https://onlypult.com/ru/blog/gid-po-rabote-v-tumblr-dlya-nachinauschih
2. https://saasscout.com/statistics/tumblr-statistics/
3. https://www.statista.com/topics/2463/tumblr/
4. https://www.insight-it.ru/highload/2012/arkhitektura-tumblr/
