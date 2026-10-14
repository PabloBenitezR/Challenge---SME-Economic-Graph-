# SME Economic Graph

Propuesta para el reto de AI Engineering: cómo construir un Economic Graph de PyMEs y cómo demostrar que genera valor incremental.

Autor: Pablo Benítez.

## Qué es

Una página web de un solo archivo (`index.html`) que explica la propuesta de punta a punta: qué problema se ataca primero, cómo se arma el grafo, qué modelo se usa, cómo se prueba el valor y cómo se ve todo junto en una Control Tower.

Los datos son simulados. Ninguna cifra es real; son coherentes entre sí para poder seguir la cadena completa del grafo al dinero.

## Cómo verla

Abre `index.html` en el navegador. No hay build, no hay dependencias que instalar y no hace falta servidor.

Las tipografías (Caveat y Source Sans 3) se cargan desde Google Fonts. Sin internet la página funciona igual, solo con tipografías del sistema.

## Qué contiene

La página tiene seis secciones, una por cada entregable del reto:

1. Por dónde empiezo: el caso de uso inicial, vender mejor a PyMEs conectadas con clientes, y por qué se escoge ese.
2. Datos y grafo: nodos (PyMEs, industrias, zonas, servicios), relaciones, y para cada relación su fuente, su fecha y su nivel de confianza. Se separa lo observado de lo inferido.
3. Enfoque de IA/ML: baseline sin grafo, features de grafo, embeddings y GNN, con una tabla de ablación y la decisión de qué se queda y qué no.
4. Arquitectura: primero tablas en el lakehouse; una base de grafos solo si las consultas lo piden.
5. Prueba de valor: piloto de 90 días, aleatorizado por comunidad del grafo, con un grupo de control que sigue el proceso actual.
6. Control Tower: la cadena completa en un solo tablero, con una métrica de conexiones útiles entre PyMEs.

## Cómo se calculan los números

Todo se calcula en el navegador con JavaScript, al final de `index.html`.

- Hay cinco industrias en el arreglo `SEGS`. Cada una trae su embudo (universo, detectadas, calificadas, activadas), las conversiones de control y tratamiento, la salud del grafo (cobertura, resolución, observado, frescura), Precision@K sin y con grafo, y las conexiones útiles.
- Las constantes económicas son `ARPU`, `MARGIN`, `LIFE`, `PROGRAM` y `CONTACT`. Con ellas, el valor de vida de un cliente sale de `ARPU x MARGIN x LIFE`.
- Las activaciones se reparten en dos brazos iguales. La diferencia de conversión se prueba con una z de proporciones agrupada, con intervalo de confianza al 95%.
- ROI = (conversiones incrementales x valor de vida - costo asignado) / costo asignado. El payback se calcula contra el margen mensual.

Si quieres probar otro escenario, cambia los valores de `SEGS` o las constantes y recarga la página. El resto se recalcula solo.

## Resultados simulados

| Indicador | Valor |
| --- | --- |
| Conversión control a tratamiento | 17.5% a 22.5% (+5.0 pp, IC 95% [3.3, 6.7]) |
| Conversiones incrementales | 200 |
| Margen incremental | 3.31 M |
| Neto después del programa | 1.91 M |
| ROI | 136% |
| Payback | 12.7 meses |
| Precision@K offline | 14.0% a 20.5% (1.46x) |
| Conexiones útiles por 1,000 contactadas | 5.8 a 14.3 |

Ablación de modelos:

| Modelo | AUC | Precision@K | Costo relativo |
| --- | --- | --- | --- |
| A. Baseline sin grafo | 0.71 | 14.0% | 1x |
| B. Más features de grafo | 0.77 | 19.9% | 1.5x |
| C. Más embeddings | 0.78 | 20.5% | 2x |
| D. GNN | 0.78 | 20.3% | 6x |

Decisión: se queda C. La GNN no mejora y cuesta el triple.

## Límites

- Los datos son inventados. La página sirve para mostrar el razonamiento y la forma de medir, no para tomar decisiones de negocio.
- Crédito y riesgo quedan fuera de esta propuesta hasta tener controles específicos.

## Archivos

- `index.html`: la propuesta completa, con estilos y lógica incluidos.
- `README.md`: este documento.
