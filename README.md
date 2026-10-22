# Diseño y construcción de una incubadora neonatal a escala

## Resumen

Se construyó un prototipo de incubadora neonatal a escala con cubierta de acetato calibre 15, base de cartón prensado, rejilla interior de acrílico y uniones con cinta doble faz. Las dimensiones previstas eran 44 × 20 × 22 cm y las finales fueron 44,5 × 23 × 25 cm, lo que representa un aumento aproximado del 32,2 % en el volumen geométrico exterior. Se implementó físicamente un sistema de pesaje con celda de carga, HX711, ESP32 y pantalla LCD I²C; con una masa patrón de 1.800 g se obtuvo una lectura de 1.817,9 g (error de 0,99 %). El circuito de temperatura se desarrolló únicamente en simulación (Proteus), con fuente regulada de 5 V, termistor PTC, comparadores LM358 y tres LED indicadores para el intervalo de 36 a 37,5 °C; además, se midió el PTC físico, cuya resistencia pasó de 2,26 kΩ a 2,70 kΩ al calentarlo. El costo total aproximado fue de $250.000 COP. El informe distingue lo comprobado físicamente de lo simulado y señala las pruebas pendientes, entre ellas la regulación real de la temperatura dentro de la cabina.

**Palabras clave:** incubadora neonatal, PTC, LM358, Proteus, ESP32, HX711, celda de carga, termorregulación.

## Introducción

Los recién nacidos, en especial los prematuros, tienen dificultad para conservar su temperatura corporal, por lo que la regulación térmica es una de las funciones principales de una incubadora neonatal [4], [5]. Las complicaciones asociadas al nacimiento prematuro figuran entre las principales causas de mortalidad infantil [6], lo que justifica el interés en estos equipos. Además de la temperatura, una incubadora clínica controla o vigila otras variables del entorno, como la humedad y el flujo de aire, y puede incorporar la medición de la masa corporal del paciente.

En esta práctica se construyó una maqueta didáctica que integra una estructura transparente, un sistema de pesaje y el diseño simulado de un circuito que detecta si la temperatura está por debajo, dentro o por encima del intervalo de 36 a 37,5 °C establecido en la guía de laboratorio [1]. La guía también solicita indicación mediante tres LED, medición de peso con visualización, estimación de costos y comparación con equipos comerciales. Existen antecedentes de incubadoras de bajo costo y de prototipos académicos similares [2], [3].

El documento se organiza en tres partes: la Parte A describe el diseño y la simulación de los circuitos de temperatura y de peso; la Parte B presenta la construcción del prototipo, sus dimensiones y su costo; y la Parte C analiza los resultados, responde las preguntas de discusión y presenta las conclusiones. A lo largo del informe se indica de forma explícita qué se comprobó físicamente y qué solo se desarrolló en simulación o en diseño.

## Objetivos

**Objetivo general:** diseñar y construir una incubadora neonatal a escala, implementar la medición de peso y desarrollar en Proteus el circuito electrónico de temperatura.

**Objetivos específicos:**

1. Construir la estructura con los materiales seleccionados.
2. Simular la fuente y el circuito con PTC y LM358.
3. Implementar y calibrar la báscula.
4. Analizar dimensiones, mediciones y costos.

# Parte A. Diseño y simulación de los circuitos

## A.1. Circuito de temperatura en Proteus

Se diseñó el circuito de temperatura en Proteus, dividido en fuente de alimentación, sensor PTC, comparadores LM358 e indicadores luminosos. El objetivo fue distinguir temperatura baja, normal y alta respecto al intervalo de 36–37,5 °C. **El circuito térmico se trabajó en simulación; no se comprobó instalado como controlador de la cabina.** Esto significa que la simulación demuestra la lógica de comparación, pero no que el prototipo físico mantenga la temperatura dentro del intervalo.

<div align="center">

<img width="1717" height="428" alt="image" src="https://github.com/user-attachments/assets/87640af2-1594-4309-8dee-df7ee7644c51" />

**[FIGURA 1: Circuito de temperatura en Proteus]**

</div>

### Fuente de 5 V

La fuente parte de un transformador reductor conectado a un puente rectificador formado por cuatro diodos 1N4007. La fuente de Proteus representó una red de 115 V eficaces y 60 Hz (aproximadamente 162,6 V de pico). El capacitor C1 de 4700 µF suaviza la tensión rectificada y el capacitor C2 de 330 nF ayuda al filtrado de entrada. El LM7805 regula la tensión a 5 V [8]; C3 de 0,1 µF y C4 de 100 nF desacoplan la salida. En la simulación se obtuvo aproximadamente **5,00 V DC**.

Para una tensión secundaria **hipotética** de 9 V eficaces, la tensión pico después de dos diodos (cada uno con una caída cercana a 0,7 V) puede estimarse como:

<p align="center"><img src="https://latex.codecogs.com/svg.image?%5Cdpi%7B140%7D%5Cbg%7Bwhite%7DV_%7Bp%7D%3D9%5Csqrt%7B2%7D-2%280%7B%2C%7D7%29%5Capprox11%7B%2C%7D33%5C%20%5Cmathrm%7BV%7D%5Cqquad%5Cqquad%281%29" alt="Ecuación 1"></p>

El rizado aproximado de un rectificador de onda completa a 60 Hz es:

<p align="center"><img src="https://latex.codecogs.com/svg.image?%5Cdpi%7B140%7D%5Cbg%7Bwhite%7D%5CDelta%20V%5Capprox%5Cfrac%7BI%7D%7B120C%7D%5Cqquad%5Cqquad%282%29" alt="Ecuación 2"></p>

Para 0,1 A y 4700 µF, el resultado sería 0,177 V. Son cálculos ilustrativos, no mediciones de la corriente real del circuito. Antes de construir una fuente física habría que verificar la potencia que disipa el regulador, el margen entre su entrada y su salida y la corriente máxima que debe entregar.

<div align="center">

</div>

### Sensor PTC y divisor de tensión

Se usó un termistor PTC, cuya resistencia aumenta al calentarse. Se comprobó con multímetro que a temperatura ambiente marcaba **2,26 kΩ** y después de aplicar aire caliente **2,70 kΩ** (Tabla 1). La variación fue:

<div align="center">

| Condición | Resistencia |
|:--|--:|
| Temperatura ambiente | 2,26 kΩ |
| Aire caliente | 2,70 kΩ |
| Incremento | 0,44 kΩ (19,47 %) |

**Tabla 1.** Resistencia del PTC físico medida con multímetro.

</div>

<p align="center"><img src="https://latex.codecogs.com/svg.image?%5Cdpi%7B140%7D%5Cbg%7Bwhite%7D%5CDelta%20R%3D2%7B%2C%7D70-2%7B%2C%7D26%3D0%7B%2C%7D44%5C%20%5Cmathrm%7Bk%5COmega%7D%5Cqquad%5Cqquad%283%29" alt="Ecuación 3"></p>

<p align="center"><img src="https://latex.codecogs.com/svg.image?%5Cdpi%7B140%7D%5Cbg%7Bwhite%7D%5C%25%5CDelta%20R%3D%5Cfrac%7B0%7B%2C%7D44%7D%7B2%7B%2C%7D26%7D%5Ctimes100%3D19%7B%2C%7D47%5C%25%5Cqquad%5Cqquad%284%29" alt="Ecuación 4"></p>

No se midió la temperatura exacta correspondiente a cada resistencia, por lo que estos datos confirman el comportamiento PTC pero no permiten calibrar directamente 36 °C y 37,5 °C.

En Proteus, el PTC se conectó entre +5 V y el nodo de señal; la resistencia R1 de 1 kΩ se conectó entre ese nodo y tierra, formando un divisor de tensión. El voltaje resultante es:

<p align="center"><img src="https://latex.codecogs.com/svg.image?%5Cdpi%7B140%7D%5Cbg%7Bwhite%7DV_%7Bsensor%7D%3D5%5Cfrac%7BR_1%7D%7BR_%7BPTC%7D%2BR_1%7D%5Cqquad%5Cqquad%285%29" alt="Ecuación 5"></p>

Con los valores de resistencia medidos, se calculan aproximadamente **1,53 V** a temperatura ambiente y **1,35 V** después de calentar el sensor. Así, en esta conexión, cuando aumenta la temperatura disminuye el voltaje del nodo de medición. Estos valores se calcularon con las resistencias del sensor físico; el modelo PTC_NICKEL de Proteus no está demostrado como equivalente exacto a ese sensor.

<div align="center">

</div>

### Comparadores LM358 y LED

El LM358 contiene dos amplificadores operacionales utilizados como comparadores [7]. Se alimentó con 5 V en el pin 8 y tierra en el pin 4. Los potenciómetros RV1 y RV2 de 10 kΩ permiten ajustar las referencias de comparación, que determinan a qué voltaje cambia el estado de cada salida.

En **U2:A**, la señal del PTC entra al pin 3 (+), la referencia RV1 al pin 2 (−) y la salida se toma del pin 1. Si el voltaje del sensor supera la referencia, la salida tiende a nivel alto y se utiliza para encender el **LED rojo D5**, asociado a temperatura baja (con poca temperatura, la resistencia del PTC es menor y el voltaje del sensor es mayor).

En **U2:B**, la referencia RV2 llega al pin 5 (+), la señal del sensor al pin 6 (−) y la salida corresponde al pin 7. Cuando la referencia supera el voltaje del sensor, la salida tiende a nivel alto y se utiliza para encender el **LED rojo D6**, asociado a temperatura alta.

Los LED rojos se conectaron con resistencias de 1 kΩ. Para representar la condición normal se añadió una etapa con **dos transistores 2N2222**, resistencias de base de 10 kΩ y el **LED verde D7**. Las salidas de los comparadores controlan los transistores: si ninguna alarma se activa, el nodo de control puede permanecer alto y encender el LED verde; si se activa alguna alarma, el transistor correspondiente lleva ese nodo hacia tierra. Esta es la lógica diseñada; no se dispone de evidencia final de las tres condiciones funcionando simultáneamente. También debe revisarse la corriente que el LM358 puede entregar a cada rama de LED.

Los valores de referencia explorados en Proteus (2,50 V, 3,00 V y 3,50 V) fueron ajustes eléctricos, no temperaturas calibradas. Con las resistencias medidas del PTC físico, el voltaje del sensor calculado está entre 1,35 y 1,53 V, por lo que esas referencias tendrían que reajustarse al rango real del sensor y calibrarse con un termómetro patrón para que correspondan a 36 °C y 37,5 °C.

<div align="center">
  
</div>

### Relé y calefactor

Se consideró un módulo de relé de 5 V para encender o apagar un elemento calefactor de acuerdo con las señales de control. El relé dispone de terminales VCC, GND e IN, y contactos COM, NO y NC. La función prevista era activar el calentamiento por debajo de 36 °C y desactivarlo al alcanzar 37,5 °C, conservando el estado anterior mientras la temperatura esté dentro de la banda [1], [4]. Esa última condición exige histéresis o una memoria adicional (por ejemplo, un comparador con realimentación o un biestable), que no se obtiene solo con los dos comparadores del esquema. **La etapa de relé y el control automático completo no quedaron verificados en la simulación documentada**; por ello no se presentan como resultados finales de funcionamiento.

### Conexiones detalladas del circuito de temperatura simulado

Las conexiones realizadas o previstas en Proteus se resumen en la Tabla 2.

<div align="center">

| Elemento | Conexión realizada o prevista en Proteus |
|:--|:--|
| Puente rectificador | Cuatro 1N4007 en puente; terminales `~` al secundario del transformador; salida positiva al filtro y negativa a GND |
| Capacitor de filtrado | 4700 µF entre positivo rectificado y GND, respetando polaridad |
| LM7805 | Entrada a tensión filtrada, terminal GND a tierra y salida a la línea de +5 V |
| Capacitores de desacoplamiento | 0,33 µF entre entrada del LM7805 y GND; 0,1 µF entre salida y GND |
| PTC | Un extremo a +5 V y el otro al nodo `V_SENSOR` |
| R1 de 1 kΩ | Entre `V_SENSOR` y GND |
| LM358 alimentación | Pin 8 a +5 V y pin 4 a GND |
| LM358 U2:A | Pin 3 a `V_SENSOR`, pin 2 al cursor de RV1, pin 1 a la rama del LED rojo de temperatura baja |
| LM358 U2:B | Pin 5 al cursor de RV2, pin 6 a `V_SENSOR`, pin 7 a la rama del LED rojo de temperatura alta |
| RV1 y RV2 de 10 kΩ | Extremos a +5 V y GND; cursores a las entradas de referencia respectivas |
| D5 y D6 | Cada LED rojo con resistencia limitadora de 1 kΩ desde la salida de su comparador hacia GND |
| Q1 y Q2, 2N2222 | Emisores a GND; bases mediante resistencias de 10 kΩ a las salidas de los comparadores; colectores unidos en `CONTROL` |
| Nodo `CONTROL` | Resistencia de 1 kΩ a +5 V y rama de LED verde D7 con resistencia de 1 kΩ hacia GND |
| Módulo de relé de 5 V | `VCC` a +5 V, `GND` a GND y `IN` a la señal de mando por verificar; contactos `COM`, `NO` y `NC` destinados a conmutar el calefactor |

**Tabla 2.** Conexiones del circuito de temperatura simulado en Proteus.

</div>

La polaridad de los LED y la lógica activa del módulo de relé deben coincidir con los componentes de la simulación. La conexión del mando `IN` no se da como verificada porque la integración final del relé quedó pendiente. El calefactor de tensión de red no se conectó como montaje físico validado.

<div align="center">

</div>

## A.2. Circuito de medición de peso

El sistema de peso sí se implementó físicamente. La **celda de carga** contiene galgas extensométricas: al deformarse por el peso, su resistencia cambia y produce una señal eléctrica muy pequeña. El **HX711** amplifica esa señal y la convierte en datos digitales de 24 bits [10]; el **ESP32** procesa las lecturas [9] y muestra el resultado en una **pantalla LCD 16 × 2 con interfaz I²C**. También se desarrolló una página accesible desde el teléfono mediante la red Wi-Fi que crea el propio ESP32.

Las conexiones principales se resumen en la Tabla 3.

<div align="center">

| Conexión | Terminal de origen | Terminal de destino |
|:--|:--|:--|
| HX711–ESP32 | VCC / GND | 3,3 V / GND |
| HX711–ESP32 | DT / SCK | GPIO 19 / GPIO 18 |
| LCD–ESP32 | SDA / SCL | GPIO 21 / GPIO 22 |
| LCD–ESP32 | VCC / GND | 5 V / GND |

**Tabla 3.** Conexiones principales del sistema de pesaje.

</div>

En la celda de carga de cuatro hilos, la conexión convencional es rojo a E+, negro a E−, verde a A+ y blanco a A− del HX711; estos colores deben coincidir con el cableado de la celda utilizada. En un montaje definitivo, el bus I²C debe adaptarse a los niveles lógicos de 3,3 V del ESP32 cuando el módulo LCD incorpora resistencias de subida a 5 V.

<div align="center">

</div>

### Conexiones detalladas de la báscula

<div align="center">

| Dispositivo | Terminal | Conectar a |
|:--|:--|:--|
| Celda de carga (código de colores habitual) | Rojo | HX711 `E+` |
| Celda de carga | Negro | HX711 `E−` |
| Celda de carga | Verde | HX711 `A+` |
| Celda de carga | Blanco | HX711 `A−` |
| HX711 | `VCC` | ESP32 `3V3` |
| HX711 | `GND` | ESP32 `GND` |
| HX711 | `DT` / `DOUT` | ESP32 `GPIO 19` |
| HX711 | `SCK` | ESP32 `GPIO 18` |
| LCD 16×2 I²C | `VCC` | ESP32 `5V` / `VIN` |
| LCD 16×2 I²C | `GND` | ESP32 `GND` |
| LCD 16×2 I²C | `SDA` | ESP32 `GPIO 21` |
| LCD 16×2 I²C | `SCL` | ESP32 `GPIO 22` |

**Tabla 4.** Conexiones detalladas de la báscula.

</div>

La dirección de la LCD utilizada en el programa fue `0x27`. El ESP32 se alimentó por USB. Se debe confirmar el orden de colores de la celda específica y evitar aplicar señales I²C de 5 V directamente a los pines del ESP32 si el adaptador de la pantalla tiene resistencias de subida a 5 V.

<div align="center">

</div>

### Procesamiento de la señal

El programa se desarrolló en Arduino IDE. Realiza la **tara** con la plataforma vacía, aplica un factor de calibración y filtra las lecturas para disminuir las fluctuaciones. Se implementó un filtro exponencial de primer orden, que combina la lectura nueva con la anterior:

<p align="center"><img src="https://latex.codecogs.com/svg.image?%5Cdpi%7B140%7D%5Cbg%7Bwhite%7Dy_n%3D%5Calpha%20x_n%2B%281-%5Calpha%29y_%7Bn-1%7D%5Cqquad%5Cqquad%286%29" alt="Ecuación 6"></p>

Se utilizaron coeficientes α de 0,85 para cambios grandes (respuesta rápida) y de 0,25 para variaciones pequeñas (mayor suavizado). Además, se definió una zona de cero de 5 g y un cambio mínimo de visualización de 2 g, para que el valor mostrado no oscile por ruido. Al detectar una carga de al menos 10 g, comienza una medición de **11 segundos**, después de la cual se conserva la lectura final. La pantalla muestra el peso y mensajes como `PESO ACTUAL`, `MIDIENDO...` y `PESO MEDIDO`. El ESP32 crea la red Wi-Fi `Bascula_ESP32`; desde el navegador, en la dirección `192.168.4.1`, se puede consultar el peso (ruta `/peso`) y realizar la tara (ruta `/tarar`). No se publican credenciales de acceso en este informe.

La masa se calcula a partir de la lectura digital y la tara:

<p align="center"><img src="https://latex.codecogs.com/svg.image?%5Cdpi%7B140%7D%5Cbg%7Bwhite%7Dm%3D%5Cfrac%7BL-L_0%7D%7BF%7D%5Cqquad%5Cqquad%287%29" alt="Ecuación 7"></p>

Donde <i>L</i> es la lectura, <i>L<sub>0</sub></i> es la lectura de tara y <i>F</i> es el factor de calibración, expresado en cuentas por gramo.

### Calibración y error

Para calibrar se tomaron lecturas sin carga y con una masa patrón de 1.800 g, usando promedios de 30 muestras en cada caso. El factor se obtuvo dividiendo la diferencia entre el promedio con carga y el promedio sin carga entre la masa patrón. En la última calibración documentada se utilizó **F = 56,95833**; una comprobación intermedia dio 1.783,3 g; tras integrar el factor al programa principal, la prueba final dio el resultado que se presenta a continuación.

En la prueba final, el patrón de **1.800 g** produjo una lectura de **1.817,9 g**. El error fue:

<p align="center"><img src="https://latex.codecogs.com/svg.image?%5Cdpi%7B140%7D%5Cbg%7Bwhite%7DE_a%3D%7C1817%7B%2C%7D9-1800%7C%3D17%7B%2C%7D9%5C%20%5Cmathrm%7Bg%7D%5Cqquad%5Cqquad%288%29" alt="Ecuación 8"></p>

<p align="center"><img src="https://latex.codecogs.com/svg.image?%5Cdpi%7B140%7D%5Cbg%7Bwhite%7DE_%7B%5C%25%7D%3D%5Cfrac%7B17%7B%2C%7D9%7D%7B1800%7D%5Ctimes100%5Capprox0%7B%2C%7D99%5C%25%5Cqquad%5Cqquad%289%29" alt="Ecuación 9"></p>

<div align="center">

| Parámetro | Resultado |
|:--|--:|
| Masa de referencia | 1.800 g |
| Lectura final | 1.817,9 g |
| Diferencia con signo | +17,9 g |
| Error absoluto | 17,9 g |
| Error porcentual | 0,99 % (0,9944 %) |

**Tabla 5.** Resultado de la prueba final de pesaje con masa patrón.

</div>

La lectura fue 17,9 g mayor que la referencia. Este valor es una comprobación puntual: con una sola medición y una sola masa no se puede afirmar nada sobre la repetibilidad, la linealidad ni la incertidumbre del sistema en todo el rango. Entre las posibles causas del error están el ruido eléctrico, las vibraciones, el apoyo desigual de la masa, la deriva del cero y diferencias entre las condiciones de calibración y de la prueba final.

<div align="center">

<img width="1280" height="889" alt="WhatsApp Image 2026-10-22 at 2 06 22 PM" src="https://github.com/user-attachments/assets/d6d98049-524d-406d-a586-5d7cebbc936c" />

</div>

# Parte B. Construcción del prototipo

## B.1. Materiales y armado

Se elaboró la cubierta exterior con **acetato calibre 15**, elegido por su transparencia, que permite observar el interior; la **base se hizo en cartón prensado**, económico y fácil de cortar; la **rejilla interior** se hizo con lámina de acrílico, que sirve de plataforma o soporte; y se utilizó **cinta doble faz** para unir las piezas sin perforaciones extensas (Tabla 6). La estructura se preparó según las dimensiones previstas, se ajustaron las piezas durante el ensamble y se incorporó el sistema de pesaje.

<div align="center">

| Material | Aplicación |
|:--|:--|
| Acetato calibre 15 | Cubierta exterior transparente |
| Cartón prensado | Base estructural |
| Lámina de acrílico | Rejilla interior |
| Cinta doble faz | Unión y fijación de piezas |
| Celda de carga, HX711, ESP32 y LCD | Medición y visualización de peso |

**Tabla 6.** Materiales empleados y su función.

</div>

Estos materiales son apropiados para una maqueta didáctica, pero no se evaluaron sus propiedades térmicas, su resistencia al fuego, su facilidad de limpieza ni su biocompatibilidad, que serían indispensables en un equipo clínico. Tampoco se dispone de ensayos que demuestren que el cartón o la cinta doble faz soporten la temperatura de operación de un calefactor.

<div align="center">

<img width="1600" height="1200" alt="image" src="https://github.com/user-attachments/assets/ac0b463d-8bc0-4ea6-818c-cf63f589563f" />

**[FIGURA 2: Prototipo final]**

</div>

## B.2. Dimensiones y cálculos

Durante el armado las dimensiones finales resultaron algo mayores que las previstas, para acomodar la estructura y los componentes (Tabla 7).

<div align="center">

| Dimensión | Diseño previsto | Construcción final |
|:--|--:|--:|
| Largo | 44 cm | 44,5 cm |
| Ancho | 20 cm | 23 cm |
| Alto | 22 cm | 25 cm |

**Tabla 7.** Dimensiones previstas y finales del prototipo.

</div>

El volumen geométrico exterior se estima como:

<p align="center"><img src="https://latex.codecogs.com/svg.image?%5Cdpi%7B140%7D%5Cbg%7Bwhite%7DV%3DL%5Ctimes%20A%5Ctimes%20H%5Cqquad%5Cqquad%2810%29" alt="Ecuación 10"></p>

<p align="center"><img src="https://latex.codecogs.com/svg.image?%5Cdpi%7B140%7D%5Cbg%7Bwhite%7DV_%7Bprevisto%7D%3D44%2820%29%2822%29%3D19%5C%2C360%5C%20%5Cmathrm%7Bcm%5E3%7D%3D19%7B%2C%7D36%5C%20%5Cmathrm%7BL%7D%5Cqquad%5Cqquad%2811%29" alt="Ecuación 11"></p>

<p align="center"><img src="https://latex.codecogs.com/svg.image?%5Cdpi%7B140%7D%5Cbg%7Bwhite%7DV_%7Bfinal%7D%3D44%7B%2C%7D5%2823%29%2825%29%3D25%5C%2C587%7B%2C%7D5%5C%20%5Cmathrm%7Bcm%5E3%7D%5Capprox25%7B%2C%7D59%5C%20%5Cmathrm%7BL%7D%5Cqquad%5Cqquad%2812%29" alt="Ecuación 12"></p>

<p align="center"><img src="https://latex.codecogs.com/svg.image?%5Cdpi%7B140%7D%5Cbg%7Bwhite%7D%5C%25%5CDelta%20V%3D%5Cfrac%7B25%5C%2C587%7B%2C%7D5-19%5C%2C360%7D%7B19%5C%2C360%7D%5Ctimes100%5Capprox32%7B%2C%7D17%5C%25%5Cqquad%5Cqquad%2813%29" alt="Ecuación 13"></p>

Por tanto, el volumen exterior final fue aproximadamente **32,2 % mayor** (unos 6,23 L) que el inicialmente previsto. El valor no representa el volumen útil de aire, pues no descuenta el espesor de los materiales ni los componentes internos.

La guía presenta como referencia una incubadora de aproximadamente 90 × 40 × 45 cm [1]; el prototipo es una versión reducida. Sus dimensiones equivalen a cerca del 49,4 % del largo, 57,5 % del ancho y 55,6 % del alto de esa referencia. Como los porcentajes no coinciden, no se trata de una reducción a escala uniforme, y no existe un registro de diseño que permita atribuir una causa única a cada ajuste.

## B.3. Costo y comparación

El costo total aproximado de los materiales y componentes empleados fue de **$250.000 COP**. No se cuenta con el valor individual verificado de cada componente, por lo que no se asignan precios parciales inventados.

El prototipo se diferencia de una incubadora comercial en que reproduce elementos de construcción y medición, mientras que los equipos clínicos, como los de los fabricantes mencionados en la guía (Dräger, Instrumentalia S.A.S. y LEEX Medical) [1], incorporan sistemas integrados de regulación, alarmas, validación, seguridad y soporte técnico, según el modelo (Tabla 8).

<div align="center">

| Criterio | Prototipo académico | Incubadora neonatal comercial |
|:--|:--|:--|
| Finalidad | Demostración didáctica | Atención clínica neonatal |
| Dimensiones | 44,5 × 23 × 25 cm | Dependen del modelo |
| Control térmico | Circuito simulado; sin validación en cabina | Sistema regulado y validado según modelo |
| Pesaje | Implementado; error de 0,99 % en una prueba | Opcional o integrado según modelo, con especificaciones propias |
| Alarmas | LED diseñados en simulación | Alarmas clínicas y protecciones según modelo |
| Costo | ≈ $250.000 COP | - |

**Tabla 8.** Comparación cualitativa entre el prototipo y una incubadora comercial.

</div>

No se establece una diferencia porcentual de precio porque no se cuenta con una cotización comercial comparable. Para hacerla se necesitarían el modelo, la ficha técnica y el precio vigente de al menos un equipo de uno de los fabricantes citados.

# Parte C. Resultados, discusión y conclusiones

## C.1. Análisis de resultados

El circuito de temperatura permitió simular la alimentación regulada a 5 V, la respuesta del PTC y la comparación de voltajes mediante el LM358. En la prueba física del termistor se observó un aumento de resistencia de **2,26 kΩ a 2,70 kΩ** (19,47 %) después de aplicar aire caliente. No se registraron temperaturas exactas para esos dos puntos, por lo que no se obtuvo una calibración en grados Celsius. En consecuencia, no es posible cuantificar el sobreimpulso, el tiempo de establecimiento, el error térmico ni la permanencia dentro del intervalo de 36 a 37,5 °C.

El sistema de peso permitió adquirir, procesar y visualizar lecturas mediante el ESP32, el HX711 y la LCD. Con una masa conocida de **1,8 kg**, se obtuvo **1,8179 kg**, equivalente a **17,9 g de error** y **0,99 % de error porcentual**. Esta prueba verifica el comportamiento en ese punto de medición, pero no permite calcular la precisión para todas las masas.

La estructura quedó con dimensiones de **44,5 × 23 × 25 cm**, usando los materiales descritos y con un costo global aproximado de **$250.000 COP**.

## C.2. Preguntas de discusión

**1. ¿Qué otras variables son críticas en el monitoreo neonatal?** Además de temperatura y peso, son importantes la humedad relativa, la circulación de aire y, en el neonato, la frecuencia cardíaca, la frecuencia respiratoria, los episodios de apnea y la saturación de oxígeno. También influyen la temperatura de la piel, la concentración de oxígeno cuando se administra oxigenoterapia, el ruido y la iluminación. Estas variables permiten evaluar tanto el entorno como las condiciones del paciente, y cada una requiere sensores adecuados y alarmas con criterios clínicos [4], [5].

**2. ¿Qué haría falta para convertir el sistema en una incubadora neonatal real?** Sería necesario integrar y verificar físicamente el control térmico, con histéresis y sensores redundantes; añadir alarmas independientes y de seguridad; asegurar la estabilidad de las condiciones ambientales, con circulación de aire y humidificación controladas; utilizar materiales seguros y limpiables, aptos para uso sanitario; garantizar la seguridad eléctrica; validar el software y la medición con varios patrones; y cumplir la gestión de riesgos, los ensayos y los requisitos de certificación aplicables a equipos médicos. La maqueta actual no es apta para uso con personas.

**3. ¿Qué semejanzas existen entre una incubadora neonatal y una servo-cuna?** Ambas buscan ayudar a conservar la temperatura del recién nacido mediante calefacción controlada, usan sensores y sistemas de regulación y pueden incorporar alarmas. La incubadora utiliza una cabina cerrada para regular el microambiente, mientras que una servo-cuna suele ser abierta y emplea un calefactor radiante, lo que da acceso directo al paciente pero cambia la transferencia de calor, el control de humedad y las pérdidas al ambiente [4].

## C.3. Conclusiones

El desarrollo del prototipo de incubadora neonatal a escala permitió integrar conocimientos de instrumentación biomédica, electrónica y construcción de estructuras, con el propósito de representar dos funciones fundamentales de una incubadora: el monitoreo de la temperatura y la medición del peso.

Se diseñó y simuló en Proteus un circuito de temperatura utilizando un termistor PTC, un amplificador operacional LM358, una fuente regulada de 5 V y componentes electrónicos complementarios. Las mediciones físicas del PTC mostraron un aumento de resistencia de 2,26 kΩ a 2,70 kΩ al aplicarle aire caliente, lo que permitió comprobar su respuesta ante cambios térmicos. Sin embargo, el circuito de regulación de temperatura no se implementó físicamente en el prototipo.

En cuanto al sistema de pesaje, se logró integrar una celda de carga con el módulo HX711, el ESP32 y una pantalla LCD I²C. Durante las pruebas se obtuvo una lectura de 1817,9 g para una masa de referencia de 1800 g, equivalente a un error relativo del 0,99 %. Este resultado permitió comprobar el funcionamiento del sistema de adquisición y visualización del peso.

La construcción de la estructura con acetato calibre 15, cartón prensado, acrílico y cinta doble faz permitió obtener un prototipo de 44,5 × 23 × 25 cm, adaptando las dimensiones inicialmente planteadas. El trabajo demuestra la posibilidad de desarrollar un modelo didáctico con materiales accesibles y un costo aproximado de $250.000 COP.

Finalmente, el siguiente paso para continuar el desarrollo sería integrar físicamente el circuito de temperatura, calibrar los umbrales de funcionamiento y comprobar experimentalmente la estabilidad térmica dentro de la estructura. Esto permitiría avanzar desde una simulación electrónica y un sistema de pesaje funcional hacia un prototipo con mayor integración de sus subsistemas.

# Referencias bibliográficas

[1] Universidad Militar Nueva Granada, *Guía de preparación de práctica de laboratorio: Diseño y construcción de una incubadora neonatal a escala*, Instrumentación Biomédica y Biosensores, revisión 3, 2025.

[2] C. G. K. Tran *et al*., “Designing a low-cost multifunctional infant incubator,” *Journal of Laboratory Automation*, vol. 19, no. 3, pp. 332–337, 2014. https://doi.org/10.1177/2211068214530391

[3] L. Restrepo-Pérez, N. Durango-Londoño, N. E. Gómez-Suárez, F. González-Ramírez y N. Rivera-Bonilla, “Prototipo de incubadora neonatal,” *Revista Ingeniería Biomédica*, vol. 1, no. 1, pp. 55–59, 2007. http://www.scielo.org.co/pdf/rinbi/v1n1/v1n1a12.pdf

[4] M. van Leeuwen *et al*., *European Standards of Care for Newborn Health: Temperature management in newborn infants*, European Foundation for the Care of Newborn Infants (EFCNI), 2018. https://newborn-health-standards.org/standards/standards-english/care-procedures/temperature-management-in-newborn-infants/

[5] R. B. Knobel-Dail, “Role of effective thermoregulation in premature neonates,” *Research and Reports in Neonatology*, vol. 4, pp. 147–156, 2014. https://doi.org/10.2147/RRN.S52377

[6] R. E. Black, S. Cousens, H. L. Johnson *et al*., “Global, regional, and national causes of child mortality in 2008: a systematic analysis,” *The Lancet*, vol. 375, pp. 1969–1987, 2010. https://doi.org/10.1016/S0140-6736(10)60549-1

[7] Texas Instruments, *LM358: Dual Operational Amplifier*, hoja de datos. https://www.ti.com/product/LM358

[8] Texas Instruments, *LM7805 / LM340: 5-V Voltage Regulator*, documentación de producto. https://www.ti.com/product/LM340

[9] Espressif Systems, *ESP32 Series Datasheet*. https://www.espressif.com/sites/default/files/documentation/esp32_datasheet_en.pdf

[10] AVIA Semiconductor, *HX711: 24-Bit Analog-to-Digital Converter (ADC) for Weigh Scales*, hoja de datos. https://cdn.sparkfun.com/datasheets/Sensors/ForceFlex/hx711_english.pdf
