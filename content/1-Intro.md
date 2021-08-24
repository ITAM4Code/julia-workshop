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

### Okay, es más rápido ¿qué tanto?

Además del artículo citado, un experimento propio:

Tarea: Llevar a cabo un solo paso del algoritmo Gauss-Jordan



## Ejemplitos de código

```julia
A = rand(3, 3)
print(A)
```
