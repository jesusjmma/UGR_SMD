# Tema 1: Sistemas OLTP y Sistemas OLAP
- **OLTP**: On Line **Analytical** Processing
- **OLAP**: On Line **Transactional** Processing

## 1.1. Niveles de gestión y sistemas de soporte
Los niveles son:
- **Decisionales (OLAP)**: Toman decisiones basadas en información
    - **Nivel Estratégico** (directores)
    - **Nivel Administrativo** (gerentes de nivel medio)
    - **Nivel del Conocimiento** (trabajadores del conocimiento y de datos)
- **Transaccional (OLTP)**: Almacenan los datos que se producen (bases de datos)
    - **Nivel Operativo** (gerentes operativos): Los que soportan el día a día del funcionamiento de la empresa

### 1.1.1. Nivel Transaccional: Sistemas OLTP
A raíz del diagrama Entidad-Relación, normalmente pasamos al modelo Relacional (de tablas) haciendo normalización para optimizar las operaciones que vamos a hacer (registrar transacciones evitando las anomalías).

Los informes "precocinados" son consultas habituales que una aplicación de gestión suele tener, además de posibilidad de altas, bajas y modificaciones de los datos.

Los informes "a medida" suelen ser consultas SQL (en una base de datos relacional) en las cuales es necesario el trabajo de un informático para poder generar dicho informe.

### 1.1.2. Elaboración de informes “a medida”
El informático tiene que:
- Localizar los datos
- Extraer los datos
- Transformar e integrar los datos
- Cargar los datos
- Presentarlos según quiere el decisor

Lo peor que puede pasar es darle datos erróneos al decisor, por lo que hay que validar los informes elaborados.

### 1.1.3. Necesidad de información en las organizaciones
El decisor debe conocer cómo va su organización y cómo van el resto de organizaciones. Y necesita verlo cuanto antes. Por tanto los informáticos tenemos que ser capaces de gestionar una base de datos, crear los cuadros de mando necesarios para que el decisor pueda utilizar la información en tiempo real.

### 1.1.4. Nivel Decisional: Sistemas OLAP
En contrapartida de las aplicaciones OLTP, que tienen informes "precocinados", pretendemos crear aplicaciones OLAP que permitan a un usuario no informático generar informes a medida.

En los sistemas OLAP necesitamos una base de datos (los datos entran mediante sistemas OLTP) y un componente ETL (extrae, transforma y carga). El componente ETL es un programa.

### 1.1.5. Sistemas OLTP y OLAP
[//]: # (Insertar tabla comparativa que te va a pasar Isabel con sus notas <3)

## 1.2. Modelo de datos Multidimensional

### 1.2.1. Origen
En la toma de decisiones, el usuario final recibe informes. Nuestro objetivo es seguir dándole estos informes. Debemos tener un modelo de datos que nos permita darle al usuario final estos informes sin que este usuario final necesique conocimientos de informática.

Utilizaremos en el modelo de datos una estructura similar a la que se usará en los informes que queramos darle al usuario final: Modelo de Datos Multidimensional.

Los componentes del modelo de datos son:
- Hechos
- Dimensiones

En el centro del Modelo de Datos Multidimensional tendremos los hechos ("cuánto") y alrededor de ellos tendremos las dimensiones ("qué", "quién", "dónde", "cuándo", "cómo", etc). Las dimensiones califican a los hechos.

### 1.2.2. Representación gráfica
Cada punto es un dato. Si no hay un punto, es porque no se da una combinación concreta de dimensiones. 

El conjunto de todos los puntos (todos los datos) es un hipercubo.

Las dimensiones se estructuran en jerarquías ("Granada" contiene a "Plaza Bib-Rambla", "2023" contiene a "04/03/2023", etc.).

### 1.2.3. BD Multidimensional
Los datos los tenemos guardados en forma de tablas. Por regla general, habrá una tabla para cada Dimensión y otra tabla para los Hechos; aunque alguna vez puede variar.

### 1.2.4. Elementos del modelo
En las jerarquías puede haber varios elementos en una misma dimensión. El nivel "Todo" es una supercategoría que engloba a todos los elementos de una dimensión.

### 1.2.5. Operaciones
Hay 4 posibles operaciones:
- roll-up
- drill-down
- slice and dice
- drill-across

Los hechos (nuestros datos) se agregan conforme ascendemos por la jerarquía de una dimensión (roll-up). Normalmente se suman, pero si remotamente no se pudiera hacer eso, pues se tendrán que agregar de otra forma.

De igual forma, conforme descendemos en la jerarquía de la dimensión se tienen que separar los datos (drill-down). Aunque lo normal es hacer *roll-up* ya que es mucho más sencillo.

Finalmente también se seleccionan los datos que queremos (slice and dice), así como también se pueden combiar datos entre varios esquemas (drill-across)

## 1.3. Conclusiones
Hemos pasado de sistemas OLTP a sistemas OLAP (estos últimos los que veremos en la asignatura).

Hemos visto el modelo de datos multidimmensional que da soporte a los sistemas OLAP.

# Tema 2: Componente ETL

## 2.1. Componente ETL y modelo lógico

**Componente ETL**: (Localizar), Extraer, Transformar, (Integrar) y Cargar

La componente ETL permite pasar de sistemas OLTP a sistemas OLAP.

## 2.2. Proceso de replicación de datos

Vamos a ver qué cosas tiene dentro el componente ETL.

Los datos que introducimos en sistemas OLAP provienen de sistemas OLTP.

¿Por qué creamos una BD aparte de las OLTP?
- Porque en los sistemas OLTP hemos priorizado la eficiencia en el almacenamiento de transacciones, mientras que el objetivo de nuestros datos es que la consulta sea eficiente. Hay que estructurar los datos de forma diferente para cambiar la eficiencia en el proceso en que la necesitamos.

Los datos se:
1. Localizan
2. Extraen
3. Transforman:
   1. Limpian
   2. Conforman
4. Integran
5. Cargan

**Proceso de replicación**:
- Los pasos del 1 al 5 corresponden a la creación del programa ETL.
- Los pasos del 6 al 11 corresponden a la ejecución del programa ETL.
- El paso 12 corresponde al mantenimiento del programa ETL.

[//]: # (Insertar tabla que te va a pasar Isabel con sus notas <3)

### 2.2.1. Correspondencia entre fuentes de datos y destino

Hay que:
- Ver las fuentes de datos que hay disponibles
- Integrarlas (difícil porque puede haber heterogeneidades de tipos diferentes: sintáctica, semántica, etc)

Algunos criterios para seleccionar los datos:
- Más precisos
- Más completos
- Más actualizados
- Con estructura compatible
- Los más cercanos a la fuente de origen

### 2.2.2. Periodicidad de las actualizaciones

El componente ETL tiene que hacer 2 actividades principalmente:
- Una carga inicial
- Una actualización de los datos

### 2.2.3. Extracción

Las herramientas ETL nos dan acceso a distintas fuentes de datos.

Hay 2 tipos de métodos para obtener datos nuevos:
- Diferidos (sin la información de cómo ha pasado de un estado a otro)
- Inmediatos (además de los estados, también conocemos los cambios concretos que han ocurrido)

Hay varias formas de obtener los cambios que han ocurrido entre estados:
- Disparadores
- Log file
- Generados por las aplicaciones

### 2.2.4. Transformación

Hay que:
- Transformar datos de varias fuentes
- Adaptar los datos al modelo de destino:
  - Cambiar la codificación
  - Cambiar el formato
  - Cambiar la granularidad (de fina a gruesa)
- Solucionar problemas:
  - Datos repetidos
  - Datos inconsistentes
  - Datos erróneos
  - Datos faltantes
- Notificar problemas y errores

Los datos se pueden corregir en:
- Fuente origen exclusivamente
- Fuente origen preferiblemente
- ETL preferiblemente
- ETL exclusivamente

### 2.2.5. Carga

Cargar los datos es crear la base de datos, crear índices, tener en cuenta la integridad referencial, convertir los datos, y hacer sumarizaciones.

## 2.3. Herramientas ETL y arquitectura

### 2.3.1. Estructura general

Las herramientas ETL tienen:
- Estructuras de las fuentes y almacenes de datos
- Metadatos de configuración
- Correspondencias y transformaciones

[//]: # (Revisar diapositiva para completar)

### 2.3.2. ETL de usuario final: Power Query

Desde 2013 la tendencia ha sido generar un autoservicio de transformación de datos y de Sistemas OLAP.

*Power Query* coge los datos, los transformar y los lleva a una herramienta interna que se llama *Power Pivot* (está dentro de *Power BI*).

*Power Query* y *Power Pivot* están tanto en *Excel* como en *Power BI*.

*Power Pivot* nos permite crear cubos OLAP.

### 2.3.3. ETL profesional

> No es importante

## 2.4. Conclusiones

> No data

# 3. Modelo de datos Multidimensional

## 3.1. Modelos de datos y niveles de abstracción

[//]: # (Isabel te va a pasar la diapositiva de OLTP vs OLAP)

Ideas $\rightarrow$ Modelo Conceptual $\rightarrow$ Modelo Lógico $\rightarrow$ Modelo Físico

A nivel conceptual:
- OLTP:
  - ODL: Lenguaje de Definición de Objetos
  - E/R: Modelo Entidad Relación
- OLAP: 
  - Modelo de Datos Multidimensional

A nivel lógico:
- OLTP:
  - Relaciones
- OLAP:
  - Clases (Object Oriented OLAP)
  - Relaciones

A nivel físico:
- OLTP:
  - Sistema de Gestión de Bases de Datos Relacional
- OLAP:
  - Sistema de Gestión de Bases de Datos Orientado a Objetos
  - Sistema de Gestión de Bases de Datos Relacional
  - Sistema de Gestión de Bases de Datos Multidimensional

## 3.2. Modelo conceptual

Tienen que presentarnos 2 cosas:
- Estructuras de datos
- Operaciones

Nota: Se usan diferentes nomenclaturas.

Está formado por:
- Niveles: Son lo que conocemos como entidades.
- Jerarquías: Está formada por niveles relacionados con cardinalidad $(1,1)\longrightarrow(1,n)$ (relación parte-todo exclusiva). Cada una tiene 1 nivel más bajo y 1 nivel más alto (*Todo*).

Nota: La relación parte-todo exclusiva es muy dificil de conseguir en la práctica, pero hay que intentarlo. Cuando no se puede conseguir, hay que buscar soluciones (las veremos más adelante).

En los **Hechos** podemos definir:
- **Bases**: El conjunto mínimo de niveles de las dimensiones que identifican unívocamente a los hechos. No tendrían por qué utilizarse todas las dimensiones para esto (aunque normalmente sí). Debemos tener las dimensiones necesarias para identificar a todos los hechos.
- **Aditividad**: Tenemos mediciones almacenadas y/o calculadas, que pueden ser:
  - Aditivas: Siempre tiene sentido sumar en todas las dimensiones
  - No aditivas: No tiene sentido sumar en ninguna dimensión
  - Semiaditivas: A veces tiene sentido sumar (depende de la dimensión)
- **Granularidad**: 

## 3.3. Modelo lógico

Nos vamos a preocupar cómo se implementa lo definido en el modelo conceptual.

- **MOLAP**: Se hacen estructuras (árboles, matrices)
- **ROLAP**: Se hacen tablas de bases de datos y se implementan las operaciones sobre esas tablas
- **HOLAP**: Bases de datos Híbridas (a veces MOLAP, a veces ROLAP)
- **O3LAP**: Esto estaba de moda en los 90, ya no se utiliza mucho

Nos vamos a centrar en **ROLAP**.

Diferentes posibilidades:
- **ROLAP - Copo de Nieve**
  - Tiene 1 tabla para los hechos y 1 tabla para cada **nivel**.
  - Se procura que no haya datos repetidos en la dimensiones (tablas normalizadas).
  - Mejor para modificar datos.
- **ROLAP - Estrella**
  - Tiene 1 tabla para los hechos y 1 tabla para cada **dimensión**.
  - Hay datos repetidos.
  - Mejor para hacer consultas.

## 3.4. Modelo físico

> No data

## 3.5. Conclusiones

> No data

# 4. Diseño Multidimensional - Sistemas Multidimensionales
## 4.1. Ciclo de vida

Cuando hablamos de diseño multidimensional, hay que hablar del componente ETL.

Puede existir un problema con los ETL: que no se puedan cargar los datos.

[//]: # (Isabel te va a pasar las diapositivas con los diagramas y tablas)

El enfoque más razonable es el **enfoque mixto**. Hay que diferenciar estos tipos de sistemas de los sistemas OLTP.

## 4.2. Diseño conceptual

Primera pregunta: ¿Por qué áreas empezamos a hacer sistemas OLAP? --> Determinar el de mayor potencial de beneficios.

> ***IMPORTANTE: VA A PREGUNTAR DE ESTO***

A partir de aquí:
1. **Seleccionar la granularidad del proceso de negocio**: El nivel de detalle de las dimensiones.
    1. **Determinar y expresar el significado de los hechos**: 
    2. **Definir las bases**:
2. **Diseñar las dimensiones**:
    1. **Niveles y jerarquías**:
3. **Seleccionar las mediciones (determinadas por las dimensiones)**: 
    1. **Aditividad y mediciones calculadas**:
4. **Estimar el número de instancias (dimensiones y hechos)**:

- Extra: Si se dispone de otros diseños multidimensionales, conformar dimensiones y mediciones.

> Recordatorio: Las bases identifican inequívocamente a los hechos.

> Ejemplo: Comedores Universitarios:
> 1. Los hechos son "*Los gramos que sobran de la comida*" y las bases serán: *Menú*, *persona*, *día*, *comedor*, ...
> 2. En "*persona*" sería nivel más bajo (*persona concreta*) y nivel más alto (*todo*). Y luego se añaden los intermedios, que pueden ser muchas cosas distintas. Lo más importante es que solo haya una única opción abajo del todo y una única opción arriba del todo (*todo*).
> 3. Le miden los *gramos*, el *importe* (no el *precio*), etc.
> 4. Debe ser una estimación realista, ni exagerada ni minorada.

## 4.2.1. Ejemplo de esquemas

En un diagrama Entidad-Relación, es muy probable tener una relación (1,n)-(1,n) [o dicho de otra forma n-m] que sea de donde tirar y marcar la relación que indicará el "cuánto".

E
## 4.3. Diseño lógico
## 4.3.1. Excepciones: dimensión degenerada
## 4.3.1. Excepciones: dimensiones con varios papeles
## 4.3.1. Excepciones: dimensión cajón de sastre
## 4.3.1. Excepciones: SCD (dimensiones lentamente cambiantes)
## 4.3.1. Excepciones: desdoblamiento de dimensiones
## 4.3.1. Excepciones: dimensión de mediciones
## 4.4. Diseño físico
## 4.4.1. Partición de la tabla de hechos
## 4.4.2. Índices de mapa de bits y de join
## 4.5. Conclusiones y bibliografía