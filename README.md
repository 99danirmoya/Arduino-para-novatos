<div align="center">
  <img src="https://github.com/medialablpwan/arduinoparanovatos/blob/main/pics/photo-1603732551658-5fabbafa84eb.jpg"/>
</div>
<br/>

<img src="https://notion-emojis.s3-us-west-2.amazonaws.com/prod/svg-twitter/1f4a1.svg" width="50"/>

<div align="justify">

# Arduino Course for Beginners

En esta página tomaré apuntes extraidos del curso “Arduino Course for Beginners”.

</div>

<div align="centre">

[![Arduino Course for Beginners](https://github.com/medialablpwan/arduinoparanovatos/blob/main/pics/Screenshot%202023-12-17%20131855.png)](https://youtu.be/zJ-LqeX_fLU)

</div>

___

<div align="justify">

## CONCEPTOS BÁSICOS

En este apartado me centraré en describir los aspectos más básicos del hardware de la placa Arduino UNO R3, la más extendida. Echando un vistazo a la placa, se observa:

</div>

<img src="https://github.com/medialablpwan/arduinoparanovatos/blob/main/pics/Screenshot_2023-09-21_104445.png" align="right" width="500px"/>

- El LED “L” está conectado al pin 13, por lo que cada vez que se ponga a “high”, “L” se encenderá

- El chip principal es el MCU ATmega328

- Contamos con 14 pines digitales (no todos sirven para lo mismo y hay algunos especiales, como 6 capaces de PWM) y 6 pines analógicos

- LED RX y TX, que se encenderán cuando la placa esté recibiendo o enviando información, respectivamente

- Botón de reset que reinicia el código sin limpiar la memoria

<br clear="right"/>

<div align="justify">

___

## CONCEPTOS TÉCNICOS

En este apartado mostraré información técnica del microcontrolador [Arduino UNO](https://docs.arduino.cc/hardware/make-your-uno-kit), especialmente aspectos de la circuitería vistos en el grado.

### ESQUEMATICO

<div align="center">
  <img src="https://github.com/medialablpwan/arduinoparanovatos/blob/main/pics/Screenshot_2023-09-27_182139.png" width="900"  style="margin: 10px;"/>
</div>
<br/>

### PCB

<div align="center">
  <img src="https://github.com/medialablpwan/arduinoparanovatos/blob/main/pics/Screenshot_2023-09-27_182120.png" width="900"  style="margin: 10px;"/>
</div>
<br/>

### MODELO 3D

<div align="center">
  <img src="https://github.com/medialablpwan/arduinoparanovatos/blob/main/pics/Screenshot_2023-09-27_182103.png" width="900"  style="margin: 10px;"/>
</div>
<br/>

### PINOUT

<div align="center">
  <img src="https://github.com/medialablpwan/arduinoparanovatos/blob/main/pics/Screenshot_2023-09-27_182305.png" width="900"  style="margin: 10px;"/>
</div>
<br/>

</div>

___

<div align="justify">

## SOFTWARE DE INTERÉS

El software de interés para programar y testear código en Arduino se divide en IDEs y simuladores:

### IDES

Contamos con dos referentes:

- [Arduino IDE](https://www.arduino.cc/en/software): es el entorno de programación nativo de Arduino

- [PlatformIO](https://platformio.org/): es una extensión de Visual Studio Code que permite trabajar con las mismas funciones que en el IDE de Arduino, sumándole el mundo de las extensiones de VSC

### SIMULADORES

Es software que permite cargar los programas a una placa y componentes simulados que se comportarán de forma fiel a la realidad pudiendo así no comprometer el hardware real en la etapa de debugging:

- [TinkerCAD](https://www.tinkercad.com/): es una web que permite trabajar, de forma bastante limitada, con Arduino UNO y algunos componentes básicos

- [Fritzing](https://fritzing.org/): es un programa que funciona igual que TinkerCAD, con la diferencia de que expande mucho más la variedad de micros y componentes

___

<div align="justify">

## PROGRAMAS

En esta sección iré escribiendo programas para hacer que funcionen diversos proyectos con variedad de placas Arduino y componentes. Aplicaré los conocimientos de los apartados que iré desarrollando en los siguientes puntos.

### MI PRIMER PROGRAMA

<table>
<tr>
<th> Digital </th>
<th> Analogic </th>
</tr>
<tr>
<td>

```c++
// Declaración de variables
int pin_LED = 13;                         // Asigno al LED el pin 13
int valor_delay = 1000;                   // Creo una variable 'int' que meteré en la función 'delay()'

// Código que sólo se ejecuta una única vez
void setup(){
	Serial.begin(115200);                   // Inicializo el serial y sus baudios
	Serial.println("Inciando programa..."); // Mensaje por serial de inicio de programa
	pinMode (pin_LED, OUTPUT);              // Establecer 'LED_BUILTIN' como salida
}

// Código que se ejecuta infinitamente
void loop(){
	digitalWrite(pin_LED, HIGH);            // Encender el LED
	Serial.println("LED encendido");        // Notificación en el serial
	delay(valor_delay);                     // Durante 1 segundo
	digitalWrite(pin_LED, LOW);             // Apagar el LED
	Serial.println("LED apagado");
	delay(valor_delay);                     // Durante 1 segundo... Y vuelta a empezar
}
```

</td>
<td>

```c++
// Declaración de variables
int pin_LED = 9; // the PWM pin the LED is attached to
int brillo = 0; // Inicializo a '0' el brillo
int desvanecimiento = 5; // how many points to fade the LED by
int valor_delay = 1000;

// Código que sólo se ejecuta una única vez
void setup(){
	Serial.begin(9600);
	pinMode(pin_LED, OUTPUT);
}

// Código que se ejecuta infinitamente
void loop(){
   analogWrite(pin_LED, brillo); // A diferencia que 'digitalWrite', 'analogWrite' demanda el pin y el valor inicial de brillo en este caso
   brillo = brillo + desvanecimiento; // Cambiar la cantidad de brillo tras cada iteración de 'loop()'
   if (brillo == 0 || brillo == 255) { // Primera condición, 'brillo' va de 0 a 255 y, tras llegar a 255, va de 255 a 0
      desvanecimiento = -desvanecimiento; // Cambio de dirección en el extremo
   }
   delay(valor_delay); // Cada iteración de 'loop()' se mantiene por 300ms
}
```

</td>
</tr>
</table>

<div align="center">
  <img src="https://github.com/medialablpwan/arduinoparanovatos/blob/main/pics/Screenshot_2023-09-21_180848.png" width="400"  style="margin: 10px;"/>
	
  <em>Esquema del circuito para el programa “Hola mundo”</em>
</div>
<br/>

### SENSOR TEMPERATURA

En este programa se usa un sensor de temperatura TMP36 en el que, en tinkerCAD, yo le digo la temperatura que hace y me lo notifica por medio de unos LEDs configurados como salidas:

<img src="https://github.com/medialablpwan/arduinoparanovatos/blob/main/pics/Screenshot_2023-09-22_184453.png" align="right" width="400px"/>

```c++
// Declaración de variables
#define LED_verde 6
#define LED_rojo 7
#define LED_azul 8
#define sensor_TMP A0

int mi_delay = 1000;

// Código que sólo se ejecuta una única vez
void setup(){
	Serial.begin(115200);
	pinMode(LED_verde, OUTPUT);
	pinMode(LED_rojo, OUTPUT);
	pinMode(LED_azul, OUTPUT);
  pinMode(sensor_TMP, INPUT);
}

// Código que se ejecuta infinitamente
void loop(){
	int lectura = analogRead(sensor_TMP);
  Serial.print("La lectura analógica del sensor es: ");
	Serial.println(lectura);
	float vout = 5.0 / 1024 * lectura;                     // Lectura de 10bits transformada a voltaje de salida
	float temperatura = vout * 100 - 50;                   // La fórmula sería: T(ºC) = lectura(bits) * (5V / 1024bits) * (100ºC / 1V) - Tdesfase(ºC)
  Serial.print("La temperatura captada es: ");
	Serial.print(temperatura);
  Serial.print("\xC2\xB0");                              // Signo de "º"
  Serial.println("C");
  
	if(temperatura < 25 && temperatura > 15){
		digitalWrite(LED_verde, HIGH);
		delay(mi_delay);
		digitalWrite(LED_verde, LOW);
		delay(mi_delay);
	}
	else if(temperatura < 15){
		digitalWrite(LED_azul, HIGH);
		delay(mi_delay);
		digitalWrite(LED_azul, LOW);
		delay(mi_delay);
	}
	else{
		digitalWrite(LED_rojo, HIGH);
		delay(mi_delay);
		digitalWrite(LED_rojo, LOW);
		delay(mi_delay);
	}
}
```

<br clear="right"/>

### FUNCIONES LED

Función para que el usuario elija un color de LED y su frecuencia de parpadeo:

<img src="https://github.com/medialablpwan/arduinoparanovatos/blob/main/pics/Screenshot_2023-09-30_180817.png" align="right" width="400px"/>

```c++
// Declaración de variables
int LED_azul = 6;
int LED_rojo = 7;
int LED_verde = 8;

// Declaración de funciones
void Parpadeo_LED(int pin, String color_LED, int valor_delay){
	digitalWrite(pin, HIGH);                                      // Encender el LED
	Serial.print("LED ");
	Serial.print(color_LED);                                      // Notificación en el serial
	Serial.println(" encendido");
	delay(valor_delay);                                           // Durante 1 segundo
	digitalWrite(pin, LOW);                                       // Apagar el LED
	Serial.print("LED ");
	Serial.print(color_LED);                                      // Notificación en el serial
	Serial.println(" apagado");
	delay(valor_delay);                                           // Durante 1 segundo... Y vuelta a empezar
}

// Código que sólo se ejecuta una única vez
void setup(){
	Serial.begin(115200);                                         // Inicializo el serial y sus baudios
	pinMode(LED_azul, OUTPUT);                                    // Establecer LEDs como salida
	pinMode(LED_rojo, OUTPUT);
	pinMode(LED_verde, OUTPUT);
}

// Código que se ejecuta infinitamente
void loop(){
	Parpadeo_LED(LED_azul, "azul", 1000);
	Parpadeo_LED(LED_rojo, "rojo", 2000);
	Parpadeo_LED(LED_verde, "verde", 3000);
}
```

<br clear="right"/>

### SENSOR DE HUMEDAD

Sensor de humedad que, por encima del 60%, nos notifica con el ‘BUILTIN_LED’ parpadeando:

<img src="https://github.com/medialablpwan/arduinoparanovatos/blob/main/pics/Screenshot_2023-09-30_180612.png" align="right" width="400px"/>

```c++
// Declaración de variables
#define pin_sensor A0
#define mi_delay 1500

// Declaración de funciones
int Parpadeo_LED_BUILTIN(){
	digitalWrite(LED_BUILTIN, HIGH);
	delay(mi_delay);
	digitalWrite(LED_BUILTIN, LOW);
	delay(mi_delay);
}

bool Mi_clima_humedo(int humedad){
	if(humedad >= 65){
		Parpadeo_LED_BUILTIN();
		Serial.println("Estás en un clima humedo");
		return true;
	}
	else{
		Serial.println("Estás en un clima seco");
		return false;
	}
}

// Código que sólo se ejecuta una única vez
void setup(){
	Serial.begin(115200);                      // Inicializo el serial y sus baudios
	pinMode(pin_sensor, INPUT);
  pinMode(LED_BUILTIN, OUTPUT);
}

// Código que se ejecuta infinitamente
void loop(){
	float lectura = analogRead(pin_sensor);
	Serial.print("La lectura analógica es: ");
	Serial.println(lectura);
  float porcentaje_humedad = (lectura*100L)/876;
  Serial.print("La lectura del sensor de humedad es del ");
  Serial.print(porcentaje_humedad);
  Serial.println("%");
  Mi_clima_humedo(porcentaje_humedad);
  Serial.println();
	delay(mi_delay);
}
```

<br clear="right"/>

### PWM

Creo un algoritmo para variar de forma continua de menos a más, y viceversa, el duty cycle de la señal PWM generada en un pin digital. Para ello, uso la función ‘analogWrite(pin, valor)’ y bucles ‘for’. Además, controlo el brillo de forma analógica también de un LED:

<img src="https://github.com/medialablpwan/arduinoparanovatos/blob/main/pics/Screenshot_2023-09-23_190211.png" align="right" width="400px"/>

```c++
// Declaración de variables
#define pin_LEDblanco 10
#define pin_LEDrojo 11
#define pin_analog A1
#define mi_delay 10

// Código que se ejecuta infinitamente
void setup()
{
	Serial.begin(115200);
	pinMode(pin_LEDblanco, OUTPUT);
	pinMode(pin_LEDrojo, OUTPUT);

}

// Código que se ejecuta infinitamente
void loop(){
	for(int desvanecer = 0; desvanecer <= 255; desvanecer++){
		analogWrite(pin_LEDblanco, desvanecer);
      	//Serial.println(desvanecer);
      	delay(mi_delay);
    }
  	
  for(int desvanecer = 255; desvanecer >= 0; desvanecer--){
  		analogWrite(pin_LEDblanco, desvanecer);
      	//Serial.println(desvanecer);
  		delay(mi_delay);
	}

	int lectura = analogRead(pin_analog);
	analogWrite(pin_LEDrojo, lectura / 4);
  	Serial.println(analogRead(pin_analog));
}
```

<br clear="right"/>

___

## VARIABLES GLOBALES Y LOCALES

La variables, dependiendo de dónde se definan, son globales o locales. Las globales se definen al principio del programa, antes del ‘setup()’ mientras que las locales se definen en cada función y son solo accesibles desde cada una de éstas.

```c++
// Declaración de variables
int variable_global = 5;                  // Variable gloabal 5 en todas las funciones de mi programa
int valor_delay = 1000;                   // Creo una variable 'int' que meteré en la función 'delay()'

// Código que sólo se ejecuta una única vez
void setup(){
	int variable_local = 10;
	Serial.begin(115200);                   // Inicializo el serial y sus baudios
	Serial.println(variable_local);         // Imprime "10"

}

// Código que se ejecuta infinitamente
void loop(){
	int variable_local = 10;
	Serial.println(variable_local);         // Imprime "15" y sin sobreescribir 'variable_local' del 'setup()', ya que son locales y cada una es diferente
	delay(valor_delay);                     // Durante 1 segundo
}
```

___

## CALIFICADORES

Son modificadores de variables que son inmutables en las ejecuciones del programa.

```c++
// Declaración de variables
const int variable_constante = 5;
int variable_global = 5;                  // Variable gloabal 5 en todas las funciones de mi programa
int valor_delay = 1000;                   // Creo una variable 'int' que meteré en la función 'delay()'

// Código que sólo se ejecuta una única vez
void setup(){
	int variable_local = 10;
	variable_constante = variable_constante + 1; // ERROR, NO PUEDO SOBREESCRIBIR UN 'const'
	Serial.begin(115200);                   // Inicializo el serial y sus baudios
	Serial.println(variable_local);         // Imprime "10"
}

// Código que se ejecuta infinitamente
void loop(){
	int variable_local = 15;
	variable_local = variable_global + variable_constante; // 'variable_local' ahora vale 5 + 5 = 10
	Serial.println(variable_local);         // Imprime "10" y sin sobreescribir 'variable_local' del 'setup()', ya que son locales y cada una es diferente
	delay(valor_delay);                     // Durante 1 segundo
}
```

___

## FUNCIÓN `#define`

Permite declarar constantes antes de que se compile el programa, muy similar a usar el método ‘const’, pero sin ocupar espacio de programa.

```c++
// Declaración de variables
#define pi 3.1416

int variable_global = 5;                  // Variable gloabal 5 en todas las funciones de mi programa
int valor_delay = 1000;                   // Creo una variable 'int' que meteré en la función 'delay()'

// Código que sólo se ejecuta una única vez
void setup(){
	Serial.begin(115200);                   // Inicializo el serial y sus baudios
}

// Código que se ejecuta infinitamente
void loop(){
	int radio = 10;
	float area_circulo = radio * radio * pi; // Declaro y asigno un valor a 'area_circulo'
	Serial.println(area_circulo);            // Imprime "314.16"
	delay(valor_delay);                      // Durante 1 segundo
}
```

___

## CALIFICADOR `static`

Son una _tag_ que permite a una variable de cualquier tipo inicializarse en ‘loop()’ a un valor inicial y, luego, sobreescribirse al valor anterior. Como inicializarla de forma global, pero ya dentro de una función de forma local.

```c++
// Declaración de variables
int variable_global = 5;                     // Variable gloabal 5 en todas las funciones de mi programa
int valor_delay = 1000;                      // Creo una variable 'int' que meteré en la función 'delay()'

// Código que sólo se ejecuta una única vez
void setup(){
	Serial.begin(115200);                      // Inicializo el serial y sus baudios
}

// Código que se ejecuta infinitamente
void loop(){
	int x = 0;
	static int y = 0;                          // 'static' inicializa una variable al valor asignado en la primera ejecución del 'loop()' 
	x++;
	y++;
	Serial.println(x);                         // Ésto imprime '1' contínuamente porque inicialicé la variable a 0 en el 'loop()', por lo que siempre está 0, 1, 0, 1, ...
	Serial.println(y);
	delay(valor_delay);                        // Durante 1 segundo
}
```

___

## OPERADORES

### COMPARATIVOS

`<`, `>`, `<=`, `>=`, `==`, `!=`

### LÓGICOS

`&&`, `||`, `!`. Debo recordar la importancia con el concepto booleano que tienen.

```c++
// Declaración de variables

// Código que sólo se ejecuta una única vez
void setup(){
	Serial.begin(115200);                      // Inicializo el serial y sus baudios
	Serial.println("Tabla de verdad '&&'");
	Serial.print("'true' y 'true': ");
	Serial.println(true && true);
	Serial.print("'true' y 'false': ");
	Serial.println(true && false);
	Serial.print("'false' y 'true': ");
	Serial.println(false && true);
	Serial.print("'false' y 'false': ");
	Serial.println(false && false);

	Serial.println();
	Serial.println();
	Serial.println();

	Serial.println("Tabla de verdad '||'");
	Serial.print("'true' o 'true': ");
	Serial.println(true || true);
	Serial.print("'true' o 'false': ");
	Serial.println(true || false);
	Serial.print("'false' o 'true': ");
	Serial.println(false || true);
	Serial.print("'false' o 'false': ");
	Serial.println(false || false);

	Serial.println();
	Serial.println();
	Serial.println();

	Serial.println("Tabla de verdad '!'");
	Serial.print("'!true': ");
	Serial.println(!true);
	Serial.print("'!false': ");
	Serial.println(!false);
}

// Código que se ejecuta infinitamente
void loop(){

}
```

___

## SENTENCIAS CONDICIONALES

`if`, `else if` y `else`.

```c++
#define num2 3.2

// Declaración de variables
bool mi_booleana = false;
int num1 = 4;

// Código que sólo se ejecuta una única vez
void setup(){
	Serial.begin(115200);                      // Inicializo el serial y sus baudios

	if(true){
		Serial.println("Ésta línea se imprime si está a 'true'");
	}

	if(false){
		Serial.println("Ésta línea no se imprime, está a 'false'");
	}	

	if(10>3 && 3<5){
		Serial.println("Las mates funcionan")
		if(true);
			Serial.println("Esta línea pertenece a un 'if' anidado");
		}
	}
	
	if(mi_booleana=true){
		Serial.println("Mi booleana está a 'true'");
  }

	if(num1 > num2 && mi_booleana){
		Serial.println("Número 1 es mayor que número 2 y booleana está a 'true'");
	}
	else if(num1 < num2 && !mi_booleana){
		Serial.println("Número 2 es mayor que número 1 y booleana está a 'false'");
	}
	else{
		Serial.println("Me pillas");
	}
}

// Código que se ejecuta infinitamente
void loop(){

}
```

___

## BUCLES Y BUCLES ANIDADOS

Como en cualquier otro lenguaje de programación:

### BUCLE `while`

```c++
// Declaración de variables

// Código que sólo se ejecuta una única vez
void setup(){
	Serial.begin(115200);
	int multiplo = 1;
	while(multiplo <= 10){         // 10 iteraciones
		Serial.println(multiplo*5);  // Empezando en 1, multiplica por 5 en cada iteración y guárdalo en la misma variable
		Serial.println(multiplo);    // Múltiplo es un número que, a cada iteración, va así: 1, 2, 3, 4, ..., 10
		multiplo++;                  // Sube el valor de iteraciones
	}
}

// Código que se ejecuta infinitamente
void loop(){
	
}
```

### BUCLE `for`

```c++
// Declaración de variables


// Código que sólo se ejecuta una única vez
void setup(){
	Serial.begin(115200);
	for(int i = 0; i < 5; i++){       // Éste bucle se encarga de repetir una acción 5 veces
		for(int j = 0; j<=i; j++){      // Éste bucle anidado se encarga de imprimir y sumar, tras cada iteración del bucle, un '+' más por línea
			Serial.print("*");
		}
	Serial.println();                 // Espaciamos tras cada línea de '*'
	}
}

// Código que se ejecuta infinitamente
void loop(){
	
}
```

### BUCLE `do... while`

<img src="https://github.com/medialablpwan/arduinoparanovatos/blob/main/pics/Screenshot_2023-09-22_213407.png" align="right" width="450px"/>

```c++
// Declaración de variables

// Código que sólo se ejecuta una única vez
void setup(){
	int multiplo = 1;
	Serial.begin(115200);
	do{
		Serial.println(multiplo*2);
		multiplo++;
	}
	while(multiplo <= 10);
}

// Código que se ejecuta infinitamente
void loop(){
	
}
```

<br clear="right"/>

___

## `BREAK`, `CONTINUE` Y `RETURN`

### `BREAK`

Rompe un bucle cuando se cumple la condición que le imponga. Muy útil para decir cuando un bucle de tipo ‘while(1)’ o ‘while(true)’ deben acabar.

```c++
// Declaración de variables

// Código que sólo se ejecuta una única vez
void setup(){
	Serial.begin(115200);
	int multiplo = 1;
	while(multiplo <= 10){
		Serial.println(multiplo*2);
		multiplo++;
		if(multiplo==5){
			Serial.println("ROMPIENDO EL BUCLE...");
			break;
		}
	}
	Serial.println("Bucle roto con éxito");
}

// Código que se ejecuta infinitamente
void loop(){
	
}
```

### `CONTINUE`

Sirve, por ejemplo, para saltar una iteración en un bucle.

```c++
// Declaración de variables

// Código que sólo se ejecuta una única vez
void setup(){
	Serial.begin(115200);
	for(int i = 0; i <= 10; i++){
		if(i == 5 || i == 8){
			continue;
		}
		Serial.println(i);
	}
}

// Código que se ejecuta infinitamente
void loop(){
	
}
```

### `RETURN`

Desde donde lo ponga, se vuelve al principio de la función.

```c++
// Declaración de variables

// Código que sólo se ejecuta una única vez
void setup(){
	Serial.begin(115200);
	for(int i = 0; i <= 7; i++){
		Serial.println(i);
		return;                    // Debido a que hice un bucle quedebería imprimir 0, 1, 2, ..., 7, pero le dí a 'return' justo en ese momento, volverá a 0
	}
}

// Código que se ejecuta infinitamente
void loop(){
	Serial.println("UNO");
	Serial.println("DOS");
	return;                      // Este 'loop()' estará eternamente imprimiendo "UNO", "DOS", "UNO", "DOS", "UNO", ...
	Serial.println("TRES");
	Serial.println("CUATRO");
}
```

___

## `SWITCH`

Es un programa secuencial que busca el primer caso a ‘true’ para ejecutarlo y, si no, sigue bajando entre los casos.

```c++
// Declaración de variables

// Código que sólo se ejecuta una única vez
void setup(){
	Serial.begin(115200);                   // Inicializo el serial y sus baudios
	int x = 3;

	switch(x){
		case 1 ... 10:                      // INTERVALOS, 'x' está comprendida entre esos dos números
			Serial.println("Halo CE");
			break;
		case 11 ... 20:
			Serial.println("Halo 2");
			break;
		case 21 ... 30:
			Serial.println("Halo 3");
			// break;                       // Si omito un break y 'x' vale el de su caso, se imprimirán los casos hastas llegar a uno con break
		case 31 ... 40:
			Serial.println("Halo 3 ODST");
			break;
		case 41 ... 50:
			Serial.println("Halo Reach");
			break;
	}
}

// Código que se ejecuta infinitamente
void loop(){
	
}
```

___

## ARRAYS

Listas de variables.

```c++
// Declaración de variables

// Código que sólo se ejecuta una única vez
void setup(){
	Serial.begin(115200);                                       // Inicializo el serial y sus baudios
	int mi_array[5] = {1 ,3, 5, 8, 2};
	Serial.println(mi_array[2]);                                // Ésto imprime 5
	mi_array[3] = 101;                                          // Ésto actualiza el valor de la cuarta posición a un 101
	int mi_variable = mi_array[0] + mi_array[1] + mi_array[2];
	Serial.println(mi_variable);                                // Imprime la suma de 1 + 3 + 5 = 9
	Serial.println();
	
	for(int i = 0; i < 5; i++){
		Serial.println(mi_array[i]);
	}
	Serial.println();

	for(int i = 0; i < 5; i++){
		mi_array[i] = mi_array[i] + 1000;                         // A cada elemento del array le sumo 1000
		Serial.println(mi_array[i]);
	}
	Serial.println();

	int sum = 0;
	for (int i = 0; i < 5; i++){
		sum = sum + mi_array[i];
	}
	Serial.println(sum);
}

// Código que se ejecuta infinitamente
void loop(){
	
}
```

___

## STRINGS

Secuencias de caracteres. Son a los caracteres lo que son los arrays a los números. Prácticamente comparten la sintaxis.

```c++
// Declaración de variables

// Código que sólo se ejecuta una única vez
void setup(){
	Serial.begin(115200);                                       // Inicializo el serial y sus baudios
	char string_1[7] = {'d' ,'a', 'n', 'i', 'e', 'l'};          // Aunque mi nombre tenga 6 caracteres, debo crear la string con un espacio extra
	Serial.println(string_1);
	
	char string_2[7] = {'d' ,'a', 'n', 'i', 'e', 'l', '\0'};    // '\0' es el caracter "null" y se puede dejar explícito también
	Serial.println(string_2);
	
	char string_3[] = "daniel";                                 // Sin indicar la dimensión, el compilador es inteligente y acomoda el tamaño a 7 automáticamente
	Serial.println(string_3);

	char string_4[7] = "daniel";
	Serial.println(string_4);
	
	char string_5[14] = "daniel";                               // No hay problema en que cree strings con dimensión superior a la palabra con la que lo inicializo, pero nunca puedo hacerlo más pequeño porque cortará la palabra
	Serial.println(string_5);
	
	String string_6 = "daniel";
	Serial.println(string_6);
	Serial.println(string_6.charAt(0));                         // Si defino una variable de tipo 'String', puedo contar con las funciones, como'.charAt()', que me devuelve el caracter en el índice puesto en el paréntesis
}

// Código que se ejecuta infinitamente
void loop(){
	
}
```

___

## FUNCIONES

Bloques de código a los que les asigno una serie de propiedades que podré llamar en otros lugares del código. Tienen la morfología ‘void Nombre(){}’. CUIDADO: dependiendo del entorno de trabajo, se deben declarar las funciones propias antes de, mínimo, ‘loop()’ puesto que ésta actúa como la ‘main()’.

### ‘void Mi_funcion()’

Las funciones de tipo “void” no devuelven nada, simplemente hacen una serie de acciones, pero sin devolver una variable.

```c++
// Declaración de variables

// Declaración de funciones
void Parpadeo_LED(){
	digitalWrite(LED_BUILTIN, HIGH);            // Encender el LED
	Serial.println("LED encendido");        // Notificación en el serial
	delay(1000);                     // Durante 1 segundo
	digitalWrite(LED_BUILTIN, LOW);             // Apagar el LED
	Serial.println("LED apagado");
	delay(1000);                     // Durante 1 segundo... Y vuelta a empezar
}

// Código que sólo se ejecuta una única vez
void setup(){
	Serial.begin(115200);                   // Inicializo el serial y sus baudios
	pinMode(LED_BUILTIN, OUTPUT);              // Establecer 'LED_BUILTIN' como salida
}

// Código que se ejecuta infinitamente
void loop(){
	Parpadeo_LED();
}
```

### ‘int Mi_funcion()’

Son funciones que devuelve una variable ‘int’

```c++
// Declaración de variables

// Declaración de funciones
int Suma(int num1, int num2, int num3){
	int resultado = num1 + num2 + num3;
	return resultado;                          // Aunque podría poner directamente return num1 + num2 + num3
}

// Código que sólo se ejecuta una única vez
void setup(){
	Serial.begin(115200);                      // Inicializo el serial y sus baudios
	int suma_1 = Suma(4, 5, 10);
	Serial.println(suma_1);
}

// Código que se ejecuta infinitamente
void loop(){
	
}
```

### ‘bool Mi_Funcion()’

Devuelve un ‘true’ o ‘false’. Esta función se explica en PROGRAMAS → SENSOR HUMEDAD

___

## LIBRERÍAS

Son llamadas a otros ficheros que contienen una serie de funciones necesarias para sensores específicos.

</div>
