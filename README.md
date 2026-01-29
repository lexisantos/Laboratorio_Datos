### Tablas y sets de datos

* **gapminder**: datos poblacionales y de desarrollo humano mundial.
* **casos_coronavirus.csv**: casos nuevos confirmados en Argentina por fecha.
* **dowjones**: dataset en *seaborn* (```sns.load_dataset("dowjones")```). Datos de índice Dow Jones de las bolsas de Estados Unidos.
* **tips**: dataset en *seaborn*. Propinas en restaurantes en función de datos de clientes.
* **penguins**: dataset en *seaborn*. Información de pingüinos.

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

En Series (como un array de Numpy, pero indexado):
```
s = pd.Series()
s.values
s.index #Default: de 0 a N-1
s.iloc[] #por posición o rango de posiciones
s.loc[] #por etiquetas (index) o rango de et.
s[[index1, index6]] #seleccionar elementos mediante lista de index
s.isin([listofvalues]) #returns Serie de booleanos
s.unique()
s.nunique()
s.value_counts() #Cuenta values repetidos
```
Además de las funciones de agregación y estadística *min, max, median, mean, var, std* (~Numpy)

Con DataFrames:
```
df = pd.DataFrame(dict)
df.head()
df.info()
display(df)

df.set_index(column, inplace:bool)
df.column #o df["column"]
df.column.dtype
df.column.unique() #pues df.column es una Serie. También vale nunique()
df.loc[]
df.iloc[]

df.plot() #style = "."
```

Agrupar datos (parecido a una Serie):

```
df_agg = df.groupby(column)
df_agg.size() #tamaño de población por grupo
df_agg.column.nunique() #cuenta países distintos por continente
df_agg.column.unique() #lista de países distintos por continente
df[[col1, col2]].drop_duplicates() #pares únicos de col1, col2
df[[col1, col2]].drop_duplicates().col1.value_counts(normalize = True) #valores 0 a 1 en proporción de la cant de apariciones en la col1.

```

#### Clase 3. Visualización con Seaborn.

<img width="1514" height="890" alt="imagen" src="https://github.com/user-attachments/assets/0188e5dc-d7ad-4e0f-848d-be41dc108c87" />

```
(
    so.Plot(data = DataFrame, x = "column1", y = "column2",
            color = "column3", pointsize = "column4")
    .add(so.Line())
    #and/or .add(so.Dot()) | .add(so.Bar())
    .add(so.Dot(), so.Agg("sum")) #agreggate data ~ group by | sum, mean, median
)
```

Se puede ajustar la escala en x (ticks y tamaño de puntos) como:
```
.add(so.Dot()).scale(x = "log", pointsize = (5, 20)) #tamaño entre 5 y 20
```

Asignación de canales por capas:
```
(
    so.Plot(data = DataFrame, x = "column1", y = "column2")
    .add(so.Line(), color = "column3") #distinción por color *según datos* sólo en las lineas
    .add(so.Dot(color = "red")) #especificar el *color* dentro de so.Dot() o so.Line()
)


filtered_column: DataFrame[filter].column2
(
    so.Plot(data = DataFrame, x = "column1")
    .add(so.Line(), so.Agg("mean"), y = "column2") #promedio por valor de column1
    .add(so.Line(color = "red"), y = filtered_column) #valor por column1 para el rango elegido
)

```







