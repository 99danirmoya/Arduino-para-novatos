<div align="centre">
	
https://images.unsplash.com/photo-1603732551658-5fabbafa84eb?ixlib=rb-4.0.3&q=85&fm=jpg&crop=entropy&cs=srgb&w=3600

</div>

<div align="justify">

# Arduino Course for Beginners

En esta página tomaré apuntes extraidos del curso “Arduino Course for Beginners”.

</div>

<div align="centre">

[![Arduino Course for Beginners](https://github.com/medialablpwan/arduinoparanovatos/blob/main/pics/Screenshot%202023-12-17%20131855.png)](https://youtu.be/zJ-LqeX_fLU)

</div>

<div align="justify">

## CONCEPTOS BÁSICOS

En este apartado me centraré en describir los aspectos más básicos del hardware de la placa Arduino UNO R3, la más extendida. Echando un vistazo a la placa, se observa:

</div>

<div align="center">
  <img src="https://github.com/medialablpwan/arduinoparanovatos/blob/main/pics/Screenshot_2023-09-21_104445.png" width="600"  style="margin: 10px;"/>
</div>
<br/>

<div align="justify">

- El LED “L” está conectado al pin 13, por lo que cada vez que se ponga a “high”, “L” se encenderá

- El chip principal es el MCU ATmega328

- Contamos con 14 pines digitales (no todos sirven para lo mismo y hay algunos especiales, como 6 capaces de PWM) y 6 pines analógicos

- LED RX y TX, que se encenderán cuando la placa esté recibiendo o enviando información, respectivamente

- Botón de reset que reinicia el código sin limpiar la memoria

</div>

## PROGRAMAS

<div align="justify">

En esta sección iré escribiendo programas para hacer que funcionen diversos proyectos con variedad de placas Arduino y componentes. Aplicaré los conocimientos de los apartados que iré desarrollando en los siguientes puntos.

### MI PRIMER PROGRAMA

<table>
<tr>
<th> Digital </th>
<th> Anaogic </th>
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

</div>
