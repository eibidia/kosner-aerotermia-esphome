# Aerotermia Kosner AQUARIS D HT R‑290 en Home Assistant (RS485 / Modbus)

Así es como metí mi aerotermia **Kosner AQUARIS D HT R‑290 MONOBLOC** en Home Assistant: todo en local, sin nube de por medio. Un ESP32 con un módulo RS485, ESPHome, y a hablar Modbus RTU con la máquina. Se leen todos los sensores y, además, se puede **controlar** (modo, consignas, ACS, modo silencio y demás).

Lo he documentado pensando en quien empieza. Vas desde por dónde entran los cables hasta ver las entidades en Home Assistant, sin saltarte pasos.

![ESPHome](https://img.shields.io/badge/ESPHome-2026.x-blue) ![Home Assistant](https://img.shields.io/badge/Home%20Assistant-local-41BDF5) ![Licencia](https://img.shields.io/badge/licencia-MIT-green)

---

## ⚠️ Antes de meter mano

Léelo, que aquí no vale el "ya lo miro luego":

- **Hay 230 V y hay propano.** Te vas a conectar al bus RS485 de una máquina enchufada a la red y con circuito de refrigerante R‑290 (propano, que es inflamable). Abrir el equipo puede **cargarte la garantía** y, si lo haces mal, es peligroso de verdad. Va bajo tu responsabilidad; si dudas, que lo vea un instalador.
- **Primero lee, luego escribe.** Empieza leyendo registros y entendiendo qué es cada cosa. No toques consignas hasta tener el mapa Modbus claro.
- **Esto no es oficial de Kosner.** Es un proyecto de aficionado, sin ninguna relación con el fabricante. "Kosner" y "AQUARIS" son de sus dueños.
- **Sobre el manual.** Las tablas de registros y códigos que ves aquí están **reescritas** a partir de lo que observé y del manual; son datos técnicos, no una copia. No incluyo el manual entero. Si añades algún recorte suyo, ponlo con atribución al fabricante y solo lo justo.

## 🔐 Nada de secretos en el repo

Aquí no hay ninguna contraseña ni clave. El firmware tira de `!secret`, y tú te creas tu `secrets.yaml` en local (git lo ignora):

```bash
cp secrets.yaml.example secrets.yaml   # y rellena con lo tuyo
```

`secrets.yaml`, `.env` y los `*.key` están en `.gitignore`. Ese fichero no se sube jamás.

---

## Qué necesitas (hardware)

| Pieza | Detalle |
|---|---|
| Placa ESP32‑S3 | DevKitC‑1 o similar (en ESPHome: `board: esp32-s3-devkitc-1`) |
| Módulo RS485 | TTL↔RS485 tipo MAX485, con pin de dirección DE/RE |
| Cable de par trenzado | Para el bus A/B hasta la placa de control de la aerotermia |
| Fuente 5 V | O alimentación USB de la placa |

El bus de esta máquina va en **Modbus RTU, dirección 251, 9600 8N2**. El detalle fino y las fotos, en la guía de conexiones.

---

## Cómo está organizado esto

Dos partes bien separadas: por un lado el cableado, por otro la configuración en Home Assistant.

| Documento | De qué va |
|---|---|
| [`docs/01-conexiones-fisicas.md`](docs/01-conexiones-fisicas.md) | Dónde van los cables RS485 (bornes H1/H2), pines del ESP32, fotos del montaje |
| [`docs/02-configuracion-esphome.md`](docs/02-configuracion-esphome.md) | Instalar ESPHome, el `secrets.yaml`, flashear la placa y adoptarla en Home Assistant |
| [`docs/03-mapa-modbus.md`](docs/03-mapa-modbus.md) | Tablas de registros **3X (lectura)** y **4X (escritura/control)** con sus escalas |
| [`docs/04-codigos-averia-proteccion.md`](docs/04-codigos-averia-proteccion.md) | Códigos **E (avería)** y **P (protección)** |
| [`docs/05-entidades-y-control.md`](docs/05-entidades-y-control.md) | Qué entidades salen en HA y cómo tocar modo/consignas/ACS/silencio |
| [`esphome/aerotermia.yaml`](esphome/aerotermia.yaml) | El firmware ESPHome completo (**saneado**, usa `!secret`) |

---

## En cuatro pasos (si ya te manejas)

1. **Conecta** el ESP32↔RS485 a la placa de control (bornes H1/H2). → [guía](docs/01-conexiones-fisicas.md)
2. `cp secrets.yaml.example secrets.yaml` y mete tu WiFi y tus claves.
3. Flashea `esphome/aerotermia.yaml` con ESPHome. → [guía](docs/02-configuracion-esphome.md)
4. Home Assistant descubre el dispositivo solo. Lo adoptas y aparecen todos los sensores y controles.

Y si te apetece, luego montas tarjetas de panel para el clima y el ACS.

---

## Gracias y aportes

Todo esto sale de trastear e ir documentando. Si mejoras algo, los PR son bienvenidos. Sin garantías (licencia MIT).
