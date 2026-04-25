# Web-Phishing-Detector

## Abstract
El robo, la estafa y la extorsión han sido actividades que se han llevado a cabo desde hace mucho tiempo. Con el avance de las tecnologías, dichas actividades no han cesado, sino que han aumentado y evolucionado para perdurar y volverse más complejas. El phishing es una de estas evoluciones; consiste en la suplantación de identidad de una persona o institución, con el fin de robar datos, obtener dinero o algún beneficio egoísta. Todo esto usando ingeniería social y diferentes tecnologías, ampliando su alcance y su efectividad. 
Existen distintos tipos de Phishing:
1. Voice Phishing (Llamadas de voz)
2. SMS Phishing (Por mensajes de texto corto o sistemas de mensajería)
3. Web Phishing (Páginas web)
4. Spear phishing (Dirigido a una persona específica)
5. Phishing por correo electrónico

Ahora más que nunca se necesitan métodos para identificar de manera eficiente y segura el phishing. Este proyecto usa un modelo de redes neuronales para la identificación y clasificación binaria de sitios web maliciosos a partir de un dataset sacado de UC Irvine Machine Learning Repository. Pero debido a la caída reciente del servidor, se ha seleccionado una copia que había en Kaggle. Dicho dataset se llama Phishing Websites.

El objetivo del proyecto es crear un MLP que reduzca el riesgo a usuarios finales de entrar a un sitio web malicioso con phishingpor medio de la identificación y clasificación de estos, que son el origen de los ataques, antes de que el usuario pueda ingresar a ellos. 

## Origen del dataset
1. Dataset obtenido de UC Irvine Machine Learning Repository. Extraído de Phishing Websites - UCI Machine Learning Repository (sin embargo, no usado por la caída del servidor).

2. Copia del dataset obtenido de Kaggle: Website Phishing <-- éste es el que usa el proyecto.

## Descripción del dataset

Tipo de dataset: Tabular/clasificación

Número de instancias: 11054

Features: 30

¿Tiene valores faltantes?: No

Sitios legítimos: 6157 instancias —-> 55.6992% 

Sitios phishing: 4897 instancias —-> 44.3007%

| ![Distribución Inicial](https://github.com/user-attachments/assets/92b2884b-4b81-4e4a-9533-644f59bbfa37) |
| :--: |
| **Figura 1:** Distribución de clases antes de la generación de datos con SMOTE. |


Hay un desequilibrio del 11.3985%. Al estar desequilibrado el set de datos, el modelo tendrá un sesgo y tenderá a detectar más sitios legítimos que sitios de phishing, lo que provocará falsos positivos en la detección. Por lo que se tendrán que aplicar técnicas de generación de datos para poder equilibrar en un 50/50 el dataset. La tecnica de generación de datos que se usará es SMOTE


El dataset contiene features que se relacionan a la identificación de sitios web maliciosos. En la siguiente tabla se hizo un desglose de las features, descripción y su uso, y el impacto que tienen hoy en día en la identificación de sitios web maliciosos.

### Features

| ID | Feature (Kaggle) | explicación |
| :--- | :--- | :--- |
| 1 | **UsingIP** | Los certificados SSL modernos (HTTPS) no suelen darse para IPs. Un atacante preferirá un dominio gratuito o barato antes que una IP, que alerta a los firewalls. y puede hacerlo ya que los dominios son relativamente baratos en paginas como namescheap, flarecloud, etc|
| 2 | **LongURL** | El phishing dirigido intenta esconder el dominio malicioso tras una cadena larga, pero los navegadores suelen ocultar la URL completa, reduciendo la efectividad de está técnica |
| 3 | **ShortURL** | Con el crecimiento de Smishing (phishing por SMS), los acortadores son la herramienta principal para ocultar enlaces fraudulentos en mensajes de texto cortos que se pueden distribuir en whatsapp, telegram, SMS, etc. |
| 4 | **Symbol@** | Los motores de búsqueda y navegadores actuales identifican como sospechosa cualquier URL que use @ para ofuscar el host. |
| 5 | **Redirecting//** | Es un error de configuración técnica que los sitios legítimos no suelen cometer. Indica una redirección forzada, un patrón muy común en phishing. |
| 6 | **PrefixSuffix-** | Explota el factor humano haciendo creer que el sitio es oficial (ej. paypal-support.com). Es una técnica de ingeniería social con alto impacto hoy. |
| 7 | **SubDomains** | Permite crear login.banco.verificacion.com. El usuario ve "banco" y se confía, sin notar que el dominio real es el que está al final (verificacion.com). |
| 8 | **HTTPS** | Si un sitio pide datos bancarios y el certificado es inválido o no existe, la probabilidad de fraude es casi 100%. |
| 9 | **DomainRegLen** | El phishing es temporal. Un atacante no pagará 5 años de registro; pagará el mínimo (1 año) porque sabe que el sitio será dado de baja rápido. |
| 10 | **Favicon** | Se basa en cargar iconos de otros servidores. Muchos sitios legítimos usan CDNs externos, por lo que medir esto genera falsos positivos. |
| 11 | **NonStdPort** | El 99% del phishing hoy en día corre en el puerto 443 (HTTPS). Los ataques sobre puertos extraños son detectados instantáneamente por antivirus. |
| 12 | **HTTPSDomainURL** | El usuario ve https en el texto de la URL y baja la guardia, sin darse cuenta que el sitio corre sobre un protocolo http inseguro. |
| 13 | **RequestURL** | Un sitio legítimo carga sus propios logos. Un ataque popular es imitar el sitio oficial cargando contenido visual desde el servidor del sitio original. |
| 14 | **AnchorURL** | Los sitios maliciosos redirigen links de "Ayuda" o "Términos" a la página oficial para no levantar sospechas; ahí se nota la suplantación. |
| 15 | **LinksInScriptTags**| Analiza la estructura del código HTML. Si el esqueleto de la página está lleno de enlaces externos, es un indicador de página maliciosa. |
| 16 | **ServerFormHandler**| Es la prueba de phishing. Si un formulario envía tus claves a un dominio distinto de donde estás, es un robo de credenciales activo. |
| 17 | **InfoEmail** | Atacantes modernos usan bases de datos en la nube o botnets. Mandar datos por un simple mail() es fácil de rastrear. |
| 18 | **AbnormalURL** | Inconsistencia: si el sitio dice "Apple" pero el host es xyz.com, la inconsistencia delata el sitio. |
| 19 | **WebsiteForwarding**| El phishing actual usa capas de redirecciones para saltarse los escaneos de seguridad. A más redirecciones, más sospechoso es el destino. |
| 20 | **StatusBarCust** | Los navegadores ya no permiten que JavaScript cambie el texto de la barra de estado por razones de seguridad. |
| 21 | **DisableRightClick**| Medida que los sitios legítimos no usan. Hoy en día ya no evita que un analista vea el código de la página. |
| 22 | **UsingPopupWindow**| Los navegadores bloquean ventanas emergentes por defecto. El atacante prefiere un formulario embebido para no ser bloqueado. |
| 23 | **IframeRedirection**| Se usa en el Clickjacking. El atacante pone un sitio real dentro de un marco invisible para engañar la interacción del usuario. |
| 24 | **AgeofDomain** | Un sitio con 10 años de antigüedad es mucho menos probable que sea un ataque que uno creado hace 48 horas. |
| 25 | **DNSRecording** | Los sitios de phishing rápidos, regularmente, no tienen configuraciones de registros DNS, correo o subdominios válidos. |
| 26 | **WebsiteTraffic** | El tráfico real es difícil y caro de falsificar. Si un sitio de "Amazon" no tiene tráfico global, es un indicador fuerte de fraude. |
| 27 | **PageRank** | Indica autoridad. Nadie enlaza voluntariamente a sitios maliciosos, por lo que no suelen tener un Page Rank alto. |
| 28 | **GoogleIndex** | Si un sitio no aparece en el buscador, es porque el atacante no quiere que Google lo analice y lo bloquee. |
| 29 | **LinksPointingToPage**| Los sitios legítimos están enlazados con otros. El phishing suele ser de una página aislada sin enlaces externos que la respalden. |
| 30 | **StatsReport** | Si el sitio ya fue reportado en PhishTank o Google Safe Browsing, la detección es inmediata y certera. |

### Estructura del dataset

@attribute UsingIP                { -1,1 }

@attribute LongURL                { 1,0,-1 }

@attribute ShortURL               { 1,-1 }

@attribute Symbol@                { 1,-1 }

@attribute Redirecting//          { -1,1 }

@attribute PrefixSuffix-          { -1,1 }

@attribute SubDomains             { -1,0,1 }

@attribute HTTPS                  { -1,1,0 }

@attribute DomainRegLen           { -1,1 }

@attribute Favicon                { 1,-1 }

@attribute NonStdPort             { 1,-1 }

@attribute HTTPSDomainURL         { -1,1 }

@attribute RequestURL             { 1,-1 }

@attribute AnchorURL              { -1,0,1 }

@attribute LinksInScriptTags      { 1,-1,0 }

@attribute ServerFormHandler      { -1,1,0 }

@attribute InfoEmail              { -1,1 }

@attribute AbnormalURL            { -1,1 }

@attribute WebsiteForwarding      { 0,1 }

@attribute StatusBarCust          { 1,-1 }

@attribute DisableRightClick      { 1,-1 }

@attribute UsingPopupWindow       { 1,-1 }

@attribute IframeRedirection      { 1,-1 }

@attribute AgeofDomain            { -1,1 }

@attribute DNSRecording           { -1,1 }

@attribute WebsiteTraffic         { -1,0,1 }

@attribute PageRank               { -1,1 }

@attribute GoogleIndex            { 1,-1 }

@attribute LinksPointingToPage    { 1,0,-1 }

@attribute StatsReport            { -1,1 }

@attribute class   (target)       { -1,1 } —- resultado del análisis

En donde:
-1 indica phishing
0 indica sospechoso
1 indica que es un sitio legítimo y seguro

Las instancias están acomodadas con estos valores de -1, 0 y 1.
Una instancia normal se ve así:
-1,1,1,1,-1,-1,-1,-1,-1,1,1,-1,1,-1,1,-1,-1,-1,0,1,1,1,1,-1,-1,-1,-1,1,1,-1,-1

Una ventaja muy clara del dataset es que ya está listo para trabajar ya que está en un vector numérico de 31 dimensiones (el número de features + el valor del target o el resultado para hacer entrenamiento supervisado con el modelo).

### Análisis de correlación de los datos

| ![Matriz](https://github.com/user-attachments/assets/fdc40d46-d640-42df-afd9-b2f9c6156e63) |
| :--: |
| **Figura 2:** Matriz de correlación de las características del dataset. |

Con esta matriz de correlaciones se confirman las features más críticas para el resultado final de la clasificación, ya que son las features que más influencian la detección. 

Dichas features son:
1. AnchorURL
2. HTTPS
3. PrefixSuffix
4. WebsiteTraffic
5. SubDomains
6. RequestURL

Estas son las features que se usarán para entrenar el modelo, por lo que todas las demás features serán eliminadas y no se tomarán en cuenta. 

# Metodología

## Preprocesamiento de los datos
Como se mencionó, el dataset no está equilibrado del todo. Y si bien se podría solo copiar y pegar filas al final del dataset, esto tendría el inconveniente de causar overfitting al aprenderse el patrón. Por lo que, en su lugar, se utilizó la técnica SMOTE (Synthetic Minority Over-sampling Technique), que es una técnica de sobremuestreo inteligente. En este caso la utilizamos para equilibrar la clase con menos instancias (que es la clase de sitios web phishing).

¿Cómo funciona?
Utiliza el algoritmo K-Nearest Neighbors (K-Vecinos más cercanos): 
1. Toma un punto o instancia de la clase que se le indique (para este caso, la clase de web phishing).
2. En esa posición, busca los vecinos más cercanos de la clase y, con base en ellos, crea una instancia nueva.

| ![Distribución SMOTE](https://github.com/user-attachments/assets/02258ca4-a4d6-4165-bc34-7eca03785092) |
| :--: |
| **Figura 3:** Distribución de clases después de la generación de datos con SMOTE (50/50). |

Nota: En el código se tiende a usar una seed que es el número 42. Todo es con el objetivo de que siempre salgan generados los mismos resultados en cada corrida; si se quisieran resultados más variados, se cambiaría el seed o se quitaría. Pero como el objetivo es demostrar, entonces por ahora se quedará así. 

Para evitar el impacto negativo del SMOTE, se tuvo que hace shuffling de todo el dataset para que al momento de hacer la separación de los datos, ambos sets tengan instancias creadas por el SMOTE.

También se tuvo que transformar el set de datos. Como el modelo que se va a usar es un modelo de clasificación binaria, se va a usar una neurona de tipo sigmoide al final del modelo para dar el veredicto final. Pero como nuestros valores van desde -1, 0 y 1, la tarea se dificulta. Por lo que se tiene que usar un formato binario (0 o 1, en donde 0 es phishing y 1 es legítimo).

## Separación del dataset
Ya que los datos han sido seleccionados, escalados y el dataset esté en equilibrio, ahora es momento de hacer la division de los datos. 
Para poder entrenar el modelo, se separo los dataset equilibrados en 4:

1. Features de entrenamiento.
3. Resultados de entrenamiento.
4. Features de prueba.
5. Resultados de prueba.
   
teniendo un porcentaje 80/20: 80 porciento al entrenamiento y 20% dedicado para las pruebas.

# Desarrollo de modelo

Según [6] Machine Learning and Neural Networks for Phishing Detection: A Systematic Review (2017–2024).  Se establece que no existe un modelo universalmente mejor sino que su rendimiento y estructura dependerá de la complejidad del problema, el tipo de features, la calidad del dataset y el preprocesamiento del mismo. Adicionalmente menciona algunos modelos usados comúnmente para este tipo de cosas:

1. Logistic Regression
3. Decision Trees 
2. Random Forest
3. Support Vector Machines (SVM)
4. Naive Bayes
5. MLP (red neuronal básica)
6. CNN (Convolutional Neural Networks)
7. RNN / LSTM (para secuencias)
8. Transformers (BERT, etc.)

Se usó un MLP para abordar el problema ya que en la literatura (ver: [6] Machine Learning and Neural Networks for Phishing Detection: A Systematic Review (2017–2024).)  Se recomienda comenzar por un MLP o por un Decision Tree. Pero hay que tener muy claro que un MLP y CNN son modelos neuronales para distintos datos: Siendo el MLP para datos tabulares y CNN para procesar imagenes. 

El modelo consta de:
1. 1 capa inicial con función de activación relu con 64 neuronas y un input size de 6 (por las 6 features que estamos manejando) y con nombre "capa_1".
2. 2 capas ocultas con función de activación relu con 32 y 16 neuronas y con nombre "capa_2" y "capa_3" respectivamente.
3. 1 final con función de activación sigmoide con 1 neurona ya que nuestros datos están en formato de 1 y 0 y nuestras clases también. Con nombre "capa_salida".

Para compilar el modelo se hizo un model.compile con un optimizador adam, la función loss "binary_crossentropy" y como métrica Recall llamandola "recall_phishing" con un umbral de 0.5 y sacando el Recall de la clase 0 (Phishing). El porque se utiliza Recall se explica en el apartado de Evaluación del modelo--> Metricas de evaluación.

Posteriormente se crea el checkpoint que es paquete que nos ayudará a guardar los pesos en un archivo con nombre de nuestra elección (en éste caso "Pesos_Phishing.weights.h5") **Muy importante: el archivo debe de tener la extensión .weights.h5 por como trabaja keras con él**. Le indicamos que debe de seguir val_recall_phishing (valor del recall de la clase phishing), también le indicamos que debe de guardar solo los pesos (que es lo que nos intersa) y que se quede con los pesos que den el mayor resultado de Recall. Dicho de otra manera: Al momento en el que se entrene el modelo, va monitorear la métrica Recall de la clase phishing (cuantos ataques hemos detectado) y cada vez que el recall mejore, se guardará los pesos que están en ese momento. 

Se entrena el modelo con model.fit, con batch size de 10, 20 épocas con su validation data y pasando el checkpoint como callback 

¿Pero que es un callback?

Un Callback es una herramienta de Keras que nos permite ejecutar acciones automáticas en etapas específicas del entrenamiento (al principio o al final de cada época). Aquí, el callback  interviene cada vez que termina una época para revisar si el rendimiento mejoró y decidir si guarda los pesos o no. Que como ya establecimos, solo guarda los pesos cuando hay una mejora en el Recall. 

Finalmente guardamos el archivo en la ruta que se le especifique dentro del drive, e indicamos que el modelo ha sido guardado exitosamente.

Al final se entrena al modelo y se guarda con el objetivo de poder almacenar los pesos del entrenamiento y colocarlos en otro modelo (cargar en otro modelo) se le debe de dar nombre a las capas para saber en dónde se asigna el peso. 

 
# Evaluación del modelo

## Evaluacion manual del modelo

Para evaluar el modelo a mano, se creó un nuevo archivo en google colab y dentro vamos a colocar los pesos en un nuevo modelo. 
Para ello, creamos una función que crea un modelo identico al que entrenamos y compilamos. Se llama la función, se crea el modelo y obtenemos los pesos del archivo en el cual guardamos los pesos del modelo que ya entrenamos. De ésta manea, no necesitamos entrenar el modelo otra vez, lo cual es útil para cuando queramos cargar un modelo entrenado sin la necesidad de tener que entrenarlo en el momento. Rápido y listo para hacer pruebas con él.

Para la prueba manual se necesita una interfaz gráfica en dónde se pueda digitar con 1 y 0 el valor de las features de la instancia que estamos creando. y con base en ello, dar un resultado aproximado si es phishing o no ¿cómo hace esto? pues para esto hay que saber que la neurona sigmoide convierte el resultado en 1 o 0 de acuerdo a un umbral, un umbral de tolerancia. Dicho umbral tiene gran incluencia al momento de clasificar los sitios, ver sección: Mejoras al proyecto --> Ajuste de umbral de detección (ajuste de hiper parámetro) para más información. 
Para determinar el porcentaje, solo se arroja el resultado en bruto de la sigmoide antes de que se convierta en 0 u 1 de acuerdo al umbral.

para la interfaz se importan las librerías: 

import ipywidgets as widgets
from IPython.display import display

Se configura la interfaz para que se digiten las 6 features y al presionar el botón "Probar" Entra el modelo a hacer una predicción del resultado y lo clasifica.

| ![Display de la instancia manual](https://github.com/user-attachments/assets/46ba7cef-8e3f-4453-bb7c-85d8ede9bf80) |
| :--: |
| **Figura 4:** Interfaz gráfica para la creación de una instancia manual y evaluación |

Aquí es importante destacar 2 cosas: 
1. Se debe de importar el scaler utilizado en el entrenamiento del modelo porque los datos crudos vienen en formato -1,0 y 1. En donde -1 es phishing,0 es sospechoso (neutral) y 1 es legítimo. por lo que si se dan los datos en crudo, sin usar el scaler, la prediccion estará mal. es como usar 1 kilo de harina cuando la receta dice 1 gramo. Básicamente necesitamos estandarizar la instancia a evaluar para que el modelo pueda manejarla. Sino se tiene, el modelo recibiría un "1" crudo. Al multiplicarlo por los pesos que aprendió (que son pequeños), el resultado final se sale de rango. Al final, el modelo utiliza decimales y le daríamos enteros. El Scaler asegura que ninguna columna pese más que otra solo por tener números más grandes. Mantiene a todas las features en el mismo nivel de importancia para que la red neuronal pueda decidir bien. 

## Métricas de evaluación

La métrica principal que se utiliza para evaluar el desempeño del modelo es el Recall del mismo. Aunque se calcule la precisión y el F1-Score para tener un panorama más completo, la toma de desiciones y ajustes del modelo se hacen con base en el Recall. Que es, en esté caso, la capacidad de detección de amenazas reales. Ésta métrica se establecio como principal por lo que representa en el modelo y su objetivo (detectar sitios maliciosos phishing para proteger al usuario de estafas) y según lo visto en [5] Phishing URL detection with neural networks: an empirical study y [6] Machine Learning and Neural Networks for Phishing Detection: A Systematic Review (2017–2024). Que lo utiliza para medir el desempeño de sus modelos de detección de phishing y es una práctica usual en el área. [8] Deep learning-based phishing classification framework for accurate detection using optimized URL intelligence. Scientific Reports. [9]  High accuracy phishing detection based on convolutional neural networks; que usaron ésta métrica en sus estudios y en sus modelos. 


### Recall (La más importante) 
El Recall mide todos los ataques de phishing reales que existen. Un Recall bajo implica que los ataques se están filtrando y llegando al usuario lo cual es terrible porque dichos usuarios están sufriendo estafas y extorsiónes. Pero no solo queda ahí, sino que también se extiende a objetivos más grandes como corporaciones y empresas PYMES que pueden ver robada su información o recibiendo ataques que reduzcan sus operaciones, y en un todo, se vean comprometidas.

*Impacto de un Recall bajo significa que el modelo es más permisivo. Los ataques de phishing que se filtran, llegan al cliente y resultan en robo de credenciales o fraude financiero.

*Justificación: Se asume que el costo de un ataque exitoso es muy superior al costo de una falsa alarma. Por lo tanto, el éxito del proyecto se define por la minimización absoluta de los Falsos Negativos.

### Precisión
Mide cuantas respuestas fueron correctas. Si la precisión es muy baja, el usuario recibirá demasiadas falsas alarmas. Esto causa que el usuario termine desactivando el antivirus porque siempre bloquea todo. Pero en este caso, es mejor dar falsas alarmas que dejar pasar el riesgo y que este se manifieste como un peligro real sobre los usuarios y empresas. Por lo mismo, no se tomará en cuenta la precisión del modelo porque lo que nos interesa es detectar el phishing y reducir su filtración, no nos interesa mucho el bloquear accidentalmente páginas legítimas. 

### F1 SCORE
El F1-Score penaliza los valores extremos, un modelo con un Recall del 100% pero una precisión baja obtendría un F1-Score pobre. Debido a que buscamos maximizar el Recall por encima del equilibrio, el F1-Score no lo consideramos una métrica significativa para medir el desempeño del modelo.

### Matriz de Confusión
Nos permite consultar con exactitud los errores del modelo. A través de ella, vemos los Falsos Positivos y Falsos Negativos, permitiéndonos verificar que el modelo esté cumpliendo con la reducción de ataques filtrados hacia el cliente.

En conclusión, el Recall es la métrica indicada para medir el desempeño del modelo por la información que aportan y lo bien establecida que está tanto en justificacion para el proyecto como en su uso general en el área.


## Resultados

| ![Matriz de Confusion y resultados](https://github.com/user-attachments/assets/b129304a-5582-4c93-9f06-6039bd021536) |
| :--: |
| **Figura 5:** Matriz de confusión y resultados de la evaluación del modelo con métricas F1Score, Recall y Precision con un umbral del 0.5 |

1. 954 (Verdaderos Negativos - TN): Son ataques de Phishing que el modelo bloqueó correctamente.
2. 868 (Verdaderos Positivos - TP): Son sitios Legítimos que el modelo dejó pasar correctamente.
3. 58 (Falsos Positivos - FP): Sitios de Phishing que el modelo clasificó como Legítimos.Es decir, que son ataques que se filtraron y llegaron al usuario final
4. 90 (Falsos Negativos - FN): Sitios seguros que el modelo bloqueó por sospecha. Qué básicamente es una falsa alarma para el usuario. 

El Recall de 93.87% representa la sensibilidad que tiene el modelo para encontrar sitios de phishing. Se podría decir que de cada 100 ataques, el modelo detectó  a más 93 amenazas. Lo cuál es un buen nivel para un modelo base.

La Precisión de 92.41% representa la confiabilidad del modelo al clasificar los sitios. Cuando el modelo marca un sitio como Phishing, tiene un 91.38% de probabilidad de estar en lo correcto. Si nos fijamos en la matriz de confusión existe un margen de error de 90 casos mal identificados. Es porque el modelo es demasiado estricto al evaluarlos (umbral de tolerancia), pero en ciberseguridad es preferible a ser demasiado permisivo y dejar pasar amenazas que pueden afectar a una compañía o a miles de usuarios. 

El F1-Score de 93.84% es el promedio armonizado entre Recall y Precision dos anteriores. Un F1-Score por encima del 90% indica que el modelo es bastante robusto robusto. Es decir, que está sacrificando demasiada precisión para obtener recall, o al revés. Es un modelo equilibrado y confiable en un 93.84%.


# Mejoras al proyecto.

## Ajuste de umbral de detección (ajuste de hiper parámetro)

El Umbral de detección establece una tolerancia al momento de clasificar los datos. normalmente está en el 0.5. es decir, que cuando un resultado es 0.5 o en adelante se considera 1 (legitimo) y abajo de eso es 0 (phishing). en este caso se puede ajustar el umbral subiendolo para detectar más rigurosamente los sitios de phishing (subiendo el umbral al 0.6 o 0.8) abajo de eso, todo es phishing y arriba de eso, todo es legítimo. Que puede dar falsos negativos pero esto podría significar una mejora porque es preferible identificar sitios que si son phishing de verdad y dejar pasar algunos que son dudosos. 

El primer modelo MLP que se hizo tenía un umbral del 0.5. Por lo que, si al final de pasar por las capas densas se tiene un resultado mayor o igual a dicho umbral, el modelo lo clasificará como un sitio web legítimo. Lo cuál tiene un enorme impacto en el Recall y los resultados del modelo. 


| ![Matriz de Confusion y resultados con umbral de 0.8](https://github.com/user-attachments/assets/213dc6ec-6853-4435-b987-db21c864bd33) |
| :--: |
| **Figura 6:** Matriz de confusión y resultados de la evaluación del modelo con métricas F1Score, Recall y Precision con un umbral del 0.8 |

Como se puede ver, el Recall sufrió una mejora drástica de poco más de tres puntos, detectando 29 sitios maliciosos más que el modelo con umbral de 0.5. Lo que se traduce como una disminución en los sitios phishing que pueden atacar a las empresas y usuarios que protegemos. El modelo solo falla en identificar 29 sitios phishing de 1012 que existen. todavía se podría rascar un poco más de sitios aumentando el umbral a 0.85 o 0.9 y haciendo modificaciones al modelo haciendo dropout en medio de algunas capas, desactivando neuronas al azar. En la literatura [6] Machine Learning and Neural Networks for Phishing Detection: A Systematic Review (2017–2024). sugiere combinar dos modelos: determinístico y probabilístico para hacer una revisión con más información y aumentar la precision del modelo y el recall. Esa sería otra posible mejora a éste modelo pero para saber si es mejora, debería aumentar el recall y disminuir el numero de falsos positivos.


## testing con dataset de la vida real (datos reales de testing. Sin implementar) 

Otra mejora que se sugiere es que, en el testing, no se utilice el mismo dataset que se uso para entrenar y validar, sino que se utilice uno con datos de la vida real (de otro dataset) para ver el desempeño del modelo en un entorno real. Esto puede tener dificultades porque usualmente se necesitaría transformar los datos de tal manera que queden como los que acepta nuestro modelo. Eso podría significar programar el procesos para sacar los datos y agruparlos tal y como espera el modelo recibirlos. Estos dataset pueden ser sacados de https://www.phishtank.com/ y https://tranco-list.eu/list/ZWYQG/1000000

Solo si se debe de tener en cuenta que no están en el mismo formato que el de nosotros o ni siquiera procesados y se tendría que sacar features, hacer el label y todo el proceso para que se pueda usar dichos datos. 

## Comparativa de los resultados

| Métrica | Modelo sin mejora de umbral (umbral = 0.5) | Modelo con mejora de umbral (umbral = 0.8) | Mejora / Impacto|
| :--- | :---: | :---: | :--- |
| **Recall** | 93.87% | **97.13%** | 39 nuevos sitios maliciososo identificados
| **Precisión** | 92.41% | **87.69%** | Se catalogan más sitios legítimos como maliciosos por error. Pero el error es asumible porque es preferente bloquear sitios legítimos a dejar pasar sitios maliciosos|
| **F1-Score** | 93.14% | **92.17%** | bajó debido a la reducción de la precisión pero no nos importa porque la métrica objetivo es el recall|
| **Falsos Negativos** | 68 | **29** | ** El modelo es más estricto y se detectarón más sitios maliciosos. Pero también se clasificaron más sitios legítimos como maliciosos.** |

La tabla comparativa nos indica que existe una mejora en el Recall al mover el umbral de 0.5 a 0.8 y que gracias a la misma se han detectado 39 sitios web maliciosos con éxito. Lo cuál es una gran mejora respecto al modelo con el umbral a 0.5. Claro, hubo una disminución en el F1 Score y la precisión pero como solo nos importa disminuir los sitios maliciosos que llegan al usuario, no las tomamos en cuenta.

En conclusión, existen múltiples acercamientos al problema pero la intención es la misma: Disminuir los ataques que llegan al usuario final y evitar su daño. 
Para está implementación se alacanzó un buen resultado y una mejora al modelo pero todavía se pueden ejecturar mejoras como implementar un moodelo híbrido que consiste de dos modelos, uno determinístico y otro probabilístico con el objetivo de mejorar la detección de sitios web maliciosos (no mejorar la presicion sino la detección) otra mejora podría venir no solo del ajuste de los hiper parametros, sino que de la arquitectura del modelo, etc. La realidad es que a partir de aquí se tendría que revisar la literatura para encontrar métodos y técnicas que suban el Recall y disminuyan los sitios web phishing que llegan al usuario. 


# Recursos y referencias 
[1] Phishing Websites - UCI Machine Learning https://archive.ics.uci.edu/dataset/327/phishing+websites

[2] Eswar Chand. Phishing Websites- https://www.kaggle.com/code/eswarchandt/website-phishing/input

[3] 19 tipos de ataques de phishing con ejemplos | Fortinet. (s. f.). Fortinet. https://www.fortinet.com/lat/resources/cyberglossary/types-of-phishing-attacks

[4] Mohammad, R. M., Thabtah, F. T., & McCluskey, L. M. (s. f.). Phishing websites features. En UC- UC Irvine Machine Learning Repository. (Documento dentro del repositorio, en data y se llama phishing websites features)

[5] Ghalechyan, H., Israyelyan, E., Arakelyan, A., Hovhannisyan, G., & Davtyan, A. (2024). Phishing URL detection with neural networks: an empirical study. Scientific Reports, 14(1), 25134. https://doi.org/10.1038/s41598-024-74725-6

[6] Lukasz Wilk-Jakubowski, J. L. W. J., Wilk-Jakubowski, L. W. J., Wilk-Jakubowski, G. W. J., & Sikora, A. S. (2025, 22 septiembre). Machine Learning and Neural Networks for Phishing Detection: A Systematic Review (2017–2024). MDPI. Recuperado 15 de abril de 2026, de https://www.mdpi.com/2079-9292/14/18/3744

[7] Patel, J. (2022, April 17). Phishing URL Detection using Artificial Neural Network. https://journal.ijresm.com/index.php/ijresm/article/view/1937

[8] Gobinath, R., & Manikandan, S. (2026). Deep learning-based phishing classification framework for accurate detection using optimized URL intelligence. Scientific Reports. https://doi.org/10.1038/s41598-026-46481-2

[9] Yerima, S. Y., & Alzaylaee, M. K. (2020, April 8). High accuracy phishing detection based on convolutional neural networks. arXiv.org. https://arxiv.org/abs/2004.03960
