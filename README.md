# Sistema de Recomendación de Chistes: Del Colaborativo a la Factorización de Matrices
## 1. Contexto y Datasets
Este notebook se centra en la implementación y análisis de un sistema de recomendación para chistes. El proyecto utiliza el dataset Jester-17M, una colección masiva de calificaciones de chistes, que se divide en dos archivos principales:

jokes.csv: Un dataset pequeño con 150 chistes, cada uno identificado por un jokeId y el texto del chiste (jokeText).

ratings.csv: El dataset principal, con más de 1.7 millones de registros, que incluye el userId (el usuario que califica), el jokeId (el chiste calificado) y la calificación (rating), que oscila en un rango de -10 a 10.

## 2. Comparativa de Enfoques de Recomendación
Se llevó a cabo una comparativa entre dos enfoques de filtrado colaborativo: el basado en usuarios y la factorización de matrices.

### A. Filtrado Colaborativo Basado en Usuarios
Este método, intuitivo en su concepto, busca usuarios con gustos similares para recomendarles chistes que a ellos les gustaron. Sin embargo, en el contexto de un dataset tan grande como el de Jester-17M, el enfoque presentó un problema crítico: un consumo excesivo de memoria. La matriz de usuario-chiste, a pesar de ser dispersa, es de un tamaño tan considerable que la similitud de pares de usuarios se vuelve computacionalmente inmanejable. Este cuello de botella impidió el escalamiento del sistema y demostró que este método no era viable para un dataset de esta magnitud.

### B. Factorización de Matrices con SVD
Para superar el problema de memoria, se optó por una técnica de factorización de matrices, específicamente la descomposición de valores singulares (SVD). En lugar de trabajar con la matriz completa, SVD la descompone en factores latentes más pequeños que representan las preferencias y características subyacentes de usuarios y chistes. Este enfoque es considerablemente más eficiente, ya que el cálculo de la similitud se realiza sobre matrices de dimensiones mucho menores, resolviendo el problema de memoria.

## 3. Optimización del Modelo
Se realizaron varias optimizaciones clave para mejorar la precisión del modelo SVD:

**Función de Similitud**: Se sustituyó el coseno de similitud por la función pearson_baseline. A diferencia del coseno, pearson_baseline considera los sesgos inherentes de los usuarios (algunos califican consistentemente alto, otros bajo) y los sesgos de los ítems (algunos chistes son consistentemente populares). Al corregir estos sesgos, la similitud calculada entre usuarios se vuelve más precisa, lo que lleva a mejores predicciones.

**Filtrado de Datos**: Para garantizar que el modelo se entrene con información de alta calidad, se incluyeron solo a aquellos usuarios que habían realizado un mínimo de dos calificaciones. Esta simple medida ayuda a eliminar el ruido de usuarios con una interacción muy limitada, lo que fortalece el modelo y mejora su capacidad para generalizar. 

## 4. Resultado y Cómo Interpretar la Recomendación
El resultado final se visualiza en un gráfico de barras que resume la decisión del modelo de recomendación.

**Barras del Historial**: Estas barras, que pueden ser de diferentes colores, representan las calificaciones reales que el usuario ya le ha dado a algunos chistes. Estas calificaciones, tanto positivas como negativas, son la base sobre la que el modelo aprende las preferencias del usuario.

**Barra de Recomendación**: La barra final (generalmente con un color distintivo) representa el chiste recomendado por el sistema. La altura de esta barra no es una calificación real, sino la calificación predicha por el modelo para un chiste que el usuario aún no ha visto.

Es crucial entender que el sistema no va a recomendar el chiste con la calificación más alta del historial del usuario. El objetivo de un sistema de recomendación es encontrar el mejor chiste nuevo que el usuario no ha visto. La calificación de 7.21 que se muestra en el gráfico, por ejemplo, representa la predicción más alta entre todos los chistes desconocidos para ese usuario. El modelo usa la información de las calificaciones positivas y negativas del historial para estimar qué tan bien le irá a un chiste nuevo.

***

En conclusión, este notebook presenta un sistema de recomendación robusto y escalable, que fue diseñado específicamente para superar los desafíos del dataset Jester-17M. La elección de SVD y las optimizaciones aplicadas demuestran una solución efectiva que equilibra la precisión predictiva con la eficiencia computacional.