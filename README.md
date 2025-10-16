# 📰 Dashboard de Noticias IoT con ESP32

Sistema IoT que integra un ESP32 con un display OLED y servicios en la nube para mostrar noticias en tiempo real, enviar telemetría y recibir control remoto desde la web.

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

## 🛡️ Seguridad y Buenas Prácticas

- **NO subas** tu archivo `config.h` con credenciales a GitHub.
- Usa `.gitignore` para proteger tus claves.
- Cambia tus claves si el repositorio es público después de hacer pruebas.
- Puedes compartir `config.h.example` SIN tus datos reales.

---

## 📝 Problemas y Soluciones

- **No conecta a WiFi:** Verifica SSID y clave. Usa solo redes 2.4GHz.
- **Display en blanco:** Verifica conexiones y voltaje (3.3V), revisa que tu display sea SPI.
- **No llegan datos a ThingSpeak:** Respeta el límite de 15s entre actualizaciones y revisa tu Write API Key.
- **Comandos no se ejecutan:** Verifica TalkBack ID/API Key y la sintaxis exacta de comandos.

Más detalles en [docs/troubleshooting.md](docs/troubleshooting.md).

---

## 📈 Mejoras Futuras

- Dashboard web propio para visualización y control
- Soporte para más APIs externas (clima, criptomonedas, etc.)
- Modo bajo consumo y actualización OTA
- Almacenamiento histórico local en SD
- Notificaciones push/webhook

---

## 🤝 Contribuciones

¡Pull requests y issues son bienvenidos!  
Por favor, no incluyas información sensible en tus contribuciones.

---

## 👤 Autor

- **ZundyTor**
- **Proyecto para Computación en la Nube**
- Universidad: [Tu Universidad]
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

> Proyecto desarrollado como parte del taller universitario de Computación en la Nube.
