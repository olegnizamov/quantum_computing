# Вариативные алгоритмы и машинное обучение


6.1. Что такое QAOA и гамильтонианы; подготовка и внедрение квантовых значений и др

Часть 14. Квантовый алгоритм приближённой оптимизации (QAOA)

Квантовый алгоритм, который позволяет находить приближённые решения задач по минимизации логических функций. Примером такой задачи является ранее рассмотренная проблема максимального разреза графа, где операция исключающего или (XOR) равных величин даёт значение 0, а при неравенстве величин будет получена 1, и общее значение функции будет расти. В качестве образцов оптимизации также можно привести задачи максимальной выполнимости (MAX-SAT) и взвешенной максимальной выполнимости (взвешенная MAX-SAT).

![](https://thumb.tildacdn.com/tild3664-6262-4461-a135-313465663466/-/resize/279x/-/format/webp/image001.png)

Логические функции и гамильтонианы

Для решения задачи на квантовом компьютере целевые функции преобразуют в гамильтонианы, каждый из которых является комбинацией Z-операций. Так задача минимизации функции переходит к нахождению базового состояния гамильтониана.

![](https://thumb.tildacdn.com/tild6436-3932-4564-a633-643363333332/-/resize/560x/-/format/webp/image003.png)

Параметризованные состояния

Так как унитарные преобразования могут быть выражены через квантовые вентили, в QAOA применяется дросселирование действия гамильтониана, где получаем эффект будто он по отдельности работает с небольшими частями функции. Происходят похожие на вращения преобразования, часть из которых действует как начальный гамильтониан, часть представляет его финальный вид. По итогу полученное из-за такого разделения функции большое число операций успешно описывает приближённое развитие системы.

![](https://thumb.tildacdn.com/tild3331-6533-4462-a666-663337366432/-/resize/560x/-/format/webp/image005.png)

Процесс оптимизации

Это гибридный метод, то есть действия данного алгоритма выполняются при помощи и квантового, и классического компьютеров. Считается, что решением задачи является состояние пары |β, γ⟩ для некоторых значений параметров. Поэтому сначала выбирается число p, исходя из которого становится ясным количество используемых членов в параметризованном состоянии, и также значения углов β и γ. Затем, используя квантовый компьютер (единственное действие, которое выполняется не классической моделью) подготавливается значение пары |β, γ⟩. Далее вычисляется величина энергии данных углов; для минимизации полученного значения регулируются β или γ. Проведя описанные шаги некоторое число раз, измеряется финальное значение, что даёт приближённое решение задачи.

Подготовка значений на квантовом компьютере

Для рассмотрения развития системы подготавливается исходное значение функции, которое по своей сути являясь суперпозицией базисных состояний, может быть представлено группой вентилей Адамара. Также вводятся и другие квантовые вентили - вращения вокруг оси X, которые отвечают за унитарные преобразования. Части гамильтониана, выражающего энергию системы, являются экспоненциальными функциями, которые зависят от Z-операций.

![](https://thumb.tildacdn.com/tild6539-6635-4239-b636-353266653632/-/resize/385x/-/format/webp/image007.png)

Внедрение значений

Собственные значения экспонент, представляющих гамильтониан, составляют диагональную матрицу противоположных значений - iγk и -iγk. Можно сказать, что они действуют как вращения вокруг оси Z. Если кубит, участвующий в гамильтониане чётный, вводится угол -iγk, в противном случае вводится положительный угол вращения. На практике для выявления чётности используется CNOT-вентиль и, для вращения соответственно, вентиль RZ.

![](https://static.tildacdn.com/tild6363-6432-4365-b335-306565636132/-/empty/image009.png)

Определение величины энергии

Оценка величины энергии в QAOA достаточно лёгкий процесс: после подготовки пары |β, γ⟩ измеряется её значение для получения строки x, затем вычисляется функция потерь С(х). Действия выполняются несколько раз, затем выводится среднее значение результатов. Затем его отправляют в минимизатор, который выдаст оптимальные параметры этого состояния.

Комментарии к алгоритму

Число частей гамильтониана может быть полиномиальным до момента, пока квантовый компьютер может продолжать эффективно подготавливать его значения. При увеличении числа p получается более мощное параметризованное состояние, которое будет приближаться к основному состоянию гамильтониана. Это даёт большую вероятность нахождения близких к минимальным значений энергии. Но и для небольших значений p может быть получено хорошее приближение. Немаловажную роль играет и выбор классического оптимизатора.

Применение QAOA

Физическое исследование о воспроизведении слежения за потоком частиц применяет квантовый отжиг и рассмотренный алгоритм QAOA. Здесь минимизируется функция, формулирующая движение и удары частиц о детекторы ускорителей. Пока работа несёт теоретический характер.

Работа QAOA в IBM Quantum Experience

(Использование пакета AQUA, выбор значения p, выбор оптимизатора, получение оптимального значения энергии, влияние шума устройства для симуляции более реального варианта, получение близкого к оптимальному решения.)

6.2. Вариационный алгоритм для поиска значений, вычисление энергии состояния и даже симуляция молекул

Часть 15. Квантовый вариационный алгоритм нахождения собственных значений (VQE)

QAOA может быть рассмотрен в качестве частного случая более общего алгоритма VQE о квантовом вариационном вычислении собственных чисел. Здесь можно аппроксимировать основное состояние произвольного гамильтониана с полиномиальным числом членов. Для этого используется входное значение из условия задачи и параметризованное унитарное преобразование –- вариационная модель, которая и даёт название данному методу.

Вариационный принцип

Гамильтониан является эрмитовой матрицей, поэтому имеет вещественные собственные числа и связанный с ними ортогональный базис собственных векторов. Входное значение для алгоритма можно представить как произведение матрицы чисел и базиса векторов, тогда как энергия является конвексной комбинацией этих значений. Наилучшим вариантом минимизации получившейся энергии от начального состояния и вариационной формы будет значение, минимальное по отношению ко всем собственным векторам.

Приближение основного состояния

Как и алгоритме QAOA, работа по этому методу начинается с выбора входного значения |ψ⟩, а также вариационной формы U(θ) и входного вектора θ. Далее на квантовом устройстве подготавливается значение |ψ(θ)⟩ и затем вычисляется его энергия E(θ). Для минимизации E(θ) значение входного вектора варьируется; алгоритм завершается, когда классический минимизатор уведомляет о нахождении минимального значения E(θ).

Вычисление энергии состояния

Алгоритмы QAOA и VQE в целом имеют сходную структуру. Однако сейчас, в VQE, необходимо получение энергии более общей формы гамильтониана, включающей элементы Паули X, Y. Вычисляя энергию такого гамильтониана, согласно линейности расчёт происходит по отдельности для частей с Z-состояниями, которое проводится как и в QAOA, и частей с состояниями другого вида, где X, Y преобразуют в Z, сопрягая с матрицами вентилей H и S. После замены элементов Паули вычисление энергии происходит относительно диагонального гамильтониана.

![](https://static.tildacdn.com/tild3964-3766-4539-a537-626135306233/-/empty/image011.png)

Симуляция молекул

VQE может быть использован для расчёта основных состояний физических или химических систем. В одном из первых исследований, внедривших данный метод, рассматривается диссоциирующая кривая молекул. Анализ такого рода трудно выполнить на классическом компьютере, так как с увеличением числа атомов экспоненциально растёт и число связей между ними, поэтому использование квантовых устройств имеет большое преимущество для дальнейшего исследования химических реакций и недоступных для изучения сейчас молекул.

![](https://static.tildacdn.com/tild3663-3561-4664-b563-356463646461/-/empty/image013.png)

Поиск возбуждённых состояний

С помощью метода VQE можно отыскать значения не только основных состояний, но и возбуждённых - собственных состояний, не являющихся основными. После получения основного состояния системы рассматривается гамильтониан, являющийся суммой оригинального гамильтониана и эрмитовой матрицы. Энергия нового гамильтониана связана с уже известным основным состоянием системы, но будет исключать само основное состояние.

Вычисление скалярных произведений параметризованных состояний

Расчёт скалярного произведения можно назвать расчётом вероятности получения того или иного элемента вычислительного базиса; для этого может использоваться квантовая схема. В начале вводится параметризованное состояние, включающее составление внутреннего произведения и вариационную форму; далее идёт сопряжённое транспонирование вариационной формы и обратное преобразование данного внутреннего произведения.

![](https://thumb.tildacdn.com/tild3137-3136-4565-b539-323832643533/-/resize/560x/-/format/webp/image015.png)

VQE для физики элементарных частиц

Алгоритм также внедряют и для исследований в физике элементарных частиц. Например, в [работе](http://andycyli.info/files/4715/5901/6653/2019_Kavli.pdf) лаборатории Fermilab рассматривается гамильтониан Раби, заключающий в себе связь материи и света. Этот физический гамильтониан может быть преобразован к виду, где присутствуют лишь действия с кубитами, поэтому позволяет использовать VQE алгоритм. Были получены основное состояние и первое возбуждённое состояние для данной системы; их сравнение с классически рассчитанными значениями показали лишь небольшие отклонения.

![](https://thumb.tildacdn.com/tild3162-6462-4439-b931-306536336437/-/resize/560x/-/format/webp/image017.png)

![](https://thumb.tildacdn.com/tild6137-3565-4632-b334-613764623530/-/resize/560x/-/format/webp/image019.png)

Рассмотрение работы алгоритма в IBM Quantum Experience

(Использование пакета AQUA, задание молекулы с двумя атомами, выбор вариационной формы, запуск алгоритма на симуляторе, объединение VQE и классического варианта нахождения собственных чисел, полное совпадение значений.)

6.3. Квантовое машинное обучение, загрузка данных и "шумные" процессоры

Часть 16. Квантовое машинное обучение

Квантовое машинное обучение является комбинацией машинного обучения и квантового вычисления. На сегодняшний день исследователи данных методов используют квантовые устройства для обработки классических данных.

QBLAS: Квантовые базовые подпрограммы линейной алгебры

Для успешной работы с классической информацией на квантовых компьютерах применяют ряд подходов. Так, одни из них увеличивают скорость работы на квантовом устройстве части классических алгоритмов машинного обучения. Ускорение частей алгоритма используют, например, в HHL-подходе, основанном на квантовом преобразовании Фурье и квантовой оценке фазы. Часто эти методы описываются QBLAS - квантовыми базовыми подпрограммами линейной алгебры, - повышающими скорость действий классического машинного обучения. Использование QBLAS ограничено из-за проблем с загрузкой входных данных, прочтения результатов и размера используемых схем.

Загрузка данных

Вопрос загрузки данных на квантовый компьютер может быть решён с помощью квантовой оперативной памяти. Идея заключается в получении классической информации в суперпозиции. На данный момент существует целый ряд похожих теоретических методов, заметно ускоряющих работу частей алгоритмов, которые пока, к сожалению, ограничены в применении и не используются на практике.

![](https://thumb.tildacdn.com/tild6464-3465-4964-b835-626566366561/-/resize/560x/-/format/webp/image021.png)

Квантовое машинное обучение и NISQ - шумные квантовые процессоры промежуточного масштаба¹

Современное квантовое вычисление часто называют эрой среднемасштабных квантовых устройств. Это нынешние квантовые компьютеры, неустойчивые к ошибкам, ограниченные числом кубитов и не вполне связываемые. Но некоторые алгоритмы, например, основанные на вариационных формах, QAOA, эффективны уже сейчас, так как их требования к шуму или числу используемых кубитов не так строги и поэтому NISQ активно применяют в современных исследований.

¹ Так называют современные квантовые устройства достаточно больших размеров, состоящие из 50-100 кубитов, однако пока не являющиеся универсальными.

6.4. Метод опорных векторов, гиперплоскость, мягкий зазор и альтернатива Лагранжа и др.

Часть 17. Квантовый метод опорных векторов

Метод опорных векторов представляют собой группу техник для сортировки информации; он достаточно популярен в классическом машинном обучении, поэтому существует его адаптация к квантовым реалиям. Суть метода заключается в поиске гиперплоскости, что разделит информацию двух отличающихся классов с максимально возможным зазором между ними.

![](https://static.tildacdn.com/tild3335-6536-4161-b035-653537326335/-/empty/image023.png)

Нахождение гиперплоскости

Рассмотрим пример сортировки элементов (xi, yi), где xi - вещественный вектор, а yi принимает значения -1 или 1, в зависимости от принадлежности точки той или иной группе. Гиперплоскость, разделяющая части с наибольшим запасом, может быть найдена решением задачи минимизации значения 12w2, где w - коэффициент гиперплоскости.

![](https://static.tildacdn.com/tild6436-3961-4134-b164-613731373437/-/empty/image027.png)

Мягкий зазор

Пример показал простой вариант линейного разделения элементов, где точки оказались по обе стороны гиперплоскости. Но в более общем случае точки могут быть разбросаны так, что невозможно полностью отделить одну группу от другой. Поэтому применяется вариант метода опорных векторов с мягким зазором, который позволяет точкам заступать в обозначенный диапазон ξi, вводя параметр C для его регулирования. Такое “нарушение границ” разрешается нечасто - с увеличением числа точек в допустимой области проводится расчёт новой гиперплоскости.

![](https://static.tildacdn.com/tild3830-6234-4664-a531-363466376164/-/empty/image029.png)

Двойственная формулировка

Вместо использования метода мягкого зазора часто применяют другую постановку задачи метода опорных векторов, использующую, например, множители Лагранжа. Задача формулируется через коэффициенты α; её решение приведет к аналогичному результату путём максимизации функции с α и значениями точек.

Нелинейное разделение

Бывает, что перед нами стоит задача сортировки элементов, которые не могут быть разделены линейно. В этом случае применяют нелинейную технику, где величины сортируются после их переноса в высокоразмерное пространство; для перехода используется карта распределения характеристик Φ(xi).

![](https://static.tildacdn.com/tild3666-6636-4163-a261-396534626635/-/empty/image031.png)

Функции ядра

Существует комбинация карты распределения характеристик и двойственной формулировки задачи метода опорных векторов, где основную часть выражения составляет вычисление произведения трансформирующих векторов Φ(xi). Это выражение называется функцией ядра, где используя исходные значения точек вычисляется их положение в новом пространстве.

![](https://static.tildacdn.com/tild6563-6663-4764-b766-313736313661/-/empty/image033.png)

Квантовое вычисление функций ядра

В [2019 году ](https://www.nature.com/articles/s41586-019-0980-2)было предложено использовать квантовые компьютеры для вычисления функций ядра. Каждый элемент переносится в гильбертово пространство при помощи вариационных схем UΦ(xi) (исходные данные используются как параметры для вариационных форм), а затем произведение величин, фунция ядра, рассчитывается на квантовом устройстве. Далее для завершения работы с методом опорных векторов минимизируется/максимизируется функция, в зависимости от выбранной техники, и в случае необходимости происходит переход к следующему элементу.

![](https://static.tildacdn.com/tild3231-3334-4734-b535-323035333863/-/empty/image035.png)

Квантовый метод опорных векторов в физике элементарных частиц

Рассмотренный метод сортировки нашёл применение и для физики элементарных частиц. Для примера приведём исследование, изучающее [соударение частиц](https://indico.cern.ch/event/868940/contributions/3814309/attachments/2080749/3495472/QMLHEP_ICHEP2020.pdf). Квантовый метод опорных векторов здесь вводят для отделения сигнала о появлении бозона Хиггса от посторонних шумов. В исследовании также анализируется эффективность квантовых и классических компьютеров, использующих различные методы сортировки для данной задачи.

![](https://thumb.tildacdn.com/tild3734-6135-4661-a431-653330306432/-/resize/560x/-/format/webp/image037.png)

Запуск квантового метода опорных векторов в IBM Quantum Experience

(Создание искусственного набора элементов для обучения классификатора, создание вариационной схемы для переноса входной информации в гильбертово пространство, выбор карты распределения характеристик, обучение квантового метода опорных векторов, добавление элементов, использование метода для сортировки новой группы элементов, изучение подготовленных на AQUA наборов данных.)

6.5. Квантовые нейросети: терминология, градиенты, сдвиг параметров, а также будущее таких нейросетей

Часть 18. Квантовые нейросети

Квантовое вычисление относится к новым дисциплинам, в связи с чем пока отсутствует стандарты используемой терминологии. Поэтому то, что здесь будет описано как квантовая нейросеть, также может называться квантовым вариационным сортировщиком, так как решает задачи классификации информации. Наиболее общая структура такой нейросети состоит из двух вариационных форм. Первая, карта распределения признаков, внедряет величины в гильбертово пространство. Вторая вариационная форма непосредственно производит сортировку полученных значений.

![](https://thumb.tildacdn.com/tild3862-3432-4466-b334-346238336531/-/resize/560x/-/format/webp/image039.png)

Работа квантовых нейросетей

В результате прохождения через вариационные формы получается значение |ψ(x,θ)⟩, зависящее от входной информации x и параметров θ. Эта величина измеряется и вычисляется её среднее значение, которое станет классификационной нормой. Выбрав функцию потерь, реальные уровни пробных элементов сравниваются со средним значением; теперь задача состоит в минимизации функции.

Градиенты и сдвиг параметров

Минимизация функции в классической теории нейросетей не обходится без вычисления градиента функции. Для ряда обобщённых вариационных схем расчёт градиента будет аналогичен вычислению схемы со сдвигом параметров x и θ. Это предусматривает два дополнительных запуска схемы с новыми значениями параметров.

Будущее квантовых нейросетей

Пока неизвестно, какие преимущества по итогу принесёт квантовое машинное обучение из-за ограниченного числа рабочих кубитов, поэтому исследования в этом направлении актуальны. Сейчас [рассматриваются](https://onlinelibrary.wiley.com/doi/10.1002/qute.201900070), например, выразимость и “мощность” запутывания вариационных схем, где каждая из них определяется параметрами, которые показывают эффективность того или иного варианта схемы. Также в [недавней работе](https://arxiv.org/pdf/2011.00027.pdf) сравниваются классический и квантовый вариант нейросети и простая квантовая модель. Анализ показал, что у более сложного варианта квантовой схемы - квантовой нейросети - информационное наполнение шире; классическая нейросеть и простая квантовая модель показали сходные характеристики, что хуже по рассматриваемым показателям качества.

![](https://thumb.tildacdn.com/tild3933-6430-4734-a533-306566633263/-/resize/560x/-/format/webp/image041.png)

Введение квантовых нейросетей в разработки физики элементарных частиц

Не обошли стороной квантовые нейросети и работы по физике элементарных частиц. Так, они нашли применение в исследовании классификации бозона Хиггса на симуляторе и в реальных квантовых устройствах. Кроме того, существует [работа](https://arxiv.org/pdf/2002.09935.pdf), описывающая сортировку случаев соударения частиц, где отмечено преимущество квантовых нейросетей для меньшего числа событий.

![](https://thumb.tildacdn.com/tild3366-6433-4163-a263-396633663666/-/resize/560x/-/format/webp/image043.png)
