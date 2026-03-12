# ⬡ COSMIC SYNTH
### Sintetizador / Rhythm Machine digital profesional
> Versión 1.0 — Desarrollado con Web Audio API · Sin dependencias externas

---

## ¿Qué es COSMIC SYNTH?

COSMIC SYNTH es un instrumento musical digital interactivo que funciona directamente en el navegador. Combina un sintetizador substractivo, una máquina de ritmos de 4 pistas y un teclado cromático de 4 octavas, todo en una sola interfaz diseñada para ser intuitiva y profesional.

No requiere instalación, plugins ni conexión a internet. Solo abre el archivo HTML y empieza a crear.

---

## Características principales

- **4 tracks de loop independientes** con grabación, mute, solo y volumen individual
- **Sintetizador substractivo** con 4 formas de onda y envelope ADSR completo
- **6 efectos de audio** por track: Filter, Resonancia, Reverb, Delay, Distorsión y Chorus
- **Teclado cromático de 4 octavas** (48 notas) controlable por teclado físico o clic
- **Metrónomo visual** de 8 subdivisiones sin sonido
- **Latencia mínima** gracias a un pool de 8 voces precreadas por track
- **Tempo ajustable** de 40 a 240 BPM
- **Visualizador de waveform** animado por track
- **Partículas visuales** reactivas al sonido

---

## Requisitos

| Requisito | Detalle |
|---|---|
| Navegador | Chrome 80+, Firefox 75+, Safari 14+, Edge 80+ |
| Web Audio API | Requerida (soportada en todos los navegadores modernos) |
| Instalación | Ninguna |
| Dependencias | Ninguna |

---

## Inicio rápido

1. Abre el archivo `cosmic-synth.html` en tu navegador
2. Pulsa **[ INICIAR ]** para activar el contexto de audio
3. Selecciona un track en el panel izquierdo
4. Configura tu sonido en el panel de efectos
5. Define **BARS** (compases del loop) y **BPM**
6. Pulsa **● REC** y toca notas en el teclado
7. El loop se graba automáticamente y entra en reproducción
8. Repite en los otros 3 tracks para construir capas

---

## Controles de teclado

El teclado físico está mapeado a la matriz cromática de la siguiente forma:

```
FILA        TECLAS              OCTAVA
────────────────────────────────────────
1  (aguda)  1 2 3 4 5 6 7 8 9 0 ' ¿    Octava 5
2           q w e r t y u i o p ´ +    Octava 4
3           a s d f g h j k l ñ { }    Octava 3
4  (grave)  z x c v b n m , . -        Octava 2
```

Cada tecla corresponde a una nota en orden cromático (C, C#, D, D#, E, F, F#, G, G#, A, A#, B).
Las notas sostenidas (#) tienen fondo más oscuro en la interfaz, igual que las teclas negras de un piano.

---

## Panel de Transport (barra superior)

| Control | Función |
|---|---|
| **● REC** | Inicia grabación en el track activo. Se detiene automáticamente al completar los compases definidos |
| **▶ PLAY** | Inicia reproducción de todos los loops |
| **■ STOP** | Detiene todo y resetea posición |
| **▼▲ BPM** | Ajusta el tempo (40–240 BPM). Afecta todos los tracks |
| **Puntos parpadeantes** | Metrónomo visual de 8 subdivisiones. El punto grande marca el tiempo 1 |
| **Contador X/Y** | Muestra el paso actual dentro del loop del track seleccionado |

---

## Panel de Tracks (izquierda)

Cada uno de los 4 tracks incluye:

| Elemento | Función |
|---|---|
| **Número (clic)** | Selecciona el track para editarlo con el panel de efectos |
| **● REC** | Graba directamente en ese track sin necesidad de seleccionarlo |
| **M — Mute** | Silencia el track sin borrar el loop |
| **S — Solo** | Escucha solo ese track, silencia los demás |
| **✕ — Clear** | Borra el loop grabado del track |
| **Waveform** | Visualizador animado que se activa cuando el track suena |
| **Slider VOL** | Volumen individual del track (0–100%) |
| **Estado** | Indica si el track tiene un loop grabado y cuántos pasos tiene |

---

## Panel de Efectos (centro superior)

### Forma de onda
| Tipo | Carácter |
|---|---|
| **SAW** (Serrada) | Agresivo, rico en armónicos. Ideal para leads y bajos |
| **SQR** (Cuadrada) | Hueco y nasal. Clásico de sintetizadores vintage |
| **SIN** (Sinusoidal) | Suave y puro. Ideal para pads y sub-bajos |
| **TRI** (Triangular) | Intermedio entre SIN y SQR. Cálido y redondo |

### Envelope ADSR
El ADSR controla la forma del sonido en el tiempo mediante sliders verticales:

| Parámetro | Rango | Descripción |
|---|---|---|
| **A — Attack** | 1–200 ms | Tiempo en llegar al volumen máximo al pulsar |
| **D — Decay** | 10–500 ms | Tiempo en bajar del pico al nivel de Sustain |
| **S — Sustain** | 0–100% | Nivel de volumen mientras se mantiene la nota |
| **R — Release** | 10–800 ms | Tiempo en apagarse al soltar la tecla |

### Efectos (knobs arrastrables — arrastrar hacia arriba para subir)

| Efecto | Rango | Descripción |
|---|---|---|
| **FILTER** | 200 Hz – 18 kHz | Filtro paso-bajo. Corta frecuencias agudas para oscurecer el sonido |
| **RESO** | 0 – 20 | Resonancia del filtro. Añade un pico en la frecuencia de corte |
| **REVERB** | 0–100% | Espacio y eco. Simula habitaciones o salas grandes |
| **DELAY** | 0–100% | Repetición rítmica sincronizada al BPM |
| **DIST** | 0–100% | Distorsión y saturación. Añade agresividad al sonido |
| **CHORUS** | 0–100% | Duplica el sonido con desfase leve. Añade amplitud y cuerpo |

### Controles de rango

| Control | Rango | Descripción |
|---|---|---|
| **OCTAVA ▼▲** | -2 a +2 | Desplaza el teclado en octavas completas |
| **BARS ▼▲** | 1 a 16 | Define la duración del loop en compases antes de grabar |

---

## Arquitectura de audio

```
Oscilador (pool × 8 voces)
        │
        ├──► Dry Gain
        ├──► Reverb Convolver
        ├──► Delay Node
        └──► Chorus Delay
              │
              ▼
        Distortion (WaveShaper)
              │
              ▼
        Filter (BiquadFilter lowpass)
              │
              ▼
        Master Gain (por track)
              │
              ▼
        AudioContext Destination
```

- Cada track tiene su propia cadena de señal completamente independiente
- Las voces del pool se reutilizan para evitar latencia de creación de nodos
- El impulso de reverb se genera proceduralmente al iniciar

---

## Flujo de trabajo recomendado

```
1. Define BPM y BARS según el estilo que buscas
   └── Rápido (140+ BPM, 1-2 bars) → electrónica, techno
   └── Medio (90-120 BPM, 2-4 bars) → hip-hop, lo-fi
   └── Lento (60-80 BPM, 4-8 bars) → ambient, downtempo

2. Track 1 → línea de bajo (SQR o SAW, octava -1 o -2)
3. Track 2 → melodía principal (SIN o TRI, efectos suaves)
4. Track 3 → pad de ambiente (SIN, reverb alto, attack largo)
5. Track 4 → efectos o contrapunto (SAW, dist o chorus)

6. Usa Mute/Solo para mezclar en vivo
7. Ajusta volúmenes individuales para balance
```

---

## Notas técnicas

- Construido íntegramente con **Web Audio API** nativa
- Sin frameworks, sin librerías externas
- El contexto de audio se inicializa con interacción del usuario (requerimiento del navegador)
- Los loops se almacenan en memoria como eventos con timestamp en pasos de corchea (1/8)
- La resolución del secuenciador es de **corcheas** (2 pasos por beat)
- El reverb se genera con un buffer de ruido exponencial de 3 segundos

---

## Créditos y licencia

**COSMIC SYNTH v1.0**  
Desarrollado como instrumento musical digital de código abierto.  
Libre para uso personal, educativo y creativo.

---

*"El cosmos entero es música. Solo hay que aprender a escucharlo."*
