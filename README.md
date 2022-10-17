# Análisis de riesgo crediticio
#### Andrés Camilo García Moreno - Ingeniería de sistemas e informatica 
#### Deyner Elías López Pineda - Ingeniería de sistemas e informatica 

## Tabla de contenido
* [Introducción](#introduccion)
* [DataSet](#dataset)
* [Pre-Procesamiento de datos](#pre-procesamiento-de-datos)
    * [Variables redundantes y nulas](#variables-redundandes-y-nulas)
    * [División de los datos](#división-de-los-datos)
* [Selección de las características](#seleccion-de-las-caracteristicas)
* [Aplicación](#aplicacion)
* [Video promocional](#video-promocional)
* [Conclusión](#conclusion)
* [Referencias Bibliográficas](#referencias-bibliograficas)

<a name = introduccion></a>

## Introducción



<a name = dataset></a>

## DataSet

Los datos se encuentran en formato CSV y cuenta con aproximadamente 466.285 registros y 74 variables, que representan las características de individuos que realizaron prestamos de dinero y características del comportamiento respecto al pago de los prestamos aprobados en el periodo comprendido entre 2007 y 2014, los datos fueron recopilados por la compañía LENDING CLUB, la cual es una compañía estadounidense que se dedica a realizar prestamos entre particulares (No requieren intervención de una institución financiera).

<a name = pre-procesamiento-de-datos></a>

## Pre-Procesamiento de datos

### Variables redundantes y nulas
Inicialmente, empezamos eliminando las variables con una cantidad mayor al 80% de datos nulos, ya que estas no aportarían algo significativo al entrenamiento de nuestro modelo. Además, eliminamos las variables redundantes como id, member_id, title, etc. También eliminamos las variables prospectivas.

![Eliminación de variables nulas](https://github.com/d3yn3r/Riesgo-Credito/blob/main/imagenes/1.%20eliminacion%20de%20nulos.png)

![Eliminación de variables redundantes](https://github.com/d3yn3r/Riesgo-Credito/blob/main/imagenes/2%20.eliminacion%20de%20variables%20redundantes%20y%20prospectivas.png)

Posteriormente, identificamos cual es nuestra variable objetivo, en nuestro caso es "loan_status" esta es una variable categórica y nos indica el estado del crédito, en ella se encuentran los siguientes estados:

* Current                                                
* Fully Paid                                             
* Charged Off                                            
* Late (31-120 days)                                     
* In Grace Period                                        
* Does not meet the credit policy. Status:Fully Paid     
* Late (16-30 days)                                      
* Default                                                
* Does not meet the credit policy. Status:Charged Off    

Al conocer los posibles estados de la variable objetivo, separamos los tipos de estado en buenos o malos y con esto creamos una nueva variable que indique el estado del crédito, 0 para variables con una connotación "mala" o también conocido como en mora y 1 para las variables con una connotación "buena"

Estas variables fueron consideradas con un estado "Bueno" o 1:
* Current                                                
* Fully Paid
* In Grace Period
* Does not meet the credit policy. Status:Fully Paid
* Late (16-30 days)

Estas variables fueron consideradas con un estado "Malo" o 1:
* Charged Off
* Default
* Late (31-120 days)
* Does not meet the credit policy. Status:Charged Off


### División de los datos

Realizar la división de los datos antes de cualquier limpieza, nos permite evitar cualquier fuga de los datos tanto del conjunto de prueba como el de entrenamiento, y con esto tener una evaluación más precisa del modelo. Los datos fueron dividos en 80% para datos de entrenamiento y 20% para datos de prueba; Como nos indica Asad Mumtaz en su artículo [[2] How to Develop a Credit Risk Model and Scorecard](https://towardsdatascience.com/how-to-develop-a-credit-risk-model-and-scorecard-91335fc01f03) se realizaran pruebas de plegado k estratificadas repetidas en la prueba de entrenamiento para evaluar preliminarmente nuestro modelo, mientras que el conjunto de prueba permanecerá intacto hasta la evaluación final del modelo. 

![Tendencia de la variable objetivo](https://github.com/d3yn3r/Riesgo-Credito/blob/main/imagenes/3.%20tendencia%20de%20las%20variables.png)

Como podemos evidenciar en la variable objetivo, los datos tienden a estar fuertemente sesgados a buenos prestamos, por lo tanto, además de un muestreo aleatorio, se estratificara la división de los datos de prueba y entrenamiento con el fin de encontrar una misma distribución entre los datos, para esto se utilizó el parámetro train_test_split de la función .stratify.

Adicional a esto, a las columnas con datos de fechas, se le realizo una conversión a numéricas.

## Selección de las características


<a name = referencias-bibliograficas> </a>
## Referencias Bibliográficas

[[1] How to Quantify Risk and Creditworthiness](https://medium.com/swlh/how-to-quantify-risk-and-creditworthiness-c76725bc2380)
[[2] How to Develop a Credit Risk Model and Scorecard](https://towardsdatascience.com/how-to-develop-a-credit-risk-model-and-scorecard-91335fc01f03)
[[3] How to Develop a Credit Risk Model and Scorecard : notebook github](https://github.com/finlytics-hub/credit_risk_model/blob/master/Credit_Risk_Model_and_Credit_Scorecard.ipynb)
[[4] Intro to Credit Scorecard](https://towardsdatascience.com/intro-to-credit-scorecard-9afeaaa3725f)
[[5] A COMPLETE GUIDE TO CREDIT RISK MODELLING](https://www.listendata.com/2019/08/credit-risk-modelling.html)