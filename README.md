## Tablas y sets de datos

* **gapminder**: datos poblacionales y de desarrollo humano mundial.
* **casos_coronavirus.csv**: casos nuevos confirmados en Argentina por fecha.
* **dowjones**: dataset en *seaborn* (```sns.load_dataset("dowjones")```). Datos de índice Dow Jones de las bolsas de Estados Unidos.
* **tips**: dataset en *seaborn*. Propinas en restaurantes en función de datos de clientes.
* **penguins**: dataset en *seaborn*. Información de pingüinos.

## Funciones vistas por clase
### Clase 1. Numpy.

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

### Clase 2. Pandas.

En **Series** (como un array de Numpy, pero indexado):
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

Con **DataFrames**:
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

### Clase 3. Visualización con Seaborn.

<img width="500" height="300" alt="imagen" src="https://github.com/user-attachments/assets/0188e5dc-d7ad-4e0f-848d-be41dc108c87" />

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

**Histograma**:

Si ```s``` corresponde a una Serie con los datos agrupados por categoría.

```
(
    so.Plot(x = s.index, y = s.values)
    .add(so.Bar())
)
```

Pero podría ser un DataFrame, y hace las cuentas según ```column1```:

```
(
    so.Plot(data = df, x = "column1")
    .add(so.Bar(), so.Hist()) #so.Hist(bins = ??), so.Bars() si no quiero que haya tanto espacio entre columnas
    .label(x = "xLabel", y = "yLabel")
)
```

**BoxPlots**:
Cálculo de cuartiles en una Serie *s*: ```s.quantile(.25, interpolation = "midpoint") #o 0.50 o 0.75```


```
s.name = "ColumnName"
(
    sns.boxplot(x = s)
)
plt.show()

```

Para clasificar por colores según columna:
```
sns.boxplot(data = df, x="column1 or index", y = "column2", hue = "column3")
plt.show()
```

**Subplots**:
```
fig, ax =plt.subplots(1,2)  #Grilla de dos gráficos.
#fig.set_figwidth(12)

#Histograma vs boxplot
(
    so.Plot(data = df, x = "column")
    .add(so.Bars(), so.Hist())
    .on(ax[0]).plot()  # Primera casilla de la grilla.
)

sns.boxplot(df, x="column", ax = ax[1])    #Segunda casilla de la grilla.
plt.show()

```

### Clase 4. Regresión Lineal.
Puede hacerse usando *Polyfit* de Seaborn:

```
(
    so.Plot(data = df, x, y)
    .add(so.Dot())
    .add(so.Line(color, linewidth), so.Polyfit(1), label = 'Label')
    .label(title, x, y)
)
```

Los estimadores de los parámetros de ajuste se pueden calcular como


<img width="206" height="140" alt="image" src="https://github.com/user-attachments/assets/744f7233-a01e-42fa-9843-8d369dc74232" />


```
b1_est = ((x-x.mean())*(y-y.mean())).sum()/(((x-x.mean())**2).sum())
b0_est = y.mean() - b1_est*x.mean()
```

Una forma más rápida de calcularlas, y de a su vez obtener las métricas, es usando la biblioteca *scikit-learn*.

```
from sklearn import linear_model #modelos lineales
from sklearn.metrics import mean_squared_error, r2_score, root_mean_squared_error

model = linear_model.LinearRegression()
model.fit(X, y) #X debe ser matriz o DataFrame. X = df[["column"]] | Pueden calcularse varias relaciones lineales respecto a una misma tira de datos 'y'

print("Coeficientes (b1's): ", modelo.coef_) #pendientes | list[float]
print("Intercept (b0): ", modelo.intercept_) #ord. al origen

vars(model) #todas las variables definidas luego del ajuste.
```

Para usar estos valores en la función y predecir nuevos resultados:

```
model.predict(df_nuevo)

#o usar los datos en x para saber cómo predice lo conocido/medido:

y_pred = model.predict(X) #ajuste lineal sobre datos
```

Cálculo de **R²**:
```
model.score(X, y)

#o también

r2_score(y, y_pred)
```

**Error cuadrático medio**:
```
mean_squared_error(y, y_pred)



root_mean_squared_error(y, y_pred)
```

### Clase 5. Cuadrados mínimos.

Usamos *Formulaic* para crear la matriz X. Si queremos 

```
from formulaic import Formula

Formula('y ~ poly(x, 3, raw=True) - 1').get_model_matrix(df_data)

#-1 quiere decir que no se considera el intercept (no hay col de 1's).
```

Si queremos modelar una oscilación con deriva lineal en función del mes:

```
mes = np.arange(521)
X = pd.DataFrame(
    {
        "mes": mes,
        "mes2": mes**2,
        #"mes3": mes**3,
        "sinx": np.sin(2*np.pi*mes/12),
        "cosx": np.cos(2*np.pi*mes/12)
        #"sin13x": np.sin(2*np.pi*mes/13),
        #"cos13x": np.cos(2*np.pi*mes/13)
    },
    index=df.index
)
```
<img width="360" height="267" alt="image" src="https://github.com/user-attachments/assets/644442fe-d983-44ae-bd0e-740cc994bf43" />

<img width="360" height="267" alt="image" src="https://github.com/user-attachments/assets/82334b4b-5f0e-4e99-a484-91ee29a2e284" />

### Clase 6. Introducción a SQL con SQLite.

```
import sqlite3
con = sqlite3.connect(":memory:") #Crear conexión a base de datos en la memoria RAM
```

Para pasar un DataFrame *df* a SQL (ineficiente si se quiere volver a DataFrame)

```
df.to_sql("sql-table-name", con, index=False, if_exists="replace")
```

Las consultas se hacen usando Pandas. Revisar [este repositorio](https://github.com/lexisantos/SQLforDataScience) con apuntes del curso de SQL. Un ejemplo genérico:

```
pd.read_sql_query("""
SELECT
    column1,
    column2,
    SUM(column3) AS alias_agg
FROM sql-table-name
WHERE condition
GROUP BY any column
""", con) #elegir la conexión donde se encuentra la tabla.
```

Esta consulta devuelve un DataFrame con la tabla filtrada.


### Clase 7. Limpieza de datos.
