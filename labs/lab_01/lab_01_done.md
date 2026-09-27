# Лабораторная работа 1. Пакет NumPy


```python
import numpy as np
score = 0
```

## Задание 1


```python
def max_after_zero(x: np.array) -> int:
    """
    Задание: найти максимальный элемент массива среди элементов, которым предшествует ноль
      
    Вход: np.array([0, 2, 0, 3])
    Выход: 3
    """
    maska = x[:-1] == 0
    return np.max(x[1:][maska])
```


```python
%%capture

x = np.array([0, 1, 12, 0, 6, 0, 10, 0])
assert max_after_zero(x) == 10, 'Тест не пройден'

x = np.array([0, 3, 2, 0, 8, 0, 1, 10])
assert max_after_zero(x) == 8, 'Тест не пройден'

print("Выполнено")
score += 1
```

## Задание 2


```python
def block_matrix(block: np.array) -> np.array:
    """
    Задание: построить блочную матрицу из четырех блоков, где каждый блок представляет собой заданную матрицу

    Вход: np.array([[1, 2], [3, 4]])
    Выход: np.array([[1, 2, 1, 2],
                     [3, 4, 3, 4],
                     [1, 2, 1, 2],
                     [3, 4, 3, 4]])
    """
    return np.block([[block, block], [block, block]])
```


```python
%%capture

block = np.array([[1, 3, 3], [7, 0, 0]])
assert np.allclose(
    block_matrix(block),
    np.array([[1, 3, 3, 1, 3, 3],
              [7, 0, 0, 7, 0, 0],
              [1, 3, 3, 1, 3, 3],
              [7, 0, 0, 7, 0, 0]])
), 'Тест не пройден'

print("Выполнено")
score += 1
```

## Задание 3


```python
def diag_prod(matrix: np.array) -> int:
    """
    Задание: вычислить произведение всех ненулевых диагональных элементов квадратной матрицы

    Вход: np.array([[3, 5, 1, 4],
                    [6, 2, 7, 9],
                    [3, 6, 0, 8],
                    [1, 3, 4, 6]])
    Выход: 36
    """
    diag = np.diag(matrix)
    return np.prod(diag[diag != 0])
```


```python
%%capture

matrix = np.array([[0, 1, 2, 3],
                   [4, 5, 6, 7],
                   [8, 9, 10, 11],
                   [12, 13, 14, 15]])
assert diag_prod(matrix) == 750, 'Тест не пройден'

print("Выполнено")
score += 1
```

## Задание 4


```python
from typing import Tuple

class StandardScaler:
    """
    Задание: класс реализует StandardScaler из библиотеки sklearn
    
    см. https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html
    В качестве входных данных метод fit принимает матрицу, в которой признаки объектов расположены в столбцах 
    Метод fit должен вычислять среднее значение (mean_) и дисперсию (var_) для каждого из признаков (столбца), 
    и сохранять их в атрубутах объекта self.mean_ и self.var_ соответственно.    
    
    Метод transform должен нормализовать матрицу с помощью предварительно вычисленных mean_ и sigma, 
    где sigma = sqrt(var_) - среднеквадратическое отклонение

    Вход: np.array([[2, 603, 250], 
                    [1, 154, 500], 
                    [7, 893, 350]])
    Выход: np.array([[-0.50800051,  0.17433393, -1.13554995],
                     [-0.88900089, -1.3025705 ,  1.29777137],
                     [ 1.3970014 ,  1.12823657, -0.16222142]])
    """
        
    def fit(self, X: np.array) -> None:
        self.mean_ = np.mean(X, axis=0)
        self.var_ = np.var(X, axis=0)

    def transform(self, X: np.array) -> np.array:
        return (X - self.mean_) / np.sqrt(self.var_)
```


```python
%%capture

matrix = np.array([[1, 4, 4200], [0, 10, 5000], [1, 2, 1000]])

scaler = StandardScaler()
scaler.fit(matrix)

assert np.allclose(
    scaler.mean_,
    np.array([0.66667, 5.3333, 3400])
), 'Тест не пройден. Некорректное значение scaler.mean_'

assert np.allclose(
    scaler.var_,
    np.array([0.22222, 11.5556, 2986666.67])
), 'Тест не пройден. Некорректное значение scaler.var_'

assert np.allclose(
    scaler.transform(matrix),
    np.array([[ 0.7071, -0.39223,  0.46291],
              [-1.4142,  1.37281,  0.92582],
              [ 0.7071, -0.98058, -1.38873]])
), 'Тест не пройден. Некорректный результат scaler.transform(matrix)'


print("Выполнено")
score += 1
```

## Задание 5


```python
def antiderivative(coefs: np.array, const: float) -> np.array:
    """
    Задание: Вычислить первообразную полинома

    coefs - массив коэффициентов полинома
    const - произвольная постоянная
    Массив коэффициентов [6, 0, 1] соответствует 6x^2 + 0x^1 + 1
    Соответствующая первообразная будет иметь вид: 2x^3 + 0x^2 + 1x + const,
    В результате получается массив коэффициентов [2, 0, 1, const]
        
    Вход: [8, 12, 8, 1], 42
    Выход: [2., 4., 4., 1., 42.]
    """
    n = len(coefs)
    stepeni = np.arange(n, 0, -1)
    return np.append(coefs / stepeni, const)
```


```python
%%capture

coefs = np.array([4, 6, 0, 1])
const = 42.0
assert np.allclose(
    antiderivative(coefs, const),
    np.array([1., 2., 0., 1., 42.])
), 'Тест не пройден.'

coefs = np.array([1, 7, -12, 21, -6])
const = 42.0
assert np.allclose(
    antiderivative(coefs, const),
    np.array([ 0.2, 1.75, -4., 10.5, -6., 42.])
), 'Тест не пройден.'

print("Выполнено")
score += 1
```


```python
print('Итоговый балл:', score)
```

    Итоговый балл: 5



```python

```
