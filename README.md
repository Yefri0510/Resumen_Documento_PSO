# Resumen del Artículo — *Particle Swarm Optimisation: A Historical Review — Up to the Current Developments* (Freitas, Lopes & Morgado-Dias, 2020)

## 1) Idea general y objetivo del artículo

El trabajo es una revisión sistemática de la evolución del **Particle Swarm Optimisation (PSO)** desde su formulación original (1995) hasta desarrollos recientes. Describe la formulación básica, problemas detectados (convergencia, estancamiento, pérdida de diversidad), modificaciones principales, variantes (niching, multi-objetivo, paralelo, híbridos) y aplicaciones prácticas. El objetivo es facilitar la identificación del **PSO** más adecuado según el tipo de problema.

## 2) Fundamentos del PSO (modelo original)

* **Concepto**: población de partículas que "vuelan" en el espacio de búsqueda; cada partícula recuerda su mejor posición personal (`p_i`) y conoce (o recibe) una mejor posición global o de vecindario (`g`).
* **Ecuaciones clave (forma estándar)**:

  * Actualización de velocidad (forma original):
    `v_{i,t+1} = v_{i,t} + φ1 * R1 * (p_{i,t} - x_{i,t}) + φ2 * R2 * (g_t - x_{i,t})`
  * Posición:
    `x_{i,t+1} = x_{i,t} + v_{i,t+1}`
    (φ1 y φ2 son coeficientes cognitivos y sociales; R1,R2 vectores aleatorios.)
    
## 3) Problemas clásicos detectados

* **Premature convergence**: el enjambre converge a óptimos locales por pérdida de diversidad.
* **Stagnation**: cuando `x_i = p_i = g` y la velocidad se hace nula, el movimiento puede detenerse.
* **Explosión de velocidad / inestabilidad**: sin límites apropiados, `v` puede crecer sin control.

## 4) Modificaciones importantes para mejorar convergencia

### 4.1 Inertia weight (ω)

* Introducido por Shi & Eberhart (1998): añade un término `ω * v_{i,t}` para controlar exploración vs. explotación. Con valores altos favorece exploración inicial; con valores pequeños favorece explotación final. Se suelen usar estrategias de ω decreciente (línea o no lineal).

### 4.2 Constriction factor (K)

* Propuesto por Clerc (1999): factor multiplicativo `K` que asegura estabilidad matemática y controla el paso efectivo (frecuente valor usado: φ ≈ 4.1 → K ≈ 0.7298). A menudo se combina con límites en `v_max`.

### 4.3 Combinaciones

* Se han probado combinaciones ω + K, adaptativas o dependientes del estado del enjambre para mejorar robustez y velocidad de convergencia.

## 5) Esquemas de vecindad / topologías

* **gbest (fully connected)**: todos comparten info → rápida convergencia, mayor riesgo de estancamiento.

* **lbest (local neighbourhoods, ring, von Neumann, star, wheel, etc.)**: información restringida → mejor rendimiento en problemas multimodales. La elección de topología es *dependiente del problema*.

* **Vecindades dinámicas / espaciales**: vecindad basada en distancia, radios crecientes, clustering social (stereotyping) — adaptaciones para balancear exploración/explotación.

## 6) Técnicas para multimodalidad y niching

* **Stretched PSO (STPSO)**: aplicar transformaciones (stretching/deflection) sobre la función objetivo para repeler el enjambre de óptimos ya encontrados y descubrir otros. Riesgo: puede introducir mínimos artificiales.
* **nbest / FDR (Fitness-Distance-Ratio)**: seleccionar influencia por razón aptitud-distancia para guiar dimensiones individuales; ayuda a localizar múltiples óptimos.
* **Subpoblaciones / Multi-Swarm / Niching / Speciation**: dividir el enjambre en sub-swarms o especies que exploran nichos distintos; útil para encontrar múltiples soluciones simultáneamente. Ej.: NichePSO, CPSO y variantes.

## 7) Otras variantes destacadas

* **Cooperative PSO (CPSO)**: dividir el espacio por dimensiones (subpoblaciones) y cooperar con vectores contexto. Útil en alta dimensión, aunque la evaluación de la función requiere arreglos d-dimensionales.
* **Adaptive PSO (APSO)**: define estados evolutivos (exploración/explotación/convergencia/jump-out) y adapta parámetros según métricas del enjambre (p. ej. factor evolutivo `e_f`). Mejora velocidad y precisión.
* **Fully Informed PSO (FIPS)**: en lugar de usar solo el mejor vecino, usar la información (contribución) de todos los vecinos — a veces ponderada por fitness o distancia. Mejora en muchos casos.

## 8) Problemas con restricciones y multi-objetivo

* **Manejo de restricciones**: enfoques por penalización dinámica, preservación de soluciones factibles (FSM), fly-back o mecanismos que reajustan la posición a factible (p. ej. ajuste por α). La elección del método depende del problema.
* **Multi-objective PSO (MOPSO)**: uso de un **repositorio externo** para almacenar soluciones no dominadas (Pareto front), selección de guías desde ese repositorio, y mutaciones para preservar diversidad. MOPSO ha mostrado buenos resultados frente a otros algoritmos multi-objetivo.

## 9) Implementaciones paralelas / escalabilidad

* PSO se presta bien al paralelismo (sub-swarms, master–slave, fine/coarse grained, MapReduce, GPUs/CUDA). Las implementaciones asíncronas y basadas en GPU muestran mejoras grandes en tiempo de ejecución; la arquitectura de comunicación (síncrona vs asíncrona) y la estrategia de migración de partículas impactan la eficiencia y resultado final.

## 10) Hibridaciones y conexiones con otros algoritmos

* **Híbridos**: PSO + GA (selección, crossover, mutación), PSO + Differential Evolution (DEPSO), PSO + Simulated Annealing, etc. La hibridación busca explotar las ventajas complementarias (p. ej. operadores genéticos para diversificar, PSO para búsqueda continua). También se aplicó ampliamente a entrenamiento de redes neuronales (PSO para optimizar pesos/arquitectura).

## 11) Aplicaciones prácticas

* PSO y variantes se aplican a optimización continua, combinatoria (versiones binarias/discretas), planificación de rutas, diseño de filtros, asignación de tareas, clasificación de imágenes, optimización multi-objetivo, ajuste de redes neuronales, y más. El artículo incluye amplia bibliografía y ejemplos de aplicaciones.

## 12) Conclusiones del artículo

* PSO es una de las técnicas de **swarm intelligence** más populares por su simplicidad y generalidad.
* Presenta problemas clásicos (convergencia prematura, estancamiento, degradación con dimensionalidad) que motivaron muchas variantes y estrategias (topologías, parámetros adaptativos, niching, hibridación y paralelización).
* No existe una única "mejor" versión: la elección de variante/topología/estrategia es fuertemente dependiente del **tipo de problema** (unimodal vs multimodal, restricciones, multi-objetivo, dimensión, presupuesto computacional).

## 13) Recomendaciones prácticas

* Para problemas **unimodales** o con alta confidencia en la convexidad: gbest o constriction factor con ω controlado suele funcionar bien.
* Para problemas **multimodales**: usar lbest, niching / subpoblaciones, FDR o STPSO para localizar múltiples óptimos y evitar sesgo por un único `g`.
* Para **problemas con muchas restricciones**: usar métodos de penalización dinámicos o preservación de soluciones factibles (FSM) según el dominio; probar también fly-back o ajuste por α si las fronteras son importantes.
* Para **alta dimensión** o gran coste de evaluación: considerar CPSO, paralelización (GPU/MapReduce) o enfoques híbridos que reduzcan evaluación de fitness.
