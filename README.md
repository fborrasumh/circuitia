# CircuitIA

Generador de **problemas de análisis de circuitos** en una sola página web: cada problema tiene su esquema y su solución **calculada** (no inventada), cada estudiante recibe valores distintos y la corrección explica sus errores típicos.

**Usar la app:** https://fborrasumh.github.io/circuitia/

## Origen de la idea

Nace de una observación de **Susana Fernández de Ávila** (Fundamentos de análisis de circuitos, UMH): en asignaturas de resolución de problemas la IA generativa solo es útil si se combina con el dibujo del circuito y la explicación paso a paso. CircuitIA parte de ese principio y pone el cálculo —no la IA— como autoridad.

## Qué hace

- **24 problemas** de un curso introductorio, en español e inglés: Ohm y Kirchhoff, divisores, serie-paralelo, puente de Wheatstone, nudos y mallas, fuentes dependientes, superposición, Thévenin y Norton, máxima transferencia, operacionales (inversor y no inversor), transitorios RC, RL y RLC, y régimen sinusoidal (impedancias, potencias, factor de potencia, resonancia, filtro RC).
- **Esquema dibujado por código** (símbolos europeos IEC o americanos ANSI), con descripción en texto para lectores de pantalla.
- **Práctica**: respuesta con prefijo y unidad, pistas en tres niveles, solución explicada con la comprobación de KCL y del balance de potencias, y repaso espaciado.
- **Diagnóstico de errores**: si la respuesta es incorrecta, la app vuelve a calcular el mismo apartado con el error típico (olvidar una resistencia, apagar una fuente de tensión abriéndola, confundir pico y eficaz, perder el signo, el prefijo…) y, si coincide, lo explica.
- **Exámenes individuales**: lista de clase + semilla → un examen distinto por estudiante, **reproducible**; impresión/PDF de enunciados y de claves, CSV de soluciones, **XML de Moodle** (una categoría por problema con una variante por estudiante) y configuración reproducible con huella.
- **Editor**: dibuja el circuito sobre la rejilla, define parámetros y apartados, y valida (el solver resuelve 60 variantes y comprueba KCL y Tellegen) antes de guardar.
- **IA opcional** (tu clave de OpenAI, Gemini o Claude): un tutor que guía sin dar la solución —el código elimina de sus respuestas cualquier cifra que coincida con un resultado— y un asistente que propone una plantilla desde un enunciado, que el solver valida y el profesorado revisa.

## Cómo se garantiza que los números son correctos

Los resultados los calcula un **solver propio** (análisis nodal modificado en complejo; continua, alterna y transitorios con regla trapezoidal y extrapolación). Se comprueba en varias capas:

| Comprobación | Resultado |
|---|---|
| Soluciones analíticas (divisores, Thévenin, operacionales, fuentes dependientes, RLC en alterna, transitorios RC, RL, RLC) | 30 comprobaciones |
| Circuitos aleatorios: KCL, teorema de Tellegen y superposición | 300 circuitos, error ~10⁻¹⁵ |
| Biblioteca: cada plantilla × 150 variantes frente a su **fórmula analítica independiente** | 24 de 24 |
| **Contraste con ngspice 42** (netlist exportada de variantes reales) | 600 variantes, 1.675 tensiones de nudo, 0 fallos |
| Interfaz, exámenes, editor e IA (Playwright) | 72 comprobaciones |

Diferencia con ngspice: ~10⁻¹⁵ en continua y alterna, ~10⁻⁸ en operacionales (ngspice usa ganancia finita) y ~10⁻⁷ en transitorios. Reproducir: `tests/run_all.sh` (requiere Node, Python con Playwright y ngspice).

## Formato abierto de plantillas

Una plantilla es un JSON: `id`, `tema`, `nivel`, `titulo`/`enun`/`sol` (es, en), `params` (conjuntos de valores: lista `v`, serie `E6/E12/E24` con `min`/`max`, o `expr` derivada), `dsl` (el esquema), `pedir` (apartados: medida del solver `V`, `I`, `P`, `Req`, `Vth`, `In`, `tau`, `tv`… o `expr`), `errores` (circuitos mal planteados para el diagnóstico) y, opcionalmente, `ac`, `trans`, `restr`, `calc`.

Lenguaje del esquema (una línea por elemento, rejilla de enteros): `R R1 0,0 4,0 {R1}` · `V Vs 0,0 0,4 {Vs}` · `W 4,0 4,4` · `GND 2,4` · `LBL 4,0 A` · `OA U1 12,0` …

## Privacidad

Ejercicios, respuestas y progreso se guardan solo en el navegador. Si usas la IA, antes del primer envío ves exactamente qué sale (el enunciado, la descripción del circuito y tu pregunta; nunca la solución ni tu nombre).

## Límites

- 24 problemas de un curso introductorio; el temario concreto de cada universidad debe contrastarse.
- Elementos ideales; en alterna, un único generador sinusoidal; sin semiconductores, acoplamientos magnéticos ni trifásica.
- El XML de Moodle se ha comprobado como XML bien formado con imágenes válidas, pero no se ha importado en todas las versiones de Moodle.
- La IA puede equivocarse (el asistente de plantillas puede dibujar un circuito distinto del deseado): el profesorado debe revisar el esquema.
- La calificación es siempre responsabilidad del profesorado.

## Autoría

Fernando Borrás Rocher y Susana Fernández de Ávila, ambos de la Universidad Miguel Hernández de Elche.

ORCID: Fernando Borrás Rocher [0000-0002-5519-4573](https://orcid.org/0000-0002-5519-4573) · Susana Fernández de Ávila [0009-0003-3155-6458](https://orcid.org/0009-0003-3155-6458)

## Cómo citar

Borrás Rocher, F. y Fernández de Ávila, S. (2026). *CircuitIA* (v1.0.0) [Software]. (DOI en trámite)

## Licencia

MIT. Véase [LICENSE](LICENSE).
