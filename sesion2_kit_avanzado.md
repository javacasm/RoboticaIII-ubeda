# Sesión 2

## Mejoramos el control de maqueen: sonido de aviso intermitente al moverse hacia atrás

Vamos a integrar en el programa del robot maqueen el programa que hacía un sonido intermitente activable con botones.

* Creamos un bucle temporal que en función de una variable numérica activa o no un sonido
* Desde la función "atrás" activamos la variable "sonando"
* Desde los demás métodos llamamos a la función "parar sonido" para detener la intermitencia, esta función pone la variable a 0 y para los sonidos inmediatamente

![](./images/programa_robot_maqueen_v2.png)

Para unir los bloques usamos la vista en javascript

* Creamos una copia del programa original que vamos a modificar, es fácil que el proceso metamos la pata.
* Abrimos los dos programas en javascript.
* Identificamos las líneas correspondientes a los bloques  que queremos copiar.
* Copiamos estas líneas al programa destino.
* Se pueden producir errores que tendremos que identificar y corregir.

[robot v2.5](https://makecode.microbit.org/S94007-50010-37042-12017)


Idea: Añadir pantalla oled a maqueen

## Pantalla LCD

Vamos a usar una pantalla LCD que conectamos a la micro:bit a través del extensor.

![](./images/lcd_microbit.jpeg)

Este LCD usa conexión I2C que sólo requiere de 4 cables: Vcc, GND, SDA y SCL.

![](./images/lcd_conexion.jpeg)

Uniremos los cables correspondientes, podemos ver los nombres de los pines en la parte de abajo del extensonr y en el LCD. Tenemos que asegurarnos de que la alimentación de V2 es de 5V para lo que comprobaremos los jumpers de alimentación.

Para controlar el LCD usaremos una extensión, hay varias en makecode. En este caso usaremos Makerkit LCD que es más versátil. 

Cargamos la extensión y añadimos los bloques para mostrar los datos y crear un carácter de usuario para mostrar los grados

Esta extensión nos permite crear caracteres de usuario

Mostramos los datos y los carateres de usuario

![](./images/programa_lcd_makebit.png)

Otra de las ventajas de esta librería es que nos facilita el cálculo de la posición y la confección de las pantallas

![](./images/programa_makerbit_seleccion_posicion.png)

[Programa test LCD](https://makecode.microbit.org/S16394-50348-99702-05700)

## Añadir pantalla LCD al mando del robot

Ahora vamos a añadir la pantalla LCD al mando de nuestro robot, mostraremos los datos de la temperatura y luz locales y posteriormente mostraremos los datos de temperatura que nos envíe el robot.

Para ello añadiremos los bloques del ejemplo que hemos hecho del LCD al mando.

* Hacemos una copia del programa del mando
* Abrimos los dos programas en javascript
* Añadimos la extensión makerbit LCD al programa del mando
* Copiamos las líneas del test LCD. Primero las del bloque "iniciar" que están al principio

```javascript
makerbit.connectLcd(39)
makerbit.showStringOnLcd1602("Hola mando", makerbit.position1602(LcdPosition1602.Pos3), 16)
makerbit.lcdMakeCharacter(LcdChar.c1, makerbit.lcdCharacterPixels(`
    . # . . .
    # . # . .
    . # . . .
    . . . # #
    . . # . .
    . . # . .
    . . . # #
    . . . . .
    `))
```

* Añadimos las líneas del bloque temporal

```javascript
loops.everyInterval(500, function () {
    makerbit.showStringOnLcd1602("T:", makerbit.position1602(LcdPosition1602.Pos17), 16)
    makerbit.showStringOnLcd1602("" + (input.temperature()), makerbit.position1602(LcdPosition1602.Pos19), 2)
    makerbit.lcdShowCharacter1602(LcdChar.c1, makerbit.position1602(LcdPosition1602.Pos21))
    makerbit.showStringOnLcd1602("Luz:", makerbit.position1602(LcdPosition1602.Pos23), 16)
    makerbit.showStringOnLcd1602("" + (input.lightLevel()), makerbit.position1602(LcdPosition1602.Pos27), 16)
})
```
* Comprobamos que no hay errores y lo probamos

![](./images/programa_mando_2.5.png)

Añadimos al bloque "iniciar" el icono que aparece en nuestro maqueen y para distinguir entre la microbit del mando y del robot cambiamos el led de arriba a la izquierda con el bloque "graficar" de la paleta LEDs. Según el icono que uses puedes usar otro punto de la pantalla

![](./images/icono_mando.png)

### Mostrando datos de telemetría del robot

Ahora vamos a hacer que robot nos envíe los datos de temperatura y los mostraremos en el LCD del mando. 

En el mando haremos que al recibir un número por radio lo muestre en el LCD

![](./images/programa_mando_2.5_recibir.png)

En el robot añadiremos un bucle temporal que cada segundo nos envíe el dato de temperatura del robot

![](./images/programa_maqueen_radio_2.5.png)

Los programas quedan así:

[Mando maqueen 2.5](https://makecode.microbit.org/S15891-40327-08248-29896)

[Robot maqueen 2.5](https://makecode.microbit.org/S62199-18030-90259-31889)

Ideas:

* Añadir una pantalla OLED o LCD a maqueen para mostrar los datos en local.
* Enviar más datos de telemetría desde maqueen. Para ello usaremos el envio por radio de datos con metadatos, donde cada dato que se envía lleva una etiqueta para identificarlo a su llegada

![](./images/programa_robot_maqueen_radio_v3.png)

Ahora el mando tiene que ver el nombre de cada paquete que recibe para mostrarlo en su zona correspondiente. También mostremos la intensidad de la señal del paquete que nos da idea de la distancia al robot (o del nivel de su batería). 

![](./images/programa_mando_radio_v3.png)

Para mostrar bien los datos reordenaremos las posiciones de la pantalla para ajustarlo

[Mando v3](https://makecode.microbit.org/S08791-16176-40876-56238)

[Robot v3](https://makecode.microbit.org/S45866-77088-58989-92614)

