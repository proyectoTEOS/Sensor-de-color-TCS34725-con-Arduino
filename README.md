# Sensor de color TCS34725 con Arduino
Hoy veremos cómo imprimir en el monitor serie algunos colores con nuestro detector TCS34725 controlado con Arduino.

### Requisitos
- Tener el IDE de [Arduino](https://www.arduino.cc/en/Main/Software) (o el de tu preferencia) con la versión más actual
- Contar con las librerías [Adafruit TCS34725](https://github.com/adafruit/Adafruit_TCS34725) y [ColorConverterLib](https://github.com/luisllamasbinaburo/Arduino-ColorConverter)
- Tener el material para hacer el proyecto

### Código
**[Aqui](https://github.com/proyectoTEOS/Sensor-de-color-TCS34725-con-Arduino/blob/master/Sensor-de-color-TCS34725-con-Arduino.ino)** podrás obtener el link del código, también dejaremos
una vista previa aquí abajo.

```c++
/*
  Created by TEOS
  Domotic with Arduino https://goo.gl/btZpjs
  YouTube https://goo.gl/k7TUSZ
  Instagram https://goo.gl/zEIjia
  Facebook https://goo.gl/eivraR
  Twitter https://goo.gl/du5Wgn
  Github https://goo.gl/Xl5IiS
  WEB https://www.proyecto-teos.com
*/

#include "Adafruit_TCS34725.h" //https://github.com/adafruit/Adafruit_TCS34725
#include "ColorConverterLib.h" //https://github.com/luisllamasbinaburo/Arduino-ColorConverter
#include <Wire.h>

uint16_t redT, greenT, blueT, clearT;
float scaleRedT, scaleGreenT, scaleBlueT;
double nuanceT, saturationT, valueT;
String nameColorT;

Adafruit_TCS34725 colorSensorT = Adafruit_TCS34725(TCS34725_INTEGRATIONTIME_50MS, TCS34725_GAIN_4X);

void setup() {
  Serial.begin(9600);
  if (!colorSensorT.begin()) {
    Serial.println("No ha sido posible iniciar el sensor");
    delay(1000);
    while (1);
  }
}

void loop() {
  colorSensorT.setInterrupt(false);
  delay(60);
  colorSensorT.getRawData(&redT, &greenT, &blueT, &clearT);
  colorSensorT.setInterrupt(true);

  scaleRedT = redT;
  scaleRedT = (scaleRedT / clearT ) * 255;

  scaleGreenT = greenT;
  scaleGreenT = (scaleGreenT / clearT ) * 255;

  scaleBlueT = blueT;
  scaleBlueT = (scaleBlueT / clearT ) * 255;

  ColorConverter::RgbToHsv(static_cast<uint16_t>(scaleRedT), static_cast<uint16_t>(scaleGreenT),
                           static_cast<uint16_t>(scaleBlueT), nuanceT, saturationT, valueT);

  nuanceT *= 360;
  Serial.println("Color detectado: " + printColorNameT() + "[" + String(nuanceT) + "]");
  delay(900);
}

String printColorNameT() {
  if (nuanceT <= 15)
    return nameColorT = "Rojo";
  else if (nuanceT <= 45)
    return nameColorT = "Naranja";
  else if (nuanceT <= 90)
    return nameColorT = "Amarillo";
  else if (nuanceT <= 150)
    return nameColorT = "Verde";
  else if (nuanceT <= 210)
    return nameColorT = "Cyan";
  else if (nuanceT <= 270)
    return nameColorT = "Azul";
  else if (nuanceT <= 330)
    return nameColorT = "Magenta";
  else
    return nameColorT = "Rojo";
}
```

### Diagrama
El siguiente esquemático muestra como se debe conectar todos los componentes con la placa.
![](https://github.com/proyectoTEOS/Sensor-de-color-TCS34725-con-Arduino/blob/master/sensor-de-color-tcs34725-con-arduino-5.jpg)
