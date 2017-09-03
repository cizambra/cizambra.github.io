---
layout: post
title: La paradoja del cumpleaños
---
![Birthday party]({{ site.url }}/images/childrens_holiday.jpg)

Hace un tiempo necesitaba generar IDs únicos para uno de mis proyectos. En realidad la tarea era
bastante sencilla, escoger cómo crear dichos IDs únicos sólo basandome en una restricción; cada
ID debía estar compuesto por 10 caracteres alfanuméricos, ni más ni menos (bueno, en verdad son
dos restricciones en una).

Buscando por algún Java package que cumpliera mi objetivo, encontré 
[una pregunta](https://stackoverflow.com/questions/41107/how-to-generate-a-random-alpha-numeric-string) 
en StackOverflow (sí, yo también lo uso). Y leyendo en detalle las respuestas, encontré lo siguiente:

> Beware the birthday paradox.

*Cuidado con la paradoja del cumpleaños*. En lo personal nunca había escuchado de ella, y me pareció
interesante así que recurrí [a Wikipedia](https://es.wikipedia.org/wiki/Paradoja_del_cumplea%C3%B1os) para informarme
más al respecto. El artículo lo explica así:

> El problema del cumpleaños, también llamado paradoja del cumpleaños, establece que de un conjunto 
de 23 personas, hay una probabilidad del 50,7% de que al menos dos personas de ellas cumplan años el
 mismo día. Para 57 o más personas la probabilidad es mayor del 99%. En sentido estricto esto no es 
 una paradoja ya que no es una contradicción lógica; no es una paradoja pero es una verdad 
 matemática que contradice la común intuición.

Luego de haber leído (y aprendido) sobre esta paradoja, me entró la duda ¿cuán probable es que al 
momento de generar un ID único de 10 caracteres ya exista otro creado con el mismo valor?, 
¿cuán únicos son realmente estos IDs?.

Saqué lápiz y papel y me puse a calcular (en verdad abrí Excel y puse los valores ahi).

De acuerdo a lo expuesto en el artículo de Wikipedia, *la probabilidad que cualquiera en una 
habitación de n personas (excluido Ud.) tengan el mismo día de cumpleaños que usted está dada por*

$$ 1 - \left( \frac{364}{365} \right)^n $$

Aplicando el mismo concepto a nuestro contexto (10 caracteres alfanuméricos),  podríamos calcular
la probabilidad de que cualquier ID en un set de tamaño n (excluido el ID que se está generando, 
llamémosle ID candidato), tenga la misma combinación de caracteres alfanuméricos que el ID candidato.

$$ 1 - \left( \frac{(26 + 26 + 10)^{10} - 1}{(26 + 26 + 10)^{10}} \right)^n =
1 -  \left( \frac{(62)^{10} - 1}{(62)^{10}} \right)^n$$
$$= 1 -  \left( \frac{(8.3929937\mathrm{e}+17) - 1}{8.3929937\mathrm{e}+17} \right)^n$$

$$= 1 - (0.999999999999999998808530018409582986761223851907918274095)^n$$

Obviamente a medida que n se incrementa, las probabilidades de colisión aumentan; en este caso
el aumento es casi imperceptible, y sólo empieza a hacerse visible cuando el orden de magnitud de n 
es $10^{17}$.

Como conclusión, van a tomar años (o una muy malísima suerte) para que haya una colisión entre dos
IDs de 10 caracteres alfanuméricos, por lo que los IDs generados podrían considerarse únicos.
