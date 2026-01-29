### Funciones vistas por clase
#### Clase 1. Numpy.

Sobre vectores:

```
v = np.array()
v.dtype
(v > 3) | (w < 3.5)
(v > 3) & (w < 3.5)

#if A and B are matrices (vector of vectors)
A@B
B.T@A.T
```

Aparte de las funciones de agregación (*sum, mean, min, max*), tenemos:

```
np.float64()
np.arange()
np.round()
np.floor()
np.sort()
np.all()
np.random.choice(x, size:int, replace:bool)
```

#### Clase 2. Pandas.

En Series:
```
s = pd.Series()
s.values
s.index #Default: de 0 a N-1
s.iloc[] #por posición o rango de posiciones
s.loc[] #por etiquetas (index) o rango de et.
s[[index1, index6]] #seleccionar elementos mediante lista de index
s.isin([listofvalues]) #returns Serie de booleanos
s.value_counts() #Cuenta values repetidos
```

Con DataFrames:













