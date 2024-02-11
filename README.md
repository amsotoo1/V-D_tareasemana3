<iframe src="https://lookerstudio.google.com/embed/reporting/8a79764f-d20f-470e-a227-694b2726b813/page/RespD" width="1200" height="2000"></iframe>



Si tienes problemas con la visualizacion del tablero, haz clic en el siguiente enlace: [Tablero de Looker](https://lookerstudio.google.com/embed/reporting/8a79764f-d20f-470e-a227-694b2726b813/page/RespD)
# **DOCUMENTACIÓN**

Fuente: https://docs.google.com/spreadsheets/d/1IPS5dBSGtwYVbjsfbaMCYIWnOuRmJcbequohNxCyGVw/edit?resourcekey#gid=1625408792968bb02bb6f493b099331d20f202ffd7ce4135e7
Teniendo en cuenta que la fuente de datos es un repositorio de información externa, se ha decidido realizar una copia dentro de los recursos de uno de los miembros del equipo, Ana María Soto Orozco. Por lo tanto, la nueva fuente de datos sería la siguiente: https://docs.google.com/spreadsheets/d/1wIp6Ssk66Yr-OFYD_uSueetlvKbWHrvXRBJYv4Zph7w/edit?usp=drive_link. Esta fuente, denominada "Estados_US", se encuentra en modo de visualización para aquellos que tengan el vínculo.
A continuación, se presenta la información de la base implementada por el equipo de trabajo para la investigación y diseño de los objetivos. Es importante destacar que esta base constituye el resultado final de la limpieza y estandarización de los campos. En la Tabla 1"Metadata de la tabla:Estados_US" se incluyen los nombres de las variables en la base de datos original, los nombres una vez modelados, la tipología y una breve descripción de cada variable.


*Tabla 1"Metadata de la tabla:Estados_US"*
| Nombre original en la base | Nombre nuevo                | Tipo de Variable | Descripción de variable                                                                           |
|-----------------------------|------------------------------|------------------|---------------------------------------------------------------------------------------------------|
| Job title                   | Cargo                        | string           | Nombre del cargo actual del profesional                                                          |
| What city do you work in?  | Ciudad                       | string           | Nombre de la ciudad donde trabaja actualmente el profesional                                      |
| How much additional monetary compensation do you get, if any (for example, bonuses or overtime in an average year)? Please only include monetary compensation here, not the value of benefits. | Compensacion_Adicional      | Double           | Valor de la compensacion adicional del profesional                                               |
| NA                          | Compensacion_Adicional_COP   | Double           | Es el campo calculado de convertir el Compensacion_anual  a una sola moneda (COP)                |
| If your job title needs additional context, please clarify here: | Contexto_cargo               | string           | Contexto adicional del cargo del usuario                                                         |
| If your income needs additional context, please provide it here: | Contexto_ingresos            | string           | Contexto adicional de los ingresos                                                                |
| If you're in the U.S., what state do you work in?              | Estado_US                    | string           | Nombre del estado en US(solo si el profesional trabaja en US)                                     |
| NA                          | Fecha_TRM                    | string           | Fecha en la que fue consultada la TRM de conversion a COP, esta fecha se deja en formato texto puesto que no va ser utilizada en ningún calculo. tener en cuenta que esta consulta se realizo por medio de https://www.google.com/finance/ |
| What is your gender?       | Genero                       | string           | Genero con el que se identifica el profesional                                                   |
| NA                          | Genero_limpio                | string           | Es el campo calculado de realizar la homologacion del campo Genero                                |
| Please indicate the currency | Moneda                     | string           | Nombre de la moneda en la que se registro el salario y la compensacional adicional                |
| What is your highest level of education completed?             | Nvl_Estudios                 | string           | Nivel de estudio mas alto completado por el profesional                                           |
| If "Other," please indicate the currency here: | Otra_Moneda       | string           | Si se especifico en moneda "Otro", aca se detalla el nombre de la moneda                           |
| What country do you work in? | Pais                         | string           | Nombre del pais donde trabaja actualmente el profesional. Este campo es el original de la data    |
| How old are you?            | Rango_Edad                   | string           | Es el rango de edad en el que se puede encontrar cada profesional                                 |
| How many years of professional work experience do you have overall? | Rango_experienciaL   | string           | Rango de años de la experiencia laboral que tiene el profesional                                  |
| How many years of professional work experience do you have in your field? | Rango_experienciaL_Cargo | string     | Rango de años de la experiencia laboral que tiene el profesional en el campo especificado anteriormente |
| What is your race? (Choose all that apply.)                    | Razas                        | string           | Razas con las que se identifica el profesional                                                   |
| What is your annual salary? (You'll indicate the currency in a later question. If you are part-time or hourly, please enter an annualized equivalent -- what you would earn if you worked the job 40 hours a week, 52 weeks a year.) | Salario_anual  | Double           | Salario anual, si el profesional trabaja a tiempo parcial o por horas, se ingreso un equivalente anualizado(40 horas a la semana, 52 semanas al año). |
| NA                          | Salario_anual_COP            | Double           | Es el campo calculado de convertir el Salario_anual a una sola moneda (COP)                       |
| NA                          | Salario_completo_COP        | Double           | Es el campo calculado de la totalizacion del Salario_anual_COP  y Compensacion_Adicional_COP      |
| What industry do you work in? | Sector_Economico           | string           | Es el sector economico en el que trabaja cada profesional                                         |
| NA                          | Sector_Economico_Limpio      | string           | Este campo es el resultado de limpiar y homologar el campo Sector_Economico, sera el campo final de la base |


# **PASO A PASO DEL MODELADO DE LA INFORMACION**
1.	**Renombrar campos:** 
Se procede a renombrar los campos provenientes del formulario con el objetivo de hacerlos más concisos y descriptivos. La Tabla 1 "Metadata de la tabla: Estados_US" contiene la homologación de los nombres.


2.	**Limpieza y estandarización de los campos país y ciudad:**
Durante la etapa inicial de limpieza, se emplea un proceso ETL para extraer caracteres especiales y identificar campos con longitud excesiva que no corresponden a ubicaciones válidas. Para aquellos campos con múltiples registros separados por diversos delimitadores, se opta por conservar únicamente el primer registro de ubicación, mientras que los campos vacíos o sin referencia a una ubicación específica se designan como 'Others'. Posteriormente, se procede a la estandarización mediante la creación de un catálogo de registros identificados, realizada manualmente con la colaboración del equipo para garantizar la presencia de registros de ubicación únicos y coherentes.


3.	**Limpieza y estandarización del campo Sector económico:**
El proceso de limpieza y estandarización del campo Sector Económico sigue un enfoque similar. Se inicia con la limpieza de caracteres especiales y la decisión de conservar únicamente el primer registro para aquellos campos con múltiples registros separados por diversos delimitadores. Los registros sin denominación clara se categorizan como "Others". Luego, se agrupan los valores similares o relacionados bajo categorías más amplias, resultando en 17 sectores económicos. Se conserva el campo original y el resultado de esta modelación se registra en un nuevo campo denominado "Sector Económico Limpio".

4.	**Limpieza y estandarización del campo Moneda:**
Se procede con la limpieza y estandarización del campo Moneda, el cual contiene información detallada sobre la moneda utilizada para los ingresos del usuario. Se eliminan caracteres especiales y se conserva una única moneda, prevaleciendo el primer registro. Los registros sin clasificación se categorizan según su ubicación.


5.	**Creación del campo Saldos anual, compensación anual y saldo completo:**
Se crea el campo "Saldos Anual", "Compensación Anual" y "Saldo Completo". Se inicia con la limpieza y transformación de los campos de saldos anual y compensación anual. Se realiza un análisis de los valores con longitud de 1 o 2, y se ajustan según la medida esperada, considerando, por ejemplo, expresiones como "80K" para indicar un salario anual de 80,000. Los registros sin valor se categorizan como 0, considerando su referencia a una moneda.
Posteriormente, se procede a generar las nuevas columnas "Salario Anual COP" y "Compensaciones COP", convirtiendo los salarios y compensaciones a pesos colombianos utilizando las tasas de cambio proporcionadas en la Tabla 2 "Catálogo de Tasas de Conversión para COP". Cabe destacar que estas tasas de cambio fueron extraídas el día 07/02/2024.
Finalmente, se calcula la suma total del campo "Salario Anual" y "Compensación Anual" en pesos colombianos, lo que da lugar al salario completo.


*Tabla 2"Catalogo de Tasas de conversión para COP"*
| Moneda | Tasa de conversión a COP |
| ------ | ------------------------ |
| AUD    | 2581                     |
| ARS    | 4,76                     |
| USD    | 3955,21                  |
| EUR    | 4264,31                  |
| GBP    | 4998,12                  |
| BRL    | 795,7                    |
| CAD    | 2940,35                  |
| CHF    | 4531,74                  |
| DKK    | 571,82                   |
| CZK    | 170,86                   |
| CNY    | 556,37                   |
| SEK    | 377,78                   |
| HKD    | 505,73                   |
| HRK    | 565,865                  |
| INR    | 47,66                    |
| HUF    | 10,98                    |
| ILS    | 1082,54                  |
| JPY    | 26,67                    |
| IDR    | 0,25                     |
| MYR    | 830,39                   |
| ZAR    | 209,53                   |
| NOK    | 373,95                   |
| TTD    | 583,04                   |
| MXN    | 231,96                  


6.	**Estandarización del campo Genero:**
Los registros no clasificados se catalogan como “Other or prefer not to answer”.
