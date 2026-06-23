# Calculadora de Caudal de Drenaje - CTE DB-HS5

**Versión:** 2.0  
**Método:** Fórmula C.3 (CTE DB-HS5, Apéndice C)  
**Última actualización:** Junio 2026

## 📋 Descripción

Herramienta de cálculo HTML5 para dimensionamiento de sistemas de drenaje en muros de sótanos. Implementa la metodología del **Código Técnico de la Edificación (CTE), Documento Básico HS-5**, válida para casos en los que el arranque del muro drenante **no alcanza la capa impermeable**.

Genera caudales de drenaje, radios de acción y dimensiona automáticamente cámaras de bombeo según la **Tabla 3.4 (CTE DB-HS5, Sección 3.3)**.

## 🎯 Características

- ✅ **Cálculo automático de s** desde cotas Z_NF y Z_drenaje
- ✅ **Radio de acción (R)** mediante fórmula R = 3000 · s · √Ks
- ✅ **Caudal unitario y total** con conversiones a m³/s, m³/h, L/s
- ✅ **Selección discreta de cámara** según Tabla 3.4 (CTE DB-HS5)
- ✅ **Informe profesional** con normas, comentarios técnicos y referencias
- ✅ **100% HTML5 vanilla** — sin dependencias, ejecución local
- ✅ **Descarga de informes** en HTML con estilos profesionales

## 📐 Parámetros de Entrada

| Parámetro | Símbolo | Unidad | Descripción |
|-----------|---------|--------|-------------|
| Coeficiente de permeabilidad | Ks | m/s | Del estudio geotécnico |
| Profundidad cara sup. capa impermeable - NF original | H | m | Desde superficie hasta capa impermeable |
| Profundidad cara sup. capa impermeable - NF drenado | h₀ | m | Nivel freático después de drenar |
| Cota Nivel Freático Original | Z_NF | m | Distancia desde superficie del terreno |
| Cota solera cámara/tubo drenante | Z_drenaje | m | Profundidad de la solera de drenaje |
| Longitud perímetro muro drenante | L | m | Perímetro total drenado |

## 🧮 Fórmulas Implementadas

### 1. Espaciamiento (s)
```
s = Z_drenaje - Z_NF
```
**Nota:** Z_drenaje siempre > Z_NF (el drenaje está más profundo)

### 2. Radio de Acción (R)
```
R = 3000 · s · √Ks
```
Determina el área de influencia efectiva del drenaje.

### 3. Caudal Unitario (q)
```
q = Ks · [0,73 + 0,27 · (H - h₀) / H] · (H² - h₀²) / (2R)
```
Caudal por metro lineal de muro drenante.

### 4. Caudal Total (Q)
```
Q_total = q · L
```
Caudal total para el perímetro completo.

## 📊 Tabla de Dimensionamiento de Cámaras (Tabla 3.4, CTE DB-HS5)

| Caudal (L/s) | Volumen mínimo (m³) |
|--------------|-------------------|
| ≤ 0,15 | 2,4 |
| ≤ 0,31 | 2,85 |
| ≤ 0,46 | 3,6 |
| ≤ 0,61 | 3,9 |
| ≤ 0,76 | 4,5 |
| ≤ 1,15 | 5,7 |
| ≤ 1,53 | 9,6 |
| ≤ 1,91 | 10,8 |
| ≤ 2,3 | 15 |
| ≤ 3,1 | 20 |
| > 3,1 | Segunda cámara |

**Criterio de selección:** Se elige el volumen correspondiente al primer rango que contiene el caudal calculado (selección discreta por mayorante).

## 🚀 Uso

### Requisitos
- Navegador moderno (Chrome, Firefox, Safari, Edge)
- No requiere conexión a internet
- No requiere instalación

### Pasos
1. Descarga `calculadora.html`
2. Abre el archivo en tu navegador (doble clic)
3. Ingresa los parámetros geotécnicos
4. Presiona "Calcular caudal q"
5. Opcionalmente, genera informe HTML con "Generar Informe HTML"

### Ejemplo de Cálculo
```
Ks:       1e-5 m/s
H:        4.50 m
h₀:       1.20 m
Z_NF:     2.50 m
Z_drenaje: 3.50 m
L:        120 m

Resultado:
s = 3.50 - 2.50 = 1.00 m
R = 3000 · 1.00 · √(1e-5) = 9.49 m
q = 1.23e-06 m³/(s·m)
Q_total = 0.000147 m³/s = 0.147 L/s
V_cámara = 2.4 m³
```

## 📝 Marco Normativo

- **CTE DB-HS5** — Evacuación de aguas (RD 314/2006)
  - Sección 3.3: Bombas de achique
  - Apéndice C: Cálculo del caudal de drenaje en muros
- **RD 1027/2007** — Instalaciones de bombeo de aguas residuales
- **RD 656/2017** — Almacenamiento de productos químicos (si aplica)
- **UNE-EN 1671** — Tuberías de drenaje en edificios

## 🔧 Estructura del Archivo

```
calculadora.html
├── <head>
│   ├── Meta tags (charset, viewport)
│   └── Estilos CSS embebidos
├── <body>
│   ├── Contenedor principal
│   ├── Diagrama SVG (Figura C.1)
│   ├── Fórmulas matemáticas
│   ├── Formulario de entrada
│   ├── Zona de resultados
│   └── Script JavaScript
└── Función generarInforme() con HTML completo
```

## 💡 Notas Técnicas

### Validaciones
- Todos los parámetros deben ser numéricos y positivos
- Z_drenaje > Z_NF (obligatorio)
- h₀ < H (obligatorio)
- Se valida automáticamente antes de calcular

### Salida del Informe
El informe generado incluye:
1. **Portada** con fecha y método
2. **Descripción del proyecto** y referencias normativas
3. **Tabla de parámetros** de entrada
4. **Cálculos paso a paso** con fórmulas
5. **Tabla de dimensionamiento** (CTE DB-HS5)
6. **Recomendaciones de diseño** (materiales, válvulas, mantenimiento)
7. **Conclusiones finales** y cumplimiento normativo

### Interpretación de Resultados

**Radio de acción bajo (R < 10 m):**
- Suelos de baja permeabilidad
- Requiere drenaje más denso

**Caudal alto (Q > 5 L/s):**
- Posible zona freática ascendente
- Considerar doble cámara de bombeo

**Volumen de cámara:**
- Mínimo CTE DB-HS5
- Tiempo de vaciado: máx. 24 h

## 📧 Contacto y Soporte

Para mejoras, reportes de errores o adaptaciones específicas a otros contextos normativos, contacta con el desarrollador.

**Descargo de responsabilidad:** Esta herramienta es una ayuda de cálculo. Es responsabilidad del proyectista verificar la aplicabilidad del método, validar los datos geotécnicos y asegurar el cumplimiento de toda la normativa aplicable en el proyecto.

---

**Licencia:** Uso libre para proyectos técnicos de ingeniería civil  
**Formato:** HTML5 standalone  
**Compatibilidad:** Windows, macOS, Linux (cualquier navegador moderno)
