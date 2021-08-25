# Álgebra lineal

Es momento de conocer a la estrella del curso: los arreglos y todo lo 
que se puede hacer con ellos. En Julia los arreglos n-dimensionales 
son "estructuras primitivas" del lenguaje en sí. Es decir, son parte 
fundamental del lenguaje y sus sintaxis, como en Matlab, y no una 
adición que requiere sintaxis específica y engorrosa como en 
Python/Numpy. Como en Matlab, todos los elementos del arreglo (vector, 
matriz, tensor) deben ser del mismo tipo.

```julia
primos = [1, 3, 5, 7]
b = ["Vector", "de", "strings"]
c = [3.1415, 2.69]
complejos = [1+0im 0+1im 1+1im]
```

Se pueden concatenar elementos "al final" de un vector con la función 
`append!`. Cabe mencionar que en Julia se tiene la convención de que 
si una función modifica a su argumento, el nombre debe terminar en 
`!`. Otros ejemplos de funciones que modifican su argumentos son 
`push!` y `pop!` que también actúan sobre vectores, y permiten usarlo 
como pilas!

```julia
append!(primos, 11) # Añadir 11 "al final" del vector.
push!(primos, 13) # Mismo efecto que append!
un_primo = pop!(primos) # Recupera el último número que añadimos.
```

Pero a diferencia de Matlab, hay algunas sutilezas. La siguiente celda 
va a tener un error. Cual es?

```julia
append!(primos, 0.69)
```

El vector `primos` tenía exclusivamente números enteros, y tratamos de 
concatenarle un float. No hay manera de convertir un float a entero 
sin perder precisión, por lo tanto Julia no lo hace. Hay manera de 
convertir `primos` a un vector de floats _a posteriori_, o si se sabe 
de antemano que el vector podría contener no-enteros, se pudo haber 
creado así desde el inicio.

```julia
# Dos maneras equivalentes de hacer lo mismo
primos = [1.0, 3.0, 5.0, 7.0]
primos = [1.0, 3, 5, 7] # Se puede hacer trampa y solo poner el punto al primer elemento
primos = Float64[1, 3, 5, 7]
```

Saber el tipo de número de un arreglo le permite al _compilador_ de 
Julia hacer optimizaciones. Podemos verificar el tipo de datos de un 
vector o arreglo.

```julia
typeof(primos)
```

Julia nos dice que tenemos un `Array` y entre `{}` nos da información 
adicional. Por ejemplo que el tipo de datos de los elementos es 
`Float64` y el número después de la coma indica la _dimensión_ en el 
sentido matemático de el arreglo. En este caso, al ser un vector, es 
un arreglo unidimensional. Si solo nos interesa saber el tipo de datos 
de elementos tenemos la función `eltype`, que es fácil recordar ya que 
suena a _element type_.

Ahora vamos a crear una matriz para ver cómo cambia `typeof`.

```julia
A = rand(3, 3)
A = [1 2 3; 11 12 13; 100 200 300] # Igual que Matlab! Que maravilla!
@show A # Un poco de magia para imprimir a A de forma bonita.
typeof(A)
```

Ahora Julia nos dice que `A` no es un vector, sino una matriz. No hace 
falta alarmarse, tras bambalinas son la misma cosa. De hecho, `Vector` 
es un _alias_ para no escribir matriz de dimensión 1.

```julia
Vector{Float64} == Array{Float64,1}
```

## Indices y views

Igual que en Matlab, es fácil accesar un elemento arbitrario de un 
arreglo.

```julia
# Obteniendo el tercer primo
primos[3]
```

Nótese la diferencia con la sintaxis de Matlab. En Matlab se usan 
paréntesis mientras que en Julia se usan brackets o paréntesis 
cuadrados, pero la esencia es la misma. Para arreglos de más `n` 
dimensiones se usan `n` índices.

```julia
idx_fila = 1
idx_columna = 2
A[idx_fila, idx_columna]
```

### Comentario corto sobre memoria

Julia, a diferencia de muchos otros lenguajes, guarda sus arreglos en 
_column major mode_. Es decir, para guardar una estructura con por 
ejemplo filas y columnas en lo que esencialmente es una fila en la 
memoria, el arreglo se guarda columna por columna.

! Aqui la foto del curso que ejemplifica esto.

Como consecuencia, añadir columnas es más rápido que añadirle filas a 
una matriz. Esto es útil cuando se hace una operación en la que se van 
guardando los resultados iteración a iteración en una matriz.

### Slices y views

De manera idéntica a Matlab, se vale tomar "cachitos" de una matriz.  
Es decir, no solo podemos obtener entradas específicas de una matriz, 
sino que podemos obtener filas, columnas o submatrices en general.

```julia
A[1, :] # Primera fila
A[:, 1] # Primera columna
A[end, :] # Última fila
A[:, end] # Última columna

A[2:end, 2:end] # A sin su primera columna y filas.
```

Usando `:` podemos elegir un rango, por ejemplo `1:10` quiere decir 
los números entre 1 al 10. Un rango vacío como `:` en el contexto de 
índices de una matriz se lee como "todo", es decir, equivale a 
`1:end`.

Igual que en Matlab los rangos pueden hacer cosas interesantes como 
tener incrementos específicos. Por ejemplo `1:0.5:5` es un rango de 
los números del 1 al 5 en incrementos de 0.5 (1, 1.5, 2, ..., 5).  
Julia permite otra cosa muy útil para indexar. Usando vectores como 
índices de una matriz podemos tomar filas o columnas no consecutivas o 
incluso reacomodar a la matriz.

```julia
A[[1, 3], :] # Tomando la primera y tercera fila de A
# Reordenando a A
orden = [3 1 2]
@show A[orden, orden]
```

Estos "slices" no usan memoria adicional. Son pequeñas ventanas a el 
objeto original, en estos ejemplos `A`. Por lo tanto, modificar un 
slice puede modificar al objeto original.

Por ejemplo, implementar un paso del algoritmo Gauss-Jordan es 
trivial:

```julia
# Pivote en la posición i, j
i, j = (2, 2)
A[i, : ] = A[i, :] / A[i, j]

for irow = 1:size(A, 1)
	if i != i
		A[irow, :] = A[irow, :] .- A[i, :] .* A[irow, j]
	end
end
```

Bonus: Los _for loops_ y condicionales se escribe exactamente igual 
que en Matlab!

## Broadcasting

Ahora bien, que son esas cosas raras en el ejemplo pasado? Que es `.-` 
por ejemplo?

Una de las ventajas que Julia introduce a la notación usual estilo 
Matlab es el concepto de _broadcasting`_. En palabras sencillas, el 
_broadcasting_ permite aplicar una función u operación a un arreglo 
elemento por elemento. Por ejemplo:

```julia
vec = [1 3 5 7 9]
vec - 1 # Error
```

Que sucedió aquí? Estrictamente hablando, la resta no está definida 
entre un elemento de $\mathbb{R}$ y $\mathbb{R}^{5}$. Pero si queremos 
restar 1 a todos los elementos de un vector sería terriblemente 
engorroso tener que crear un vector de tamaño 5, llenarlo de unos y 
restarlos. Por eso tenemos la notación de broadcasting, que en su 
forma general se invoca poniendo un `.` antes de realizar una 
operación o llamar una función.

```julia
vec = [1 3 5 7 9]
vec .- 1 # Hace lo que esperamos
```

Con base en este ejemplo uno podría preguntar qué necesidad hay añadir 
sintaxis engorrosa. Qué no puede Julia ser inteligente por mi y ver lo 
que estoy tratando de hacer en esa resta o al evaluar `sin([pi, pi/2, 
2*pi])`? Probablemente podría, pero se nos sigue escapando la magia 
del broadcasting.

Por ejemplo, tomemos la matriz exponencial. Para hacer esto en Matlab 
se necesitan dos funciones completamente distintas: `exp` y `expm`. La 
función `exp` evalúa la función exponencial en cada entrada de la 
matriz, mientras que `expm(A)` regresa _la_ matriz exponencial de `A`.  
Esto es mucho más simple en Julia:

```julia
# Exponencial de la matriz A
exp(A)

# Exponencial evaluada en cada entrada de A
exp.(A)
```

Y ese es solo un pequeño ejemplo. El broadcasting es una de los 
grandes beneficios de usar Julia aunque ahora aún no sea obvio. Por el 
momento, pensemos que nos ahorra escribir muchos _for loops_.

# Álgebra Lineal por fin

Empecemos por crear una matriz de 6•6 con entradas aleatorias

```julia
A = rand(6, 6) # Se le puede especificar el rango de las entradas a rand como primer argumento
```

Similar a Matlab, se puede transponer una matriz como `A'`. Pero 
cuidado, esto en realidad quiere decir `A` adjunta. Demos un vistazo.

```julia
A'
```

Julia ahora nos informa que mágicamente `A` pasó de ser tipo `Matrix` 
a ser de tipo `adjoint(Matrix{Float64}`. Qué pasó??

Muchas transformaciones en Julia "cambian" el tipo de la matriz 
transformada. Eso no quiere decir que haya dejado de ser una matriz, 
sino que Julia tiene información adicional para hacer optimizaciones, 
cambiar el algoritmo que resuelve ciertas operaciones, o incluso 
factorizar de cierta forma la matriz para ahorrar cómputo. Por 
ejemplo, la adjunta en Julia es "perezosa". Calcular el producto 
`A*A'` se puede hacer ahorrando calcular la transpuesta gracias a que 
Julia está consciente de las optimizaciones posibles. Y así como 
`adjoint` hay `diagonal`, `triangular`, entre muchas muchas otras.

Esta información es extremadamente útil para una de las operaciones 
fundamentales del álgebra lineal computacional: el resolver el sistema 
$A\vec{x} = \vec{b}$.

```julia
A = rand(5, 5)
b = rand(5)
x = A\b
@show norm(A*x-b)
```

El operador `\` igual que en Matlab resuelve el sistema, y cuando no 
tiene solución da la solución al problema de mínimos cuadrados. Es 
importante recordar que `\` *siempre* es la manera preferida de 
resolver un sistema lineal, casi nunca queremos usar `inv(A)` no solo 
por la propagación del error numérico sino también porque Julia 
utiliza la información que tiene sobre la matriz para usar el 
algoritmo más eficiente para resolver el sistema. Por ejemplo, para 
una matriz cuadrada general utiliza la factorización LU, para una 
rectangular la factorización QR, si es triangular se puede obtener 
directo, y así sucesivamente.

## Intermisión

Este es un gran momento para conocer otra de las grandes ventajas de 
Julia y su entorno (llamado REPL). La línea de comandos de Julia tiene 
distintos modos. Hasta ahora se ha visto más o menos así:

```julia
julia>  A = rand(5, 5)
```

Si escribimos `?` en una línea vacía entraremos a un modo distinto, el 
modo de ayuda. Hay que usarlo para que nos diga más sobre cómo 
resuelve sistemas lineales:

```julia
help> \
```

Si tienes duda de algún método, función u operación puedes usar este 
modo ayuda para buscar más información sin tener que dejar la linea de 
comandos. Y ese no es el único truco que tiene bajo la manga. Escribir 
`]` en una línea vacía te lleva al modo manejador de paquetes, 
escribir `;` te lleva al modo _system shell_ que te permite 
interactuar con la línea de comandos de tu computadora sin tener que 
cerrar Julia.
