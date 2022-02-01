# 21741 - Arquitectures Avançades

## [***Petri Nets***](https://github.com/aNDREUET648/aa.aa_PetriNets)


## Taula de contingut
- [Introducció](#introducció)
- [Enunciat](#enunciat)
- [Requisits mínims](#requisits-mínims)
- [Versión Avanzada](#versión-avanzada)
- [Estructura](#estructura)
- [Client Side](#client-side)
  - [Página principal](#página-principal)
    - [Front-end](#front-end)
    - [Back-end](#back-end)
- [Server Side](#server-side)
- [Bibliografia i Eines](#bibliografia-i-eines)

### Introducció

El ràpid desenvolupament de s'aprenentatge profund ha implicat que ens donem compte que els processadors originals no poden cumplir amb 
Durante el rápido desarrollo del aprendizaje profundo, las personas descubrieron que los procesadoresoriginales no podían cumplir con los cálculos específicos a gran escala de las redes neuronales, y secomenzó a diseñar una gran cantidad de chips especiales para esta aplicación.
La Unidad de procesamientode tensor de Google (TPU para abreviar) es un tipo de diseño representativo que se completó anteriormente.TPU utiliza una unidad de aceleración de computación matricial basada en un diseño de matriz sistólica, quepuede acelerar el cálculo de redes neuronales. .
Esta serie de artículos utilizará los materiales públicosrelacionados con TPU V1, hará ciertas simplificaciones, especulaciones y modificaciones, para escribir unaversión simple de Google TPU, a fin de comprender con mayor precisión las ventajas y limitaciones de TPU.

EL FIN DE LA LEY DE MOORE deja a las arquitecturas de dominio como el futuro de la informática. Un ejemplo pionero ejemplo es la unidad de procesamiento tensorial (TPU) de Google, desplegada por primera vez en 2015, y que hoy proporciona servicios a más de mil millones de personas. Ejecuta redes neuronales profundas (DNN) entre 15 y 30 veces más rápido y con una eficiencia energética entre 30 y 80 veces superior a la de las CPUs actuales y las GPU de tecnologías similares.
Todas las leyes exponenciales deben llegar a su fin. En 1965, Gordon Moore predijo que el número de de transistores por chip se duplicaría cada uno o dos años. A pesar de la afirmación de lo contrario en la portada de Communications (enero de 2017) que "Los informes sobre mi muerte son muy exageradas", la Ley de Moore está efectivamente, está terminando. El chip DRAM presentado en 2014 contenía ocho mil millones de transistores, y un chip DRAM de 16 mil millones 16 mil millones de transistores no se producirá en masa hasta 2019, pero la Ley de Moore predice un aumento de cuatro veces

Por lo tanto, no nos queda nada bajo la manga para continuar mejoras importantes en costo-rendimiento y eficiencia energética para arquitecturas de propósito general. Debido a que el presupuesto de energía es limitado (debido a los límites de electromigración, mecánicos y térmicos de los chips), si queremos un mayor rendimiento (operaciones más altas / segundo), necesitamos    reducir la energía por operación.

Por lo tanto, así como el campo cambió de uniprocesadores a multiprocesadores en la última década por necesidad, la desesperación es la razón por la que los arquitectos ahora están trabajando en DSA. La nueva normalidad es que un competidor consistirá en procesadores estándar para ejecutar programas grandes convencionales, como sistemas operativos, junto con procesadores específicos del dominio que realizan solo una gama estrecha de tareas, pero las hacen extremadamente bien. Por lo tanto, tales computadoras   serán    mucho más heterogénea que los chips multinúcleo homogéneos del    pasado.



---

### Enunciat

La práctica consiste en crear un panel de datos (Data Dashboard). Este panel cogería los datos de una base de datos de su elección en PhPMyAdmin, Xampp. Es un ejemplo de una aplicación web distribuida que tiene un front-end y un back-end.

El desarrollo de la practica será utilizando:
  - Bootstrap: para configurar el front-end.
  - JavaScript y JQuery: para poder modificar los contenidos del DOM de la página web. 
  - Se puede usar Ajax de JQuery para extraer los datos de los archivos del servidor Apache (Xampp) i modificar los contenidos del DOM de la web en base a lo recibido.
  - HighCharts y/o HighMaps: para hacer las visualizaciones en el dashboard.
  - Xampp, php: para el servidor web.

#### Requisits mínims

  - La parte superior del tablero debe tener una barra de navegación que contenga un vínculo con una clase navbar-brand para el título del dashboard y un atributo href igual a "#", un vínculo al sitio web de la UIB y otro vínculo a la fuente de datos. Puede agregar otros enlaces si lo desea.
  - Mínimo tres visualizaciones con Highcharts y/o highmaps.

#### Versión avanzada

  Una versión avanzada opcional de la práctica puede ser mediante extraer datos de una API. API son las siglas de Application Programming Interface (Interfaz de programación de aplicaciones). 
  Una API proporciona una forma conveniente para que dos aplicaciones se comuniquen entre sí. 
  El beneficio es que, si los datos alguna vez cambian, su aplicación web automáticamente tendrá los datos correctos.

  [Enunciado Original](./Data%20Dashboard/Enunciado%20de%20la%20pr%C3%A1ctica.pdf)

---

### Estructura


```
Data Dashboard
├── index.php                             Página web principal (.php)
├── Enunciado de la práctica.pdf          Documento original de la práctica
├── images                                Directorio de las imágenes
│   └── black_flag_logo-768x768.png       Logo de la barra de navegación
│   └── favicon.ico                       Icono de la página (Favorite Icon)
│   └── Página Principal.png              Imagen de muestra de la página finalizada
├── js
│   └── client.js                         JavaScript referenciado en la página principal (index.php) 
│                                         donde realizo mis peticiones jQuery.post al servidor
└── server                                Carpeta "lado servidor"
    └── conexion.php                      Conexión con la base de datos
    └── server.php                        Gestiono los query de MySQL y los devuelvo al cliente
    └── sql
        └── Chinook_MySql.sql             Archivo de datos SQL (versión original)
        └── Chinook - Estructura BD.pdf   Documento que muestra como está construida la BD
        └── chinook               
            └── .IBD, .FRM, .OPT files    Archivos de estructura y formato de la base de datos

```
---

### Client Side

#### Página principal

A continuación vemos una imagen ejemplo de lo que la página muestra al acceder a ella

 ![Sample Page](./Data%20Dashboard/images/sample_page.png?raw=true "Muestra de la página resultante")

##### Front-end

La página principal `index.php` es bastante escueta ya que casi el grueso del código lo gestiona el back-end que se encuentra en el archivo *JavaScript* `client.js`. Así que aquí lo que hacemos es:

- En el `<head>` realizo las llamadas a las librerías *jQuery*, *HighCharts* y hoja de estilo de *Bootstrap* mediante el uso de los CDN disponibles y no tenerlos cargados localmente.
- En el `<body>`:
  -  Diseño (mediante *Bootstrap*) mi barra de navegación y mi estructura (2x2 Grid) donde irán colocadas cada una de mis gráficas.
  -  Realizo la llamada a mi back-end `client.js`.
  -  Incluyo el *Bootstrap JavaScript* al final de la página (justo antes del `</body>`) por recomendación de [ellos mismos](https://getbootstrap.com/docs/5.1/getting-started/introduction/#js).

##### Back-end

  En este archivo `client.js` realizo:
  - Realizo 4 peticiones al servidor (una por cada gráfica) mediante una petición *Ajax HTTP POST*. Para ello uso el método $.post() de *jQuery*
  - Recibo la información del servidor
  - Proceso los datos y los adapto para acomodarlo a los arrays que necesitaré
  - Defino el tipo de gráficas que necesitaré y las parametrizo (usando la biblioteca de [*HighCharts*](https://www.highcharts.com/docs/index))
  - Los datos arrays obtenidos anteriormente los inserto en mis gráficas para que puedan ser representados en mi página Web

#### Server Side

En el servidor, tenemos hospedada nuestra base de datos *MySQL* `chinook` a la que accederemos a través del archivo `server.php`. Este archivo:
  - Establecerá la conexión con nuestra base de datos a través del fichero `conexion.php`
  - Mediante el uso de la variable *Superglobal* ***$_POST*** recopilará los datos que le ha enviado el cliente la petición HTTP POST
  - Dependiendo del tipo de solicitud ejecutará una de las distintas consultas (*query*) de mi base de datos que tengo el servidor

Para comunicar mi base de datos MySQL con mi aplicación PHP `server.php` usaré la ***API PHP mysqli extension*** mediante llamadas a funciones (procedural).

`conexion.php`
```
<?php 
$server = "localhost"; // El servidor que utilizaremos, en este caso será el localhost 
$user = "andreu"; // El usuario que tiene privilegios en la base de datos 
$pass = "andreu"; // La contraseña del usuario que utilizaremos 
$database = "chinook"; // El nombre de la base de datos 
$connection = mysqli_connect($server, $user, $pass);
if (!$connection) { 
    die('<strong>No pudo conectarse:</strong> ' . mysqli_connect_error()); // es equivalente a exit()
}
mysqli_select_db($connection, $database); 
?>
```

El código de arriba crea una conexión con el servidor de MySQL hospedado en *$server=localhost* y a continuación selecciona la base de datos por defecto empleará *mysqli_select_db($connection, 'chinook')*

`server.php`
```
$sql = "SELECT...";
$query = mysqli_query($connection, $sql);
$resultado =  mysqli_fetch_all($query,MYSQLI_NUM);
echo json_encode($resultado);
```
De forma resumida este el código que realiza el servidor: 
  - Realiza una consulta a la base de datos con `mysqli_query`
  - Obtiene todas las filas de resultados y devuelve el conjunto de resultados como un array numérico con `mysqli_fetch_all`
  - Devuelve los resultados al cliente que hizo la petición codificándolo antes en formato *JSON* con `json_encode`


---

### Bibliografia i Eines

  - [w3schools](https://www.w3schools.com/default.asp) - Tutoriales HTML, PHP, JavaScript, jQuery
  - [jQuery](https://jquery.com/) - Biblioteca multiplataforma de JavaScript. Manuales de referencia (Write less, do more)
  - [php](https://www.php.net/) - Lenguaje de programación. Manuales de referencia
  - [Bootstrap](https://getbootstrap.com/) - Biblioteca multiplataforma para el diseño de sitios y aplicaciones web (front-end side). Templates y documentación de referencia
  - [HighCharts](https://www.highcharts.com/) - Biblioteca en JavaScript para visualización de gráficos (back-end side). Templates y documentación de referencia
  - [MySQL and PHP](https://downloads.mysql.com/docs/apis-php-en.pdf) - Documentación de la API MySQL con PHP.


---
[aNDREUET648](https://github.com/aNDREUET648)
