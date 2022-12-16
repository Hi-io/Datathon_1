# Datathon
Modelo predictivo de las estancias hospitalarias, predice si un paciente se va a menos de 8 días o más de 8 días.

![c005fb97ec133a7cc8515e04c96ffa8f-hospital-simple-building](https://user-images.githubusercontent.com/110522512/208148963-666019bd-c5a0-4531-88cf-4cc345b84010.png)

## Descripción del problema
Un importante Centro de Salud lo ha contratado con el fin de poder predecir si un paciente tendrá una estancia hospitalaria prolongada o no, utilizando la información contenida en el dataset asociado, la cual recaba una muestra histórica de sus pacientes, para poder administrar la demanda de camas en el hospital según la condición de los pacientes recientemente ingresados.

Para esto, se define que un paciente posee estancia hospitalaria prolongada si ha estado hospitalizado más de 8 días. Por lo que debe generar dicha variable categórica y luego categorizar los pacientes según las variables que usted considere necesarias, justificando dicha elección.

## Proceso

### 1. Análisis exploratorio de los datos (EDA)

Procedemos con el análisis de los datos, para este caso lo que yo hice fue buscar relación entre los 14 parámetros para encontrar algún patrón.
Esto se puede hacer graficando la data o incluso filtrando el mismo dataframe.




Estos son algunos de los patrones encontrados en el EDA:

#### Relación entre Departamento y Estancia:
~~~
def over_8(x):
    return x>8
for a in df['Department'].unique().tolist():
    hiro=df[df.Department.isin([a])]['Stay (in days)'].tolist()
    print('Probabilidad de quedarse por',a,':',sum(over_8(x) for x in hiro)/len(hiro))

Output:
Probabilidad de quedarse por gynecology : 0.5475685752330226 
Probabilidad de quedarse por anesthesia : 1.0
Probabilidad de quedarse por TB & Chest disease : 0.9650464807436919
Probabilidad de quedarse por radiotherapy : 0.6165596250650929
Probabilidad de quedarse por surgery : 0.9995201535508638
~~~

#### Relación entre Pacientes de Ginecología y Habitaciones disponibles:
~~~
extra={}
for i in df["Available Extra Rooms in Hospital"].unique().tolist():
    extra[i]=len(df[df['Stay (in days)']<8][df["Available Extra Rooms in Hospital"]==i][df['Department']=='gynecology'])
extra

Output:
{4: 10275,
 2: 9321,
 7: 193,
 3: 9596,
 5: 3573,
 10: 37,
 6: 858,
 1: 925,
 21: 0,
 8: 66,
 11: 47,
 13: 0,
 9: 17,
 14: 0,
 24: 0,
 0: 50,
 12: 51,
 20: 0}
~~~

#### Relación entre Departamento y Facility Code:

![a1a44f9a-dcb7-4d52-8201-9061e55d85b7](https://user-images.githubusercontent.com/110522512/208150424-4ada72da-6133-4e8b-a172-407986c1bbd6.jpg)

#### Relación entre Visitas y el género:
![download](https://user-images.githubusercontent.com/110522512/208150592-574d8b13-32bd-4d87-ba66-79caa67ff1a9.png)

#### Relación entre el seguro y los días:
![a183c735-6a8f-477b-be63-630155ac6b40](https://user-images.githubusercontent.com/110522512/208150721-a7f403ea-ccdd-45cf-8ce8-897ee71d10e2.jpg)

Un tip para encontrar relaciones rápidamente:

Si usamos el arbol de decisiones en el dataset, el algoritmo será capaz de encontrar los patrones de una forma muy rápida, podemos hacer un **tree.plot_tree( )** para revisar los descartes y acelerar el proceso del EDA.



Estos son los patrones que se pueden encontrar a simple vista, los patrones en 3 o más dimensiones que talvez no somos capaces de ver, lo podrá encontrar el modelo KNN, ya que este trabajará con los 14 parámetros a la vez.


### 2. Aplicación del modelo

Para este modelo vamos a usar un el algoritmo de  K-Nearest-Neighbors, para ver más detalles se pueden ver en el Notebook.

Preferimos usar este modelo antes que el arbol de decisiones por ser capaz de encontrar  patrones multidimensionales, mientras que el decision tree compara sólo 2 dimensiones a la vez, el KNN es capaz de encontrar patrones entre los 14 parámetros a la vez.



![44207644-59d9-439b-86a7-8608de7b2432](https://user-images.githubusercontent.com/110522512/208154605-ae0d2412-b752-41f2-b1cb-1bd30ba49f19.jpg)



Lo que hace el modelo es primero identificar a los parámetros que te descarten con un 0 de entropy
Este dato se consigue de 2 maneras:
* Con un buen proceso de EDA
* Aplicando el Decision Tree y revisar sus parámetros para las ramificaciones

![image](https://user-images.githubusercontent.com/110522512/208153566-8caef8f4-8ab8-4aee-94ec-c0931c32b302.png)
 Por ejemplo en nuestor caso, usamos el Decision Tree para poder encontrar patrones mas no lo aplicamos en el modelo final.
 ahí podemos encontrar cómo va descartando por X[6] que significa edad, o X[1] que significa Departamento.
 
 Al tenerlos identificados, vamos a guardar los datos para poder filtrarlos después.
 
 ¿Porqué no lo filtramos ahora?

Podríamos filtrarlo ahora pero eso reduciría la muestra y el KNN sería menos preciso, por lo que vamos a:

* 1. Identificar las variables con 0 de Entropy y las guardamos para usarlas después.
* 2. Entrenar el modelo usando KNN (conseguimos el número K haciendo una iteración del 1 al 30)
* 3. Aplicamos la predicción del KNN
* 4. Aplicamos el filtro con las variables guardadas.


![df1b8852-3c10-4e11-9b41-cd07fbe2eb32](https://user-images.githubusercontent.com/110522512/208157688-aa39d61f-0b59-4e71-ab2d-db0ff9d53337.png)

Entonces, usando el KNN y filtrando data conseguida a partir del Decision Tree y el EDA:

Logramos conseguir un 70% de precisión en el modelo de predicción.


