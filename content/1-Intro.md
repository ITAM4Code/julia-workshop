---
jupyter:
  jupytext:
    cell_metadata_filter: -all
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.11.1
  kernelspec:
    display_name: Julia 1.6.1
    language: julia
    name: julia-1.6
---

# Introducción a Julia

> We want a language that's open source, with a liberal license. We 
> want the speed of C with the dynamism of Ruby. We want a language 
> that's homoiconic, with true macros like Lisp, but with obvious, 
> familiar mathematical notation like Matlab. We want something as 
> usable for general programming as Python, as easy for statistics as 
> R, as natural for string processing as Perl, as powerful for linear 
> algebra as Matlab, as good at gluing programs together as the shell. 
> Something that is dirt simple to learn, yet keeps the most serious 
> hackers happy. We want it interactive and we want it compiled.
>
> (Did we mention it should be as fast as C?)

Los creadores de Julia: [Why we created 
Julia](https://julialang.org/blog/2012/02/why-we-created-julia/)

Es decir:

- (Casi) tan rápido como C.
- Dinámico y general como Python.
- Notación obvia para matemáticas y poderoso para álgebra lineal como 
  Matlab.
- Tan bueno para estadística como R.

## Filosofía y porqué Julia

> Schools (and this includes all educational activities) influence the 
> future of society through what they teach. They should teach 
> exclusively free software, so as to use their influence for the 
> good. To teach a proprietary program is to implant dependence, which 
> goes against the mission of education. By training in use of free 
> software, schools will direct society's future towards freedom, and 
> help talented programmers master the craft.

Richard Stallman: [Free Software Is Even More Important 
Now](https://www.gnu.org/philosophy/free-software-even-more-important.en.html)

----

En el ITAM es muy común el uso de Matlab. Parte del objetivo de este 
curso es presentar a Julia como una mejor alternativa a Matlab en 
general y como alternativa seria a otros lenguajes como Python y R.  
Una de sus ventajas más notorias es ser órdenes de magnitud [más 
rápido](https://towardsdatascience.com/r-vs-python-vs-julia-90456a2bcbab) 
que los lenguajes mencionados anteriormente, pero escribir código 
rápido no requiere dos doctorados.

Comparado a Matlab, Julia es mucho más rápido, software libre (no más 
depender de la licencia académica del ITAM!) y mucho más adecuado a 
tareas generales. Matlab suele ser bueno y útil para el álgebra lineal 
gracias a su sintaxis limpia y familiar. Julia la conserva y añade 
mucho poder. 

Comparado a R, Julia también es mucho más rápido y más adecuado a 
tareas generales más allá de el cómputo estadístico. Julia tiene la 
habilidad de manipular Data Frames como R, integrarse con $\LaTeX$ 
fácilmente y producir visualizaciones de alta calidad. Pero Julia a 
diferencia de R si presta para todo tipo de programas, lo cual no es 
fácil decir de R.

Quizás la comparación más apta sea con Python. Python es uno de los 
lenguajes de programación más populares y es usado para casi toda la 
clase de cómputo que se pueda imaginar, y goza de un ecosistema 
envidiable. Entonces, ¿por qué Julia y no Python? Python es un 
lenguaje *muy* lento para estándares modernos. Y en particular para el 
uso que buscamos darle, el cómputo numérico es, siendo generosos, 
menos que disfrutable. Por ejemplo, tomemos una operación común en el 
álgebra lineal: conjugar una matriz.

$$
A = P D P^{-1}
$$

En Python, usando numpy que es la librería que permite la 
multiplicación matricial, esta operación se haría como:

```python
# No ideal
A = np.dot(P, np.dot(D, np.inv(P)))

# Menos malo
A = np.linalg.multi_dot([P, D, np.inv(P)])
```

Mientras que en Julia, no podría ser más simple:

```julia
A = P * D * P^-1
```

El ejemplo de Python tiene dos formas distintas de hacerse porque 
ambas se pueden encontrar en código "profesional", a pesar de que la 
primera es considerada mala práctica. La sintaxis compleja 
necesariamente introduce oportunidades para malas prácticas, y crea 
situaciones en las que no es fácil escribir buen código. Por ejemplo, 
la implementación de `multi_dot` permite reacomodar las matrices para 
acelerar el cálculo y reducir la propagación del condicionamiento 
numérico [^1], pero uno no sabría eso sin leer la documentación.  
Julia hace estas optimizaciones y otras mucho más avanzadas de manera 
directa y transparente al usuario.

[^1]: [Documentación de numpy](https://numpy.org/doc/stable/reference/generated/numpy.linalg.multi_dot.html#numpy.linalg.multi_dot)

## Variables

La asignación de variables tiene exactamente la misma forma que en 
Python y Matlab:

```julia
x = 3.141592653
x
```

Julia también es relajado con los tipos de datos de las variables. Por 
ejemplo podemos reasignar `x` de ser un número a ser un _string_.

```julia
x = "Hola ITAM! 👋"
x
```

Igual que en otros lenguajes, podemos imprimir los valores de las 
variables en cualquier momento. Y al momento de imprimir también 
podemos hacer lo que Julia llama _string interpolation_, que no es más 
que incluir valores de variables directamente en un string. Para usar 
esa funcionalidad escribimos `$variable` dentro del string, Julia se 
encarga del resto.

```julia
nombre = "Jorge"
print("Hola! $nombre, bienvenido al curso")
```

### Estructuras de datos

Julia tiene arreglos n-dimensionales, que es parte de lo que nos 
interesa. Pero también tiene algunas otras estructuras de datos que 
son muy útiles, por ejemplo:

1. Tuplas
2. Diccionarios
3. Data frames

Las tuplas, como en mate, son estructuras ordenadas, inmutables, de 
datos. Vale la pena mencionar que no necesariamente deben contener 
datos del mismo tipo. Por ejemplo:

```julia
tupla = (1, 3.1415, "cadena")
tupla
```

Al ser una colección ordenada, podemos tener acceso a sus elementos 
con la sintaxis de índices:

```julia
tupla[2] # Esperamos que regrese 3.1415
```

Nótese que los índices empiezan en 1 y no en cero, tal como lo hace 
Matlab.

Sin embargo, si tratamos de modificar una tupla vamos a toparnos con 
un error:

```julia
tupla[2] = 1.692

entero, float, cadena = tupla
```

Otra bondad de las tuplas es que pueden ser "desempacadas" explícita o 
implícitamente. Es decir, podemos asignar una variable por cada 
elemento de la tupla, o utilizar la sintaxis de _splatting_, la cual 
vamos a ver más tarde.

Otra estructura de datos muy útil es el diccionario. Como el nombre 
sugiere, es una asignación entre llaves y valores. Se puede pensar 
como una función con dominio el conjunto de las llaves. Las llaves 
actúan como índices y nos permiten acceder a la información del 
diccionario, pero no necesariamente tienen que ser llaves numéricas.

```julia
# Creamos un diccionario con Dict
prof1 = Dict(
			"nombre" => "Miguel",
			"Departamento" => "Estadística",
			"c.u" => 123456,
			2 => [1, 2, 3]
		)
```

Como se ve en el ejemplo anterior, tanto las llaves como los valores 
pueden ser cualquier tipo de datos (por ahí está escondido un vector).  
Podemos obtener los valores como se esperaría.

```julia
print("Bienvenido al curso $prof1["nombre"]") # Algo va a salir mal...  Qué es?
```

Finalmente, antes de ver la estrella del curso, vamos a darle un 
vistazo rápido a los Data Frames, que son increíblemente útiles para 
análisis de datos. Son tan útiles que vamos a verlos a profundidad en 
otra sesión. En vez de hacer uno a mano, que tiene poca gracia, vamos 
a combinar el poder de los paquetes `DataFrames` y `CSV` para analizar 
los datos de casos positivos de COVID-19 en la CDMX (approx. 500 
observaciones).

```julia
using CSV, DataFrames

pruebas = CSV.read("../data/casos_positivos.csv", DataFrame)
pruebas
```

A partir de aquí se puede hacer todo tipo de filtrado, transformación, 
visualizaciones y todo lo que se pueda querer, pero no vale la pena 
apresurar el tema y se verá en otra sesión.
