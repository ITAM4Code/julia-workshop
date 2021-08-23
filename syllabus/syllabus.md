# Temario tentativo para curso de Julia

# Temas a cubrir

- Básicos: Tipos de datos, variables, estructuras de repetición y 
  control
- Funciones
- Álgebra Lineal
- Operadores
- Plotting
- DataFrames

Tomando como base cursos y guías ofrecidas como learning resources 
oficiales & de otros lados:
- JuliaAcademy - [Introduction to Julia](https://github.com/JuliaAcademy/Introduction-to-Julia)
- JuliaAcademy - [Julia for Data Science](https://github.com/JuliaAcademy/DataScience)
- Jorge Rotter - [Python para ciencias aplicadas](https://github.com/jarotter/python_ciencias_aplicadas)

## Introducción
Introducción corta pensada principalmente para personas que ya saben 
programar. Quizás enfoque en mostrar las diferencias entre Matlab, 
Python y Julia [(ejemplo)](https://cheatsheets.quantecon.org/).

Un buen punto de partida: El curso de [python 
científico](https://github.com/jarotter/python_ciencias_aplicadas/blob/master/notebooks/1-intro.md) 
impartido por Jorge Rotter

- Intro
	- Filosofía Open Source y porqué Julia

- Introducción a tipos de datos, y manejo de variables
	- Presentar el árbol de data types de Julia
	- Presentar tuple y dictionary como estructuras de datos que 
	  pueden ser nuevas para las personas que vienen de Matlab
	- Listas de comprensión
	- Breve mención de que unicode es válido para nombres (se pueden 
	  usar comandos de LaTeX para nombrar cosas)

- Estructuras de control y repetición
	- For tradicional y estilo Matlab iterando sobre vectores
	- Breve ejemplo de ifs y otros

- Funciones
	- Definición tradicional y forma "inline"
	- Breve comentario sobre la diferencia entre función y método
	- Funciones anónimas
	- Multiple Dispatch

- Introducción a constructos particulares de Julia y _type stability_.
	- Breve comentario sobre macros

## Segunda sesión: Bases del cómputo científico

- Álgebra Lineal
Buen punto de partida: Curso de [Julia 
Academy](https://github.com/JuliaAcademy/DataScience/blob/main/02.%20Linear%20Algebra.ipynb) 
impartido Dr. Huda Nassar.
	- Remarcar la sintaxis natural (v.s Numpy por ejemplo)
	- Tipos de matrices, factorizaciones, vectores, etc...
	- Broadcasting (e.g `exmp` en Matlab y cómo se hace en Julia)

- Graphing
	- Usando Plots.jl
	- Mencionar la posibilidad de usar múltiples backends (e.g Tikz)
	- Gráficas básicas
	- Breve mención de recipes.
	- Mención de otros paquetes especializados en gráficas 
	  estadísticas

## Siguientes sesiones: Domain specific Julia
- Ecuaciones Diferenciales
- Optimización
- Estadística
- Economía
- SciML?
