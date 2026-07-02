# 4) Códigos de avería (E) y protección (P) — Aerotermia

La máquina distingue dos familias de códigos, y conviene tenerlo claro:

- **Protección (P)** → normalmente es **transitoria**. La máquina se está protegiendo (presiones, temperaturas, anticongelación…) y suele quitarse sola cuando la condición se normaliza. No es que esté rota.
- **Avería (E)** → es un **fallo** de verdad (un sensor, una comunicación, el compresor, la electrónica…). Aquí sí hay que mirar.

En el firmware, los registros `3X0090–0095` llevan las averías E01–E96 y los `3X0096–0100` las protecciones P01–P80, cada bit un código. El texto que ves en Home Assistant sale de decodificar esos bits.

> Descripciones puestas con mis palabras a partir del comportamiento observado y del manual. Para el detalle oficial, tira del manual del equipo. Un código con "sub‑variantes" puede afinar más el motivo.

## Protecciones (P) — suelen ser transitorias

| Código | Motivo |
|---|---|
| **P01** | Protección por flujo de agua |
| **P02** | Alta presión |
| **P03** | Baja presión |
| **P04** | Sobrecalentamiento de la batería exterior (T3) |
| **P05** | Temperatura de descarga alta |
| **P06** | Anticongelación del agua de salida |
| **P07** | Anticongelación de la tubería del condensador |
| **P08** | Presión media (paro) |
| **P10** | Sensor de baja presión |
| **P11 / P12** | Ventilador DC nº1 / nº2 |
| **P13** | Válvula de 4 vías |
| **P14** | Detección de fuga de refrigerante |
| **P21** | Bomba DC anómala |
| **P25** | Sensor de presión de salida |

## Averías (E) — fallo real

| Código | Motivo |
|---|---|
| **E01** | Comunicación con el controlador |
| **E02** | Sensor de descarga (TP) |
| **E03** | Sensor de la batería exterior (T3) |
| **E04** | Sensor ambiente exterior (T4) |
| **E05** | Sensor de línea de líquido (T5) |
| **E06** | Sensor de retorno / aspiración (TH) |
| **E07** | Sensor de ACS (TW) |
| **E08** | Sensor de agua de entrada (TA) |
| **E09** | Sensor de agua de salida (TB) |
| **E10** | Comunicación control ↔ accionamiento |
| **E13** | Sensor de presión de descarga |
| **E14** | Baja presión (LPS) |
| **E15 / E16** | Bus DC bajo / alto |
| **E17** | Corriente de entrada CA |
| **E18** | IPM |
| **E19** | PFC |
| **E20** | El compresor no arranca |
| **E21** | Pérdida de fase |
| **E22** | Reinicio del IPM |
| **E23** | Sobrecorriente del compresor |
| **E24** | Temperatura alta del PFC |
| **E25** | Circuito de detección de corriente |
| **E26** | Desfase |
| **E27** | Sensor del PFC |
| **E28** | Comunicaciones |
| **E29** | Temperatura alta del IPM |
| **E30** | Sensor del IPM |
| **E34** | Tensión CA anómala |
| **E35** | EEPROM |
| **E36** | Reinicio / apagado |
| **E49** | Sensor de agua final (TC) |
| **E50** | Sensor solar (Tso) |
| **E51** | Sensor Tro del control cableado |
| **E52** | Sensor de la zona 2 (TZ2) |
| **E53 / E54** | Sensor del depósito de inercia (TE1 / TE2) |
| **E56** | PS1 |
| **E57 / E58** | Sensor de gas |
| **E59** | Módulo desconectado |

> Un ejemplo real: **P10** ("sensor de baja presión") apareció con la máquina en reposo, compresor parado y presiones igualadas — y se quitó sola. Máquina perfectamente sana. Por eso una P no es motivo de alarma; una E sí conviene investigarla.

---

⬅️ Antes: [3) Mapa Modbus](03-mapa-modbus.md) · ➡️ Siguiente: [5) Entidades y control](05-entidades-y-control.md)
