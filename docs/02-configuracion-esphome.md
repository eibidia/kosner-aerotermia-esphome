# 2) Configuración en ESPHome y Home Assistant — Aerotermia

Ya tienes el cacharro cableado. Toca darle firmware y que Home Assistant lo vea.

## Paso 0 — instalar ESPHome

Si usas Home Assistant OS o Supervised, lo más cómodo es el **add‑on de ESPHome Device Builder** (Ajustes → Complementos → tienda → ESPHome). Si no, vale también ESPHome por línea de comandos (`pip install esphome`). Cualquiera de los dos sirve.

## Paso 1 — tu `secrets.yaml`

Copia la plantilla y rellénala con lo tuyo:

```bash
cp secrets.yaml.example secrets.yaml
```

Dentro van tu WiFi, la clave de cifrado de la API y la contraseña OTA. La clave de la API la generas con:

```bash
openssl rand -base64 32
```

Recuerda: `secrets.yaml` no se sube al repo (está en `.gitignore`).

## Paso 2 — mete la configuración

Coloca [`esphome/aerotermia.yaml`](../esphome/aerotermia.yaml) en tu ESPHome, junto a `secrets.yaml`. Un par de cosas que quizá quieras tocar antes:

- El nombre del dispositivo y la IP fija (o quita el bloque `manual_ip:` y tira de DHCP).
- Comprueba que los pines del `uart` (TX GPIO17, RX GPIO18) y el `flow_control_pin: GPIO21` coinciden con tu montaje.

## Paso 3 — el primer flasheo va por USB

La placa viene virgen, así que la **primera vez hay que grabarla por cable USB** (el WiFi/OTA todavía no existen en ella). Conéctala al ordenador o al servidor de HA, dale a *Install → Plug into this computer / your Home Assistant server*, y espera a que compile y suba.

A partir de ahí, **el resto de actualizaciones ya son por OTA** (por WiFi), sin cables.

> Si no aparece el puerto serie, casi siempre es el cable: muchos cables USB son solo de carga y no llevan datos. Prueba otro.

## Paso 4 — adoptarla en Home Assistant

Cuando arranque y se conecte al WiFi, Home Assistant la **descubre sola**: te saldrá un aviso de "nuevo dispositivo ESPHome descubierto". La adoptas, pega la clave de cifrado si te la pide, y listo.

De golpe tienes en HA todos los sensores de la máquina (temperaturas, presiones, caudal, compresor…) y los controles (modo, consignas, ACS, silencio).

## Comprobar que todo va

- En **Ajustes → Dispositivos**, el dispositivo debe salir con sus entidades y sin marcar "no disponible".
- Mira los logs del dispositivo en ESPHome. Si el Modbus va fino, verás lecturas cambiando. Si ves *timeouts*, repasa el cableado A/B (y prueba a invertirlos) y que la dirección de esclavo es la **251**.

---

⬅️ Antes: [1) Conexiones físicas](01-conexiones-fisicas.md) · ➡️ Siguiente: [3) Mapa Modbus](03-mapa-modbus.md)
