# APP_CAL.Caudales-AA-v2
Calculo de caudales para bombeo en climatización. Generación de informe.

Actúas como ingeniero MEP senior. FASE 1: ANÁLISIS DE REQUISITOS.
Haz las preguntas necesarias para definir completamente el alcance: entradas, salidas, normativa aplicable, restricciones, casos de uso. Al terminar, resume los requisitos en un listado estructurado.
────────────────────────────────────────────────────────────

CONTEXTO DE LA HERRAMIENTA:
ID: MEP-002
Nombre: Calculo de Caudales para climatización
Disciplina MEP: HVAC
Tecnología objetivo: HTML/JS
Descripción: Calcular los caudales de agua para las bombas de climatización según la carga térmica y salto a vencer. Basándote en normativa actual y en formulas termodinámicas en SI, hay que calcular el caudal de bombeo necesario para una instalación de climatización tanto para frío como para caliente, según la carga térmica a vencer y condiciones necesarias.

Análisis de la FASE 1: Cálculo de Caudales de Bombeo (HVAC)Entiendo que el objetivo es desarrollar una herramienta de cálculo de caudales de agua ($V$) para circuitos de climatización, operando bajo las siguientes premisas técnicas y normativas:1. Variables Críticas Identificadas:Potencia Térmica ($P$): La carga total a vencer (kW) obtenida de los cálculos de cargas (frío/calor).Salto Térmico ($\Delta T$): Factor determinante para la eficiencia. Según el RITE (IT 1.2.4.2.1), los saltos térmicos deben optimizarse para reducir el consumo de bombeo.Estándar Típico: $5^\circ\text{C}$ en refrigeración y $10^\circ\text{C}$ o $20^\circ\text{C}$ en calefacción (calderas de condensación o aerotermia).Propiedades del Fluido: Consideración de la densidad ($\rho$) y el calor específico ($C_p$) del agua en función de la temperatura media de trabajo. Debemos prever la posibilidad de mezclas con etilenglicol (anticongelante), lo cual altera significativamente el caudal necesario.Régimen de Bombeo: Caudal constante vs. Caudal variable (según demanda).2. Marco Normativo de Referencia (España/EU):RITE (Reglamento de Instalaciones Térmicas en los Edificios): Especialmente la IT 1.2.4.2 sobre Eficiencia Energética en el Transporte de Fluidos, que limita la potencia específica de las bombas ($SFP$).UNE-EN 12828: Sistemas de calefacción en edificios.CTE HE-2: Rendimiento de las instalaciones térmicas.3. Límites del Cálculo:Para esta aplicación, el cálculo se centrará en la determinación del caudal nominal, asumiendo que la pérdida de carga del sistema (m.c.a.) será un dato de entrada o un cálculo secundario para la selección de la bomba, pero no el objeto principal de la fórmula termodinámica de caudal.

NORMATIVA APLICABLE:
  · RITE
  · CTE
  · Normas UNE
  · Formulas Termodinámicas
  · propiedades de los fluidos

────────────────────────────────────────────────────────────

ACCIÓN REQUERIDA: Haz las preguntas necesarias para definir el alcance antes de pasar al ITR.
