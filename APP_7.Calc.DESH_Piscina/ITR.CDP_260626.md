
# CALCULADORA AVANZADA DE DESHUMECTACIÓN EN PISCINAS CUBIERTAS

**ID_INFORME:** ITR.CDP_260626-01 | **VERSIÓN:** 1.0

**ESTADO:** ESPERANDO ACEPTACIÓN

### 1. ESPECIFICACIONES TÉCNICAS

* **Orden del Usuario:** Desarrollo de una aplicación web (SPA en fichero único) para el cálculo de la carga latente (kg/h de agua a deshumectar) en recintos de piscinas cubiertas, considerando evaporación de la lámina de agua, ocupación y carga por ventilación exterior. Se utilizará la interfaz visual proporcionada en el archivo base (colores corporativos, diseño en 3 pestañas).
* **Normativa:** * RITE (Reglamento de Instalaciones Térmicas en los Edificios).
* ASHRAE Handbook - HVAC Applications (Places of Assembly - Natatoriums).
* VDI 2089 (Building services in swimming baths).


* **Parámetros Validados:**
* *Superficie de agua ($A$):* [m²] (Validación: > 0)
* *Temperatura del agua ($T_w$):* [ºC] (Rango típico: 24ºC - 36ºC)
* *Temperatura del aire interior ($T_{int}$):* [ºC] (Norma general: $T_w$ + 1ºC a 3ºC para minimizar evaporación)
* *Humedad Relativa interior ($HR_{int}$):* [%] (Rango típico de confort y preservación: 55% - 65%)
* *Caudal de ventilación exterior ($Q_v$):* [m³/h] (Validación: $\ge$ 0)
* *Condiciones exteriores de diseño:* Se requerirá $T_{ext}$ [ºC] y $HR_{ext}$ [%] para calcular psicrométricamente la carga de ventilación exacta de cualquier ubicación, en lugar de predefinir un delta fijo.
* *Ocupantes ($N$):* Número de personas.
* *Factor de Actividad ($F$):* Seleccionable (Uso privado/tranquilo, Uso público/medio, Uso SPA/agitado).
* *Margen de Seguridad:* [%] (Recomendado 15-20%).


* **Hipótesis y simplificaciones adoptadas:**
* Presión atmosférica estándar asumida a nivel del mar ($P_{atm}$ = 101.325 kPa) para el cálculo de la humedad absoluta.
* Se asume mezcla perfecta del aire en el recinto.
* La carga de ocupación se fija conservadoramente en 0.15 kg/h por persona (actividad ligera-media en entorno húmedo).



### 2. LÓGICA DE INGENIERÍA Y CÁLCULOS

Se ha indexado la metodología simplificada aportada y se ha escalado a un modelo psicrométrico analítico completo para que la aplicación calcule rigurosamente las propiedades del aire basándose en las temperaturas introducidas por el usuario.

* **Check de datos aportados:** Comprendida la relevancia del caudal de ventilación exterior (muy penalizante en climas húmedos y cálidos como Benahavís en verano).
* **Desarrollo analítico psicrométrico:**
Para obtener la presión de saturación del vapor de agua ($P_{sat}$) a una temperatura $T$ [ºC], se empleará la aproximación de Magnus-Tetens:

$$P_{sat}(T) = 0.6112 \cdot \exp\left(\frac{17.67 \cdot T}{T + 243.5}\right) \text{ [kPa]}$$


*Presión parcial del vapor en el aire ($P_a$):*

$$P_a = P_{sat}(T_{aire}) \cdot \frac{HR}{100} \text{ [kPa]}$$


*Humedad Absoluta ($x$):*

$$x = 0.622 \cdot \frac{P_a}{P_{atm} - P_a} \text{ [kg agua / kg aire seco]}$$


* **Desglose de Cargas (Formulación MEP):**
1. **Evaporación de la lámina de agua ($\dot{m}_e$):** Ecuación basada en ASHRAE/VDI.

$$\dot{m}_e = A \cdot (P_w - P_{a,int}) \cdot F \text{ [kg/h]}$$



Donde $P_w = P_{sat}(T_w)$, y $F$ es el factor de actividad empírico (ej. 0.12 para SPA).
2. **Carga por ocupación ($\dot{m}_p$):**

$$\dot{m}_p = N \cdot 0.15 \text{ [kg/h]}$$


3. **Carga latente por ventilación ($\dot{m}_v$):**

$$\dot{m}_v = Q_v \cdot \rho_{aire} \cdot (x_{ext} - x_{int}) \text{ [kg/h]}$$



*(Nota: Si $x_{int} > x_{ext}$, el aire exterior seca el ambiente y el término se vuelve favorable/negativo, pero para diseño de deshumectadora en verano se asume el caso pésimo).*
4. **Carga Total de Diseño ($\dot{m}_{total}$):**

$$\dot{m}_{total} = (\dot{m}_e + \dot{m}_p + \dot{m}_v) \cdot \left(1 + \frac{\text{Margen}}{100}\right) \text{ [kg/h]}$$





### 3. HISTORIAL DE REQUERIMIENTOS ACUMULADOS

* **Visión General y Origen:** Inicialización del proyecto basado en el prompt de la v1.0 y la plantilla HTML aportada.
* **Historial de Requerimiento:**
* [Pendiente] Interfaz con 3 pestañas (Panel de Cálculo, Informe, Documentación).
* [Pendiente] Input de datos de geometría, condiciones interiores, exteriores y ocupación.
* [Pendiente] Motor de cálculo psicrométrico en JS.
* [Pendiente] Generación de informe justificativo imprimible detallando fórmulas.
* [Pendiente] Uso de MathJax para renderizar ecuaciones dinámicas en LaTeX.



### 4. ESPECIFICACIONES DE LA VERSIÓN ACTUAL (v1.0)

* **Descripción:** Creación del armazón HTML/CSS/JS replicando el estilo `elecnor-blue` (004687) y `elecnor-orange` (FF8200). Se estructurarán tres fieldsets principales: Parámetros del Recinto/Agua, Parámetros de Ocupación/Actividad, y Condiciones Exteriores/Ventilación.
* **Lógica de negocio:** Se implementará un script nativo en JS que intercepte los cambios en los inputs, ejecute las ecuaciones psicrométricas y actualice el DOM en tiempo real, mostrando la comparativa de cargas (Evaporación vs. Personas vs. Ventilación) y el resultado final con margen de seguridad.
* **Manejo de errores:** Bloqueo de cálculos y avisos visuales si $T_{aire} < T_{agua}$ (riesgo extremo de evaporación cruzada) o si caudales son negativos.

### 5. STACK TECNOLÓGICO

* HTML5 semántico.
* CSS3 puro (sin frameworks, variables CSS nativas, media queries para responsive y `@media print` para exportación del informe a PDF).
* Vanilla JavaScript (ES6) para la lógica de cálculo y manipulación del DOM.

### 6. BITÁCORA DE CAMBIOS (Changelog)

* **v1.0**: Creación del Informe Técnico de Requisitos inicial basado en los datos de partida proporcionados por el usuario. Formalización de la metodología de cálculo empírico hacia cálculo psicrométrico normativo.

### 7. FUNDAMENTOS DE INGENIERÍA Y NORMATIVA

* **Sistema Internacional (SI):** Todas las variables operan en kg, m, s, kW, kPa.
* **Marco Normativo:** ASHRAE Fundamentals (Psicrometría), VDI 2089 (Piscinas), RITE (Ventilación en locales).
* **Fórmulas:** Detalladas en la Sección 2. Se empleará presión en kPa y Humedad Absoluta para un rigor termodinámico absoluto en el balance de masa.

### 8. LIBRERÍAS TÉCNICAS

* HTML/JS: Integración de `MathJax` (v3, vía CDN) para renderizado elegante de las ecuaciones. Cero dependencias externas adicionales para asegurar la autocontención del archivo.

### 9. IMPLEMENTACIÓN

*(Pendiente de aprobación)*

---

⚠️ ESPERANDO ACEPTACIÓN TÉCNICA. Por favor, revisa los puntos anteriores y escribe 'ACEPTO INFORME' para proceder con el código, o indica los cambios necesarios. En caso de aceptar el informe, solicita también el lenguaje en cual se hará el código.
