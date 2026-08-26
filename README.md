[README (3).md](https://github.com/user-attachments/files/31443438/README.3.md)
# YOLO# Sistema de Detección de Carros y Motocicletas con YOLOv8 y ESP32

Práctica de laboratorio — **Programa de Ingeniería Mecatrónica**
Universidad Militar Nueva Granada · Periodo 2026-2

**Autora:** Sara Sofía García Valderrama
📧 est.saras.garcia@unimilitar.edu.co

---

## 📌 Descripción

Este proyecto implementa un sistema de visión artificial capaz de diferenciar entre **carros** y **motocicletas** en tiempo real usando el modelo preentrenado **YOLOv8** (clases nativas de COCO), y de comunicar el resultado de la detección a un **ESP32** para encender un LED indicador:

- 🔴 **LED rojo (GPIO 25)** → se enciende al detectar un **carro**.
- 🟢 **LED amarillo (GPIO 26)** → se enciende al detectar una **moto**.

El repositorio contiene tres entregas relacionadas:

1. **Sistema base** (Actividad 1): script en Python + YOLOv8 + firmware ESP32 en Arduino/C++.
2. **Actividad 2 — Ampliación**:
   - Simulación del circuito en **Wokwi** usando **MicroPython** (en vez de Arduino/C++).
   - Revisión conceptual de la arquitectura **YOLO**.
   - **Integración física** del sistema con un carro de juguete y una moto de juguete.

---

## 🗂️ Estructura del repositorio

```
├── Actividad-1-Base/
│   ├── deteccion.py              # Script de captura + inferencia YOLOv8 + envío serial
│   └── firmware_esp32.ino        # Firmware Arduino/C++ para el ESP32
│
├── Actividad-2-Wokwi-MicroPython/
│   ├── main.py                   # Firmware equivalente en MicroPython
│   └── diagram.json              # Circuito para simular en wokwi.com
│
├── evidencias/
│   ├── fotos/
│   │   ├── deteccion_carro.jpg       # Detección de "car" + consola CARRO (LED Rojo ON)
│   │   ├── deteccion_moto.png        # Detección de "motorcycle" + consola MOTO (LED amarillo ON)
│   │   └── montaje_fisico.png        # Protoboard con ESP32 y LED conectados
│   └── videos/
│       ├── deteccion_carro_1.mp4     # Video de demostración: carro (ángulo 1)
│       ├── deteccion_carro_2.mp4     # Video de demostración: carro (ángulo 2)
│       └── deteccion_moto.mp4        # Video de demostración: moto
│
├── Informe_Actividad2_YOLO_ESP32_IEEE.pdf   # Informe formato IEEE
└── README.md
```

---

## ⚙️ Hardware utilizado

| Componente        | Conexión                                   |
|-------------------|---------------------------------------------|
| ESP32 DevKit v1    | USB al computador (alimentación + serial)  |
| LED rojo (carro)   | GPIO 25 → resistencia 220 Ω → ánodo LED    |
| LED verde (moto)   | GPIO 26 → resistencia 220 Ω → ánodo LED    |
| Cátodo de ambos LEDs | GND                                      |
| Cámara             | Cámara integrada del computador (`cv2.VideoCapture(0)`) |

---

## 🧠 ¿Cómo funciona?

1. El script `deteccion.py` captura video de la cámara y ejecuta inferencia con `yolov8n.pt` (Ultralytics) en cada fotograma.
2. Se filtran únicamente las detecciones con confianza **> 50 %** para las clases `car` y `motorcycle`.
3. Según el resultado, el script envía por puerto serie (115200 baudios) uno de tres comandos de texto:
   - `CARRO`
   - `MOTO`
   - `NONE`
4. El ESP32 recibe el comando y enciende el LED correspondiente (o los apaga si es `NONE`).

---

## ▶️ Cómo ejecutar

### 1. Sistema base (Python + ESP32 en Arduino/C++)

```bash
pip install ultralytics opencv-python pyserial
```

- Cargar `firmware_esp32.ino` en el ESP32 desde el IDE de Arduino.
- Editar `PUERTO_SERIAL` en `deteccion.py` con el puerto COM asignado al ESP32.
- Ejecutar:

```bash
python deteccion.py
```

### 2. Simulación en Wokwi (MicroPython)

1. Ir a [wokwi.com](https://wokwi.com/) y crear un nuevo proyecto **ESP32 - MicroPython**.
2. Reemplazar el `diagram.json` generado por el de `Actividad-2-Wokwi-MicroPython/diagram.json`.
3. Copiar el contenido de `main.py` en el editor de Wokwi.
4. Iniciar la simulación y escribir `CARRO`, `MOTO` o `NONE` en el monitor serie para validar el encendido de cada LED.

### 3. Integración física con carro y moto de juguete

1. Montar el circuito de LEDs en protoboard según la tabla de conexiones.
2. Cargar el firmware en el ESP32 (Arduino/C++ o el equivalente MicroPython).
3. Ejecutar `deteccion.py` y presentar el carro o la moto de juguete frente a la cámara.
4. Verificar en consola el mensaje `Detección actual: CARRO (LED Rojo ON)` o `MOTO (LED amarillo ON)`, y que el LED físico correspondiente se enciende.

---

## 📊 Resultados

- **Carro de juguete:** detectado como `car` con confianza de hasta **0.81**. LED rojo encendido de forma consistente.
- **Moto de juguete:** detectada como `motorcycle` con confianza de **0.32–0.42** (menor por el tamaño reducido del objeto). LED amarillo encendido correctamente.
- Solo un LED permanece encendido a la vez, respetando la lógica de exclusión mutua del firmware.

Evidencia fotográfica y en video disponible en [`/evidencias`](./evidencias).

### 📷 Fotos

| Detección de carro | Detección de moto |
|---|---|
| ![Detección carro](./evidencias/fotos/deteccion_carro.jpg) | ![Detección moto](./evidencias/fotos/deteccion_moto.png) |

| Montaje físico (protoboard + LED) |
|---|
| ![Montaje físico](./evidencias/fotos/montaje_fisico.png) |

### 🎥 Videos

**Detección del carro de juguete (LED rojo encendido) — video 1**

<video src="https://github.com/user-attachments/assets/REEMPLAZAR-video-carro-1" controls width="500"></video>

📹 Archivo: [Video 1](video1.mp4)
[`evidencias/videos/deteccion_carro_1.mp4`](video2.mp4)
[`evidencias/videos/deteccion_carro_1.mp4`](video%203.mp4)



**Detección del carro de juguete (LED rojo encendido) — video 2**

<video src="https://github.com/user-attachments/assets/REEMPLAZAR-video-carro-2" controls width="500"></video>

📹 Archivo: [`evidencias/videos/deteccion_carro_2.mp4`](./evidencias/videos/deteccion_carro_2.mp4)

**Detección de la moto de juguete (LED verde encendido)**

<video src="https://github.com/user-attachments/assets/REEMPLAZAR-video-moto" controls width="500"></video>

📹 Archivo: [`evidencias/videos/deteccion_moto.mp4`](./evidencias/videos/deteccion_moto.mp4)

> ⚠️ **Nota sobre los videos:** GitHub solo reproduce el video directamente en el README cuando se sube arrastrándolo en la caja de edición del archivo en la web (esto genera automáticamente un enlace `user-attachments/assets/...` que sí es reproducible). Al subir el `.mp4` como archivo normal del repositorio (por Git o "Add file"), GitHub no lo reproduce en línea, solo lo descarga. Para que los tres videos se vean incrustados como en la tabla de fotos:
> 1. Ve a este mismo README en la web de GitHub y presiona "Edit".
> 2. Arrastra cada archivo `.mp4` dentro del cuadro de texto del editor.
> 3. GitHub subirá el video y pegará automáticamente una etiqueta `<video src="https://github.com/user-attachments/assets/...">`.
> 4. Reemplaza las URL `REEMPLAZAR-video-...` de arriba por las que te genere GitHub y guarda los cambios.
>
> Mientras tanto, el enlace de descarga/reproducción directa (📹 Archivo) funciona siempre.

---

## 📚 Referencias

- Ultralytics, *YOLOv8 Docs*: https://docs.ultralytics.com/
- T.-Y. Lin *et al.*, "Microsoft COCO: Common Objects in Context," ECCV, 2014.
- Wokwi ESP32 Simulator: https://wokwi.com/
- Repositorio del curso — Explicación de la arquitectura YOLO: [dialejobv/aplicacion_sistemas_embebidos](https://github.com/dialejobv/aplicacion_sistemas_embebidos)
- Repositorio base del proyecto: https://github.com/anaescobarj/Actividad-2

---

## 📄 Informe completo

El informe detallado en formato IEEE (dos columnas), con el paso a paso de las tres actividades y las figuras de evidencia, se encuentra en:
[`Informe_Actividad2_YOLO_ESP32_IEEE.pdf`](./Informe_Actividad2_YOLO_ESP32_IEEE.pdf)
