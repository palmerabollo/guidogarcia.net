---
author: admin
date: 2011-08-15 18:31:34+00:00
draft: false
title: 'BigDecimal : detalles que hacen que Java de asco'
type: post
url: /blog/2011/08/15/bigdecimal-detalle-que-hacen-que-java-de-asco/
categories:
- Software
tags:
- java
---

Sirva como ejemplo el siguiente fragmento de código, en el que se utiliza la clase `BigDecimal` que ofrece Java para trabajar con precisión arbitraria en operaciones con números en coma flotante:


{{< highlight java >}}
BigDecimal num = new BigDecimal(2);
num = num.add(new BigDecimal(3.2));
System.out.println(num.toString());
System.out.println(num.doubleValue());
{{< / highlight >}}

Para empezar, esta es la salida del programa, que no necesita comentarios:


    5.20000000000000017763568394002504646778106689453125
    5.2


En Java, salvo el caso del operador "+" para la concatenación de cadenas, tampoco existe sobrecarga de operadores, por lo que una simple operación como sumar dos números se realiza de forma poco natural:

{{< highlight java >}}
BigDecimal num1 = new BigDecimal(2);
BigDecimal num2 = new BigDecimal(2);
System.out.println(num1 + num2); // error de compilación
System.out.println(num1.add(num2)); // contra natura
{{< / highlight >}}

Y el operador "==" seguirá comparando objetos en lugar de valores:

{{< highlight java >}}
System.out.println(num1 == num2); // output = false
{{< / highlight >}}


Para rematar la faena, el método `BigDecimal#equals` considera diferentes números con el mismo valor y distinta escala (por ejemplo 2.0 y 2.00 serían diferentes), obligando a utilizar el método [`compareTo`](http://download.oracle.com/javase/1,5.0/docs/api/java/math/BigDecimal.html#compareTo(java.math.BigDecimal)) en este caso.

Nota: Desde aquí hago un llamamiento para que en Java 8 también podamos comparar cadenas con el operador "==" en lugar del engorroso `equals`. Piedad.

Como colofón, no es posible utilizar un `BigDecimal` como clave en un `SortedMap` o como elemento de un `SortedSet`, ya que el orden natural no es consistente con equals.

Me gustaría conocer una comparativa con C#, si alguien se aventura que deje un comentario.
