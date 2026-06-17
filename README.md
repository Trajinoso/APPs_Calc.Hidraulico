# Calc Pozo Fecales
### Calculador de Pozo de Bombeo de Aguas Residuales Fecales
**Versión actual:** v2.2 | **Normativa:** CTE DB-HS5 (Tabla 4.1) | **Tecnología:** HTML5 + JS (autocontenido)

---

## Descripción general

**Calc Pozo Fecales** es una herramienta de cálculo técnico para el dimensionado de pozos de bombeo de aguas residuales fecales, desarrollada para uso en Oficina Técnica MEP. Permite determinar, de forma encadenada y automática, el caudal de aporte a partir de las Unidades de Descarga de los aparatos sanitarios instalados, el volumen útil del pozo según el criterio de fabricantes de bombas sumergidas (Grundfos, KSB, Flygt), la verificación del ciclo de bombeo y la geometría total del pozo.

La herramienta es un archivo HTML autocontenido, sin dependencias externas de servidor ni frameworks, con exportación de memoria de cálculo en PDF mediante jsPDF.

---

## Características principales

- Tabla de aparatos sanitarios completa según **CTE DB-HS5 Tabla 4.1** (21 tipos de aparato, uso privado/público)
- Cálculo de caudal de aporte por fórmula normativa: `Q_ap = 0,5 · √UD`
- Dimensionado de volumen útil con fórmula estándar de fabricantes: `Vu = Qb(m³/h) / (4 · n_max)`
- Visualización simultánea de Vu calculado y Vu aplicado (con aviso si Vu_calc < 1 m³)
- Verificación de ciclo de bombeo: tiempos de llenado/vaciado, arranques reales por hora
- Módulo de geometría total del pozo con diagrama SVG dinámico (H₁, Hn, H₂, h, Ht)
- Encadenamiento automático entre módulos: los resultados fluyen sin intervención manual
- Exportación de memoria de cálculo en **PDF** con diseño oscuro técnico
- Interfaz dark industrial, optimizada para pantalla de escritorio

---

## Módulos de cálculo

### Módulo 1 — Caudal de aporte por Unidades de Descarga (UD)
- Tabla interactiva con los 21 aparatos sanitarios de CTE DB-HS5 tabla 4.1
- Toggle uso privado / uso público que actualiza las UD aplicables en tiempo real
- Cálculo automático de UD parciales y UD totales acumuladas
- Fórmula de conversión: `Q_ap = 0,5 · √UD_totales` (l/s)
- Resultado propagado automáticamente al Módulo 2

### Módulo 2 — Volumen útil del pozo (Vu)
- Caudal de aporte recibido en lectura desde Módulo 1 (campo readonly)
- Caudal de bomba: `Q_b = 1,25 · Q_ap`
- Volumen útil: `Vu = Q_b(m³/h) / (4 · n_max)` — fórmula estándar Grundfos/KSB/Flygt
  - Condición de derivación: ciclo mínimo + arranques máximos → `Q_aporte = Q_b / 2`
- Se muestra el **Vu calculado** directamente, sin aplicar ningún mínimo
- Toda la cadena de cálculo posterior (Hn, ciclo de bombeo, geometría) usa este Vu calculado
- Altura útil: `Hn = Vu / (π · (D/2)²)`

### Módulo 3 — Ciclo de bombeo y verificación de arranques
- Q bomba propagado automáticamente desde Módulo 2
- Cálculo de tiempos: `t_ll = Vu / Q_ap`, `t_v = Vu / (Q_b - Q_ap)`
- Ciclo total: `T = t_ll + t_v` | Arranques reales: `n_real = 60/T`
- Verificación semáforo: ✓ CUMPLE / ✗ EXCEDE respecto a n_max admisible

### Módulo 4 — Geometría total del pozo
- Entradas: H₁ (base bomba), H₂ (seguridad sobre nivel máximo), h (enterramiento/solera)
- Altura útil Hn propagada automáticamente desde Módulo 2
- Profundidad total: `Ht = H₁ + Hn + H₂ + h`
- Diagrama SVG dinámico proporcional a los valores introducidos con cotas etiquetadas

### Memoria de cálculo
- Resumen completo de todos los módulos con trazabilidad de fórmulas y valores intermedios
- Exportación en **PDF** (jsPDF, diseño oscuro técnico, paginación automática)

---

## Normativa y referencias técnicas

| Referencia | Aplicación |
|---|---|
| CTE DB-HS5 | Evacuación de aguas. Tabla 4.1: UDs por aparato sanitario |
| Grundfos Engineering Manual | Fórmula Vu = Qb/(4·n_max) |
| KSB — Sewage Pumping Stations | Criterio de ciclo mínimo / arranques máximos |
| Flygt — Pump Station Design Guide | Confirmación coeficiente 1/4 en fórmula de Vu |

---

## Tabla de aparatos sanitarios (CTE DB-HS5 tabla 4.1)

| # | Aparato | Subtipo | UD privado | UD público |
|---|---|---|---|---|
| 1 | Lavabo | — | 1 | 2 |
| 2 | Bidé | — | 2 | 3 |
| 3 | Ducha | — | 2 | 3 |
| 4 | Bañera (con o sin ducha) | — | 3 | 4 |
| 5 | Inodoro | Con cisterna | 4 | 5 |
| 6 | Inodoro | Con fluxómetro | 8 | 10 |
| 7 | Urinario | Pedestal | — | 4 |
| 8 | Urinario | Suspendido | — | 2 |
| 9 | Urinario | En batería | — | 3,5 |
| 10 | Fregadero | De cocina | 3 | 6 |
| 11 | Fregadero | Laboratorio/restaurante | — | 2 |
| 12 | Lavadero | — | 3 | — |
| 13 | Vertedero | — | — | 8 |
| 14 | Fuente para beber | — | — | 0,5 |
| 15 | Sumidero sifónico | — | 1 | 3 |
| 16 | Lavavajillas | — | 3 | 6 |
| 17 | Lavadora | — | 3 | 6 |
| 18 | Cuarto de baño (lav+inod+bañ+bidé) | Inodoro con cisterna | 7 | — |
| 19 | Cuarto de baño (lav+inod+bañ+bidé) | Inodoro con fluxómetro | 8 | — |
| 20 | Cuarto de aseo (lav+inod+ducha) | Inodoro con cisterna | 6 | — |
| 21 | Cuarto de aseo (lav+inod+ducha) | Inodoro con fluxómetro | 8 | — |

---

## Bitácora de desarrollo

### v2.2 — 2026-06-16
**Cambios:**
- Módulo 2: eliminado el mínimo de 1 m³. El **Vu calculado** (`Qb / 4·nmax`) se usa directamente en toda la cadena de cálculo sin corrección posterior.
- Altura útil Hn, ciclo de bombeo (t_ll, t_v, T, n_real) y geometría total (Ht) calculados siempre con el Vu real de la fórmula.
- Eliminados los campos "Vu aplicado" y aviso ⚠ — interfaz simplificada a tres tarjetas de resultado en Módulo 2: Vu (m³), Vu (litros), Hn (m).
- README actualizado a v2.2 con bitácora completa.

### v2.0 — 2026-06-16
**Cambios mayores respecto a v1.0:**
- **Módulo 1 corregido:** fórmula `Q_ap = 0,5 · √UD` mostrada directamente en l/s y m³/h, igual que en Módulo 2.
- **Módulo 2 corregido:** campo Q_aporte recibe valor propagado del Módulo 1 en l/s mediante campo readonly (eliminada desconexión entre módulos). Toda la cadena de cálculo usa este valor sin conversión errónea.
- **Módulo 4 — Geometría del pozo (NUEVO):** entradas H₁, H₂, h (enterramiento) + Hn propagado automáticamente. Cálculo de profundidad total `Ht = H₁ + Hn + H₂ + h`. Diagrama SVG dinámico proporcional con cotas etiquetadas y zonas coloreadas por tramo.
- **Exportación PDF:** reemplaza exportación .txt anterior. Usa jsPDF (CDN Cloudflare). Diseño oscuro técnico con cabecera, separadores, tipografía monoespaciada, paginación automática y pie de página con fecha.
- Diagrama SVG del pozo con representación de: nivel de suelo, nivel máximo, nivel mínimo, bomba sumergida, tubería de entrada, cotas H₁/Hn/H₂/h/Ht.
- Encadenamiento completo entre los 4 módulos: cualquier cambio en la tabla de aparatos recalcula toda la cadena en cascada.

### v1.0 — 2026-06-16
**Primera versión funcional:**
- Módulo 1: tabla de 21 aparatos según CTE DB-HS5 tabla 4.1 con toggle uso privado/público. Cálculo de UD totales y conversión a Qd mediante `0,682 · UD^0,45 − 0,14` (UD ≥ 5) o ΣQmín (UD < 5).
- Módulo 2: volumen útil con fórmula `Vu = Qd × 60 / (4 × n_max)` en l/s (fórmula preliminar, corregida en v2.0).
- Módulo 3: ciclo de bombeo — tiempos de llenado/vaciado, arranques reales, verificación semáforo OK/EXCEDE.
- Exportación de memoria en formato .txt.
- Interfaz dark industrial con tarjetas de resultado, fórmulas en caja monoespaciada y tabla de aparatos con UD parciales.

---

## Flujo de cálculo encadenado

```
[Tabla aparatos] → UD totales
       ↓
Q_ap = 0,5 · √UD  [l/s]
       ↓
Q_b = 1,25 · Q_ap  [m³/h]
       ↓
Vu_calc = Q_b / (4·n_max)  [m³]
Vu_aplic = max(Vu_calc, 1,0 m³)
       ↓
Hn = Vu_aplic / (π·r²)  [m]
       ↓
t_ll, t_v, T, n_real → Verificación arranques
       ↓
Ht = H₁ + Hn + H₂ + h  [m]  → Geometría total
```

---

## Tecnología

- **HTML5 + CSS3 + JavaScript vanilla** — sin frameworks, sin build tools
- **jsPDF 2.5.1** (CDN: cdnjs.cloudflare.com) — generación de PDF en cliente
- **SVG inline** — diagrama de pozo dinámico y proporcional
- Archivo único autocontenido, ejecutable directamente en navegador

---

## Autor y contexto

Desarrollado para uso en Oficina Técnica MEP (Instalaciones Mecánicas, Eléctricas y de Fontanería/Saneamiento). Área de aplicación: proyectos de edificación e infraestructuras en España bajo normativa CTE y reglamentos complementarios.
