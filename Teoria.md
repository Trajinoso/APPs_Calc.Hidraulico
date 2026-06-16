# Anexo Técnico: Justificación Hidrodinámica del Punto Crítico en Pozos de Bombeo

Este documento técnico detalla la justificación matemática y física del **divisor 4** en la fórmula de dimensionamiento del volumen útil $V_u$. Su propósito es aportar la argumentación necesaria para memorias de proyecto o debates técnicos, demostrando por qué este método es el único que garantiza la protección del motor en el escenario más desfavorable.

---

## 1. El Concepto del Ciclo Dinámico

En una Estación de Bombeo de Aguas Residuales, el agua de aporte $Q_{ap}$ entra de forma continua al pozo, mientras que la bomba extrae agua de forma intermitente a un caudal constante $Q_b$.

El tiempo total de un ciclo completo $T$, que define el intervalo entre dos arranques consecutivos del motor, es la suma de dos fases diferenciadas:

1. **Tiempo de llenado $t_{ll}$:** La bomba está parada. El pozo se llena desde el nivel mínimo hasta el de arranque debido únicamente al caudal de aportación $Q_{ap}$.
2. **Tiempo de vaciado $t_v$:** La bomba está en funcionamiento. El pozo se vacía desde el nivel de arranque hasta el de parada a una velocidad neta equivalente a la diferencia entre lo que extrae la bomba y lo que sigue entrando, expresada como $Q_b - Q_{ap}$.

Matemáticamente, expresamos el tiempo total del ciclo $T$ en horas como:

$$T = t_{ll} + t_v = \frac{V_u}{Q_{ap}} + \frac{V_u}{Q_b - Q_{ap}}$$

Al agrupar los términos en una sola fracción mediante común denominador, la ecuación se transforma en:

$$T = \frac{V_u \cdot (Q_b - Q_{ap}) + V_u \cdot Q_{ap}}{Q_{ap} \cdot (Q_b - Q_{ap})} = \frac{V_u \cdot Q_b}{Q_{ap} \cdot (Q_b - Q_{ap})}$$

---

## 2. Deducción del Punto Crítico: El Peor Escenario

El número de arranques por hora $n$ es la inversa del tiempo total del ciclo, de modo que $n = 1/T$. Por lo tanto, la frecuencia de arranques en función del caudal de entrada se define mediante la ecuación:

$$n(Q_{ap}) = \frac{Q_{ap} \cdot (Q_b - Q_{ap})}{V_u \cdot Q_b}$$

Como diseñadores, el objetivo no es calcular el pozo para un caudal fijo, sino **asegurar que el motor jamás supere su límite de arranques, independientemente de cómo varíe el caudal de aportación a lo largo del día**.

Para encontrar qué caudal de aporte $Q_{ap}$ produce la mayor cantidad de arranques por hora, aplicamos el cálculo diferencial. Derivamos la función $n(Q_{ap})$ respecto a $Q_{ap}$ e igualamos a cero para localizar su punto máximo:

$$\frac{dn}{dQ_{ap}} = \frac{1}{V_u \cdot Q_b} \cdot \frac{d}{dQ_{ap}} (Q_{ap} \cdot Q_b - Q_{ap}^2) = 0$$

$$\frac{1}{V_u \cdot Q_b} \cdot (Q_b - 2Q_{ap}) = 0$$

Al despejar $Q_{ap}$, obtenemos el criterio fundamental de diseño:

$$Q_{ap} = \frac{Q_b}{2}$$

> **Conclusión Física:** La situación más desfavorable, que se traduce en el menor tiempo de ciclo y el máximo número de arranques, ocurre exactamente cuando **el caudal que entra al pozo es la mitad del caudal que la bomba es capaz de extraer**. En este punto simétrico, el tiempo de llenado es idéntico al tiempo de vaciado, es decir, $t_{ll} = t_v$.

---

## 3. Obtención del Divisor 4

Una vez determinada la condición del peor escenario posible, introducemos este valor en la ecuación de la frecuencia de arranques para conocer el número máximo de ciclos $n_{max}$ que sufrirá el motor:

$$n_{max} = \frac{\frac{Q_b}{2} \cdot (Q_b - \frac{Q_b}{2})}{V_u \cdot Q_b}$$

Operando el numerador:

$$n_{max} = \frac{\frac{Q_b}{2} \cdot \frac{Q_b}{2}}{V_u \cdot Q_b} = \frac{\frac{Q_b^2}{4}}{V_u \cdot Q_b}$$

Simplificando el término $Q_b$ en el numerador y el denominador, la ecuación se reduce a:

$$n_{max} = \frac{Q_b}{4 \cdot V_u}$$

Finalmente, al despejar el **Volumen Útil $V_u$**, obtenemos la expresión estándar exigida en alta ingeniería y empleada en los manuales de los principales fabricantes de bombas como Grundfos, KSB o Flygt:

$$V_u = \frac{Q_b}{4 \cdot n_{max}}$$

### Definición de Variables en el Sistema Internacional:

* **$V_u$**: Volumen útil del depósito de recepción en metros cúbicos. Representa la capacidad neta comprendida entre el nivel de arranque y el nivel de parada.
* **$Q_b$**: Caudal nominal de la bomba en metros cúbicos por hora en su punto de trabajo real, definido por la intersección con la curva resistente de la tubería de impulsión.
* **$n_{max}$**: Número máximo de arranques por hora permitidos por el fabricante del motor, cuyo valor típico oscila entre 6 y 15 arranques por hora según la potencia y el tipo de arrancador.

---

## 4. Argumentación Técnica para la Defensa del Diseño

Si un revisor o un cliente plantea utilizar la fórmula estática simplificada $V_u = T \cdot Q_b$, esta deducción demuestra que dicho método asume erróneamente que el caudal de aporte es nulo mientras la bomba se vacía.

Al aplicar el **divisor 4**, estás justificando científicamente dos ventajas clave:

1. **Seguridad absoluta del motor:** El pozo se diseña para absorber la peor combinación de flujos posible. Si el caudal de aporte real del proyecto es mayor o menor a $Q_b/2$, la bomba arrancará menos veces de las previstas, trabajando siempre en una zona segura.
2. **Prevención de la septicidad:** Al no sobredimensionar el depósito un 400%, lo que ocurriría sin el divisor 4, impides el estancamiento prolongado del agua residual. Esto minimiza la generación de ácido sulfhídrico, eliminando los malos olores y la corrosión prematura en los equipos y la obra civil.
