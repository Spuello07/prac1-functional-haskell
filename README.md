# Práctica 1 – Programación Funcional en Haskell  

**Curso:** Lenguajes y Paradigmas de Programación  
**Integrantes:**  
- Santiago José Puello Berrocal  
- Juan Daniel Vivas Granada  
- Sebastian Ibarra Prada  

---

##  Objetivos  
- Aplicar conceptos básicos de programación funcional.  
- Desarrollar funciones en Haskell para manejo de listas, aproximación numérica y procesamiento de señales.  
- Documentar los resultados de cada función implementada y los problemas/soluciones encontradas.  

---

##  Estructura del repositorio  
- `src/RemoverDatos.hs` — Elimina elementos fuera de un intervalo.  
- `src/OrdenDesc.hs` — Ordena una lista de `Float` en orden descendente.  
- `src/Exponencial.hs` — Aproxima `e^x` mediante la serie de Taylor.  
- `src/Coseno.hs` — Aproxima `cos(x)` mediante la serie de Taylor.  
- `src/Logaritmo.hs` — Aproxima `ln(1+x)` mediante la serie de Taylor (requiere |x| < 1).  
- `src/DCT.hs` — Implementa la Transformada Discreta del Coseno (DCT).  

---

##  Cómo usar las funciones y resultados de pruebas  

### 1. Remover datos fuera de un intervalo  
Archivo: `src/RemoverDatos.hs`  
Se eliminan los elementos que no estén dentro de un intervalo definido por dos valores.  

**Prueba:**  
Entrada: lista [1, 21, 5, -4], intervalo entre 0 y 5.  
Resultado: [1, 5]  

---

### 2. Orden Descendente  
Archivo: `src/OrdenDesc.hs`  
Ordena una lista de números flotantes en orden descendente.  

**Prueba:**  
Entrada: lista [1, 25, 5, -4].  
Resultado: [25, 5, 1, -4]  

---

### 3. Exponencial (serie de Taylor)  
Archivo: `src/Exponencial.hs`  
Se aproxima el valor de e^x utilizando la suma de la serie de Taylor.  

**Prueba:**  
Entrada: x = 2, N = 10 términos.  
Resultado aproximado: 7.388994  
Valor real (función `exp` en Haskell): 7.389056  

---

### 4. Coseno (serie de Taylor)  
Archivo: `src/Coseno.hs`  
Se aproxima el valor de cos(x) utilizando la suma de la serie de Taylor.  

**Prueba:**  
Entrada: x = 2, N = 10 términos.  
Resultado aproximado: -0.4161468  
Valor real (función `cos` en Haskell): -0.4161468  

---

### 5. Logaritmo Natural (serie de Taylor)  
Archivo: `src/Logaritmo.hs`  
Se aproxima el valor de ln(1+x) utilizando la serie de Taylor.  

**Prueba:**  
Entrada: x = 0.5, N = 10 términos.  
Resultado aproximado: 0.405465  
Valor real (función `log(1.5)` en Haskell): 0.405465  

---

### 6. Transformada Discreta del Coseno (DCT)  
Archivo: `src/DCT.hs`  
Se implementa la transformada discreta del coseno sobre una lista de valores.  

**Prueba:**  
Entrada: lista [1,2,3,4,5,6,7,8,9,10].  
Resultado (redondeado a 4 decimales):  
[17.3925, -9.0249, 0, -0.9667, 0, -0.3162, 0, -0.1279, 0, -0.0359]  

---

### Código 1: RemoverDatos  
Al inicio se intentó llamar la función con una tupla `(1,5)`, pero Haskell esperaba dos enteros separados, lo que causó un error de sintaxis.  
Luego aparecieron problemas en la definición de la función porque una versión usaba un argumento y otra tres, generando un error de consistencia.  
También se escribió `otherwise = [] : removerDatos c n xs`, lo que provocó un error de tipos ya que se intentaba agregar una lista dentro de otra de enteros.  

**Solución:** se corrigió la llamada pasando los parámetros separados, se uniformizó la definición con `removerDatos _ _ [] = []` y se ajustó el `otherwise` para que solo continuara la recursión.  
La versión final quedó funcional: cubre el caso base, usa guards para filtrar los números en el rango indicado y devuelve correctamente los elementos que cumplen la condición.  

---

### Código 2: OrdenDesc  
Al inicio la función `cambiarVal` se definió con una firma incorrecta, indicando que recibía solo un Float cuando en realidad necesitaba un número y una lista, lo que causó errores de compilación.  
Luego el caso base también estaba mal planteado, pues devolver `[]` hacía que el elemento nunca se insertara y se perdieran datos.  
Más adelante, en la línea `s >= x` se escribió `s : x : xs`, lo que solo colocaba el número al frente sin ordenar recursivamente el resto de la lista, dejando el resultado incompleto.  

**Solución:** ajustar el tipo, corregir el caso base para devolver `[s]` y modificar la lógica de inserción para que el algoritmo ordene toda la lista en orden descendente.  

---

### Código 3: Exponencial  
En el primer intento hubo varios errores de sintaxis y lógica: se escribió mal `fromIntegral`, se aplicó factorial de forma incorrecta, la función `suma` no tenía caso base y la recursividad estaba mal planteada porque se incrementaba `x` en lugar de iterar sobre `n`.  
En el segundo intento se corrigieron los errores tipográficos y de paréntesis, pero la función seguía recursiva de manera infinita ya que `n` nunca disminuía.  
En el tercer intento se agregó correctamente el caso base y se corrigieron detalles, pero la lógica aun confundía `x` con el índice de iteración.  

**Solución:** se ajustó la recursividad para que `n` se reduzca en cada paso, lo que permitió construir la suma finita de términos y aproximar `e^x` correctamente.  
El error central fue conceptual: confundir el papel de `x` con el de `n` en la expansión de Taylor.  

---

### Código 4: Coseno  
En el primer intento el programa tenía varios problemas básicos: se escribieron mal algunas palabras, la función factorial no estaba bien hecha, el `main` estaba mal declarado y la suma no tenía un punto de parada. Esto hacía que el código se quedara corriendo sin dar un resultado correcto.  

En el segundo intento se corrigieron algunos de estos detalles y se agregó un caso base, pero la forma de calcular el factorial seguía siendo incorrecta y la aproximación al coseno todavía salía mal.  

**Solución:** en la versión final se usó la fórmula correcta del coseno, con la suma alternada de potencias de `x` divididas entre el factorial de `2n`. También se arreglaron los errores de escritura y de conversión de números. Con esto el programa ya da una aproximación válida del coseno, mostrando que el error principal al inicio fue no entender bien la fórmula y no manejar bien la estructura de la función.  

---

### Código 5: Logaritmo Natural  
En este código los problemas principales fueron que no se usó bien el cambio de signo en cada término y que la fórmula del logaritmo no estaba escrita de forma correcta. También hacía falta un caso base claro para que la función supiera dónde detenerse, y la serie solo funciona en un rango limitado de valores, aunque en el ejemplo con 0.5 sí sirve.  

**Solución:** escribir bien la fórmula, usar el cambio de signo adecuado y agregar un caso base. Con estas correcciones el programa ya aproxima el logaritmo de manera correcta. El error central fue no entender bien la fórmula y confundir la manera de escribirla en el código.  

---

### Código 6: DCT  
En este código los problemas fueron algo difíciles de ver, ya que al trabajar solo con funciones primitivas la construcción se volvió más complicada. La raíz cuadrada estaba hecha para devolver cero cuando el número era menor o igual a cero, lo que afectaba algunos resultados. También se usó un pi aproximado en vez de uno más exacto, lo que reducía la precisión de los cálculos. Además, la forma de recorrer la lista para calcular cada valor quedaba poco práctica y algo enredada, sobre todo con listas grandes.  

**Solución:** mejorar la raíz cuadrada para que maneje mejor esos casos, usar un valor de pi más preciso y organizar de manera más clara el recorrido de la lista. Con eso la función ya entrega los resultados esperados. El problema principal estuvo en que, al usar solo lo más básico del lenguaje, fue fácil que estos errores pasaran al principio, además de que la fórmula era un poco confusa y rara de entender, entonces la lógica del código no fue muy clara.  

---

##  Conclusiones  
- Se aplicaron los conceptos de programación funcional en Haskell de forma práctica.  
- Se comprendió el uso de series de Taylor para aproximar funciones matemáticas.  
- La DCT mostró una aplicación real de programación funcional en el procesamiento de señales.  
- Se logró documentar y probar cada función, garantizando claridad y reproducibilidad en GitHub.  
