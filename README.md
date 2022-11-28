# ПЕРЦЕПТРОН. [in GameDev]
Отчет по лабораторной работе #4 выполнил(а):
- Демина Анастасия Викторовна
- РИ210942
- Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | * | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

## Цель работы
### Познакомится с понятием перцептрона и использованием его на практике.

## Задание 1
### В проекте Unity реализовать перцептрон, который умеет производить вычисления: OR, AND, NAND и XOR.
Ход работы:
- Cоздала новый пустой 3D проект на Unity и добавила пустой объект, навесив на него скрипт, описывающий работу перцептрона.
#### OR
- В Unity создала элементы в Ts, описывающие логику OR:

!Произошел баг интерфейса при создании первого элемента, но смогла заполнить необходимые значения (Elem = 0, Elem = 0, Output = 0)

![Image alt](https://github.com/cutterror/DA-in_gameDev-lab4/blob/main/images/num1/OR/1.png)
![Image alt](https://github.com/cutterror/DA-in_gameDev-lab4/blob/main/images/num1/OR/2.png)
- Запустила при значении Train(8)
- Перцептрон успешно обучился после 4й этирации выведя значение totalError равное 0:

![Image alt](https://github.com/cutterror/DA-in_gameDev-lab4/blob/main/images/num1/OR/3.png)
- При подстановке данных получены правильные значения, отражающие логику логического "или":

![Image alt](https://github.com/cutterror/DA-in_gameDev-lab4/blob/main/images/num1/OR/4.png)

#### AND
- Аналогично создала элементы в Ts, описывающие логику AND:

![Image alt](https://github.com/cutterror/DA-in_gameDev-lab4/blob/main/images/num1/AND/1.png)
![Image alt](https://github.com/cutterror/DA-in_gameDev-lab4/blob/main/images/num1/AND/2.png)

- Запустила при значении Train(8)
- Перцептрон успешно обучился после 3й этирации, получен правильный вывод, отражающий логику логического "и":

![Image alt](https://github.com/cutterror/DA-in_gameDev-lab4/blob/main/images/num1/AND/3.png)

#### NAND
- Аналогично создала элементы в Ts, описывающие логику NAND:

!Из-за бага интерфейса получилось ввести значения вывода первого элемента Ts только при 5ти элементах, лишний после удалила

![Image alt](https://github.com/cutterror/DA-in_gameDev-lab4/blob/main/images/num1/NAND/1.png)
![Image alt](https://github.com/cutterror/DA-in_gameDev-lab4/blob/main/images/num1/NAND/2.png)

- Запустила при значении Train(8)
- Перцептрон успешно обучился только после 8й этирации, получен правильный вывод, отражающий логику "не и":

![Image alt](https://github.com/cutterror/DA-in_gameDev-lab4/blob/main/images/num1/NAND/3.png)

#### XOR

- Аналогично создала элементы в Ts, описывающие логику XOR:

![Image alt](https://github.com/cutterror/DA-in_gameDev-lab4/blob/main/images/num1/XOR/1.png)
![Image alt](https://github.com/cutterror/DA-in_gameDev-lab4/blob/main/images/num1/XOR/2.png)

- Запустила при значении Train(8)
- Перцептрон не обучился с первого раза за 8 этираций, получен неправильный вывод:

![Image alt](https://github.com/cutterror/DA-in_gameDev-lab4/blob/main/images/num1/XOR/3.png)

- Перезапустила несколько раз при Train(8), перцептрон так же не обучался со значением totalError равным 4.
- Увеличила количество эпох до 16, 32, 100, 300, 1000. Значение totalError так же равно 4.
- Из информации, данной в лекции и найденной в интернете, ясно, что однослойного перцептрона для решения задачи XOR не достаточно.

![Image alt](https://github.com/cutterror/DA-in_gameDev-lab4/blob/main/images/num1/XOR/4.png)

#### .


## Задание 2
### Построить графики зависимости количества эпох от ошибки обучения. Указать от чего зависит необходимое количество эпох обучения.
Ход работы:
Составила графики зависимости значения TotalError(среднее количество от 10ти попыток) от количества эпох по задачам. 

- Для подсчёта статистики изменила скрипт, чтобы на консоль выводилось среднее значение TotalError за 1000 попыпок по переменному количеству эпох(от 1 до 15):

```py
	double sumTotalError = 0;

	void Train(int epochs)
	{
		InitialiseWeights();
		
		for(int e = 0; e < epochs; e++)
		{
			totalError = 0;
			for(int t = 0; t < ts.Length; t++)
			{
				UpdateWeights(t);
			}
			sumTotalError += totalError;
		}
	}

	void Start () {
		int attemptsNumber = 1000;
		int maxEpochsNumber = 15;
		for (int epoch = 1; epoch <= maxEpochsNumber; epoch++)
        	{
			for (int attempt = 0; attempt < attemptsNumber; attempt++)
			{
				Train(epoch);
			}
			double meanTotalError = sumTotalError / (attemptsNumber * epoch);
			Debug.Log("Mean total error over " + epoch + " epochs: " + meanTotalError);
			sumTotalError = 0;
		}
	}
```

Пример вывода (OR):

![Image alt](https://github.com/cutterror/DA-in_gameDev-lab4/blob/main/images/num2/1.png)

#### График OR

![Image alt](https://github.com/cutterror/DA-in_gameDev-lab4/blob/main/images/num2/2.png)

#### График AND

![Image alt](https://github.com/cutterror/DA-in_gameDev-lab4/blob/main/images/num2/3.png)

#### График NAND

![Image alt](https://github.com/cutterror/DA-in_gameDev-lab4/blob/main/images/num2/4.png)

#### Очевидно, что чем сложнее задача, тем большее количество эпох потребуется для обучения перцептрона, но так же успешность обучения зависит и от случайности. Из графиков можно сделать вывод, что AND и NAND более сложные задачи, чем OR.

## Задание 3
### Построить визуальную модель работы перцептрона на сцене Unity. 
Ход работы:

![Image alt](https://github.com/cutterror/DA-in_gameDev-lab4/blob/main/images/1.gif)

В данном эксперименте реализован оператор OR, сфера является условной единицей, а пол-- результирующий объект. При соприкосновении, выводит 1.

## Выводы

В ходе работы поняла принцип действия перцептрона, взаимосвязи его компонентов и процесса обучения. Реализовала перцептрон, который умеет производить вычисления: OR, AND, NAND. Убедилась в том, что однослойного перцептрона для решения задачи XOR не достаточно. Сделала вывод по поводу связи количества эпох, необходимых для обучения и ошибки обучения. Сделала свою визуальную модель работы перцепрона на сцене Unity.


| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| GitHub | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
