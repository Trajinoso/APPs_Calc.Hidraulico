# ENTORNO DE INSTALACIONES MEP: CALCULADORA DE DESHUMECTACIÓN Y TÉRMICA DE PISCINAS
**ID_INFORME:** ITR.CDP_260626-01 | **VERSIÓN:** 1.2  
**ESTADO:** ACEPTADO E IMPLEMENTADO

Este repositorio contiene la herramienta profesional autocontenida para el dimensionado mecánico y de fluidos en piscinas cubiertas climatizadas (natatorios), garantizando trazabilidad completa desde los requisitos técnicos hasta las ecuaciones de balance de masa y energía.

## HISTORIAL ACUMULADO DE INFORMES TÉCNICOS (ITR)

### Versión 1.0 - Balance de Masa Básico
* **Alcance:** Modelado analítico de la carga latente de deshumectación ($\dot{m}_{total}$) considerando evaporación libre superficial, ocupación y caudal de renovación exterior.
* **Normativa:** ASHRAE Handbook (Natatoriums), RITE.

### Versión 1.1 - Balance de Energía Térmica
* **Alcance:** Adición del módulo de transferencia de calor para el agua del vaso. Diferenciación entre la potencia de arranque inicial (sensible en función del tiempo de puesta en régimen) y potencia de mantenimiento continuo (pérdidas latentes por evaporación + pérdidas por transmisión estructural).

### Versión 1.2 - Optimización Geométrica y Psicrometría Vectorial (Versión Actual)
* **Cambio Geométrico:** Sustitución de la entrada manual de volumen por el cálculo automatizado en función de la profundidad media del vaso ($V = A \cdot h_{vaso}$).
* **Lógica de Ventilación:** Corrección algebraica según la ecuación exacta del flujo de masa de agua extraído o introducido por la ventilación:
    $$\dot{m}_v = Q_v \cdot \rho_{aire} \cdot (x_i - x_e)$$
    Donde si el aire exterior es más seco ($x_i > x_e$), la ventilación actúa con un vector de secado favorable reduciendo de forma neta la carga demandada a la deshumectadora frigorífica.
* **Motor Psicrométrico:** Implementación de las ecuaciones de estado basadas en la formulación de Magnus-Tetens para la obtención automática de humedades absolutas sin requerir ábacos manuales.

---

## BITÁCORA DE CAMBIOS (CHANGELOG)
* **v1.0 (26/06/2026):** Definición de variables base y coeficientes de actividad de la lámina de agua.
* **v1.1 (26/06/2026):** Interconexión de la tasa de evaporación libre con las necesidades de intercambio de placas térmicas.
* **v1.2 (26/06/2026):** Refactorización del balance psicrométrico de ventilación a formato algebraicó neta y automatización de volumen del vaso por cota hidráulica.

---

## STACK TECNOLÓGICO
* **Frontend:** HTML5 Semántico y CSS3 con optimización de hojas de estilo para impresión A4 (Ingeniería Report-Ready).
* **Cálculo:** JavaScript ES6 Nativo (Vanilla JS) sin dependencias de servidor.
* **Renderizado Matemático:** MathJax v3 para visualización de ecuaciones dinámicas en LaTeX.