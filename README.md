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

Relación entre Departamento y Estancia
~~~
def over_8(x):
    return x>8
for a in df['Department'].unique().tolist():
    hiro=df[df.Department.isin([a])]['Stay (in days)'].tolist()
    print('Probabilidad de quedarse por',a,':',sum(over_8(x) for x in hiro)/len(hiro))
~~~
Output:
* Probabilidad de quedarse por gynecology : 0.5475685752330226 
* Probabilidad de quedarse por anesthesia : 1.0
* Probabilidad de quedarse por TB & Chest disease : 0.9650464807436919
* Probabilidad de quedarse por radiotherapy : 0.6165596250650929
* Probabilidad de quedarse por surgery : 0.9995201535508638




### 2. Aplicación del modelo.




### 3. Conclusión.


