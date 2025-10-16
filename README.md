# 📰 Dashboard de Noticias IoT con ESP32

Sistema IoT que integra un ESP32 con un display OLED y servicios en la nube para mostrar noticias en tiempo real, enviar telemetría y recibir control remoto desde la web.

---

## 🚦 Fases del Proyecto

| Fase | Descripción | Estado |
|------|-------------|--------|
| **Fase 1** | **Integración y prueba de hardware**: ESP32 + OLED, WiFi, NewsAPI | ✅ Completo |
| **Fase 2** | **Interfaz y robustez**: Mejoras visuales, manejo de errores, estadísticas | ✅ Completo |
| **Fase 3** | **Conexión cloud**: Envío de datos y telemetría a ThingSpeak | ✅ Completo |
| **Fase 4** | **Control remoto**: Recepción y ejecución de comandos desde la nube | ✅ Completo |
| **Fase 5** | **Documentación**: Manual de usuario, instalación, diagramas y README | ✅ Completo |

---

## 🚀 Descripción

Este proyecto corresponde a un taller universitario de **Computación en la Nube**. El sistema permite visualizar titulares de noticias en un display OLED, obtenidos desde NewsAPI, y enviar estadísticas y estado del dispositivo a la nube usando ThingSpeak. Además, permite recibir comandos remotos desde la nube para controlar la categoría, país o frecuencia de actualización de las noticias, entre otras funciones.

---

## 🛠️ Características

- Visualización de titulares de noticias en display OLED SSD1306 (SPI)
- Soporte para múltiples categorías y países (cambiables remotamente)
- Integración con NewsAPI para obtener titulares en tiempo real
- Envío de estadísticas de dispositivo a ThingSpeak (uptime, memoria, señal WiFi, etc.)
- Control remoto vía TalkBack de ThingSpeak (cambiar categoría, país, intervalo, reinicio...)
- Rotación automática de titulares
- Manejo robusto de errores y reconexión WiFi
- Código documentado y modular

---

## 🖥️ Demostración

| Pantalla Inicial | Noticias | Comando Remoto |
|------------------|---------|---------------|
| ![init](docs/images/init.png) | ![news](docs/images/news.png) | ![command](docs/images/command.png) |

> **[Video demostrativo (YouTube)](https://youtu.be/xxxxxxx)**  
> **[Dashboard en ThingSpeak](https://thingspeak.com/channels/3118653)**

---

## 📦 Estructura del Proyecto

```
Dashboard-News-IoT/
├── README.md
├── src/
│   └── main.cpp
├── include/
│   ├── config.h          # Tus credenciales (NO subir a GitHub)
│   └── config.h.example  # Plantilla de configuración
├── docs/
│   ├── architecture.png
│   ├── circuit.png
│   └── screenshots/
├── platformio.ini
└── .gitignore
```

---

## 🧰 Componentes y Conexiones

### Hardware

- **ESP32 DevKit** (ESP32-D0WD-V3 recomendado)
- **Display OLED SSD1306** (128x64, 7 pines, SPI)
- Cable USB Micro-B (programación y alimentación)

### Conexión SPI (ejemplo):

| Pin Display | Pin ESP32 | Función |
|-------------|-----------|---------|
| GND         | GND       | Tierra  |
| VCC         | 3.3V      | Alimentación |
| SCL         | GPIO 18   | SCLK    |
| SDA         | GPIO 23   | MOSI    |
| RES         | GPIO 4    | Reset   |
| DC          | GPIO 2    | Data/Command |
| CS          | GPIO 5    | Chip Select |

![Diagrama de conexiones](docs/circuit.png)

---

## 💾 Instalación

### 1. Prerrequisitos

- Visual Studio Code + extensión PlatformIO
- Driver USB para tu ESP32 (CP2102/CH340)
- Cuenta gratuita en [NewsAPI.org](https://newsapi.org/)
- Cuenta gratuita en [ThingSpeak](https://thingspeak.com/)

### 2. Clona el repositorio

```bash
git clone https://github.com/tuusuario/Dashboard-News-IoT.git
cd Dashboard-News-IoT
```

### 3. Configura tus credenciales

Copia la plantilla y completa tus datos en `include/config.h`:

```bash
cp include/config.h.example include/config.h
```

Edita con tus claves WiFi, API keys y Channel IDs.

### 4. Conecta el hardware y sube el código

- Conecta el ESP32 por USB
- En PlatformIO, selecciona el puerto correcto
- Haz clic en **Upload** (o presiona el botón BOOT si es necesario)
- Abre el **Serial Monitor** (115200 baudios)

---

## ⚙️ Configuración rápida (`config.h`)

```cpp
const char* WIFI_SSID = "TuWiFi";
const char* WIFI_PASSWORD = "TuClave";
const char* NEWS_API_KEY = "TuApiKeyNewsapi";
const char* NEWS_COUNTRY = "us";
const char* NEWS_CATEGORY = "technology";
const char* THINGSPEAK_WRITE_API_KEY = "TuKeyThingSpeak";
const unsigned long THINGSPEAK_CHANNEL_ID = 3118653;
const unsigned long TALKBACK_ID = 1234567;
const char* TALKBACK_API_KEY = "TuTalkbackKey";
```

---

## 🌐 APIs y Servicios

- **NewsAPI**: [https://newsapi.org/](https://newsapi.org/)
- **ThingSpeak Cloud**: [https://thingspeak.com/](https://thingspeak.com/)
- **TalkBack (ThingSpeak)**: Para comandos remotos

---

## 🎮 Control Remoto vía TalkBack

Agrega comandos en tu panel TalkBack de ThingSpeak para controlar el ESP32 desde la web.  
**Ejemplos:**

- Cambiar categoría: `CATEGORY_SPORTS`, `CATEGORY_TECHNOLOGY`, `CATEGORY_BUSINESS`, etc.
- Cambiar país: `COUNTRY_MX`, `COUNTRY_US`, `COUNTRY_ES`, etc.
- Cambiar intervalo: `INTERVAL_30`, `INTERVAL_60`, etc. (en segundos)
- Actualización inmediata: `UPDATE_NOW`
- Reiniciar el ESP32: `RESTART`
- Enviar reporte: `STATUS`

El ESP32 verifica y ejecuta comandos cada 30 segundos.

---

## ❗ Errores Comunes y Soluciones

### Error: El display no muestra información

**Síntoma:**  
- El monitor serial indica todo "OK", pero el display OLED no muestra nada.

**Causas y soluciones:**
- El display SSD1306 está configurado para SPI (7 pines); si usas I2C (4 pines), necesitas adaptar el código y conexiones.
- Verifica que los pines físicos coincidan con los definidos en el código.
- Asegúrate de conectar VCC a 3.3V y GND a GND.
- Si el display sigue sin mostrar, revisa el voltaje y prueba otro display.

### Error: No se pueden subir programas al ESP32

**Síntoma:**  
```
A fatal error occurred: Failed to connect to ESP32: No serial data received.
```
**Soluciones:**
- Presiona y mantén el botón **BOOT** en el ESP32 justo cuando empiece la subida y hasta que diga "Writing at 0x...".
- Cierra el Serial Monitor antes de subir.
- Usa un cable USB de datos (no solo de carga).
- Verifica el puerto en `platformio.ini` con `upload_port = COM5` (o el puerto correcto).
- Cambia la velocidad de subida: `upload_speed = 115200`.

### Error: No aparecen noticias en el display

**Síntoma:**  
- El serial muestra "No se encontraron noticias" o el JSON recibido tiene `"totalResults":0`.

**Soluciones:**
- Cambia el país y/o categoría en tu `config.h` a una combinación que tenga noticias (ej: `us` y `technology`).
- Prueba primero con NewsAPI en navegador.
- Si la API key es incorrecta o hay límite, revisa el código de respuesta (401 = API key incorrecta, 429 = límite excedido).

### Error: ThingSpeak responde con "0" (update rejected)

**Síntoma:**
- El serial muestra `Respuesta: 0` al enviar datos a ThingSpeak.

**Soluciones:**
- Respeta el límite de 15 segundos entre actualizaciones en cuentas gratis.
- Verifica que tu Write API Key y Channel ID sean correctos.
- Asegúrate que los 8 campos estén habilitados en tu canal de ThingSpeak.

### Error: "multiple definition" al compilar

**Síntoma:**  
```
multiple definition of `WIFI_SSID'; ...
```
**Solución:**
- Sólo debe haber un archivo `.cpp` con funciones `setup()` y `loop()` en `/src` al compilar. Elimina o comenta otros archivos de prueba (`test_*.cpp`).

### Error: Comandos TalkBack no se ejecutan

**Síntoma:**  
- Agregas varios comandos a la vez y todos se ejecutan uno tras otro.

**Solución:**
- TalkBack funciona como una *cola* FIFO. Para pruebas controladas, agrega los comandos uno por uno y espera a que el ESP32 ejecute cada uno antes de agregar el siguiente.

---

## 📈 Mejoras Futuras

- Dashboard web propio para visualización y control
- Soporte para más APIs externas (clima, criptomonedas, etc.)
- Modo bajo consumo y actualización OTA
- Almacenamiento histórico local en SD
- Notificaciones push/webhook

---


---

## 👤 Autor

- **ZundyTor**
- **Proyecto para Computación en la Nube**
- Universidad: Fundacion Universitaria Los Libertadores
- Octubre 2024

---

## 📄 Licencia

MIT License.  
Ver archivo [LICENSE](LICENSE) para más detalles.

---

## 🙏 Agradecimientos

- NewsAPI.org
- ThingSpeak Cloud
- Adafruit (librerías display)
- Espressif
- Comunidad de PlatformIO

---
