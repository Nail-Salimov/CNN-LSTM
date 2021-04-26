# Модель на основе CNN-LSTM для прогнозирования цен на акции
Данные о ценах на акции имеют характеристики временных рядов. В то же время, на основе долгой краткосрочной памяти машинного обучения (LSTM), которая имеет преимущества анализа взаимосвязей между данными временных рядов с помощью функции памяти, встатье предлагается метод прогнозирования курса акций на основе CNN-LSTM. Результат  гибрида сравнивался с результатами MLP, CNN, RNN, LSTM, CNN-RNN и другие модели прогнозирования.

Данные, использованные в этом исследовании, касаются дневных цен на акции с 1 июля 1991 г. по 31 августа 2020 г., включая 7127 торговых дней. Выбираны восемь характеристик, включая цену открытия, максимальную цену, минимальную цену, цену закрытия, объем, оборот, взлеты и падения и изменения.  CNN применяется для эффективного извлечения характеристик из данных, которые являются элементами за предыдущие 10 дней. А затем мы применяется LSTM для прогнозирования курса акций на основе извлеченных данных характеристик. Согласно результатам экспериментов, CNN-LSTM может обеспечить надежное прогнозирование курса акций с высочайшей точностью прогноза. Этот метод прогнозирования не только дает новую исследовательскую идею для прогнозирования цен на акции, но и дает ученым практический опыт изучения данных финансовых временных рядов.

##### **Модель CNN-LSTM**

CNN имеет свойство обращать внимание на наиболее очевидные особенности в прямой видимости, поэтому он широко используется в проектировании признаков. LSTM имеет свойство расширяться в соответствии с последовательностью времени и широко используется во временных рядах. В соответствии с характеристиками CNN и LSTM устанавливается модель прогнозирования запасов на основе CNN-LSTM. Схема структуры модели показана на рисунке, а основная структура - это CNN и LSTM, включая входной уровень, одномерный слой свертки, уровень объединения, скрытый слой LSTM и уровень полного соединения.




![alt text](https://static-01.hindawi.com/articles/complexity/volume-2020/6622927/figures/6622927.fig.001.svgz)







##### **CNN**












CNN - сетевая модель, предложенная Lecun et al. в 1998 г. CNN - это своего рода нейронная сеть с прямой связью, которая обладает хорошей производительностью при обработке изображений и обработке естественного языка. Его можно эффективно применять для прогнозирования временных рядов. Локальное восприятие и распределение веса CNN может значительно уменьшить количество параметров, тем самым повышая эффективность обучения модели. CNN в основном состоит из двух частей: сверточного слоя и объединяющего слоя. Каждый слой свертки содержит множество ядер свертки, и его формула расчета показана в формуле. После операции свертки сверточного слоя признаки данных извлекаются, но размеры извлеченных признаков очень высоки, поэтому для решения этой проблемы и снижения затрат на обучение сети после свертки добавляется слой объединения. слой, чтобы уменьшить размер объекта:


![plot](./images/1.png)



где представляет выходное значение после свертки, tanh - функция активации, x - входной вектор, k - вес ядра свертки и b - смещение ядра свертки.

##### **LSTM**

LSTM - это сетевая модель, предложенная Шмидхубером и др. в 1997 г. LSTM - это сетевая модель, разработанная для решения давних проблем градиентного взрыва и исчезновения градиента в RNN. Он широко используется в распознавании речи, эмоциональном анализе и анализе текста, поскольку имеет собственную память и может делать относительно точные прогнозы. В последние годы он стал применяться и в области прогнозирования фондового рынка. В стандартной RNN есть только один повторяющийся модуль, и его внутренняя структура проста. Обычно это загар. Однако четыре модуля LSTM аналогичны стандартным модулям RNN и работают особым интерактивным образом. Ячейка памяти LSTM состоит из трех частей: затвора забывания, входного затвора и выходного затвора, как показано на рисунке.


























Процесс расчета LSTM выглядит следующим образом:


1) Выходное значение последнего момента и входное значение текущего времени вводятся в ворота забывания, а выходное значение ворот забывания получается после расчета, как показано в следующей формуле:


где диапазон значений равен f (0,1), W- вес элемента забывания, b- смещение элемента забывания, x- входное значение текущего времени и h- выходное значение последнего момента.


2) Выходное значение последнего времени и входное значение текущего времени вводятся во входной вентиль, а выходное значение и состояние ячейки-кандидата входного вентиля получают после расчета, как показано в следующих формулах:






где диапазон значений i равен (0,1), Wi - вес входного элемента, b\_i- смещение входного элемента, Wc- вес входного элемента-кандидата, и b\_c - смещение входного элемента-кандидата


3) Обновите текущее состояние ячейки следующим образом:




где диапазон значений C\_t равен

4) Выход h и вход x принимаются как входные значения выходного элемента в момент времени t , а выходной o сигнал выходного элемента получается следующим образом






где диапазон значений o равен (0,1), W- вес выходного элемента, а b- смещение выходного элемента.

5) Выходное значение LSTM получается путем вычисления выходного сигнала выходного вентиля и состояния ячейки, как показано в следующей формуле




##### **Процесс обучения и прогнозирования CNN-LSTM**








































Основные шаги следующие:

1) Входные данные: введите данные, необходимые для обучения CNN-LSTM.

2) Стандартизация данных: поскольку во входных данных имеется большой пробел, для лучшего обучения модели принят метод стандартизации *z-*оценки для стандартизации входных данных, как показано в следующей формуле:




где y\_i - стандартизованное значение, x\_i- входные данные, x̅- это среднее значение входных данных, а *s*- стандартное отклонение входных данных.

3) Инициализировать сеть: инициализировать веса и смещения каждого уровня CNN-LSTM.

4) Расчет слоя CNN: входные данные последовательно проходят через сверточный слой и слой объединения в слое CNN, выполняется извлечение признаков входных данных и получается выходное значение.

5) Расчет уровня LSTM: выходные данные уровня CNN вычисляются через слой LSTM, и получается выходное значение.

6) Расчет выходного слоя: выходное значение уровня LSTM вводится в полный уровень соединения для получения выходного значения.

7) Ошибка вычисления: выходное значение, вычисленное выходным слоем, сравнивается с реальным значением этой группы данных, и получается соответствующая ошибка.

8) Чтобы определить, удовлетворяется ли условие завершения: условия завершения - выполнение заранее определенного количества циклов, вес ниже определенного порога, а частота ошибок прогнозирования ниже определенного порога. Если одно из условий завершения выполнено, обучение будет завершено, обновится вся сеть CNN-LSTM и перейдем к шагу 10; в противном случае перейдите к шагу 9.

9) Обратное распространение ошибки: распространите вычисленную ошибку в противоположном направлении, обновите вес и смещение каждого слоя и перейдите к шагу 4, чтобы продолжить обучение сети.

10) Сохранить модель: сохранить обученную модель для прогнозирования.

11) Входные данные: введите входные данные, необходимые для прогнозирования.

12) Стандартизация данных: входные данные стандартизированы по формуле (8).

13) Прогнозирование: введите стандартизированные данные в обученную модель CNN-LSTM, а затем получите соответствующее выходное значение.

14) Стандартизированное восстановление данных: выходное значение, полученное с помощью модели CNN-LSTM, является стандартизированным значением, а стандартизованное значение восстанавливается до исходного значения. Как показано в следующей формуле (9).

15) Результат вывода: вывод восстановленных результатов для завершения процесса прогнозирования.


##### **Данные**

В этом эксперименте в качестве экспериментальных данных выбран Shanghai Composite Index (000001). Ежедневные торговые данные за 7127 торговых дней с 1 июля 1991 г. по 31 августа 2020 г. получены из базы данных wind. Каждый фрагмент данных содержит восемь элементов, а именно цену открытия, максимальную цену, минимальную цену, цену закрытия, объем, оборот, взлеты и падения и изменения. Некоторые данные представлены в таблице.



##### **Реализация модели**

Чтобы оценить эффект прогнозирования CNN-LSTM, в качестве критериев оценки методов используются средняя абсолютная ошибка (MAE), среднеквадратичная ошибка (RMSE) и *R-*квадрат.

#### **Результаты**

После использования обработанных данных обучающего набора для обучения MLP, CNN, RNN, LSTM CNN-RNN и CNN-LSTM, соответственно, модель, завершенная обучением, используется для прогнозирования данных тестового набора, а реальное значение сравнивается с прогнозируемым. значение , как показано на рисунках































Результаты показывают, что производительность CNN-LSTM является лучшей среди шести методов. Что касается точности прогнозирования, MAE составляет 27,564, а RMSE - 39,688, что является наименьшим среди шести моделей прогнозирования и имеет высокую точность прогнозирования с точки зрения эффективности прогнозирования, а *R^*2  CNN-LSTM составляет 0,9646, что улучшено на 2,2%, 0,6%, 0,5% и 0,2% соответственно по сравнению с четырьмя другими методами. Следовательно, CNN-LSTM, предложенная в этой статье, превосходит другие четыре сравнительные модели с точки зрения степени соответствия и значения ошибки. Он может хорошо предсказывать цену закрытия следующего дня и служить ориентиром для инвестиций инвесторов.

#### **Выводы**
Согласно хронологическим характеристикам данных о ценах на акции, в этой статье предлагается CNN-LSTM для прогнозирования цены закрытия акций на следующий день. В этом методе в качестве входных данных используются цена открытия, максимальная цена, минимальная цена, цена закрытия, объем, оборот, взлеты и падения, а также изменение данных о запасах, полностью используя характеристики временной последовательности данных о запасах. CNN используется для извлечения характеристик входных данных. LSTM используется для изучения извлеченных данных характеристик и прогнозирования цены закрытия акции на следующий день. В этой статье в качестве примера для проверки экспериментальных результатов используются соответствующие данные Shanghai Composite Index. Результаты экспериментов показывают, что CNN-LSTM имеет самую высокую точность прогнозирования и лучшую производительность по сравнению с MLP, CNN, RNN, LSTM и CNN-RNN. MAE и RMSE - самые маленькие из всех методов,*R^*2 близок к 1. CNN-LSTM подходит для прогнозирования цен на акции и может служить релевантным ориентиром для инвесторов, чтобы максимизировать отдачу от инвестиций. CNN-LSTM также предлагает практический опыт исследования данных финансовых временных рядов. Однако у модели все же есть недостатки. Например, он учитывает только влияние данных о ценах акций на цены закрытия и не учитывает эмоциональные факторы, такие как новости и национальная политика, в прогнозе. Наша будущая исследовательская работа в основном направлена на усиление анализа настроений в отношении новостей, связанных с акциями, и национальной политики, чтобы обеспечить точность прогнозов по запасам.
