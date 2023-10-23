# proyectoSPD
Primera parte del parcial


![Primera parte del proyecto](https://github.com/AlvaGabriell/proyectoSPD/blob/main/Imagenes%20Proyectos/1er%20proyecto.png?raw=true)

[Link proyecto 1era parte](https://www.tinkercad.com/things/gAemcLuC6ex-parte-1-proyecto-gabriel-alva/editel?sharecode=HLoxwAN4L0HkfC0XOirH4gJgcmiTHVmwimBpfrpmqdE)

Desarrollar un contador en arduino que muestre una determinada serie de numeros en 2 displays de 7 segmentos conectados mediante multiplexacion.
```

Codigo:

```
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
![Segunda parte del proyecto](https://github.com/AlvaGabriell/proyectoSPD/blob/main/Imagenes%20Proyectos/Parte%202.png?raw=true)

[Link proyecto 2da parte](https://www.tinkercad.com/things/3h7caC1fXgx-copy-of-copy-of-copy-of-copy-of-tremendous-bombul-bruticus/editel?sharecode=ePjGwDPxoj7QeDdOtZlXsQdHHba6zLB88qycONYH1Sw)
