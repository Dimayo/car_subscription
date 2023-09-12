# Анализ данных для сервиса подписки на автомобили
https://github.com/Dimayo/car_subscription_project/blob/main/subscription_data.ipynb <br>
Библиотеки python: pandas, matplotlib, pickle, scipy

## Цель проекта
Необходимо было проверить гипотезы, сформулированные продуктовой командой, а также ответить на ряд вопросов о действиях клиентов на сайте, что поможет в дальнейшем улучшить качество обслуживания.

Гипотезы:
* Органический трафик ничем не отличается от платного по показателю CR (конверсии) в целевые события.
* Трафик с мобильных устройств ничем не отличается от трафика с десктопных устройств по показателю CR (конверсии) в целевые события.
* Трафик из городов присутствия (Москва и область, Санкт-Петербург) не отличается от трафика из других регионов по показателю CR (конверсии) в целевые события.
  
Вопросы продуктовой команды:
* Из каких источников/кампаний/устройств/локаций к нам приходит самый целевой трафик (как по объему трафика, так и по CR)?
* Какие автомобили пользуются наибольшим спросом? Какие автомобили имеют лучший CR в целевых событиях?
* Стоит ли нам увеличить свое присутствие в социальных сетях и давать там больше рекламы?
  
Использовались данные Google Analytics. Файл GA Sessions содержит информацию о посещениях сайта, такую как идентификатор посетителя, дату посещения, время посещения, канал привлечения и т. д. Файл GA Hits содержит информацию о действиях посетителя, например: дату события, время события, тип события и и т. д.




