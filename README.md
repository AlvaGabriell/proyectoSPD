# proyectoSPD
Primera parte del parcial


![Primera parte del proyecto](https://github.com/AlvaGabriell/proyectoSPD/blob/main/Imagenes%20Proyectos/1er%20proyecto.png?raw=true)

[Link proyecto 1era parte](https://www.tinkercad.com/things/gAemcLuC6ex-parte-1-proyecto-gabriel-alva/editel?sharecode=HLoxwAN4L0HkfC0XOirH4gJgcmiTHVmwimBpfrpmqdE)

Desarrollar un contador en arduino que muestre una determinada serie de numeros en 2 displays de 7 segmentos conectados mediante multiplexacion.
```
```
Codigo:

prendedigito recibe el display a prender.
alterna los estados para que los display trabajen individualmente
pero mantengan la multiplexacion 
```
void prendeDigito(int digito)
{
  if(digito== UNIDAD)
  {
  	digitalWrite(UNIDAD,LOW);
    digitalWrite(DECENA,HIGH);
    delay(TIMEDISPLAYON);
  }
  else if (digito == DECENA)
  {
  	digitalWrite(UNIDAD,HIGH);
    digitalWrite(DECENA,LOW);
    delay(TIMEDISPLAYON);
  }
  else
  {
  	digitalWrite(UNIDAD,HIGH);
    digitalWrite(DECENA,HIGH);
  }
```
mostrar cantidad recibe la cantidad,
enciende los leds y hace la operacion para obtener la decena y luego mostrarla 
con la funcion prendeDigito.
vuelve a encender los leds y luego hace otro calculo para obtener la unidad y tambien mostrarla.
```
void mostrarCantidad(int count)
{
  prendeDigito(APAGADOS);
  mostrarDigito(count/10);
  prendeDigito(DECENA);
  prendeDigito(APAGADOS);
  mostrarDigito(count -10*((int)count/10));
  prendeDigito(UNIDAD);
}

}
```
botonApretado 
toma los valores de los 3 pulsadores 
y controla que una presionada sea considerada como tal
aun manteniendo presionado el boton. 
```
int botonApretado(void)
{
	sube=digitalRead(SUBE);
  	baja=digitalRead(BAJA);
 	reset=digitalRead(RESET);
  	if(sube)
 	subePrevia=1;
  Serial.println(sube);
 
    if(baja)
 	bajaPrevia=1;
  
  	if(reset)
 	resetPrevia=1;

  
  if(sube==0 && sube != subePrevia)
  {
  	subePrevia=sube;
    return SUBE;
  }
  
    if(baja==0 && baja != bajaPrevia)
  {
  	bajaPrevia=baja;
    return BAJA;
  }
  
  	if(reset==0 && reset != resetPrevia)
  {
  	resetPrevia=reset;
    return RESET;
  }
  return 0;
}
```
![Segunda parte del proyecto](https://github.com/AlvaGabriell/proyectoSPD/blob/main/Imagenes%20Proyectos/2da%20parte.png?raw=true)

[Link proyecto 2da parte](https://www.tinkercad.com/things/j6wnG5Uch1y-copy-of-proyecto-3ra-parte-gabriel-alva/editel?sharecode=e7PHru82s6GhBbzSDvSmPRiu67KVlHurrE_U3AlngWc)

Sustituye uno de los botones por un interruptor deslizante (switch) de dos posiciones.
Dependiendo de la posición del interruptor, el display debe mostrar o bien el contador (como
en la Parte 1) o los números primos en el rango de 0 a 99. Agregar un sensor de temperatura y un motor al proyecto.

Codigo : 
Se utiliza una funcion para detectar los numeros primos en las iteraciones del loop.
```
int esPrimo(int numero)
{
    if (numero <= 1) {
        return 0; 
    }

    for (int i = 2; i * i <= numero; ++i) 
    {
        if (numero % i == 0) 
        {
            return 0; 
        }
    }

    return 1;
}
```
En el loop creamos 2 iteraciones para que el funcionamiento del programa varie segun el valor del switch.
dentro del loop hago una validacion que me permita saber el valor del switch y que asi la iteracion rompa si mi switch cambio el estado.
como funcionalidad adicional en la condicion que validaba el switch tambien agregue una temperatura previamente mapeada lo cual ... 
Solo va mostrar numeros del 1 al 99 si la temperatura es mayor a 15 y si la ultima mencionada es mayor a 40 tambien enciende el motor.
```
void loop()
{
    if(digitalRead(SWITCH))
  {
    for(int i=0;i<=99;i++)
    {
      lecturaCuenta = analogRead(SENSOR);
      temperatura=map(lecturaCuenta,20,358,-40,125);
      if(digitalRead(SWITCH) && temperatura >15)
      {
       	Serial.println(temperatura);
        if(temperatura >40)
        {
          digitalWrite(MOTORPSICO,ENCENDIDO);
          delay(100);
          printCount(i);
        }else
        {
          delay(100);
          printCount(i);
          digitalWrite(MOTORPSICO,APAGADO);
        }
      }
      else
      {
        analogWrite(MOTORPSICO,APAGADO);
        printCount(APAGADO);
      	break;
      }
  }
  if(!digitalRead(SWITCH))
  {
    for(int i=0;i<=99;i++)
    {
      if(!digitalRead(SWITCH))
      {
        if(esPrimo(i))
        {          
        	delay(500);
          	printCount(i);
        }
      }
      else
      {
        printCount(APAGADO);
      	break;
      }
    }
  } 
  }  
}

```
![Tercera parte del proyecto](https://github.com/AlvaGabriell/proyectoSPD/blob/main/Imagenes%20Proyectos/3ra%20parte.png?raw=true)

[Link proyecto 3ra parte](https://www.tinkercad.com/things/0rye6J9KcCW-copy-of-copy-of-copy-of-copy-of-copy-of-tremendous-bombul/editel?sharecode=kDBteFMoHSxJfpAspxQrGbwArsarErNFaJ4RSPqmqfk)


1. Considerando el último número de tu número de documento (DNI o documento de
identidad), agrega un componente adicional que afecte el funcionamiento del proyecto.


Para el proyecto se agrega el sensor de luz y en el loop, si el sensor recibe que no la hay, no hace ninguna de las 2 opciones recicladas del a parte 2.
```
void loop()
{
  if(valorSensorLuz = analogRead(SENSORLUZ))
  {
    if(digitalRead(SWITCH))
  {
    for(int i=0;i<=99;i++)
    {
      lecturaCuenta = analogRead(SENSOR);
      temperatura=map(lecturaCuenta,20,358,-40,125);
      if(digitalRead(SWITCH) && temperatura >15 && analogRead(SENSORLUZ))
      {
       	Serial.println(temperatura);
        if(temperatura >40)
        {
          digitalWrite(MOTORPSICO,ENCENDIDO);
          delay(100);
          printCount(i);
        }else
        {
          delay(100);
          printCount(i);
        }
      }
      else
      {
        analogWrite(MOTORPSICO,APAGADO);
        printCount(APAGADO);
      	break;
      }
    }
  }
  if(!digitalRead(SWITCH) && analogRead(SENSORLUZ))
  {
    for(int i=0;i<=99;i++)
    {
      if(!digitalRead(SWITCH))
      {
        if(esPrimo(i))
        {          
        	delay(500);
          	printCount(i);
        }
      }
      else
      {
        printCount(APAGADO);
      	break;
      }
    }
  } 
  }  
}
```


