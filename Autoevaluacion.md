# Bitácora de Aprendizaje — Unidad de Inferencia Estadística
### [Volver a la portada](index.md)
### Pruebas de Hipótesis Unimuestrales aplicadas a datos ENEMDU (Región de Loja)

## 1. Aprendizajes consolidados

Esta unidad me permitió pasar de entender las pruebas de hipótesis como fórmulas aisladas a aplicarlas sobre un problema real, con todas las decisiones metodológicas que eso implica antes de escribir una sola línea de código.

- **Criterio de selección entre Z y T.** Comprendí que la elección no depende únicamente del tamaño de muestra, sino, sobre todo, de si la varianza poblacional es conocida o no. Al desconocer la varianza poblacional del saldo neto de percepción económica en Loja, la prueba T de Student era la opción correcta, y el tamaño de muestra (n = 300) solo entró en juego para justificar la aproximación a la normalidad vía el Teorema del Límite Central, no para elegir la prueba en sí.
- **Transformación de variables ordinales en variables analizables.** El dataset original solo ofrecía respuestas categóricas (Mejor/Igual/Peor). Aprendí a aplicar la técnica de *saldo neto* (+1/0/-1), estándar en índices de confianza del consumidor, para poder calcular una media con sentido estadístico sobre una variable que originalmente no lo permitía.
- **Lectura correcta del valor-p.** Quizás el aprendizaje más importante fue afianzar que el valor-p es la probabilidad de observar los datos (o algo más extremo) *asumiendo que H₀ es cierta*, y no la probabilidad de que H₀ sea cierta. Esta distinción, que antes manejaba de forma mecánica, ahora la puedo explicar y defender con un ejemplo concreto.
- **Uso de librerías especializadas en lugar de fórmulas manuales.** Trabajar con `scipy.stats.ttest_1samp` y `statsmodels.stats.proportion.proportions_ztest` me mostró cómo estas funciones encapsulan el cálculo del estadístico y el valor-p, pero también la importancia de calcular manualmente piezas intermedias (media, desviación estándar con `ddof=1`, grados de libertad) para no perder el control conceptual de lo que la función hace "por dentro".

## 2. Dificultades algorítmicas superadas

- **Codificación geográfica del dataset.** El primer obstáculo fue puramente técnico: el campo `ciudad` del CSV no traía ceros a la izquierda, así que un filtro ingenuo por los dos primeros caracteres del código devolvía provincias incorrectas (o vacías, como me ocurrió al filtrar Loja y obtener 0 registros en el primer intento). Resolví esto normalizando el campo con `zfill(6)` antes de extraer el código de provincia, verificando el resultado contra la distribución esperada de provincias grandes como Pichincha y Guayas para confirmar que el mapeo era correcto.
- **Elegir la cola correcta del contraste.** Al principio planteé pruebas de dos colas por defecto, sin justificar la dirección. Tuve que releer el enunciado del problema regional (¿existe deterioro?, ¿existe mayoría pesimista?) para darme cuenta de que las hipótesis alternas debían ser direccionales (`alternative='less'` y `alternative='larger'`), y que usar una prueba de dos colas habría diluido la potencia estadística del contraste sin aportar nada al problema planteado.
- **Justificar la normalidad sin datos individualmente normales.** Inicialmente asumí que necesitaba probar normalidad sobre los datos crudos (categóricos), lo cual no tenía sentido. Superar esta confusión implicó distinguir entre la normalidad de los datos individuales y la normalidad de la *distribución muestral del estadístico*, apoyándome en el Teorema del Límite Central dado el tamaño de muestra (n = 300).
- **Interpretar magnitudes muy pequeñas del valor-p.** Al obtener un valor-p en notación científica (≈ 2.5 × 10⁻⁷), tuve que asegurarme de comunicar su magnitud sin caer en errores comunes como decir que "no hay probabilidad" de que ocurra, sino que la probabilidad es extremadamente baja pero no nula, y que eso es justamente lo que hace estadísticamente sólido el rechazo de H₀.

## 3. Reflexión final

Esta unidad reforzó que la inferencia estadística no es solo "correr una función y leer un número": exige justificar cada supuesto (varianza conocida o no, normalidad, dirección de la hipótesis) antes de interpretar el resultado, y exige poder explicar ese resultado en el lenguaje del problema real —en este caso, la percepción económica de los hogares de una región específica del Ecuador— y no solo en el lenguaje abstracto de la estadística.
