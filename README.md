# Анализ данных для сервиса подписки на автомобили
https://github.com/Dimayo/car_subscription/blob/main/subscription_data.ipynb <br>
Библиотеки python: pandas, matplotlib, pickle, scipy, seaborn, numpy

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

## Описание проекта
Использовались данные Google Analytics. Файл GA Sessions содержит информацию о посещениях сайта, такую как идентификатор посетителя, дату посещения, время посещения, канал привлечения и т. д. Файл GA Hits содержит информацию о действиях посетителя, например: дату события, время события, тип события и и т. д.

## Что было сделано
Была произведена загрузка файлов, удаление дубликатов и заполнение пропусков в них. В датафрейме GA Hits был создан новый признак с названиями моделей автомобилей:
```
df_hits_new['model'] = df_hits_new.hit_page_path
                       .apply(lambda x: x.split('/')[3] + ' ' + x.split('/')[4]
                       if len(x.split('/')) > 4 and x.split('/')[1] == 'cars'  
                       else 'other')
```
Также был создан признак который содержит целевые действия:
```
df_hits_new['target_action'] = df_hits_new.event_action.apply(lambda x: 1 if x in 
['sub_car_claim_click', 'sub_car_claim_submit_click',
'sub_open_dialog_click', 'sub_custom_question_submit_click',
'sub_call_number_click', 'sub_callback_submit_click', 'sub_submit_success',
'sub_car_request_submit_click'] 
else 0)
```
Создана таблица с группировкой по сессиям:
```
df_mod = df_hits_new.groupby(['session_id']).agg({'model': ','.join, 'target_action': 'sum'})
```
Признак с целевыми действиями преобразован в бинарный:
```
df_mod['target_action'] = df_mod.target_action.apply(lambda x: 1 if x > 0 else 0)
```

В GA Sessions были удалены лишние столбцы:
```
df_sessions_new = df_sessions.drop(['visit_number','utm_keyword','utm_adcontent',
                                    'device_screen_resolution','device_browser',
                                    'device_model','device_os'], axis=1)
```
Далее таблицы были объединены по столбцу session_id:
``` 
df = df_sessions_new.merge(df_mod, on='session_id', how='inner')
```
Для проверки гипотез о конверсии использовался биномиальный критерий:
```
T = (m1/n1 - m2/n2)/((m1+m2)/(n1+n2)*(1 - (m1+m2)/(n1+n2))*(1/n1 + 1/n2))**0.5
P = min(2*stats.norm.cdf(T), 2 - 2*stats.norm.cdf(T))
print("Statisctic: ",T,", p-value: ",P)
```
## Результат
### Проверка гипотез
Органический трафик статистически значимо отличается от платного с точки зрения CR 

<img src="https://github.com/Dimayo/car_subscription/assets/44707838/04abb284-2bf2-4ce2-bca0-57ce33aaa4d3" width="600"> <br> <br>

Трафик с мобильных устройств значимо отличается от трафика с десктопных устройств с точки зрения CR 

<img src="https://github.com/Dimayo/car_subscription/assets/44707838/53dd67f5-eae9-4835-a5f9-70c1687fa4cd" width="600"> <br> <br>

Трафик из городов присутствия (Москва и область, Санкт-Петербург) значимо отличается от трафика из иных регионов с точки зрения CR

<img src="https://github.com/Dimayo/car_subscription/assets/44707838/b60477db-699b-413b-a8ff-7771434c44da" width="600"> <br> <br>

### Ответы на вопросы продуктовой команды
**<p>Из каких источников к нам идет самый целевой трафик с точки зрения объема трафика и с точки зрения конверсии?</p>**
Целевой трафик по устройствам с точки зрения объема:

<img src="https://github.com/Dimayo/car_subscription/assets/44707838/5592718c-e801-417d-8960-421980735d74" width="600"> <br> <br>

C точки зрения конверсии:

<img src="https://github.com/Dimayo/car_subscription/assets/44707838/c50a3210-a60c-498c-9f8a-e386b70c9d32" width="600"> <br> <br>

Целевой трафик по городам с точки зрения объема:

<img src="https://github.com/Dimayo/car_subscription/assets/44707838/ad060a19-91b9-4143-89d6-f79bd41a1476" width="600"> <br> <br>

С точки зрения конверсии:

<img src="https://github.com/Dimayo/car_subscription/assets/44707838/8c3ebdb3-8dbe-48c6-a4e2-3bbac404e236" width="600"> <br> <br>

**<p>Какие авто пользуются наибольшим спросом? У каких авто самый лучший показатель конверсии?</p>**
Целевой трафик по модели авто с точки зрения объема:

<img src="https://github.com/Dimayo/car_subscription/assets/44707838/1a3d4dda-5aea-4fd0-a554-21d3725b645e" width="600"> <br> <br>

С точки зрения конверсии:

<img src="https://github.com/Dimayo/car_subscription/assets/44707838/0f93d6c2-d59a-48bb-b151-e92726c4b227" width="600"> <br> <br>

**<p>Стоит ли нам увеличивать свое присутствие в соцсетях и давать там больше рекламы?</p>**
Не стоит т.к. показатель конверсии 1.6% ниже среднего (2.9%)

<img src="https://github.com/Dimayo/car_subscription/assets/44707838/1ea59e6a-d78a-4a07-b50c-ce8ffa1eab69" width="600"> <br> <br>




















