---
layout: post
title: Complejidad Ciclomática
---
El otro día tuve que lidiar con Ruby — no había trabajado con Ruby hace mucho...como un año, o algo así. Todo salió bien, por suerte no estaba tan oxidado como pensaba, o al menos eso era lo que creía. Cuando vino el momento del deployment, me encontré con esta joya emitida por [Rubocop](https://github.com/rubocop-hq/rubocop):
```
Cyclomatic complexity for on is too high. [8/6]
```
A decir verdad no recordaba haber escuchado ese concepto "Cyclomatic complexity", así que me apoyé en mi viejo
amigo Wikipedia para tener una idea de más o menos a que se refería el término.

> La Complejidad Ciclomática (en inglés, Cyclomatic Complexity) es una métrica del software en ingeniería del software que proporciona una medición cuantitativa de la complejidad lógica de un programa. Es una de las métricas de software de mayor aceptación, ya que ha sido concebida para ser independiente del lenguaje.

Interesante, la pregunta que te podrás estar haciendo es *¿cómo medir la complejidad lógica de un programa?*

Resulta que el mismo artículo tiene la respuesta:

> El nombre “Complejidad Ciclomática” puede resultar engañoso para un programador ya que la idea de esta métrica no es contar los bucles (for, while, do…) en el código de un programa sino en el resultado de contar el número de ciclos diferentes que se siguen en un fragmento de código de un programa habiendo creado una rama imaginaria desde el nodo de salida al nodo de entrada del diagrama de flujo correspondiente a este fragmento de código. Un nombre más adecuado podría ser Complejidad condicional ya que el cálculo de esta métrica se ajusta más al hecho de buscar condiciones que contar ejecuciones de predicados dentro de bucles.

En resúmen,**complejidad ciclomática** es cuántas ramas posibles producto de las diversas condiciones existen para una secuencia lógica dentro de un programa (o una función). Básicamente el error que tenía era producto de tener demasiados escenarios posibles para mi función.

**¿La solución?** simplificar mi código, Ruby me estaba diciendo que mi función estaba haciendo demasiadas cosas. **¿Cómo simplifiqué mi código?** extrayendo la lógica de mi función en subfunciones.

Aquí un ejemplo de lo que hice

```
def my_function expected_hash
  return if expected_hash.blank?
  return if expected_hash[:key1].is_a? MyModule::FirstClass

  if expected_hash[:key2] == ExpectedConstKey
    if expected_hash[:key3].is_a? String
      puts "My expected output"
    else
      puts "This is an example of another output"
    end
  else
    return
  end
end
```

```
def my_function expected_hash
  return if expected_hash.blank?
  return if expected_hash[:key1].is_a? MyModule::FirstClass

  puts_or_return(expected_hash)
end

def puts_or_return expected_hash  
  if expected_hash[:key2] == ExpectedConstKey
    if expected_hash[:key3].is_a? String
      puts "My expected output"
    else
      puts "This is an example of another output"
    end
  else
    return
  end
end
```