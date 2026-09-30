# Bitacora de tecnicas avanzadas

Laboratorio 07: Tecnicas Avanzadas de Prompting.

Herramienta de IA usada: (escribe aqui cual usaste: ChatGPT / Gemini / Claude / Copilot)

## Ejercicio 2: Zero-shot, one-shot y few-shot

Tarea: clasificar 5 comentarios como Positivo, Negativo o Neutral.
Etiquetas correctas: Positivo, Negativo, Neutral, Negativo, Positivo.

Prompt zero-shot:

```text
Clasifica estos comentarios de clientes:
1. Me encanto, llego rapido
2. No lo recomiendo
3. Es aceptable por el precio
4. Pesima atencion, no vuelvo
5. Excelente calidad, lo volveria a comprar
```

Prompt one-shot:

```text
Clasifica cada comentario como Positivo, Negativo o Neutral.
Ejemplo: "Me gusto mucho" -> Positivo
Comentarios:
1. Me encanto, llego rapido
2. No lo recomiendo
3. Es aceptable por el precio
4. Pesima atencion, no vuelvo
5. Excelente calidad, lo volveria a comprar
```

Prompt few-shot:

```text
Clasifica cada comentario. Responde solo con el formato de los ejemplos.

"Me gusto mucho" -> Positivo
"Que decepcion" -> Negativo
"Esta bien, nada especial" -> Neutral

"Me encanto, llego rapido" ->
"No lo recomiendo" ->
"Es aceptable por el precio" ->
"Pesima atencion, no vuelvo" ->
"Excelente calidad, lo volveria a comprar" ->
```

Resultados (completa segun lo que TE respondio la IA):

| Tipo | Aciertos (de 5) | Formato de la respuesta | Todas con el mismo formato (Si/No) |
|------|-----------------|-------------------------|------------------------------------|
| Zero-shot | 5 | Lista libre, con explicaciones o tabla | No |
| One-shot | 5 | Parecido al ejemplo, pero puede agregar texto extra | A veces |
| Few-shot | 5 | Una linea por comentario: "texto" -> etiqueta | Si |

<!-- VERIFICA: cambia estos valores si tu IA respondio distinto -->

## Ejercicio 3: Chain of Thought

Pedido directo:

```text
Un producto cuesta S/ 120. La tienda aplica un descuento del 25 % y luego suma el 18 % de IGV sobre el precio con descuento. Un cliente compra 3 unidades. Cuanto paga en total? Responde solo con el numero.
```

Pedido paso a paso:

```text
Un producto cuesta S/ 120. La tienda aplica un descuento del 25 % y luego suma el 18 % de IGV sobre el precio con descuento. Un cliente compra 3 unidades. Cuanto paga en total? Resuelvelo paso a paso: muestra cada calculo y comprueba el resultado antes de dar la respuesta final.
```

Verificacion con calculadora:

| Paso | Calculo | Resultado |
|------|---------|-----------|
| 1. Precio con descuento | 120 x 0,75 | 90 |
| 2. Precio con IGV | 90 x 1,18 | 106,20 |
| 3. Total por 3 unidades | 106,20 x 3 | 318,60 |

Resultados:

| Pedido | Respuesta de la IA | Muestra los pasos (Si/No) | Correcta (Si/No) |
|--------|--------------------|---------------------------|------------------|
| Directo | 318,60 (escribe lo que respondio tu IA) | No | Si |
| Paso a paso | 318,60 (escribe lo que respondio tu IA) | Si | Si |

<!-- VERIFICA: si tu IA se equivoco en el directo, anotalo aqui -->

Por que es util ver el razonamiento: aunque el numero final sea correcto, con los pasos puedo comprobar cada calculo contra mi calculadora. Si hubiera un error, sabria exactamente en que paso esta (por ejemplo, si aplico el IGV antes del descuento).

## Ejercicio 4: Role prompting

Prompt A (sin rol):

```text
Explica que es una variable en programacion.
```

Prompt B (rol docente):

```text
Actua como profesor de programacion que explica a estudiantes que nunca han programado. Explica que es una variable en programacion.
```

Prompt C (rol senior):

```text
Actua como desarrollador Java senior que explica a un companero de trabajo. Explica que es una variable en programacion.
```

| Version | Vocabulario (sencillo/tecnico) | Usa ejemplos o codigo | A quien le sirve mas |
|---------|-------------------------------|-----------------------|----------------------|
| A. Sin rol | Intermedio, general | Un ejemplo corto de codigo | A cualquiera, sin publico definido |
| B. Rol docente | Sencillo | Comparaciones de la vida diaria (caja con etiqueta) | A principiantes |
| C. Rol senior | Tecnico (tipo de dato, memoria, alcance) | Codigo Java | A programadores con experiencia |

<!-- VERIFICA: ajusta segun las respuestas que obtuviste -->

## Ejercicio 5: Descomposicion

Pedido de una sola vez:

```text
Crea un sistema de inventario para una tienda.
```

Pedido por pasos (todos en el mismo chat):

```text
Paso 1: Voy a crear un sistema de inventario para una tienda pequena en Java. Lista los 5 requisitos principales del sistema.
Paso 2: Con esos requisitos, disena las clases necesarias. Para cada clase indica sus atributos con su tipo de dato.
Paso 3: Escribe el codigo Java de la clase Producto con sus atributos, un constructor y los metodos get y set.
Paso 4: Revisa el codigo de la clase Producto y propone 3 mejoras concretas.
```

Registro por paso:

- Paso 1: la IA entrego una lista de 5 requisitos (escribe cuales).
- Paso 2: la IA entrego el diseno de clases con atributos y tipos (escribe cuales).
- Paso 3: la IA entrego el codigo de la clase Producto coherente con el diseno.
- Paso 4: la IA propuso 3 mejoras concretas (escribe cuales).

Comparacion: el pedido de una sola vez dio una respuesta general y poco revisable. El pedido por pasos me dejo revisar cada parte antes de seguir, y el codigo final fue coherente con los requisitos y las clases anteriores.

Compilacion (opcional): `javac Producto.java` termino sin errores y genero `Producto.class`. (Si no lo hiciste, borra esta linea.)

## Ejercicio 6: Prompt estructurado y autocritica

Prompt basico:

```text
Dame casos de prueba para un login.
```

Prompt estructurado y mensaje de autocritica:

```text
<rol>Actua como analista de pruebas de software.</rol>
<contexto>Login web con correo y contrasena. La cuenta se bloquea despues de 3 intentos fallidos.</contexto>
<tarea>Piensa paso a paso que puede fallar y escribe 6 casos de prueba.</tarea>
<formato>Tabla con las columnas: ID, escenario, datos de entrada, resultado esperado.</formato>
```

```text
Revisa tu tabla: faltan casos limite como campos vacios, correo sin @ o contrasena con espacios? Agrega los que falten e indica cuales agregaste.
```

Evaluacion de la tabla final:

| Que revisar | Cumple (Si / No) |
|-------------|------------------|
| Tiene las 4 columnas pedidas? | Si |
| Incluye el bloqueo despues de 3 intentos? | Si |
| Incluye casos con campos vacios? | Si |
| Indica que casos agrego en la autocritica? | Si |
| Hay algun caso repetido o que no tenga sentido? | No (anota aqui si encontraste alguno) |

<!-- VERIFICA: responde segun la tabla real que te dio tu IA -->
