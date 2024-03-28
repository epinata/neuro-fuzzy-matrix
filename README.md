задаем объект класса FuzzyInferenceSystem, который будет далее полностью опи-сывать нашу систему.

nfm = NeuFuzMatrix.NFM(X, Y)
Для работы доступны два способа построения нейро сети – с заданием пра-вил и без. Для выбора метода нужно присвоить полю algorithm соответствующее строковое значение.

nfm.algorithm="With rules"
Для создания признака или лингвистической переменной выбираем метод create_feature и передаем ему 5 параметров: строковое значение с названием ЛП, строковое значение со системой измерения, значение типа float для указания ми-нимального значения, значение типа float для указания максимального значения и логического значение, где true – входная переменная, а false – выходная.

f_temp = nfm.create_feature("Температура", "C", 0, 150, True)
f_flow =  nfm.create_feature("Расход", "м3/ч", 0, 8, True)
f_pressure  = nfm.create_feature("Давление", "МПа", 0, 100, False)
Для создания предиката или терма лингвистической переменной нужно вы-брать метод create_ predicate и передать ему 3 параметра: лингвистическая пере-менная, которой принадлежит терм, строковое значение с названием терма, функ-цию принадлежности. Доступно три функции принадлежности: гауссовская Gauss, передаем два значения, колоколообразная Bell, передаем три значения, и сигмоид-ная Sigmoid передаем два значения. 

p_temp_low = nfm.create_predicate(f_temp, 'низкая', ['Gauss',{'mean':30,'sigma':10}])
p_temp_normal = nfm.create_predicate(f_temp, 'средняя', ['Gauss',{'mean':70,'sigma':10}])
p_temp_high = nfm.create_predicate(f_temp, 'высокая', ['Gauss',{'mean':120,'sigma':10}])
p_flow_low = nfm.create_predicate(f_flow, 'малый', ['Gauss',{'mean':2,'sigma':1}])
p_flow_normal = nfm.create_predicate(f_flow, 'средний', ['Gauss',{'mean':4,'sigma':1}])
p_flow_high = nfm.create_predicate(f_flow, 'большой', ['Gauss',{'mean':6,'sigma':1}])
Для задания правил нужно выбрать метод create_rule и передать ему список условий, заключение и вес правила. Условия в правиле соединены между собой связкой “и”, поэтому если в правиле присутствует связка “или”, правило следует разбить на несколько правил.

r_1 = nfm.create_rule([p_temp_low, p_flow_low], p_pressure_low, 1)
r_2 = nfm.create_rule([p_temp_normal], p_pressure_normal, 0.7)
Для обучения нейросети нужно вызвать метод trein, передав ему необходимые параметры, это может быть количество эпох обучения, точность ошибки, шаг для градиентного спуска.

ts = np.loadtxt(file_path, usecols=[0,1,2])
X = ts[:,0:2]
Y = ts[:,2]
nfm.train(epochs=500)
Для вывода графика ошибки вызывается метод plotErrors, для вывода графика с результатами выходного параметра после обучения plotResults, графиков при-надлежности функций нужно вызвать метод show_ plotMF.

nfm.plotErrors()
nfm.plotResults()
nfm.plotMF(f_temp)
