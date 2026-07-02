# 5) Entidades y control en Home Assistant — Aerotermia

Cuando adoptas el dispositivo, en Home Assistant te aparecen dos grandes grupos: **cosas que lees** y **cosas que tocas**.

## Lo que lees (sensores)

- **Temperaturas**: agua de entrada (TA) y salida (TB), depósito de inercia (TE1), ACS (TW), batería exterior (T3), ambiente exterior (T4), descarga del compresor (TP), aspiración (TH) y el objetivo de salida de agua (TOut).
- **Circuito frigorífico**: alta y baja presión, corriente y frecuencia del compresor, apertura de la EXV, velocidad del ventilador.
- **Hidráulico**: caudal de agua, PWM y estado de la bomba, válvula de 4 vías, presión de agua.
- **Estado y diagnóstico**: descongelación, anticongelación, retorno de aceite, desinfección… y los **códigos** de avería/protección ya traducidos a texto (ver [4) Códigos E/P](04-codigos-averia-proteccion.md)).

## Lo que tocas (control)

| Entidad | Qué hace |
|---|---|
| **Modo** (select) | Calefacción · Refrigeración · Automático |
| **Climatización** (switch) | Enciende/apaga la producción de clima |
| **ACS** (switch) | Enciende/apaga el agua caliente sanitaria |
| **Consigna calefacción** (number) | Temperatura de agua objetivo en calefacción |
| **Consigna refrigeración** (number) | Temperatura de agua objetivo en refrigeración |
| **Consigna ambiente** (number) | Temperatura ambiente objetivo (para control por curva) |
| **Consigna ACS** (number) | Temperatura objetivo del ACS |
| **Silencio** (select) | Apagado · Nivel 1 · Nivel 2 |
| **Antilegionela** (switch) | Ciclo de desinfección del ACS |
| **Apoyo eléctrico** (switch) | Resistencia eléctrica de apoyo forzada |

### Consigna de agua vs consigna de ambiente

La máquina puede trabajar de dos maneras: fijando la **temperatura del agua** directamente, o fijando la **temperatura ambiente** y dejando que ella calcule el agua con su **curva de compensación** por temperatura exterior. Por eso hay consignas de las dos clases.

Un sensor informativo, **Modo de control**, te dice en cuál está ahora mismo: *agua fija*, *agua + curva*, *ambiente fija* o *ambiente + curva*. Es útil para saber si tocar la "Consigna ambiente" va a tener efecto: si la máquina está en un modo de *agua*, mover la consigna de ambiente no hará nada.

### Un par de avisos

- **Silencio** tiene tres estados (apagado / nivel 1 / nivel 2). El nivel 2 es el más silencioso.
- **Apoyo eléctrico**: consume bastante. Si lo pones en el panel, mételo con confirmación para no darle sin querer.

---

⬅️ Antes: [4) Códigos de avería y protección](04-codigos-averia-proteccion.md)
