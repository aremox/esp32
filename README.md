# 🔌 Athom Smart Plug - ESPHome Custom Firmware con Vigilante Web

Este repositorio contiene la configuración avanzada de ESPHome para los enchufes inteligentes **Athom Smart Plug (Modelos V2 y V3)**. La principal mejora es un sistema de **"Vigilante de Red" (Watchdog)** que monitoriza la conexión a Internet y reinicia automáticamente el dispositivo conectado (ideal para routers o nodos mesh) si detecta una caída persistente.

---

## ✨ Características Destacadas

* **Vigilante Web (Watchdog):** Realiza pings HTTP automáticos. Si fallan `X` intentos, apaga el relé, espera `Y` segundos y lo vuelve a encender.
* **Pausa de Seguridad:** Tras un reinicio por fallo, el sistema entra en una pausa de 10 minutos para evitar bucles mientras el router arranca.
* **Monitorización de Energía Completa:** Lectura en tiempo real de:
    * Potencia (W)
    * Voltaje (V)
    * Corriente (A)
    * Energía Diaria y Total (kWh) con factor de potencia.
* **Modo de Restauración Inteligente:** Selector en Home Assistant para definir el estado tras un corte de luz (Siempre ON, Siempre OFF, o Recordar estado).
* **Optimización de Red:** Configuración de IP estática y DNS manual para una resolución de nombres ultra rápida.
* **Diagnósticos Detallados:** Sensores de Uptime, señal WiFi en %, contador de reinicios totales y registro de hora exacta de la última acción del vigilante.

---

## 🛠️ Modelos Soportados

| Característica | Athom Plug V2 | Athom Plug V3 |
| :--- | :--- | :--- |
| **Chip** | ESP8285 / ESP8266 | ESP32-C3 (RISC-V) |
| **Memoria Flash** | 2 MB | 4 MB |
| **Framework** | Arduino (v3.0.2 compatible) | ESP-IDF |
| **Pines (Relé)** | GPIO12 | GPIO5 |
| **Pines (Botón)** | GPIO5 | GPIO3 |
| **Pines (LED)** | GPIO13 | GPIO6 |

---

## ⚙️ Configuración del Vigilante

Puedes ajustar estos valores en la sección `substitutions` del archivo YAML:

* **`comprobacion_cada`**: (Def: `60s`) Tiempo entre cada test de conexión.
* **`max_fallos_y`**: (Def: `3`) Cuántos fallos seguidos activan el reinicio.
* **`url_objetivo`**: (Def: `http://google.com`) Dirección para comprobar internet.
* **`tiempo_apagado_z`**: (Def: `10s`) Tiempo que el enchufe permanece apagado antes de rearmarse.

---

## 🚀 Guía de Instalación Rápida

1.  **Preparar Secrets:** Asegúrate de tener configurado tu archivo `secrets.yaml` con:
    ```yaml
    wifi_ssid: "TU_RED_WIFI"
    wifi_password: "TU_PASSWORD"
    ```
2.  **Cargar Firmware:** * Si es la primera vez o da error de espacio (OTA), carga un firmware **mínimo** (solo WiFi y OTA).
    * Luego, carga el archivo completo correspondiente a tu versión (V2 o V3).
3.  **Limpieza:** Si cambias de versión de framework, usa la opción **"Clean Build Files"** en ESPHome antes de compilar.

---

## 📊 Entidades Generadas

Al integrar en **Home Assistant**, obtendrás:
- **Switch:** Control manual del relé.
- **Binary Sensor:** Estado de conectividad "Estado Web Vigilante".
- **Sensors:** Todas las métricas eléctricas y contadores de fallos/reinicios.
- **Select:** Menú desplegable para el comportamiento de encendido.
- **Text Sensor:** IP local, Uptime formateado y log de última acción.

---

## 🛡️ Notas de Seguridad y Rendimiento

- **SSL:** Se desactiva la verificación SSL (`verify_ssl: false`) para evitar que el proceso de cifrado bloquee el procesador del enchufe (especialmente en el ESP8266).
- **Watchdog de Tarea:** En el modelo V3 (ESP32-C3), el tiempo de espera del sistema se ha subido a 20s para garantizar estabilidad durante las peticiones HTTP.

---
*Desarrollado para mantener la red siempre activa sin intervención manual.* 🌐🤖
