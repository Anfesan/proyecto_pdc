# Proyecto programación de computadores: Juego de ahorcado en python
## Grupo los 7 pecados de la programación:
### Steffy Geraldine Fernández González
### Andrés Felipe Sánchez Gómez
### Nilson Daniel Dueñas López

[![Logo-Blood-Small.png](https://i.postimg.cc/2ytcCvyY/Logo-Blood-Small.png)](https://postimg.cc/QKpkbFcY)

## 1. Base de datos
#### La base de datos se creó en Excel, guardándose como archivo .csv para ser importado en python, luego, se crearon dataframes con la librería 'pandas' y por último, se creó una función para que se escoja una palabra aleatoria dentro de una categoría de palabras elegida por el usuario.
```mermaid
flowchart TD;
    A(Elección de categoría de palabras por el usuario)
    A-->B[Se crea una base de datos de 1000 palabras y de 10 categorías en Excel];
    B-->C[Se importa la base de datos como archivo .csv];
    C-->D[Para importar dentro del notebook de Google colab, se usa el url de descarga del archivo];
    D-->E[Se crea el dataframe con la librería pandas];
    E-->F[Se crea una función para elegir una palabra aleatoria con la librería random];
    F-->G[Se caracteriza cada categoría con un número];
    G-->H;
    H{¿La categoría se define con el entero x?} -- Sí --> I[Elegir una palabra aleatoria de la categoría];
    H -- No --> J[Seguir buscando la categoría correspondiente con el entero];
    J-->H;
    I-->K(Fin)
```
```python
import pandas as pd #para el dataframe
import random as rnd #para elegir una palabra aleatoria

url_base_datos = "https://drive.google.com/uc?id=1rBEQxTlNzrt7oDwD8OklrCUn-DZ70xRJ" #importar base de datos(formato .csv)
df_base_datos = pd.read_csv(url_base_datos) #hacer el dataframe con la base de datos
```
```python
def categoria_elegida_usuario(categoria_usuario,df_base_datos):
  categorias = list(df_base_datos) #guardar en una lista los titulos de las columnas del dataframe
  if 1 <= categoria_usuario <= len(categorias): # si el usuario eligió del 1 al 10
    categoria_seleccionada = categorias[categoria_usuario - 1] #corrección para que los indices no estén desfasados
    palabra_aleatoria = rnd.choice(df_base_datos[categoria_seleccionada]) # se elige una palabra aleatoria dentro de la categoria seleccionada, llamando el indice (ya corregido) de la misma
    return palabra_aleatoria
  else:
    return "Opción no válida, por favor elija un número del 1 al 10 dentro de las categorías disponibles "

print(" 1.Animales \n 2.carreras universitarias \n 3.ciudades \n 4.comida \n 5.cuerpo humano \n 6.deportes \n 7.objetos de la casa \n 8.paises \n 9.elementos de la tabla periódica \n 10.universidad ")
categoria_usuario = int(input("Elija una categoría digitando el número correspondiente "))
funcion_prueba = categoria_elegida_usuario(categoria_usuario,df_base_datos)
```
## 2. Comparador de la palabra y contador de intentos.
#### En principio esto se encarga de tomar la palabra y transferir cada caracter a una matriz para luego ser comparada con el input 
```python
palabra_dividida=[]
palabra_avance=[]
contador_vidas=6
letra_usuario = str
letra_intentos=[]
for i in range(0,len(funcion_prueba)):
    palabra_dividida.append(funcion_prueba[i])
    palabra_avance.append("")
# print(palabra_dividida)
# print(palabra_avance)
while contador_vidas>0:
    letra_usuario = str(input("Ingrese una letra : "))
    while letra_usuario in letra_intentos:
        letra_usuario = str(input("Ingrese una letra diferente: "))
    letra_intentos.append(letra_usuario)
    if letra_usuario in palabra_dividida :
            print("Correcto")
            for i in range(0,len(funcion_prueba)):
                if palabra_dividida[i]==letra_usuario:
                    palabra_avance[i]=letra_usuario
    else:
        contador_vidas-=1
        print("Incorrecto")
    print(palabra_avance)
```
### Definiendo las listas variables, listas y diccionarios necesarias siendo:
* palabra_dividida: sera la base para comparar el input (es posible con un string, lo decidimos hacer haci por comodidad)
* palabra_avance: sera donde se guarde los aciertos del input, y sera la usada para la condicion de victoria
* vidas: el contador de errores
* letra_usuario: input del usuario, se corrobora vs letra_intentos
* letra_intentos: donde se almancenan cada input unico del usuario
* dificultad_usuario: se define dependiendo del input de dificultad y de este dependera la cantidad de vidas_iniciales
* vidas_iniciales: con la dificultad definida, toma diferentes valores
```python
palabra_dividida=[]
  palabra_avance=[]
  dificultad_usuario=0
  print(" 1.Facil \n 2.Medio \n 3.Dificil")
  while dificultad_usuario != 1 and dificultad_usuario != 2 and dificultad_usuario != 3:
    dificultad_usuario = input("Elija una dificultad : ")
    if dificultad_usuario.isdigit():
      dificultad_usuario = int(dificultad_usuario)
    else:
      print("Opción no válida, por favor elija un número del 1 al 3")
      continue
    if dificultad_usuario==1:
      vidas_iniciales=6
    elif dificultad_usuario==2:
      vidas_iniciales=4
    elif dificultad_usuario==3:
      vidas_iniciales=2
    else:
      print("Opción no válida, por favor elija un número del 1 al 3")
  vidas=0
  letra_usuario =[]
  letra_intentos=[]
``` 
###Globales o Banderas definidas al incio
* continua: Bandera para continuar el juego con mas jugadores
* puntaje: Se usara en el resultado de cada partida para dar el resultado de dicha partida
* puntajes: Diccionario donde se definira, "Jugador" mas el numero del ciclo que va y su value es el puntaje de dicho ciclo, ej: Jugardo 1: 45, Jugador 2: 30
* contador_juegos: Lleva el conteo de ciclos de juegos, se usa para definir el key de puntajes
```python
continuar=True
puntaje=0
puntajes={}
contador_juegos=1
``` 

### Continuar, Mas jugadores
* Todo el juego se encierra dentro de un bucle dependiente de continuar (el cual se define con un input al finalizar cada juego)
* se usa: si_no como comprobante de ser un input valido y luego se asigna el nuevo True o False a continuar para seguir o no el bucle
```python
while continuar:
.............
si_no=input("Deseas continuar jugando? Y/N")
  if si_no.isalpha():
    si_no=si_no.upper()
    if si_no=="Y":
      continuar=True
    elif si_no=="N":
      continuar=False
      print("Gracias por jugar")
    else:
      print("Opción no válida")
      break
  else:
    print("Opción no válida")
    continue
while (si_no!=False) or (si_no!=True):
    si_no=input("Deseas continuar jugando? Y/N")
    if si_no.isalpha():
      si_no=si_no.upper()
    else:
      print("Opción no válida")
      continue
    if si_no=="Y":
      continuar=True
    elif si_no=="N":
      continuar=False
      print("Gracias por jugar")
      break
    else:
      print("Opción no válida")
```  

### Dificultad
* Se usa un bucle para confirmar que el input hace parte de la seleccion de dificultas, o pedir el input nuevamente
```python
print(" 1.Facil \n 2.Medio \n 3.Dificil")
  while dificultad_usuario != 1 and dificultad_usuario != 2 and dificultad_usuario != 3:
    dificultad_usuario = input("Elija una dificultad : ")
    if dificultad_usuario.isdigit():
      dificultad_usuario = int(dificultad_usuario)
    else:
      print("Opción no válida, por favor elija un número del 1 al 3")
      continue
    if dificultad_usuario==1:
      vidas_iniciales=6
    elif dificultad_usuario==2:
      vidas_iniciales=4
    elif dificultad_usuario==3:
      vidas_iniciales=2
    else:
      print("Opción no válida, por favor elija un número del 1 al 3")
```

### Creacion de listas:
* Primero se crea una con cada caracter de la palabra
* Se crea tambien una lista con indices vacios para tener el largo de la palabra donde se pueda ingresar los aciertos

```python
for i in range(0,len(funcion_prueba)):
    palabra_dividida.append(funcion_prueba[i])
    palabra_avance.append("")
```
### Comparador:
1. Primera parte:
  
+ while: Todo esta rodeado por un loop dependiente de el flag vidas
+ input: Se pide una letra, entra a un loop para confirmar que esta letra no esta en la lista letra_intentos
+ letra_intentos: si se cumple con lo anterior se pasa a añadir la letra a letra_intentos
            
2.  Segunda parte
+Comparador: si la letra esta en palabra_dividida se dice Correcto y se pasa a comparar la letra en cada posicion de la lista, en el incide que este se cumpla se reemplaza el vacio con letra_usuario
+Sino: se resta una vida y se dice Incorrecto
```python
while vidas<=vidas_iniciales:
      letra_usuario = str(input("Ingrese una letra : "))
      while letra_usuario in letra_intentos:
          letra_usuario = str(input("Ingrese una letra diferente: "))
      letra_intentos.append(letra_usuario)
      if letra_usuario in palabra_dividida :
            print("Correcto")
            for i in range(0,len(funcion_prueba)):
                if palabra_dividida[i]==letra_usuario:
                    palabra_avance[i]=letra_usuario
      else:
          vidas+=1
          if vidas<=vidas_iniciales:
            print("Incorrecto")
            mostrar_escenario(vidas)
            print("Te quedan ",vidas_iniciales-vidas," vidas")
      print(palabra_avance)
      if palabra_avance==palabra_dividida:
        print("Felicidades encontraste la palabra")
        break
      elif vidas==vidas_iniciales:
        print("Perdiste")
        break

```
### Conteo Aciertos, Puntaje (calculo e impresion), Puntajes Jugadores
* Se recorre la lista de avance y se suman los aciertos
* Se da el calculo del puntaje de la ronda
* Se añade el Jugador[#] y su puntaje a el diccionario de puntajes y se imprime
```python
  for letras in palabra_avance:
    if letras!="":
      puntaje+=1
  print("Tu puntaje es de {punt}".format(punt=int(puntaje/len(palabra_dividida)*100)))
  puntaje_global+=puntaje/len(palabra_dividida)*100
  puntajes["Jugador{0}".format(contador_juegos)]=int(puntaje/len(palabra_dividida)*100)
  puntaje=0
  for i in puntajes:
    print(i,puntajes[i])
```
## 3. Interfaz gráfica
```mermaid
graph TD
    A[Inicio] --> B[Función mostrar_escenario]
    B --> C{¿Cuántos errores?}
    C --> D0[0 errores: Mostrar horca vacía]
    C --> D1[1 error: Mostrar cabeza]
    C --> D2[2 errores: Mostrar cabeza y cuerpo]
    C --> D3[3 errores: Mostrar cabeza, cuerpo y un brazo]
    C --> D4[4 errores: Mostrar cabeza, cuerpo y ambos brazos]
    C --> D5[5 errores: Mostrar cabeza, cuerpo, brazos y una pierna]
    C --> D6[6 errores: Mostrar figura completa]
    D0 --> E[Fin]
    D1 --> E
    D2 --> E
    D3 --> E
    D4 --> E
    D5 --> E
    D6 --> E
```
```python
def mostrar_escenario(vidas):
    ahorcado = {
        0: '''
          ------
          |    |
          |
          |
          |
          |
        ---------''',
        1: '''
          ------
          |    |
          |    O
          |
          |
          |
        ---------''',
        2: '''
          ------
          |    |
          |    O
          |    |
          |
          |
        ---------''',
        3: '''
          ------
          |    |
          |    O
          |   /|
          |
          |
        ---------''',
        4: '''
          ------
          |    |
          |    O
          |   /|\\
          |
          |
        ---------''',
        5: '''
          ------
          |    |
          |    O
          |   /|\\
          |   /
          |
        ---------''',
        6: '''
          ------
          |    |
          |    O
          |   /|\\
          |   / \\
          |
        ---------'''
    }
    print(ahorcado[vidas])
```
#### En esta sección del código, se creó una función llamada mostrar_escenario, cuyo propósito es mostrar visualmente el progreso de un juego de ahorcado. Dentro de la función, se incluyó un diccionario que contiene varias representaciones gráficas, cada una mostrando una parte diferente del dibujo del ahorcado. El diccionario maneja diferentes situaciones, desde cuando no se ha cometido ningún error y la horca está vacía, hasta cuando el dibujo del ahorcado está completamente formado. Cada clave en el diccionario corresponde a un número de errores, y cada valor es una representación gráfica de cómo debería verse la figura del ahorcado en ese momento del juego.
