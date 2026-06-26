
# CALCULADORA AVANZADA MEP: DESHUMECTACIÓN Y POTENCIA TÉRMICA EN PISCINAS

**ID_INFORME:** ITR.CDP_260626-01 | **VERSIÓN:** 1.1

**ESTADO:** ESPERANDO ACEPTACIÓN

### 1. ESPECIFICACIONES TÉCNICAS

* **Orden del Usuario:** Desarrollar una herramienta web SPA para el cálculo integral de recintos de piscinas cubiertas. El alcance incluye ahora dos módulos principales:
1. Cálculo de la carga latente (kg/h de agua a deshumectar) por evaporación, ocupación y ventilación.
2. Cálculo de la potencia térmica requerida para el vaso de la piscina (kW), tanto para el calentamiento inicial como para el mantenimiento operativo.


* **Normativa:**
* RITE (Reglamento de Instalaciones Térmicas en los Edificios).
* ASHRAE Handbook - HVAC Applications.
* VDI 2089 (Building services in swimming baths).


* **Parámetros Validados (Nuevos añadidos para el Módulo Térmico):**
* *Superficie de agua ($A$):* [m²] (Validación: > 0)
* *Volumen del vaso ($V$):* [m³] (Validación: > 0)
* *Temperatura del agua ($T_w$):* [ºC]
* *Temperatura inicial/red ($T_{red}$):* [ºC] (Rango típico: 10ºC - 15ºC)
* *Tiempo de calentamiento inicial ($t_{cal}$):* [h] (Valores comunes: 6h, 12h, 24h, 48h)
* *Temperatura del aire interior ($T_{int}$):* [ºC]
* *Humedad Relativa interior ($HR_{int}$):* [%]
* *Caudal de ventilación exterior ($Q_v$):* [m³/h]
* *Condiciones exteriores de diseño:* $T_{ext}$ [ºC] y $HR_{ext}$ [%].
* *Ocupantes ($N$):* Número de personas.
* *Pérdidas térmicas adicionales estimadas:* Transmisión y ventilación del vaso [kW] (Input configurable, por defecto 3 kW).


* **Hipótesis y simplificaciones adoptadas:**
* Capacidad calorífica específica del agua ($c_p$) = 1.16 Wh/(kg·K).
* Densidad del agua = 1000 kg/m³.
* Calor latente de vaporización del agua ($h_{fg}$) aproximado a 0.68 kWh/kg.



### 2. LÓGICA DE INGENIERÍA Y CÁLCULOS

Se mantiene la lógica psicrométrica rigurosa para la deshumectación y se incorpora el análisis térmico básico del vaso.

* **Check de datos aportados:** Comprendida la necesidad de disociar la potencia de arranque (muy dependiente del tiempo objetivo) de la potencia de mantenimiento operativo (fuertemente vinculada a la tasa de evaporación calculada en el paso previo).
* **Módulo 1: Carga Latente y Deshumectación**
*(Mantenido de v1.0)*
1. Evaporación de la lámina ($\dot{m}_e$): $\dot{m}_e = A \cdot (P_w - P_{a,int}) \cdot F$ [kg/h]
2. Ocupación ($\dot{m}_p$): $\dot{m}_p = N \cdot 0.15$ [kg/h]
3. Ventilación ($\dot{m}_v$): $\dot{m}_v = Q_v \cdot \rho_{aire} \cdot (x_{ext} - x_{int})$ [kg/h]
4. Carga Total: $\dot{m}_{total} = (\dot{m}_e + \dot{m}_p + \dot{m}_v) \cdot 1.20$ [kg/h] (margen del 20%)


* **Módulo 2: Potencia Térmica del Vaso (NUEVO)**
1. **Energía de Calentamiento Inicial ($Q_{ini}$):**

$$Q_{ini} = V \cdot 1000 \cdot c_p \cdot (T_w - T_{red}) \text{ [Wh]}$$



Dividiendo entre 1000 para pasar a kWh:

$$Q_{ini} = V \cdot 1.16 \cdot (T_w - T_{red}) \text{ [kWh]}$$


2. **Potencia de Arranque Requerida ($P_{arranque}$):**

$$P_{arranque} = \frac{Q_{ini}}{t_{cal}} \text{ [kW]}$$


3. **Pérdidas Térmicas por Evaporación ($P_{evap}$):**
Se reaprovecha la tasa de evaporación del Módulo 1 ($\dot{m}_e$).

$$P_{evap} = \dot{m}_e \cdot h_{fg} = \dot{m}_e \cdot 0.68 \text{ [kW]}$$


4. **Potencia Total de Mantenimiento ($P_{maint}$):**

$$P_{maint} = P_{evap} + P_{otras} \text{ [kW]}$$



*(Donde $P_{otras}$ engloba transmisión a través del vaso y renovación superficial).*



### 3. HISTORIAL DE REQUERIMIENTOS ACUMULADOS

* **Visión General y Origen:** Evolución de una calculadora de deshumectación simple a un aplicativo integral para dimensionamiento de instalaciones mecánicas (clima y térmico) en piscinas cubiertas.
* **Historial de Requerimiento:**
* [Pendiente] Interfaz con 3 pestañas (Panel de Cálculo, Informe, Documentación).
* [Pendiente] Input de datos de geometría, volumen, condiciones psicrométricas y térmicas.
* [Pendiente] Motor de cálculo psicrométrico en JS (Humedad Absoluta y Presiones de Saturación).
* [Añadido en v1.1] [Pendiente] Motor de cálculo termodinámico para potencias del vaso (Arranque vs Mantenimiento).
* [Pendiente] Generación de informe justificativo imprimible detallando fórmulas MEP y balances de masa/energía.
* [Pendiente] Renderizado de ecuaciones con MathJax.



### 4. ESPECIFICACIONES DE LA VERSIÓN ACTUAL (v1.1)

* **Descripción:** Ampliación del ITR para incluir requerimientos de potencia térmica del agua. La UI se ajustará para contener un fieldset extra dedicado a "Parámetros del Vaso y Calentamiento". Los resultados mostrarán dos dictámenes: 1) Capacidad de Deshumectadora y 2) Potencia del Intercambiador de Calor.
* **Lógica de negocio:** Interconexión de módulos. La tasa de evaporación ($\dot{m}_e$) calculada en el bloque psicrométrico se inyectará dinámicamente como variable de entrada en el bloque de cálculo de pérdidas térmicas para asegurar la coherencia del modelo.
* **Manejo de errores:** Bloqueos si $T_{red} \ge T_w$ (arrojará un aviso de que el calentamiento inicial no es necesario). Se validará que el tiempo de calentamiento sea mayor a 0 para evitar divisiones por cero.

### 5. STACK TECNOLÓGICO

* HTML5 semántico.
* CSS3 puro (variables CSS corporativas, `@media print`).
* Vanilla JavaScript (ES6) - Arquitectura modular para separar lógica psicrométrica de lógica termodinámica.

### 6. BITÁCORA DE CAMBIOS (Changelog)

* **v1.0:** Creación del ITR inicial centrado exclusivamente en el balance de masa de agua (deshumectación).
* **v1.1:** Incorporación de ecuaciones de balance de energía para el cálculo de potencia térmica del vaso (calentamiento inicial y mantenimiento por pérdida latente y transmisión).

### 7. FUNDAMENTOS DE INGENIERÍA Y NORMATIVA

* **Sistema Internacional (SI) + Unidades Prácticas:** Se usará kWh y kW para energía y potencia térmica por ser el estándar comercial en intercambiadores y calderas.
* **Fórmulas adicionales:** * Energía latente: $Q_L = \dot{m} \cdot h_{fg}$
* Energía sensible: $Q_S = m \cdot c_p \cdot \Delta T$


* **Constantes:** $c_p$ = 1.16 Wh/(kg·ºC); $h_{fg}$ = 0.68 kWh/kg.

### 8. LIBRERÍAS TÉCNICAS

* HTML/JS: `MathJax` (v3).

### 9. IMPLEMENTACIÓN

*(Pendiente de aprobación)*

---

