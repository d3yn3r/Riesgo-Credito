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
    * [CHI-Cuadrado](#chi-cuadrado)
    * [F ANOVA](#f-anova)
* [Matriz de correlación](#matriz-de-correlación)
* [ONE-HOT-ENCONDING y actualización del conjunto de datos de prueba](#one-hot-enconding)
* [WoE Binning e ingeniería de funciones](#woe-binning-e-ingeniería-de-funciones)
    * [WoE Binning](#woe-binning)
    * [Valor de la información-IV](#valor-de-la-información-iv)
* [Entrenamiento del modelo](#entrenamiento-del-modelo)
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

<a name = variables-redundantes-y-nulas></a>

### Variables redundantes y nulas
Inicialmente, empezamos eliminando las variables con una cantidad mayor al 80% de datos nulos, ya que estas no aportarían algo significativo al entrenamiento de nuestro modelo. Además, eliminamos las variables redundantes como id, member_id, title, etc. También eliminamos las variables prospectivas.

![Eliminación de variables nulas](https://github.com/d3yn3r/Riesgo-Credito/blob/main/imagenes/1.%20eliminacion%20de%20nulos.png)

IMAGEN 1: Eliminación de variables nulas

![Eliminación de variables redundantes](https://github.com/d3yn3r/Riesgo-Credito/blob/main/imagenes/2%20.eliminacion%20de%20variables%20redundantes%20y%20prospectivas.png)

IMAGEN 2: Eliminación de variables redundantes

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

<a name = división-de-los-datos></a>

### División de los datos

Realizar la división de los datos antes de cualquier limpieza, nos permite evitar cualquier fuga de los datos tanto del conjunto de prueba como el de entrenamiento, y con esto tener una evaluación más precisa del modelo. Los datos fueron dividos en 80% para datos de entrenamiento y 20% para datos de prueba; Como nos indica Asad Mumtaz en su artículo [[2] How to Develop a Credit Risk Model and Scorecard](https://towardsdatascience.com/how-to-develop-a-credit-risk-model-and-scorecard-91335fc01f03) se realizaran pruebas de plegado k estratificadas repetidas en la prueba de entrenamiento para evaluar preliminarmente nuestro modelo, mientras que el conjunto de prueba permanecerá intacto hasta la evaluación final del modelo. 

![Tendencia de la variable objetivo](https://github.com/d3yn3r/Riesgo-Credito/blob/main/imagenes/3.%20tendencia%20de%20las%20variables.png)

IMAGEN 3: Tendencia de la variable objetivo

Como podemos evidenciar en la variable objetivo, los datos tienden a estar fuertemente sesgados a buenos prestamos, por lo tanto, además de un muestreo aleatorio, se estratificara la división de los datos de prueba y entrenamiento con el fin de encontrar una misma distribución entre los datos, para esto se utilizó el parámetro train_test_split de la función .stratify.

Adicional a esto, a las columnas con datos de fechas, se le realizo una conversión a numéricas.

<a name = Selección-de-las-caracteristicas>

## Selección de las características

Utilizaremos las siguientes tecnicas con el fin de hayas las caracteristicas mas adecuadas para nuestro proyecto. primero usaremos la tecnica de CHI-cuadrado para las caracteristicas categoricas y F ANOVA para las caracteristicas numericas

<a name = chi-cuadrado></a>

### CHI-cuadrado
Al aplicar esta tecnica,podemos ver en la siguiente imagen los resultados, y escogeremos las cuatro caracteristicas principales.

![CHI-Cuadrado](https://github.com/d3yn3r/Riesgo-Credito/blob/main/imagenes/4.%20chi%20cuadrado.png)
IMAGEN 4: CHI-Cuadrado

<a name = f-anova></a>

### F ANOVA
Al aplicar esta tecnic, podemos ver en los resultados que muestra una amplia gama de valores para 32 funciones, con valores desde 23.513 hasta 0.39, pero inicialmente escogeremos las 20 caracteristicas principales.

![F ANOVA](https://github.com/d3yn3r/Riesgo-Credito/blob/main/imagenes/5.%20f%20anova.png)
IMAGEN 5: F ANOVA

<a name = matriz-de-correlación></a>

## Matriz de correlación

Procedemos a realizar una matriz de correlacion entre las 20 variables principales, y en esta encontramos que las variables out_prncp_inv y total_pymnt_inv tienen una alta correlacion, por lo tanto las eliminaremos del conjunto de datos.

![Matriz de correlación](https://github.com/d3yn3r/Riesgo-Credito/blob/main/imagenes/6.%20matriz%20de%20correlacion.png)
IMAGEN 6: Matriz de correlación

<a name = one-hot-enconding></a>

## One-Hot-Enconding y actualización del conjunto de datos de prueba 

Con esta tecnica se crearan 4 variables nuevas categoricas y se aplicaran todas la tecnicas usadas en el conjunto de datos de entrenamiento al conjunto de datos de prueba.
La creacion de las nuevas variables se hace con el fin de calcular los pesos de WoE y los valores de informacion IV de las categorias, primero se creara un conjunto de datos con las variables nuevas y posteriormente se unira a los conjuntos de datos de prueba y entrenamiento.

<a name = woe-binning-e-ingeniería-de-funciones></a>

## WoE Binning e ingeniería de funciones
La creacion de las nuevas variables es uno de los pasos mas criticos en el desarrollo del modelo, esta se hara de forma manual con el fin de tener un mayor control sobre el proceso de creacion del modelo.

<a name = woe-binning></a>

### WoE Binning
WoE Binning es una de las tecnicas usadas para la seleccion de caracteristicas en modelos de riesgo crediticio; WoE es una medida predictiva de una variable independiente en relacion con la variable objetivo y mide el grado en que una caracteristica especifica puede difrerenciarse en la variable objetivo, Un WoE positivo significa que la proporción de buenos clientes es mayor que la de malos clientes y viceversa para un valor de WoE negativo.

La fórmula para calcular WoE es la siguiente:

![formula de WoE](https://github.com/d3yn3r/Riesgo-Credito/blob/main/imagenes/7.%20woe.png)
IMAGEN 7: Formula de calculo de WoE

<a name = valor-de-la-informacion-iv></a>

### Valor de la información-IV
IV nos ayuda a clasificar las funciones en funcion de su importancia relativa pero esta IV solo es útil como técnica de selección e importancia de características cuando se utiliza un modelo de regresión logística binaria.

IV se calcula de la siguiente manera:

![formula iv](https://github.com/d3yn3r/Riesgo-Credito/blob/main/imagenes/8.%20iv%201.png)
IMAGEN 8: Formula de calculo de IV

Según Siddiqi², por convención, los valores de IV en la calificación crediticia se interpretan de la siguiente manera:

![tabla iv](https://github.com/d3yn3r/Riesgo-Credito/blob/main/imagenes/9.%20iv%20tabla.jpeg)

IMAGEN 9: Tabla de valores para IV

Realizando los calculos de WoE binning y IV, utilizaremos 3 funciones:
* calcular y mostrar valores WoE e IV para variables categóricas
* calcular y mostrar valores WoE e IV para variables numéricas
* trazar los valores de WoE contra los contenedores para ayudarnos a visualizar WoE y combinar contenedores de WoE similares

luego de realizar los calculos procedemos a visualizar los resultados obtenidos.

![resultados woe y iv](https://github.com/d3yn3r/Riesgo-Credito/blob/main/imagenes/10.%20woe%20y%20iv%20resultados.png)
IMAGEN 10: resultados woe y iv


![grafico woe x grado](https://github.com/d3yn3r/Riesgo-Credito/blob/main/imagenes/11.%20grafica%20woe%20x%20grado.png)
IMAGEN 11: grafico WoE por Grado

Podemos ver en el gráfico anterior que hay un aumento continuo en WoE en los diferentes grados. Por lo tanto, no necesitamos combinar ninguna característica y deberíamos dejar estos 7 grados como están.

Luego de visualizar los resultados de WoE y IV, procedemos a realizar la selección de las variables que se dejaran y las que se eliminaran segun el resultado de IV, posteriormente las caracteristicas pre-seleccionadas seran tratadas de la siguiente manera.

* No se combinaran o crearan categorias dado el WoE discreto y monotono y la ausencia de valores faltantes: grade, verification_status,term.
* Combine contenedores WoE con observaciones muy bajas con el contenedor vecino: home_ownership, purpose.
* Combine contenedores de WoE con valores de WoE similares, posiblemente con una categoría faltante separada: int_rate, annual_inc, dti, inq_last_6mths, revol_util, out_prncp, total_pymnt, total_rec_int, total_rev_hi_lim, mths_since_earliest_cr_line, mths_since_issue_d,mths_since_last_credit_pull_d.
* Ignorar características con un valor IV bajo o muy alto: emp_length, total_acc, last_pymnt_amnt, tot_cur_bal,mths_since_last_pymnt_d_factor.

Para ciertas caracteristicas de variables numericas con valores atipicos, se calculara y trazara WoE, seran excluidos y luego asignados a una categoría separada propia.

luego de explorar las funciones e identificar las categorías que se crearán, definiremos una clase de 'transformador' personalizado utilizando las clases de la libreria sci-kit learn's, "BaseEstimatory" y "TransformerMixin".

<a name = entrenamiento-del-modelo></a>

## Entrenamiento del modelo

Para el entrenamiento de nuestro modelo ajustaremos un modelo de regresión logística en nuestro conjunto de entrenamiento y lo evaluaremos usando "RepeatedStratifiedKFold", para esto se ha definido que el parametro "class_weight" de la "LogisticRegressionclase" sea "balanced". Esto obligará al modelo de regresión logística a aprender los coeficientes del modelo mediante el aprendizaje sensible a los costos, es decir, penalizar los falsos negativos más que los falsos positivos durante el entrenamiento del modelo. El aprendizaje sensible a los costos es útil para conjuntos de datos desequilibrados, que suele ser el caso en la calificación crediticia.

Nuestra métrica de evaluación será el área bajo la curva, característica operativa del receptor (AUROC), una métrica ampliamente utilizada y aceptada para la calificación crediticia. RepeatedStratifiedKFold dividirá los datos mientras conserva el desequilibrio de clase y realizará la validación k-fold varias veces.

Después de realizar la validación de k-pliegues en nuestro conjunto de entrenamiento y estar satisfechos con AUROC, ajustaremos la canalización en todo el conjunto de entrenamiento y crearemos una tabla de resumen con los nombres de las funciones y los coeficientes devueltos por el modelo.

Nuestro AUROC en el conjunto de pruebas sale a 0.866 con un Gini de 0.732, ambos considerados como puntajes de evaluación bastante aceptables.

![ROC curve](https://github.com/d3yn3r/Riesgo-Credito/blob/main/imagenes/12.%20roc%20curve.png)

IMAGEN 12: ROC Curve


![PR Curve](https://github.com/d3yn3r/Riesgo-Credito/blob/main/imagenes/13%20pr%20curve.png)

IMAGEN 13: PR Curve


Teniendo el modelo ya entrenado, este nos devuelve valores binarios 0 para negar un credito y 1 para aprobarlo, luego procedemos a realizar un caso de prueba, para esto creamos un conjunto de datos y lo evaluamos con el modelo entrenado y obtuvimos los resultados esperados.

![Caso de prueba](https://github.com/d3yn3r/Riesgo-Credito/blob/main/imagenes/14.%20dataframe%20de%20prueba.png)
IMAGEN 14: Caso de prueba

![Aplicando caso de prueba](https://github.com/d3yn3r/Riesgo-Credito/blob/main/imagenes/15%20prediccion%202.png)
IMAGEN 15: Aplicando el modelo

![Resultados de caso de prueba](https://github.com/d3yn3r/Riesgo-Credito/blob/main/imagenes/16%20prediccion%203.png)
IMAGEN 16: Resultado de predección


<a name = aplicacion ></a>

## Aplicación

[Pagina web de la aplicación]()

<a name = video-promocional></a>

## Video promocional

[URL del video promocional]()

<a name = conclusion></a>

## Conclusión


<a name = referencias-bibliograficas> </a>

## Referencias Bibliográficas

[[1] How to Quantify Risk and Creditworthiness](https://medium.com/swlh/how-to-quantify-risk-and-creditworthiness-c76725bc2380)<br>
[[2] How to Develop a Credit Risk Model and Scorecard](https://towardsdatascience.com/how-to-develop-a-credit-risk-model-and-scorecard-91335fc01f03)<br>
[[3] How to Develop a Credit Risk Model and Scorecard : notebook github](https://github.com/finlytics-hub/credit_risk_model/blob/master/Credit_Risk_Model_and_Credit_Scorecard.ipynb)<br>
[[4] Intro to Credit Scorecard](https://towardsdatascience.com/intro-to-credit-scorecard-9afeaaa3725f)<br>
[[5] A COMPLETE GUIDE TO CREDIT RISK MODELLING](https://www.listendata.com/2019/08/credit-risk-modelling.html)<br>
[[6] Baesens, B., Roesch, D. y Scheule, H. (2016). Análisis de riesgo de crédito: Técnicas de medición, aplicaciones y ejemplos en SAS. John Wiley & Sons](https://github.com/d3yn3r/Riesgo-Credito)<br>
[[7] Siddiqi, N. (2012). Scorecards de riesgo crediticio: desarrollo e implementación de puntajes crediticios inteligentes. John Wiley & Sons](https://github.com/d3yn3r/Riesgo-Credito)<br>