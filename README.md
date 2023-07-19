# Предсказание коэффициента восстановления золота из золотосодержащей руды
Ноутбук проекта находится в файле [gold_recovery_prediction.ipynb](https://github.com/ArtemV0ronin/gold_recovery_prediction/blob/main/gold_recovery_prediction.ipynb)  
Альтернативная [ссылка](https://drive.google.com/file/d/1bICdQ9W_yFPJjbaUXMnmTPGtKblYwNie/view?usp=sharing) на Google Collab.

## Задача проекта
Компания «Цифры», разрабатывающая решения для эффективной работы промышленных предприятий, предоставила реальные данные с параметрами добычи и очистки золотосодержащей руды с одного из месторождений. Необходимо на основании этих данных подготовить прототип модели машинного обучения для предсказния коэффициента восстановления золота из золотосодержащей руды. Это поможет оптимизировать производство, чтобы не запускать предприятие с убыточными характеристиками.

## Описание данных
1) Данные состоят из 3 файлов:

- `gold_recovery_train_new.csv` - тренировочные данные;   
- `gold_recovery_test_new.csv` - тестовые данные;     
- `gold_recovery_full_new.csv` - полные сырые данные.
- 
Данные индексируются датой и временем получения информации (признак `date`). Соседние по времени параметры часто похожи.  

Некоторые параметры недоступны, потому что замеряются и/или рассчитываются значительно позже. Из-за этого в тестовой выборке отсутствуют некоторые признаки, которые могут быть в обучающей. Также в тестовом наборе нет целевых признаков.


## Используемый стек
![Static Badge](https://img.shields.io/badge/sklearn-red)
![Static Badge](https://img.shields.io/badge/LinearRegression-red)
![Static Badge](https://img.shields.io/badge/RandomForestRegressor-red)
![Static Badge](https://img.shields.io/badge/GridSearchCV-red)
![Static Badge](https://img.shields.io/badge/StandardScaler-red)
![Static Badge](https://img.shields.io/badge/pandas-red)
![Static Badge](https://img.shields.io/badge/numpy-red)
![Static Badge](https://img.shields.io/badge/matplotlib-red)
![Static Badge](https://img.shields.io/badge/seaborn-red)
![Static Badge](https://img.shields.io/badge/phik-red)
![Static Badge](https://img.shields.io/badge/tqdm-red)
![Static Badge](https://img.shields.io/badge/time-red)

---

## Итоги проекта
В результате работы была разработана модель для предсказания коэффициента восстановления золота из золотосодержащей руды на основании данных по параметрами добычи и очистки, предоставленных компанией "Цифра".
Модель представляет из себя 2 различных модели на базе RandomForestRegressor - одна для этапа флотации (rougher), вторая для финального этапа (final).

Метрики моделей на тестовой выборке:

sMAPE этапа флотации = 8.11   
sMAPE финального этапа = 8.79  
**Итоговая sMAPE = 8.62**

Гиперпараметры итоговых моделей:

<table>
<tr>
  <th>Параметр</th>
  <th>Модель для rougher</th>
  <th>Модель для final</th>
</tr>
<tr>
  <td>max_depth</td>
  <td>5</td>
  <td>1</td>
</tr>    
<tr>
  <td>min_samples_leaf</td>
  <td>1</td>
  <td>1</td>
</tr>   
<tr>
  <td>min_samples_split</td>
  <td>3</td>
  <td>2</td>
</tr> 
<tr>
  <td>n_estimators</td>
  <td>49</td>
  <td>6</td>
</tr>
<tr>
  <td>random_state</td>
  <td>321</td>
  <td>12</td>
</tr>
</table>

Визуализация важности характеристик:

![rougher_top5](https://github.com/ArtemV0ronin/gold_recovery_prediction/blob/main/media/feature_importances_rougher_top_5.jpg)

![final](https://github.com/ArtemV0ronin/gold_recovery_prediction/blob/main/media/feature_importances_final.jpg)

## Отчет о выполнении проекта:

1 В ходе предобработки данных было выполнено:

- обработаны все пропуски в тренировочной выборке;
  - удалены объекты, в которых пропущено более 4 признаков;
  - удалены объекты с пропусками в столбцах, в которых доля пропусков меньше 0.5%;
  - оставшиеся пропуски были заменены на смежное предыдущее значение.
- удалены все пропуски в тестовой выборке.
    
2 В ходе исследовательского анализа было выявлено:

- концентрация золота после флотации имеет более широкое распределение с большим кол-вом выбросов. Среднее значение концентрации возрастает примерно в 1.7 раз. На этапе очистки распределение схожее, но смещено вправо, что говорит об увеличившихся значениях концентрации продукта. На финальном этапе распределение становится еще более узкое, при еще большем кол-ве выбросов. Всё это говорит о том, что качество выходного продукта значительно возросло, золото становится чище.
- в случае с серебром ситуация обратная. На каждом этапе содержание серебра в руде уменьшается примерно в 1.5 раза. В тоже время распределение сужается, что говорит о более качественной очистке с каждым следующим этапом. Кол-во нулевых значений примерно схожее с золотом;
- концентрация свинца характеризуется примерно равными состояниями на финальном этапе и этапе первичной очистки. На этапе флотации средняя концентрация свинца была примерно на треть меньше;
- распределение размеров гранул сырья на обучающей и тестовой выборках близкое к нормальному. Друг от друга распределения сильно не отличаются, за исключением выделяющегося большого кол-ва значений 40-45 test выборки на этапе флотации;
- с каждым следующим этапом обработки распределение суммарной концентрации веществ сужается, а амплитуда значений растет.

3 Всего в ходе работы были протестированы следующие модели:

LR - LogisticRegression  
RFC - RandomForestClassifier

Для каждого этапа (`rougher` и `final`) создавалась своя модель.

Метрика sMAPE на тренировочной выборке на кросс-валидации:

LR - 6.67 для флотации и 9.96 для финального этапа;  
RFC - 6.54 для флотации и 9.93 для финального этапа.

В результате работы лучшей моделью оказалась модель случайного леса RFC.
