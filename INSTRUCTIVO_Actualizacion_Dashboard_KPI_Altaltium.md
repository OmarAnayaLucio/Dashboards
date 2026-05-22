# INSTRUCTIVO DE ACTUALIZACIÓN — Dashboard KPI Ejecutivo
## Altaltium Real Estate Solutions
**Para uso del Analista de Datos + Prompt de contexto para Claude (IA)**
**Versión: 2.0 · Preparado por: Gerente de Marketing · Denisse Hansen**

---

> **¿Cómo usar este documento?**
> Este instructivo tiene **dos destinatarios**:
> - 📋 **El Analista:** secciones marcadas con 🔵 describen qué hacer, qué subir y qué verificar.
> - 🤖 **Claude (IA):** secciones marcadas con 🟢 indican exactamente dónde va cada dato dentro del `index.html`.
>
> **Flujo de trabajo:**
> 1. El analista sube los archivos **uno por uno** en el orden indicado.
> 2. Claude reporta los datos extraídos de cada archivo (totales, desglose, anomalías) en el **chat**, como texto.
> 3. El analista confirma o corrige.
> 4. Se repite hasta terminar todos los archivos.
> 5. **Una vez confirmados TODOS los datos, Claude genera el `index.html` actualizado completo.** No se hacen actualizaciones parciales.

---

## ⚙️ ANTES DE EMPEZAR — DATOS QUE CLAUDE DEBE SOLICITAR

Al iniciar una sesión de actualización, Claude debe pedir obligatoriamente:

1. **Período a reportar** → fecha de inicio y fecha de fin, incluyendo hora. Ejemplo: `"8 de mayo 2026, 09:00 — 15 de mayo 2026, 23:59"`
2. **Rotación de bloques de esa semana** → indicada por Denisse. Ejemplo: `"B1 → V4, B2 → V1, B3 → V2"`. Si no se indica, preguntar antes de continuar.

Si falta cualquiera de estos dos datos, **no iniciar la carga de archivos**.

---

## 🏗️ ESTRUCTURA DEL EQUIPO — REFERENCIA ESENCIAL

Claude debe tener presente esta estructura para clasificar correctamente cada código que aparezca en los archivos.

### Bloques rotativos (B1, B2, B3)

Tres bloques de asesores que rotan de portal asignado cada semana. Solo reciben de **V1, V2 y V4**. El portal asignado cada semana es indicado por Denisse.

- Todos los leads del portal asignado llegan primero al **asesor en turno** del bloque.
- Si el asesor no atiende en 5 minutos, el lead se traspasa en **cascada** al siguiente asesor del bloque.

### Ventas 3 (V3) — Gerencia aparte, no rota

- Asesores fijos de Ventas 3 más **opcionadores**.
- V3 es el portal de **propiedades de venta tradicional**. Los leads llegan directamente al **opcionador de la propiedad**, identificado por el código de publicación (ej. `THMK26`, `RSPV`, etc.).
- Si el opcionador no atiende en 5 minutos, el lead baja en cascada al siguiente opcionador.
- **V3 no se homologa en el dashboard de pasantes.** Sus métricas van aparte.

### Tabla de categorías de personas

| Categoría | Códigos conocidos | Métricas que aplican |
|-----------|-------------------|---------------------|
| **Pasantes** | LR01, AJ05, BR04, NA03, JR02, LM06, EL07, ALC / ALC-RU | Registro CRM + Traspaso |
| **MKT-AG** | MKT-AG | Registro CRM + Traspaso + Origen (Redes / Cambaceo / MKT) |
| **Teléfono de Ventas** | MM99, DIr1, VG5 | Solo Registro CRM |
| **Equipo Omar (histórico)** | LP36, YR05, CB34, FL19, VS37, AB2, CV4 y variantes | No deberían tener publicaciones activas. Si aparece uno de estos códigos, significa que esa publicación no ha sido homologada al pasante que la gestiona. El registro y traspaso de esos leads lo hacen los **pasantes**, no el equipo Omar. |
| **Opcionadores V3** | THMK26, KRMK26, RSPV, SRMK, SVMK, MQi02, SLi05, y otros | No se homologan en el dashboard de pasantes |

> ⚠️ **Asesores removidos por inactividad:** Gael Dorantes (ID:60), Jesus Salas (ID:83), Astrid Medina (ID:64). Si aparecen en cualquier archivo del período **actual**, reportar a Denisse antes de incluirlos.
>
> ⚠️ **Equipo Omar:** Los 200 espacios del ex-equipo Omar fueron reemplazados por 60 espacios con Teléfono de Ventas (MM99, DIr1, VG5). Si aparece un código del equipo Omar en un período actual, significa que esa publicación **no ha sido homologada** al pasante que la gestiona. Reportarlo a Denisse. El registro y traspaso de esos leads corresponde a los **pasantes**, no al código Omar.

---

## 🗂️ ORDEN ESTRICTO DE CARGA DE ARCHIVOS

| # | Archivo | Nombre esperado | Datos que aporta |
|---|---------|----------------|-----------------|
| 1 | CRM | `crm_leads_[periodo].csv` | Registros, transferencias, pipeline, ventas |
| 2 | Interesados V1 | `Interesados_V1-[periodo].xlsx` | Interesados únicos y por código en V1 |
| 3 | Interesados V2 | `Interesados_V2-[periodo].xlsx` | Interesados únicos y por código en V2 |
| 4 | Interesados V3 + V4 | `Interesados_V3-[periodo].xlsx` + `Interesados_V4-[periodo].xlsx` | Interesados V3 (opcionadores) y V4 |
| 5 | Rendimiento V1 | `Rendimiento_periodo_V1-[periodo].xlsx` | Exposición + visualizaciones V1 (confirma estructura) |
| 6 | Rendimiento V2, V3, V4 | (los tres juntos) | Exposición + visualizaciones V2, V3, V4 |
| 7 | Citas | `CITAS_[periodo].xlsx` | Detalle de citas agendadas y atendidas |

> Si el archivo de citas no está listo al momento de generar, se crea el dashboard con un **placeholder** y se actualiza después.

---

## 📤 QUÉ REPORTA CLAUDE DESPUÉS DE CADA ARCHIVO

Tras procesar cada archivo, Claude presenta en el **chat** (como texto, no como archivo):

1. **Totales del período** relevantes del archivo.
2. **Desglose por código / categoría.**
3. **Hallazgos:** códigos nuevos, anomalías, valores en cero inesperados, inconsistencias.
4. **Preguntas de clasificación** si hay códigos no reconocidos.

El analista confirma o corrige. Solo entonces se pasa al siguiente archivo.

---

---

# PASO 1 — CRM

## 🔵 Para el Analista

**¿Qué subir?** `crm_leads_[periodo].csv` — reporte semanal con leads registrados y transferidos.

**¿Qué debe contener?** Columnas mínimas: nombre/código del registrador, asesor asignado, estado (transferido / pendiente / pipeline), fecha de registro.

**Checklist de verificación tras el reporte de Claude:**
- [ ] ¿El total de leads registrados = suma de todos los pasantes + MKT-AG + Teléfono de Ventas?
- [ ] ¿Los leads transferidos ≤ leads registrados?
- [ ] ¿Pendientes = registrados − transferidos?
- [ ] ¿Cada asesor de los bloques tiene sus leads asignados?
- [ ] ¿Hay leads sin asesor (S/H = sin hogar)?
- [ ] ¿Aparecen códigos del equipo Omar o asesores removidos?

---

## 🟢 Para Claude (IA) — Datos a extraer y dónde van

**Extraer del CRM:**
- Leads registrados por código (pasante / MKT-AG / Teléfono de Ventas)
- Leads transferidos por código
- Leads recibidos (asignados) por asesor → popula `leadsMap`
- Total CRM, total transferidos, pendientes, pipeline abierto

**Métricas calculadas:**
- Tasa de traspaso = transferidos / registrados × 100
- Tasa de registro = total CRM / total interesados portal × 100 *(se calcula al tener portales)*

**Dónde va en `index.html`:**

```javascript
// leadsMap — nombre exacto del asesor tal como aparece en const DATA
const leadsMap = {
  "[Nombre Asesor]": [número],
  ...
};
```

```html
<!-- KPI: Leads registrados CRM -->
<div class="kv">[TOTAL_CRM]</div>
<div class="ks">Pasantes=[#] · MKT-AG=[#] · Tel.Ventas=[#] · S/H=[#]</div>

<!-- KPI: Leads transferidos -->
<div class="kv">[TOTAL_TRANSF]</div>

<!-- KPI: Pendientes traspaso -->
<div class="kv">[PENDIENTES]</div>
<div class="ks">[TOTAL_CRM] registrados - [TOTAL_TRANSF] transferidos</div>

<!-- KPI: Tasa traspaso -->
<div class="kv">[TASA_TRASPASO]%</div>

<!-- KPI: Pipeline abierto -->
<div class="kv">[PIPELINE]</div>

<!-- Alerta MKT-AG -->
<strong>Área MKT (MKT-AG):</strong> [#] leads registrados · [#] transferidos · Origen: Redes Sociales / Cambaceo / Marketing.
```

```javascript
// Cards de pasantes — Registrados CRM y Transferidos
// (Interesados portal se completa en Paso 2-4)

// renderBlockView — crm y transf por bloque
renderBlockView('b1', '[PORTAL_B1]', '[COLOR]',
  [{code:'[COD]', int:[INT], crm:[CRM_PAS]}, ...],
  {int:[INT], exp:[EXP], vis:[VIS],
   crm:[CRM_BLOQUE], transf:[TRANSF_BLOQUE], transfPct:'[PCT]%', pend:[PEND],
   cag:[CITAS_AG], cat:[CITAS_AT]});
// Repetir b2, b3, v3

// WEEKLY — campo crm y seguimiento de la semana actual
global: { ..., crm: [TOTAL_CRM], seguimiento: [PIPELINE], ... }
```

---

---

# PASO 2 — INTERESADOS V1

## 🔵 Para el Analista

**¿Qué subir?** `Interesados_V1-[periodo].xlsx`

**Checklist de verificación:**
- [ ] ¿El total de interesados V1 es coherente con semanas anteriores?
- [ ] ¿Los códigos corresponden a pasantes asignados a V1 según el rol?
- [ ] ¿Hay códigos no reconocidos o inesperados?

---

## 🟢 Para Claude (IA)

**Extraer:**
- Interesados únicos totales de V1
- Desglose por código de pasante

**Dónde va:**
- Campo `int` del portal V1 en `renderBlockView` del bloque que tiene V1 esa semana
- Actualizar el KPI general de interesados (parcial, se completa al tener todos los portales)
- Card del pasante correspondiente: campo "Interesados portal (V1)"

---

# PASO 3 — INTERESADOS V2

## 🔵 Para el Analista

**¿Qué subir?** `Interesados_V2-[periodo].xlsx`

**Checklist de verificación:**
- [ ] ¿Los códigos corresponden a pasantes asignados a V2 según el rol?
- [ ] ¿El total es razonable respecto a V1 y semanas anteriores?

---

## 🟢 Para Claude (IA)

Mismo esquema que V1. Actualizar campo `int` del portal V2 en el bloque correspondiente y card del pasante.

---

# PASO 4 — INTERESADOS V3 + V4

## 🔵 Para el Analista

**¿Qué subir?** `Interesados_V3-[periodo].xlsx` y `Interesados_V4-[periodo].xlsx` (pueden subirse juntos).

**Importante sobre V3:** Los interesados de V3 llegan por el código del opcionador de la propiedad (ej. `THMK26`, `RSPV`). **No se asignan a un pasante ni a un bloque.** Claude los reporta separadamente como "Interesados V3 por opcionador".

**Checklist de verificación:**
- [ ] ¿Los códigos de V3 corresponden a opcionadores conocidos?
- [ ] ¿Los códigos de V4 corresponden a pasantes asignados a V4 según el rol?
- [ ] ¿El total de los 4 portales es coherente con el total del CRM?

---

## 🟢 Para Claude (IA)

Al tener todos los portales, calcular y actualizar:

```html
<!-- KPI principal — Interesados portal total -->
<div class="kv">[V1+V2+V3+V4]</div>
<div class="ks">V1=[INT_V1] · V2=[INT_V2] · V3=[INT_V3] · V4=[INT_V4]</div>

<!-- Tasa de registro (ahora calculable) -->
<div class="kv">[CRM/INT_TOTAL * 100]%</div>
<div class="ks">[TOTAL_CRM] CRM / [INT_TOTAL] interesados portal</div>
```

```javascript
// chart de interesados por portal
hbars(document.getElementById('ch-int'), [
  {lbl:'V2', v:[INT_V2], col:'#1E8E3E'},
  {lbl:'V1', v:[INT_V1], col:'#1A73E8'},
  {lbl:'V3', v:[INT_V3], col:'#7B2FBE'},
  {lbl:'V4', v:[INT_V4], col:'#D56E0C'}
], [INT_MAX]);
```

---

# PASO 5 — RENDIMIENTO V1 (confirma estructura)

## 🔵 Para el Analista

**¿Qué subir?** `Rendimiento_periodo_V1-[periodo].xlsx`

Este archivo se carga primero solo para confirmar que la estructura de columnas no cambió respecto a la semana anterior. Si cambió, Claude lo reporta antes de seguir.

**Checklist de verificación:**
- [ ] ¿Las columnas de exposición y visualizaciones están en el lugar esperado?
- [ ] ¿Los valores son coherentes con el período reportado?

---

## 🟢 Para Claude (IA)

Extraer exposición y visualizaciones totales de V1. Reportar estructura de columnas para que el analista confirme antes de procesar V2/V3/V4.

---

# PASO 6 — RENDIMIENTO V2, V3, V4

## 🔵 Para el Analista

**¿Qué subir?** Los tres archivos de rendimiento restantes (pueden subirse juntos si la estructura es igual a V1 confirmada).

**Checklist de verificación:**
- [ ] ¿La exposición total es coherente con semanas anteriores?
- [ ] ¿Hay publicaciones sin código o con código inválido (ej. "CM200", vacíos)?

---

## 🟢 Para Claude (IA)

Al tener todos los rendimientos, actualizar:

```javascript
// renderGlobalCharts — exposición y visualizaciones
pbars(document.getElementById('ch-exp'), [
  {lbl:'V1', v:[EXP_V1]/[EXP_TOTAL]*100, col:'#1A73E8', sub:'[EXP_V1_FORMAT]'},
  {lbl:'V2', v:[EXP_V2]/[EXP_TOTAL]*100, col:'#1E8E3E', sub:'[EXP_V2_FORMAT]'},
  {lbl:'V4', v:[EXP_V4]/[EXP_TOTAL]*100, col:'#D56E0C', sub:'[EXP_V4_FORMAT]'},
  {lbl:'V3', v:[EXP_V3]/[EXP_TOTAL]*100, col:'#7B2FBE', sub:'[EXP_V3_FORMAT]'}
]);
pbars(document.getElementById('ch-vis'), [/* mismo orden */]);

// renderFunnel — embudo principal
const d = {
  portal_exp: [EXP_TOTAL],
  portal_vis: [VIS_TOTAL],
  portal_int: [INT_TOTAL],
  crm: [CRM_TOTAL],
  seguimiento: [PIPELINE],
  citas_ag: [CITAS_AG],   // placeholder hasta tener citas
  citas_at: [CITAS_AT],
  firmados: [FIRMADOS],
  pagados: [PAGADOS]
};

// renderBlockView — exp y vis por bloque/portal
renderBlockView('b1','[PORTAL_B1]','[COLOR]', [...pasantes...],
  {int:[INT], exp:[EXP_PORTAL_B1], vis:[VIS_PORTAL_B1], ...});
// Repetir b2, b3, v3

// WEEKLY — exp y vis de la semana actual
global: { ..., exp: [EXP_TOTAL], vis: [VIS_TOTAL], ... }
ger: {
  V1: {portal_int:[INT_V1]},
  V2: {portal_int:[INT_V2]},
  V3: {portal_int:[INT_V3]},
  V4: {portal_int:[INT_V4]}
}
```

Si hay publicaciones sin homologar, actualizar la alerta:
```html
<strong>Publicaciones sin homologar:</strong> V1: [#] códigos vacíos · V3: [detalle].
```
Si no hay errores, mostrar alerta verde o eliminar el bloque de alerta.

---

---

# PASO 7 — CITAS

## 🔵 Para el Analista

**¿Qué subir?** `CITAS_[periodo].xlsx`

Si el archivo no está listo aún, indicarlo. Claude generará el dashboard con un placeholder visible en la tabla de citas y los KPIs en cero.

**¿Qué debe contener el archivo?** Una fila por cita con: fecha, nombre del cliente, asesor, gerente, bloque, número de cita (cita 1, cita 2...), tipo (Presencial / Inmueble), asistencia (SI / NO), producto de interés (Premium / Venta Tradicional / S/D).

**Checklist de verificación:**
- [ ] ¿El total de citas agendadas es coherente con lo reportado por los gerentes?
- [ ] ¿Hay citas duplicadas (mismo cliente + misma fecha)?
- [ ] ¿Todos los asesores con citas están en la lista DATA del dashboard?
- [ ] ¿El producto está clasificado en los valores esperados?
- [ ] ¿La tasa de asistencia (atendidas/agendadas) es coherente?

---

## 🟢 Para Claude (IA)

**Extraer y calcular:**
- `citasMap` → citas agendadas y atendidas por asesor
- `citasDetalle` → una fila por cita en el formato exacto del dashboard
- Totales: citas agendadas, atendidas, canceladas, tasa de asistencia
- Desglose por bloque/portal para `renderBlockView`

**Dónde va:**

```javascript
const citasMap = {
  "[Nombre Asesor]": {ag: [#AGENDADAS], at: [#ATENDIDAS]},
  // Solo asesores con al menos 1 cita agendada
};

const citasDetalle = [
  [1, "2026-MM-DD", "Nombre Cliente", "Asesor", "Gerente", "B1|B2|B3|V3",
   "cita 1|cita 2", "Presencial|Inmueble", "SI|NO", "Premium|Venta Tradicional|S/D"],
  // ...
];
```

```html
<!-- KPI: Citas agendadas -->
<div class="kv" style="color:var(--green)">[TOTAL_CITAS_AG]</div>

<!-- KPI: Citas atendidas -->
<div class="kv" style="color:var(--green)">[TOTAL_CITAS_AT]</div>

<!-- KPI: Citas canceladas -->
<div class="kv" style="color:var(--amber)">[TOTAL_CITAS_CANCEL]</div>

<!-- KPI: Tasa de asistencia -->
<div class="kv" style="color:var(--green)">[TASA_ASIST]%</div>
<div class="ks">[TOTAL_CITAS_AT] / [TOTAL_CITAS_AG] citas</div>

<!-- Alerta en Vista General -->
<strong>Citas:</strong> [TOTAL_CITAS_AG] agendadas, [TOTAL_CITAS_AT] atendidas (tasa [TASA_ASIST]%).
```

```javascript
// renderBlockView — cag y cat por bloque
{..., cag: [CITAS_AG_BLOQUE], cat: [CITAS_AT_BLOQUE]}

// WEEKLY — citas de la semana actual
global: { ..., citas_ag: [TOTAL_CITAS_AG], citas_at: [TOTAL_CITAS_AT], ... }
ger: {
  V1: {citas_ag:[#], citas_at:[#]},
  V2: {citas_ag:[#], citas_at:[#]},
  V3: {citas_ag:[#], citas_at:[#]},
  V4: {citas_ag:[#], citas_at:[#]}
}
```

---

---

# PASO 8 — ACTUALIZACIÓN DE TENDENCIAS HISTÓRICAS

## 🔵 Para el Analista

Con todos los datos confirmados, se añade la semana al historial. Verificar:
- [ ] ¿La nueva semana tiene clave `W[N+1]`?
- [ ] ¿Las fechas y horas del período son correctas?
- [ ] ¿Se deben actualizar MONTHLY o QUARTERLY si la semana cierra un mes/trimestre?

---

## 🟢 Para Claude (IA)

```javascript
// Agregar al FINAL del array WEEKLY:
{
  key: 'W[N]',
  label: '[DD_INICIO]-[DD_FIN] [MES]',
  short: '[DD_INICIO][ABREV_MES]',
  dF: '2026-MM-DD',
  dT: '2026-MM-DD',
  note: '[NOTA_DEL_PERÍODO]',
  global: {
    portal_int: [INT_TOTAL],
    exp: [EXP_TOTAL],
    vis: [VIS_TOTAL],
    crm: [CRM_TOTAL],
    seguimiento: [PIPELINE],
    citas_ag: [CITAS_AG],
    citas_at: [CITAS_AT],
    firmados: [FIRMADOS],
    pagados: [PAGADOS]
  },
  ger: {
    V1: {portal_int:[INT_V1], crm:[CRM_V1], citas_ag:[CAG_V1], citas_at:[CAT_V1]},
    V2: {portal_int:[INT_V2], crm:[CRM_V2], citas_ag:[CAG_V2], citas_at:[CAT_V2]},
    V3: {portal_int:[INT_V3], crm:[CRM_V3], citas_ag:[CAG_V3], citas_at:[CAT_V3]},
    V4: {portal_int:[INT_V4], crm:[CRM_V4], citas_ag:[CAG_V4], citas_at:[CAT_V4]}
  }
}

// Actualizar semanas visibles por defecto (últimas 3):
let tendSelWeeks = ['W[N-2]', 'W[N-1]', 'W[N]'];
```

Si cierra un mes, agregar o actualizar la entrada correspondiente en `MONTHLY`. Si cierra trimestre, actualizar `QUARTERLY`.

---

---

# PASO 9 — CONTRATOS (Firmados y Pagados)

Si en la semana hubo contratos, el analista indica explícitamente: número de firmados, número de pagados, asesor responsable (si aplica).

## 🟢 Para Claude (IA)

Actualizar:
- `renderFunnel`: campos `firmados` y `pagados`
- KPI de cada bloque: tarjeta "Firmados / Pagados"
- Tabla de bloques: columnas `Firmados` y `Pagados`
- `WEEKLY`, `MONTHLY`: campos `firmados` y `pagados`

---

---

# GENERACIÓN FINAL DEL DASHBOARD

## 🔵 Para el Analista

Solo después de confirmar **todos los datos de todos los archivos**, solicitar a Claude:

> *"Genera el dashboard completo con todos los datos confirmados."*

Claude entregará el `index.html` actualizado como **un solo archivo descargable**. No se entregan versiones parciales ni borradores.

## 🟢 Para Claude (IA)

Al generar el archivo final:
1. Aplicar **todos** los cambios acordados en la sesión en un solo `index.html`.
2. Verificar internamente que no haya valores `undefined`, `NaN` ni inconsistencias de suma.
3. Actualizar título, header, pie de página y chip de fecha con el período confirmado.
4. Entregar el archivo completo.

---

---

# REFERENCIA RÁPIDA — MÉTRICAS POR ÁREA

| Área | Métricas |
|------|---------|
| **MKT / Pasantes** | Interesados únicos por portal · Leads registrados en CRM · Tasa de registro (CRM / interesados) · Leads transferidos · Pendientes de traspaso · Tasa de traspaso (transferidos / registrados) |
| **Ventas** | Leads recibidos (transferidos a asesores) · Pipeline abierto · Citas agendadas / atendidas / canceladas · Cerrados sin venta · Firmados · Pagados |

> **Regla clave:** La tasa de registro puede superar 100% porque un pasante puede registrar leads provenientes de múltiples portales simultáneamente.

---

# CHECKLIST FINAL ANTES DE PUBLICAR

- [ ] Período (fecha + hora inicio y fin) correcto en título, header y pie de página
- [ ] Rotación bloque↔portal correcta para la semana
- [ ] Todos los asesores del rol vigente están en la tabla (sin extras ni removidos activos)
- [ ] Total CRM = suma de pasantes + MKT-AG + Teléfono de Ventas (± S/H)
- [ ] Total interesados portal = V1 + V2 + V3 + V4
- [ ] Tasa de registro calculada correctamente
- [ ] Tasa de traspaso calculada correctamente
- [ ] Tasa de asistencia calculada correctamente
- [ ] Tabla de citas completa y sin duplicados
- [ ] Nueva semana añadida al historial WEEKLY
- [ ] `tendSelWeeks` apunta a las últimas 3 semanas
- [ ] Sin valores `undefined` ni `NaN` en el dashboard
- [ ] Códigos del equipo Omar (si aparecieron) fueron reportados a Denisse

---

## 📌 REGLAS PERMANENTES PARA CLAUDE

1. **Nunca modificar la estructura HTML ni el CSS.** Solo actualizar valores de datos JS y texto de elementos HTML señalados.
2. **Verificar sumas antes de insertar.** Confirmar que los totales cuadran.
3. **Preservar nombres exactos.** Los nombres en `leadsMap` y `citasMap` deben coincidir con `const DATA` (tildes, capitalización y aliases incluidos).
4. **Dato no disponible = `null`**, no `0` ni `undefined`, para que el dashboard muestre `s/d` correctamente.
5. **El dashboard se genera una sola vez, al final**, con todos los datos confirmados.
6. **Los reportes intermedios van en el chat como texto**, nunca como archivo.
7. **Siempre pedir período con fecha y hora** al iniciar la sesión.
8. **Siempre pedir rotación de bloques** antes de procesar cualquier archivo.
9. **Colores de portales:** V1=#1A73E8 · V2=#1E8E3E · V3=#7B2FBE · V4=#D56E0C

---

*Altaltium Real Estate Solutions · Dashboard KPI Ejecutivo · LA LLAVE DE TU FUTURO*
