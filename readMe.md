# Primera parte

## Integrantes

* Sebastian L. Sobrino
* Lautaro Testa
* Tomas del Rio
* Valentina Vanesa Cisneros
* Francisco Belen

## Proyecto: Contador de dos digitos.

[![Trabajo-parte-1.png](https://i.postimg.cc/Pqc3Sm8Z/Trabajo-parte-1.png)](https://postimg.cc/R37LFJdV)

## Descripción
El proyecto consiste en un contador digital interactivo que muestra valores de 0 a 99 en dos displays de 7 segmentos. Esto es útil en aplicaciones donde se requiere llevar un conteo visual, como un marcador, un temporizador o un contador de productos. El contador utiliza 3 botones: Uno para sumar, otro para restar y el ultimo funciona para reiniciar a 0 el contador.

## Función principal
La función "loop" controla un contador de 0 a 99 utilizando dos displays de 7 segmentos y tres botones. Lee el estado de los botones de suma, resta y reset, ajustando el contador en consecuencia. La técnica de multiplexación se utiliza para mostrar los dígitos en los displays alternativamente. La función prenderLuces gestiona la visualización de las unidades y decenas del contador en los displays. Las funciones auxiliares configuran los segmentos de los displays, controlan qué display se enciende y gestionan el estado de los LEDs.

```
void loop() {      

  reset = digitalRead(RESET);
  sumar = digitalRead(SUMA);
  restar = digitalRead(DISMINUYE);
  
  if(sumar == 0)
  {
  	contador++;
   
      if(contador > 99)
    {
      contador = 0;
    }
  }
  
  if(restar == 0)
  {
    contador--;
    if(contador < 0)
    {
      contador = 99;
    }
  }
  
  if(reset == 0)
  {
     contador = 0;
  } 
  
  prenderLuces(contador);
}
```

## Link al proyecto

https://www.tinkercad.com/things/9I9aw647I0a-trabajoparte1/editel?sharecode=m3-GeQ2a5ULr5s4BRVGlyH1TbC9pf_xpekv5YyVTMmk

# Segunda parte

## Modificación con Interruptor Deslizante y Números Primos

[![Trabajo-parte-dos.png](https://i.postimg.cc/cHQqLHnt/Trabajo-parte-dos.png)](https://postimg.cc/XG712nxn)

## Descripción
Para esta segunda parte se Sustituye uno de los botones por un interruptor deslizante (switch) de dos posiciones.
Dependiendo de la posición del interruptor, el display debe mostrar o bien el contador (como en la Parte 1) o los números primos en el rango de 0 a 99. 

## Función principal
En la funcion principal, leemos el estado de los botones asi como del interruptor que nos avisa si el contador sera de numeros enteros o primos y el sensor de fuerza.
Si el sensor posee un valor mayor a 90, el contador permite sumar o restar numeros (Ya sea primos o enteros dependiendo del valor del interruptor.
En caso de que el sensor de fuerza sea menor a 90, el contador queda estancado en 0 hasta que se ejerza una fuerza mayor.
```
void loop() {      

  interruptor = digitalRead(INTERRUPTOR);
  sumar = digitalRead(SUMA);
  sensor = analogRead(SENSOR);
  restar = digitalRead(DISMINUYE);
  
  
  if (sensor > 90)
  {
    if(interruptor == 1)
    {
      if(sumar == 0)
      {
        contador++;

        if(contador > 99)
        {
          contador = 0;
        }
      }

      if(restar == 0)
      {
        contador--;
        if(contador < 0)
        {
          contador = 99;
        }
      }


      prenderLuces(contador);
    }

    else
    {
      if (verificarNumeroPrimo(contadorPrimo))
      {

        if(sumar == 0)
        {
          numeroAuxiliar = 1;
          contadorPrimo++;

          if(contadorPrimo > 97)
          {
            contadorPrimo = 2;
          }
        }
        else if(restar == 0)
        {
          numeroAuxiliar = 0;
          contadorPrimo--;

          if(contadorPrimo < 2)
          {
            contadorPrimo = 97;
          }
        }
      }
      else 
      {
        contadorPrimo = obtenerNumerosPrimos(contadorPrimo,numeroAuxiliar);
      }

      prenderLuces(contadorPrimo);
    }
  }
  else
  {
    contador = 0;
    prenderLuces(contador);
  }
}
```
## Modificaciones y comoponentes adicionales.
Para esta segunda parte se agrego un sensor de fuerza que permite detectar la cantidad de fuerza que se ejerce. Su funcion en el proyecto consiste en permitir el uso del contador y sus botones dependiendo la fuerza que este capte. En caso de que el sensor tome una fuerza superior a los 4 N, el usuario podra utilizar cualquiera de los botones para modificar el contador. Caso contrario, si la fuerza no supera ese minimo, el contador quedara estatico en 0.

Un componente adicional que se podria integrar al proyecto para mejorarlo seria el motor de aficionado. Es un dispositivo electromecánico que convierte la energía eléctrica en movimiento mecánico rotativo. Está diseñado para funcionar a velocidades variables, lo que lo hace adecuado para proyectos donde se requiere el control preciso de la velocidad. Una forma de integrarlo al proyecto seria usarlo como parametro visual del aumento o disminucion del contador. En caso de que el contador suba, la velocidad del motor subira. Si el contador disminuye, tambien lo hara el motor.

## Link al proyecto

https://www.tinkercad.com/things/430neKav5gY-trabajopartedos/editel?sharecode=RwcGpelVcMMDU0PCWGOCcKOKnmYCw1Bsvba_B7HmY1Q

# Tercer parte

## Modificación según el Último Número de Documento
[![Trabajo-parte-tres.png](https://i.postimg.cc/X7R7wFpG/Trabajo-parte-tres.png)](https://postimg.cc/bsRhPZ0p)

## Descripción
1. Considerando el último número de tu número de documento (DNI o documento de identidad), agrega un componente adicional que afecte el funcionamiento del proyecto. Describe en detalle qué hace este nuevo componente y cómo se integra en el proyecto. (En este caso, el componente a integrar sera una fotoresistencia)

## Función principal
En la funcion principal, leemos el estado de los botones asi como del interruptor que nos avisa si el contador sera de numeros enteros o primos y el sensor de fuerza.
Si el sensor posee un valor mayor a 90, el contador permite sumar o restar numeros (Ya sea primos o enteros dependiendo del valor del interruptor.
En caso de que el sensor de fuerza sea menor a 90, el contador queda estancado en 0 hasta que se ejerza una fuerza mayor.
En caso de que la fotoresistencia no tome valores superiores a 50, se activara el modo de suspension en caso de inactividad (usamos 2 segundos como tiempo de inactividad).
De esta forma el contador se apagara hasta que detecte algun tipo de actividad. Si toma valores superiores a 50, el contador no se apagara a pesar de la inactividad
```
void loop() {      

  interruptor = digitalRead(INTERRUPTOR);
  sumar = digitalRead(SUMA);
  sensor = analogRead(SENSOR);
  restar = digitalRead(DISMINUYE);
  luz = analogRead(FOTORESISTENCIA);
  
  if (sumar == 0 || restar == 0) //En caso de que se presione el boton, se avisara que el usuario esta interactuando con el contador.
  {
    tiempoUltimaActividad = millis(); // Hubo actividad, actualizar el tiempo de actividad
    enSuspension = false; // Salir del modo de suspensión si estábamos en él
  }

  // Verifica si se debe entrar en modo de suspensión
  if (!enSuspension && (millis() - tiempoUltimaActividad) > tiempoSuspension) 
  {
    if (luz < minLuz) {
      // Activar el modo de suspensión si está oscuro
      enSuspension = true;
      prenderDisplay(1,1);
    }
  }
  
  if (!enSuspension) {
  
    if (sensor > 90)
    {
      if(interruptor == 1)
      {
        if(sumar == 0)
        {
          contador++;

            if(contador > 99)
          {
            contador = 0;
          }
        }

        if(restar == 0)
        {
          contador--;
          if(contador < 0)
          {
            contador = 99;
          }
        }


        prenderLuces(contador);
      }

      else
      {
        if (verificarNumeroPrimo(contadorPrimo))
        {

          if(sumar == 0)
          {
            numeroAuxiliar = 1;
            contadorPrimo++;

              if(contadorPrimo > 97)
            {
              contadorPrimo = 2;
            }
          }
          else if(restar == 0)
          {
            numeroAuxiliar = 0;
            contadorPrimo--;

            if(contadorPrimo < 2)
            {
              contadorPrimo = 97;
            }
          }
        }
        else 
        {
          contadorPrimo = obtenerNumerosPrimos(contadorPrimo,numeroAuxiliar);
        }

        prenderLuces(contadorPrimo);
      }
    }
    else
    {
      contador = 0;
      prenderLuces(contador);
    }
  }
}
```

## Modificación
Una fotoresistencia es un componente electrónico que cambia su resistencia eléctrica en respuesta a la cantidad de luz que incide sobre ella. En el proyecto decidí usarlo para determinar si el entorno está oscuro o iluminado. Cuando la luz  cae por debajo de cierto valor(definido como minLuz en este caso), se activa el modo de suspensión. Esto significa que, si no se detecta actividad (pulsaciones de botón) durante un período de tiempo, el contador entra en un estado de suspensión para ahorrar energía. Cuando se reanuda la actividad (como una pulsación de botón), el contador se vuelve a encender.

## Link al proyecto

https://www.tinkercad.com/things/hEzW1mVNeFj-trabajopartetres/editel?sharecode=w4j9loCLPL3Yih_YujR5-KS927g_7mwelvu0srSY2fM
