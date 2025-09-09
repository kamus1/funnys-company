#### Este modelo consideraba que se podían abrir y cerrar fabricas, pero se encontró que no había ninguna razón para cerrar una y abrir otra fabrica debido a los costos de abrir una nueva.
Por lo que este modelo solo aumentaba la complejidad de la definición del modelo.

### Parámetros o constantes
$CF_{pc}$= Costo fijo de tener la planta p en la ciudad c, entre años es fijo.
$CA_{pc}$= Costo de abrir una nueva planta de tipo p en la ciudad c, entre años es fijo
$CP_{pc}$ = Costo de producir una unidad en la planta de tipo p en la ciudad c
$CAP_p$ = Capacidad planta tipo p

### Variables
$X_{pca}:$ La planta de tipo p está abierta en la ciudad c en el año a
```python
p = [ 
  1: Planta Pequeña
  2: Planta grande ]
c = [
1: Antofagasta
2: Valpo
3: Santiago
4: Rancagua
5: Conce
6: P. Montt ]

a = [1,2,3] 

r=[
1: region 1
2: region 2
3: r3
4: r4
5: r5
6: r6
]

t=[
1: Transporte tipo 1 
2: Transporte tipo 2
	3: Transporte tipo 3]
```

$O_{pca}$ : La planta tipo p se abre (open) en la ciudad c en el año a. La diferencia con la variable X, es que esta nos dice si fue abierta una nueva planta, no si está actualmente abierta.

$A_{pcart}$ : Cantidad (Amount) de unidades enviadas desde la planta p en la ciudad c a la region r en el año a usando el transporte t.

### Dominios:
$X_{pca} \in \{0,1\}$ 
$O_{pca} \in \{0,1\}$ 

$A_{pcart} \geq 0$ 


### FO:

		Min z = costo fijo (fijo + aperturas) + costo variable producción + costo variable de transporte

= $\sum_{a=1}^3 \sum_{c=1}^6 \sum_{p=1}^2 X_{pca} * CF_{pc}$
 $+ \sum_{a=1}^3 \sum_{c=1}^6 \sum_{p=1}^2 CA_{pc} * O_{pca}$
  $+ \sum_{a=1}^3 \sum_{c=1}^6 \sum_{p=1}^2 \sum_{r=1}^6 \sum_{t=1}^3 (CP_{pc} + CT_{crt} )* A_{pcart}$



### Restricciones

1. Restricciones para la variable $O_{pca}$
```pyhon
0 0 0
0 0 1
0 1 0
0 1 1
1 0 0
1 0 1 -> solamente aquí se abre 2 veces una misma planta
1 1 0
1 1 1
```

Es linealizar lo siguiente $X_{pca} * ( X_{pca} - X{pc,a-1})$
si es el primer año el año anterior se considera como 0, excepto por rancagua, que parte abierta.

Se considera que el primer año es igual al valor de X. si se activa X el primer año entonces se abrió una nueva planta, (esto es para evitar el $X_{pc,a-1}$ sin año previo).
$O_{pc1} = X_{pc1}  \forall p, \forall c \neq 4$ 

Detecta cada salto 0 -> 1 (apertura) y permite cierres.
$O_{pca} \geq X_{pca} - X_{pc,a-1}$  
$O_{pca} \le X_{pca}$ 
$O_{pca} \le 1 - X_{pc,a-1}$  esta evita pagar apertura cuando ya estaba abierta
$\forall p, \forall c \neq4, \forall a \in \{2,3\}$

2. Fijar a rancagua pequeña c=4.
 - Como supuesto, es que se parte con rancagua y se queda ahí, no se construye otra:
	$X_{p=1,c=4,a} = 1$ Pequeña activada
	$X_{p=2,c=4,a} = 0$ Grande desactivada
	 $O_{p=1,c=4,a} = 0$  No se abre tipo pequeña en cualquier año (ya estaba abierta), igual se cobra 0, si se reabre según la tabla.
	 $O_{p=2,c=4,a} = 0$ No se abre tipo grande
	 $\forall a \in \{1,2,3\}$

3. Restricciones para satisfacer demanda
	$D_{ra} = \sum_{p=1}^2 \sum_{c=1}^6 \sum_{t=1}^3  A_{pcart} \forall r,a$  

4. No pasarse de la cantidad de producción en 1 planta, capacidad por planta.

$\sum_{r=1}^6 \sum_{t=1}^3 A_{pcart} \le CAP_{p} * X_{pca}$  $\forall p,c,a$ 

5. solo una alternativa de tamaño de planta por ciudad y año.
	$X_{1,ca} + X_{2,ca} \le 1 \forall c,a$


	
	