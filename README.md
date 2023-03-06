# neuronet_holes_in_leaves
Измерение площади повреждения листьев фитофагами
Елена Николаевна Устинова
При изучении взаимодействий растений и фитофагов часто возникает задача оценки степени повреждения листьев. Выполнение этих измерений вручную (например, выделение нужной области и подсчет числа пикселей в программах Photoshop или ImageJ) – это очень трудоемкий процесс, однако в связи с тем, что повреждения зачастую находятся не в центре листа, а затрагивают его край, который может иметь сложную форму, то автоматизация данного процесса нетривиальна, хотя попытки предпринимались уже неоднократно. 
В 2016 году было выпущено мобильное приложение BioLeaf, которое оценивает уровень дефолиации с помощью кривых Безье для восстановления исходной границы листьев [1]. Хотя этот метод повышает надежность и скорость, он не полностью автоматизирован и достраивать край листа необходимо вручную. Автоматический метод оценки уровня дефолиации основан на обнаружении углов поврежденных участков и их соединении [2]. Однако метод не подходит для сильно поврежденных листьев, когда требуемые методом углы отсутствуют на изображении. Задача реконструкции края листа возникает и для оптимизации методов идентификации растений по изображению, поэтому имеется также несколько работ по данной теме без последующей оценки площади реконструируемого участка [3, 4]. 
Наиболее удачное решение было получено при оценке повреждений листьев сои с использованием сверточных нейронных сетей [5]. В данной работе в качестве тренировочного датасета были использованы листья с искусственно созданными на них повреждениями, при этом модель показала отличные результаты на реальных данных (Среднеквадратическая ошибка (RMSE) составляла всего 4.57 даже для сложных случаев, когда лист был поврежден очень сильно). Это очень хороший показатель, который был бы удовлетворителен для моих целей, однако в данной работе использовались листья сои, которые имеют очень простую форму, а также ровный край. Также весь тренировочный датасет состоял только из искусственно сгенерированных повреждений, которые не всегда похожи на реальные погрызы, оставляемые насекомыми.
Целью моей работы является использование нейронных сетей для измерения площади повреждения листьев. Я планирую использовать в своей работе положительный опыт предыдущих исследователей, но добавлю несколько важных для меня изменений. Поскольку для обучения модели необходим очень большой датасет, то я буду использовать предложенный бразильскими исследователями [5] метод генерации повреждений на изображениях, однако дополню его собственными данными реальных погрызов. У меня имеются сканы поврежденных фитофагами листьев разной степени сложности (примерно по 500 изображений каждого типа): вырезанные квадратики 2 х 2 см (рис. 1А), листья кипрея и иван-чая с прямым краем (рис. 1Б), листья крапивы с зубчатым краем (рис. 1В). Для обучения нейронной сети будет необходимо сначала подсчитать число пикселей, относящихся к повреждению, на изображениях с естественными повреждениями. Размер полученной выборки реальных данных можно будет увеличить за счет аугментаций вращения изображения. Также я хотела бы попробовать использовать в своей модели не черно-белые, а цветные изображения, поскольку процесс бинаризации может быть затруднен для жухлых, пожелтевших листьев и данный этап обработки также хотелось бы автоматизировать.
 
Рисунок 1. Пример имеющихся данных. А – простая форма, Б – более сложный вариант формы, но с прямым краем, В – зубчатый край.

Я планирую использовать традиционные сверточные нейронные сети (CNN) для регрессии, заменяя последний слой полносвязным слоем, состоящим из одного нейрона и сигмоидной функции активации. В предыдущих исследованиях наилучшие результаты показала архитектура сети на основе AlexNet, так что я тоже попробую использовать эту архитектуру.
Таким образом, план моей работы состоит из следующих этапов:
1.	Подсчет числа пикселей (в Photoshop), относящихся к повреждению, на изображениях с естественными повреждениями (февраль).
2.	Генерация датасета с искусственными повреждениями (март).
3.	Обучение модели с архитектурой сети на основе AlexNet на листьях разного типа сложности (по отдельности). Использование бинаризованных и цветных изображений (апрель).
4.	Сравнение результатов точности моделей для разных типов данных (май).
5.	Формулировка выводов, написание статьи.

1.	Machado B.B., Orue J.P., Arruda M.S., Santos C.V., Sarath D.S., Goncalves W.N., Rodrigues-Jr J.F. 2016. BioLeaf: A professional mobile application to measure foliar damage caused by insect herbivory. Computers and electronics in agriculture, 129, 44–55.
2.	Nazare-Jr, A.C., Menotti, D., Neves, J.M.R., Sediyma, T., 2010. Automatic detection of the damaged leaf area in digital images of soybean. In: International Conference on Systems, Signals and Image Processing. pp. 1–4. 
3.	Vieira G.D.S., de Sousa N.M., Rocha B., Fonseca A.U., Soares F. 2021. A Method for the Detection and Reconstruction of Foliar Damage caused by Predatory Insects. IEEE 45th Annual Computers, Software, and Applications Conference (COMPSAC). 1502–1507.
4.	Hussein B.R., Malik O.A., Ong W.H., Slik J.W.F. 2021. Reconstruction of damaged herbarium leaves using deep learning techniques for improving classification accuracy. Ecological Informatics, 61, 101243.
5.	da Silva L.A., Bressan P.O., Gonçalves D.N., Freitas D.M., Machado B.B., Gonçalves, W.N. 2019. Estimating soybean leaf defoliation using convolutional neural networks and synthetic images. Computers and electronics in agriculture, 156, 360–368.
