# ESPECTROS · LYAPUNOV para VCV Rack

**Ph.D. Mauricio Vásquez Arias**
Profesor titular – Investigador asociado · Escuela de Artes y Humanidades, Universidad EAFIT

LYAPUNOV suelta dos péndulos dobles casi idénticos. Lo que suena, lo que se ve y lo que sale como voltaje es cómo se separan:

- los gemelos son el estéreo;
- cada uno compone sus propias notas en una afinación no temperada;
- el instante en que dejan de parecerse, el **horizonte de predictibilidad**, es un evento que se puede patchear.

## Qué lo distingue

Otros módulos de VCV Rack ya usan el péndulo doble: Loco CHAOS, Vult Caudal y SignalFunctionSet Gravity. Lo usan como fuente de voltajes caóticos. LYAPUNOV se construye sobre otra pregunta: **¿cuándo deja de ser predecible un sistema determinista?** De ahí sale lo que lo hace propio:

- **Dos gemelos, no uno.** A y B se integran a la vez, separados por δ0.
- **La separación se mide.** δ, el exponente λ y el horizonte salen como voltaje.
- **Estéreo que nace del caos.** A a la izquierda, B a la derecha: empiezan en mono y se abren solos.
- **Dos compositores.** Cada gemelo produce sus notas en 38 afinaciones no occidentales, no temperadas o espectrales, más archivos Scala. Coinciden hasta el horizonte y luego se separan.
- **Escuchar solo el caos.** En A−B no suena nada mientras los gemelos coinciden.
- **Instrumento de audio.** Audificación o FM a frecuencia audible, con limitador que nunca satura.
- **Pantalla que explica.** Los dos péndulos con sus estelas, y log δ desde SOLTAR: la pendiente de esa recta es λ.

## Principios de diseño aplicados

- **Sonoridad, matemática, expresividad.**
  - La fórmula no es una metáfora: es la ecuación de Euler-Lagrange, integrada con Runge-Kutta 4, con dos péndulos a la vez.
  - Lo que se mide (δ, λ, horizonte) sale como voltaje.
  - Los gestos (soltar, perturbar, sincronizar, golpear, congelar) son el instrumento.
- **Microtonalidad y espectralismo como principios compositivos.**
  - La afinación no es un filtro final: las notas de los vuelcos y las portadoras de FM nacen en los grados de la escala.
  - La perilla ATRACCIÓN va de las alturas que da la física (0 %) a la escala fija (100 %).
  - BASE es la raíz, y V/OCT transpone la escala entera.
- **Comportamiento generativo.** El sistema toca solo. El intérprete no dicta notas: perturba, empuja y detiene un sistema que compone.
- **Ver lo que se escucha.** La pantalla es una lección de física: dos trayectorias que se superponen (se ven blancas) hasta que se abren en cian y magenta. Al lado, la recta de log δ cuya pendiente es el exponente de Lyapunov, y el horizonte marcado en ámbar.
- **Conservar la dinámica sin saturar.** Cada salida pasa por un paso alto, un compresor suave y un limitador con anticipación, como en la suite ESPECTROS.
- **Ligero.** Usa entre 0,4 % y 0,9 % de un núcleo. El integrador se subdivide solo cuando la frecuencia sube, para no perder la física.

## Instalación

1. Instala o actualiza **VCV Rack 2.6** o posterior (gratuito, en vcvrack.com) y ábrelo una vez.
2. Abre la biblioteca ESPECTROS en **Chrome**. El enlace está en «About», en la columna derecha de este repositorio.
3. Pulsa **INSTALAR EN VCV RACK**. La primera vez, elige la carpeta `Rack2`; la página explica dónde está.
4. Abre VCV Rack y busca **LYAPUNOV** (marca ESPECTROS) en el navegador de módulos.

En otros navegadores, usa «Descargar el archivo» y copia el `.vcvplugin` en la carpeta de plugins de Rack. En Rack, esa carpeta se abre desde Ayuda → Abrir carpeta de usuario.

Cada cambio que llega a este repositorio se compila solo para Mac, Linux y Windows con GitHub Actions, y la biblioteca se actualiza. Para compilar a mano con el Rack SDK: `make RACK_DIR=/ruta/al/Rack-SDK install`.

## El panel (24 HP)

### Fila 1 · la física

| Control | Qué hace |
|---|---|
| **f0** | Frecuencia natural del péndulo (0,05–220 Hz). Es la gravedad: el tempo del caos. Baja, es un gesto lento para FM y voltajes; alta, la oscilación se oye. |
| **BASE** | Portadora de FM, raíz de la afinación y registro de las notas (27,5–880 Hz). |
| **ENERGÍA** | Altura desde la que se sueltan. Actúa en el próximo SOLTAR. |
| **IMPULSO** | Negativo: fricción, el caos se apaga. Cero: conservativo. Positivo: bombea energía, y el sistema nunca pasa de 4,5 g. |
| **δ0** | Separación inicial de los gemelos (10⁻⁵ a 10°). Más pequeña, más tarda el horizonte. |
| **TIEMPO** | Escala del tiempo físico (0–2×). En 0 la física se detiene. |

### Fila 2 · la escucha

| Control | Qué hace |
|---|---|
| **MEZCLA** | 0 = A · 1 = A+B · 2 = A−B. En A−B solo se oye la diferencia entre los gemelos. |
| **AFINACIÓN** | Selector de 38 afinaciones, más un archivo Scala propio. El nombre aparece en la pantalla y en el menú de clic derecho. |
| **ATRACCIÓN** | 0 = las alturas de la física · 100 % = los grados de la escala. |
| **RÉGIMEN** | **peq**: oscilaciones pequeñas, sin caos; los gemelos no se separan. **grand**: caos. **div**: caos que se vuelve a soltar solo al cruzar el horizonte; es un ciclo de predicción y pérdida. |
| **SONIDO** | **audif**: la segunda masa ES la forma de onda. **FM**: los ángulos modulan dos osciladores. Portadora ← θ2, moduladora ← θ1, índice ← \|sin(θ1−θ2)\|. |
| **NIVEL** | Nivel de salida. |

### Gestos (botón + entrada de disparo)

- **SOLTAR:** suelta los dos péndulos desde ENERGÍA.
- **PERTURBAR:** empuja a B desde donde está; la divergencia recomienza.
- **SINCRO:** hace B = A + δ0, como volver a empezar sin soltar.
- **GOLPE:** patea a los dos. El voltaje del disparo es la fuerza (10 V = 1).
- **CONGELAR** (entrada de compuerta, abajo a la izquierda): mientras está alta, el tiempo se detiene con un glissando breve.

### Entradas de control

- **V/OCT:** transpone BASE y la afinación. En audificación también transpone f0, así que el teclado toca el caos.
- **f0:** 1 V/oct.
- **ENERG:** 0–10 V.
- **IMPUL:** ±5 V.
- **δ0:** 2 V por década.
- **TIEMPO:** 5 V = ×1.
- **MEZCLA:** 5 V por paso.
- **ATRAC:** 0–10 V.

### Salidas (placa iluminada)

| Salida | |
|---|---|
| **A**, **B** | Cada gemelo por separado: el par estéreo. |
| **MEZCLA** | Según la perilla MEZCLA. |
| **δ** | Separación de los gemelos, logarítmica (0–10 V). Sube en rampa recta mientras son predecibles. |
| **λ** | Exponente de Lyapunov de tiempo finito (1 V = 0,1). |
| **HORIZ** | Compuerta alta desde que los gemelos dejan de parecerse (δ > 0,5). Su flanco es el instante de la pérdida de predicción. |
| **ESTADO** | Polifónica de 8 canales: x/y de las cuatro masas (±5 V). Útil para paneo, filtros o un osciloscopio XY. |
| **NOTA A / GATE A** | Cada vuelco de A (la segunda masa pasa por arriba) produce una nota en la afinación, en V/OCT con 0 V = C4. |
| **NOTA B / GATE B** | Lo mismo para B. Al principio coinciden con las de A; tras el horizonte, cada gemelo sigue su propia melodía. |

Clic derecho en el módulo:
- cargar una afinación Scala (.scl);
- ver la nota cultural de la afinación elegida;
- decidir si el régimen de divergencia se suelta solo.

La afinación Scala queda guardada en el patch.

## Afinaciones incluidas

- **Nativa:** sin atracción; la física decide.
- **Indonesia:** slendro, pelog, pelog bem, selisir.
- **India:** 22 shrutis, ragas Bhairav, Yaman, Todi y Marwa.
- **Mundo árabe:** maqam Rast, Bayati, Saba, Hijaz, Sikah; Safi al-Din (17).
- **Persia:** dastgah Shur y Chahargah.
- **Turquía:** makam Rast (53 comas), Uşşak, Hicaz.
- **Etiopía:** tezeta, bati, ambassel, anchihoye.
- **África:** mbira nyamaropa, amadinda, timbila.
- **Asia:** thai pi phat, modo gong, doce lü, hirajōshi, Ryūkyū.
- **Espectrales:** serie armónica 8–16, serie subarmónica, Bohlen-Pierce (tritava).
- **Referencia temperada:** 12-TET y 19-EDO.

## Ideas de patch

1. **Canon que se desgarra.** NOTA A y GATE A a una voz, NOTA B y GATE B a otra. Empiezan en unísono y, tras el horizonte, se vuelven dos melodías independientes. HORIZ dispara un cambio de sección.
2. **Estéreo de Lyapunov.** A y B a la mezcla, paneados a los extremos. Con δ0 muy pequeño el sonido nace en el centro y se abre lentamente; SINCRO lo vuelve a cerrar.
3. **Respiración.** δ → corte de un filtro: el timbre se abre a medida que se pierde la predicción. RÉGIMEN en div para que el ciclo se repita solo.
4. **Biofeedback.** Una señal lenta (por ejemplo, el Scíon) a IMPULSO o a TIEMPO: el cuerpo empuja o frena el caos. CONGELAR con un pedal para suspenderlo.
5. **Solo el caos.** MEZCLA en 2 (A−B) con FM y una afinación espectral. Mientras los gemelos coinciden hay silencio; el sonido aparece exactamente cuando dejan de ser iguales.

## Notas técnicas

- La física es la de la suite ESPECTROS: péndulo doble m = L = 1, g = w0², integración RK4 a ~8 kHz, con subpasos automáticos a frecuencias altas.
- Todo el estado es de cada instancia, así que varios LYAPUNOV pueden convivir en un patch.
- Los relojes internos van en doble precisión, para no detenerse en sesiones largas.
- El código está en `src/`:
  - `Nucleo.hpp`: el instrumento, sin dependencias de Rack;
  - `Lyapunov.cpp`: el módulo y la pantalla;
  - `Afinaciones.hpp`: el catálogo;
  - `Scala.hpp`: lector de archivos Scala.
- Pruebas del núcleo en `test/`. Ninguna configuración satura ni produce valores inválidos.
- Licencia: GPL-3.0-or-later. Es la que exige la biblioteca de VCV para plugins de código abierto. Puede cambiarse si se prefiere otra distribución.
