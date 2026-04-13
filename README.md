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

## Origen del dataset
1. Dataset obtenido de UC Irvine Machine Learning Repository. Extraído de Phishing Websites - UCI Machine Learning Repository (sin embargo, no usado por la caída del servidor).

2. Copia del dataset obtenido de Kaggle: Website Phishing <-- este es el que usa el proyecto.

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
| 2 | **LongURL** | El phishing dirigido intenta esconder el dominio malicioso tras una cadena larga, pero los navegadores suelen ocultar la URL completa, reduciendo la efectividad de está téncnica |
| 3 | **ShortURL** | Con el crecimiento de Smishing (phishing por SMS), los acortadores son la herramienta principal para ocultar enlaces fraudulentos en mensajes de texto cortos que se peuden distribuir en whatsapp, telegram, SMS, etc. |
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

![Distribución Después](https://github.com/user-attachments/assets/02258ca4-a4d6-4165-bc34-7eca03785092)
*Figura 3: Distribución de clases después de la generación de datos con SMOTE (50/50).*

Nota: En el código se tiende a usar una seed que es el número 42. Todo es con el objetivo de que siempre salgan generados los mismos resultados en cada corrida; si se quisieran resultados más variados, se cambiaría el seed o se quitaría. Pero como el objetivo es demostrar, entonces por ahora se quedará así. 

Para evitar el impacto negativo del SMOTE, se tuvo que hace shuffling de todo el dataset para que al momento de hacer la separación de los datos, ambos sets tengan instnacias creadas por el SMOTE.

También se tuvo que transformar el set de datos. Como el modelo que se va a usar es un modelo de clasificación binaria, se va a usar una neurona de tipo sigmoide al final del modelo para dar el veredicto final. Pero como nuestros valores van desde -1, 0 y 1, la tarea se dificulta. Por lo que se tiene que usar un formato binario (0 o 1, en donde 0 es phishing y 1 es legítimo).

## Separación del dataset
Ya que los datos han sido seleccionados, escalados y el dataset esté en equilibrio, ahora es momento de hacer la division de los datos. 
Para poder entrenar el modelo, se separo los dataset equiulibrados en 4:

1. Features de entrenamiento.
3. Resultados de entrenamiento.
4. Features de prueba.
5. Resultados de prueba.
   
teniendo un porcentaje 80/20: 80 porciento al entrenamiento y 20% dedicado para las pruebas.

## Recursos y referencias: 
[1] Phishing Websites - UCI Machine Learning https://archive.ics.uci.edu/dataset/327/phishing+websites

[2] Eswar Chand. Phishing Websites- https://www.kaggle.com/code/eswarchandt/website-phishing/input

[3] 19 tipos de ataques de phishing con ejemplos | Fortinet. (s. f.). Fortinet. https://www.fortinet.com/lat/resources/cyberglossary/types-of-phishing-attacks

[4] Mohammad, R. M., Thabtah, F. T., & McCluskey, L. M. (s. f.). Phishing websites features. En UC- UC Irvine Machine Learning Repository. (Documento dentro del repositorio, en data y se llama phishing websites features)
