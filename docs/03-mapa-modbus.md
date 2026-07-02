# 3) Mapa Modbus — Aerotermia Kosner AQUARIS D HT R‑290

Este es el mapa que uso en el firmware. Los datos (qué hay en cada registro y con qué escala) están sacados de observar la máquina y del manual, y los presento aquí con mis propias tablas y descripciones. Para el detalle oficial completo, el manual del equipo manda.

## Cómo se habla con la máquina

- **Modbus RTU**, **dirección de esclavo 251** (0xFB), **9600 8N2**, CRC‑16.
- **Lectura** de registros `3X` con función **0x04**.
- **Escritura/control** de registros `4X` con **0x03 / 0x06 / 0x10** (`register_type: holding` en ESPHome).
- Direcciones en **decimal, base 0** (`3X0042` → `address: 42`).

> Las temperaturas y corrientes van **÷10** (el registro trae el valor ×10). Tensión, Hz, kPa, rpm, pasos y % vienen directos. `S16` = con signo, `U16` = sin signo.

## Registros 3X — lectura (función 0x04)

Estos son los que el firmware expone como sensores:

| Dir | Tipo | Escala | Qué es |
|---|---|---|---|
| 9 | U16 | bits | Modo actual (bit2 ACS, bit1 calefacción, bit0 refrigeración) |
| 38 | U16 | bits | Estado del sistema (desinfección, anticongelación, descongelación, retorno de aceite, temp baja no arranca…) |
| 40 | S16 | ÷10 | Temp ambiente interior |
| 42 | S16 | ÷10 | Temp **agua de entrada** (TA) |
| 43 | S16 | ÷10 | Temp **agua de salida** (TB) |
| 44 | S16 | ÷10 | Temp **depósito de inercia** (TE1) |
| 46 | S16 | ÷10 | Temp **depósito ACS** (TW) |
| 49 | S16 | ÷10 | Temp **batería exterior** (T3) |
| 50 | S16 | ÷10 | Temp **ambiente exterior** (T4) |
| 52 | S16 | ÷10 | Temp **descarga del compresor** (TP) |
| 53 | S16 | ÷10 | Temp **de aspiración** (TH) |
| 56 | S16 | ÷10 | Temp **objetivo de salida** de agua (TOut) |
| 61 | U16 | ÷10 | Presión de agua de salida (bar) |
| 62 | U16 | — | PWM de la bomba (%) |
| 64 | U16 | ÷10 | Caudal de agua (m³/h) |
| 70 | U16 | — | Apertura de la EXV (pasos) |
| 72 | U16 | — | Velocidad del ventilador (rpm) |
| 77 | U16 | ÷10 | Corriente del compresor (A) |
| 79 | U16 | — | Frecuencia real del compresor (Hz) |
| 80 | U16 | 0/1 | Estado de la bomba de agua principal |
| 81 | U16 | 0/1 | Estado de la válvula de 4 vías |
| 86 | U16 | — | Alta presión (kPa) |
| 87 | U16 | — | Baja presión (kPa) |
| 90–95 | U16 | bits | Errores E01–E96 (16 bits por registro) → ver [códigos E/P](04-codigos-averia-proteccion.md) |
| 96–100 | U16 | bits | Protecciones P01–P80 → ver [códigos E/P](04-codigos-averia-proteccion.md) |
| 103 | U16 | bits | Modo de funcionamiento real (espera / refrigeración / calefacción / ACS) |

## Registros 4X — escritura / control (función 0x03/0x06/0x10)

Con estos controlas la máquina. **Ve con cuidado**: escribir aquí manda de verdad sobre el equipo.

| Dir | Tipo | Qué hace / valores |
|---|---|---|
| 2100 | U16 | **Modo**: 1 refrigeración · 2 calefacción · 4 automático |
| 2101 | U16 | **Zonas activas**: 0 todo off · 1 Zona1 on · 2 Zona2 on · 3 todo on |
| 2102 | U16 | **ACS** on/off: 0 apagado · 1 encendido |
| 2103 | U16 | **Funciones extra** (bits): bit6 desinfección · **bits5‑4 modo silencio** (0 off / 1 nivel1 / 2 nivel2) · bit3 resistencia eléctrica de apoyo |
| 2105 | S16 | Consigna de **ACS** (÷10 °C) |
| 2106 | S16 | Consigna de **refrigeración** Zona1 (÷10 °C) |
| 2107 | S16 | Consigna de **calefacción** Zona1 (÷10 °C) |
| 2114 | S16 | Consigna de **temperatura ambiente** (÷10 °C) |

### Dos trampas que me costaron

- **Las consignas van ÷10.** El registro guarda °C×10 (p. ej. `430` = 43,0 °C), **aunque el manual dé a entender que es directo**. En ESPHome se arregla con `multiply: 10` en cada `number`.
- **El "modo silencio" no son dos bits sueltos**, es un **campo de 2 bits** (bits 4‑5 del registro 2103) con valores 0/1/2. Si lo tratas como bits independientes acabas escribiendo el registro completo y te cargas los otros bits (desinfección, apoyo). En el firmware se resuelve leyendo el registro entero, cambiando solo esos dos bits y volviéndolo a escribir.

---

⬅️ Antes: [2) Configuración en ESPHome](02-configuracion-esphome.md) · ➡️ Siguiente: [4) Códigos de avería y protección](04-codigos-averia-proteccion.md)
