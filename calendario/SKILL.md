# Skill 01 — Calendario Editorial

## Cuándo se activa
Juan pide un calendario, un plan de contenido, una parrilla semanal o mensual, o dice "qué publicamos esta semana/mes".

---

## Formato de salida — regla principal

| Situación | Formato |
|---|---|
| Juan pide el calendario **con visualización** o dice "en HTML" / "el dashboard" | → Generar HTML interactivo con la arquitectura del calendario establecido (ver sección abajo) |
| Juan pide el calendario para **editar/exportar/compartir** o dice "en Sheets" | → Entregar estructura como CSV/tabla para importar a Google Sheets |
| Juan pide **solo el plan** o una referencia rápida | → Markdown compacto |
| **Default si no especifica:** | → Markdown. Ofrecer al final: "¿Lo quieres también en HTML interactivo o en Sheets?" |

El HTML es el formato premium: lo usas para operar, hacer seguimiento y medir. El Markdown es para planear rápido. Sheets es para tener el repositorio en Drive.

---

## Qué preguntar si falta información

Haz solo las preguntas que falten. No preguntes lo que ya está claro.

1. ¿Para qué periodo? (semana / mes / trimestre)
2. ¿Hay eventos, lanzamientos, reuniones, coyunturas o fechas regulatorias que considerar?
3. ¿Prioridad comercial del periodo? (ESG / LSO / Formación / todas)
4. ¿El objetivo principal es autoridad, demanda, tráfico al diagnóstico o conversación comercial?

**Si Juan no da más información, asume:**
- LinkedIn de Juan es el canal motor. La página de Horizonte respalda.
- Frecuencia base: 4 posts/semana LinkedIn (L/M/J/V) + 1 carrusel IG/semana + 1 artículo web/quincenal.
- Balance de pilares: P1 25% · P2 25% · P3 20% · P4 15% · P5 15%.
- CTA primario siempre: diagnóstico ESG (`plataforma-esg-hs.netlify.app`).

---

## Cadencia semanal canónica LinkedIn

| Día | Hora COT | Pilar | Tipo |
|---|---|---|---|
| Lunes | 8:00 | P1 o P2 | Marco regulatorio / educacional |
| Martes | 7:30 | P5 o P3 | Contrarian con dato |
| Jueves | 8:00 | P3 o P2 | Caso anonimizado / behind-the-scenes |
| Viernes | 7:30 | P1 o P4 | Framework operativo |

---

## Triggers regulatorios como ancla editorial

Cuando hay evento regulatorio, entra con prioridad. Post en 24-48h.

- **EUDR** — entrada en vigor pymes exportadoras a UE: 30 junio 2026
- **NIIF S1/S2** vía Circular 031/2024 SFC — emisores de valores
- **CSRD spillover** — proveedores de multinacionales europeas
- **Decreto PPDS** (Diálogo Social, firmado junio 2026) — oportunidad LSO
- **Decreto 1893/2024** — consulta previa
- **Taxonomía Verde Colombia** (DNP) — financiación verde

---

## Reciclaje entre canales

```
POST LINKEDIN (lunes o martes)
├── Carrusel Instagram (misma semana, versión visual)
├── Historia Instagram (CTA al diagnóstico)
├── Artículo pilar Web/Blog (cada 2 semanas, versión SEO)
└── Guion WhatsApp (plantilla de seguimiento)
```

---

## Reglas de composición del calendario

- No repetir el mismo pilar más de 2 veces seguidas.
- Cada mes: al menos 1 CTA directo al diagnóstico ESG, 1 caso anonimizado (marcar `[VERIFICAR]` si no hay insumo real), 1 cierre con tagline de marca.
- Cada pieza lleva: día/semana · canal · pilar · tema · audiencia · formato · ángulo/tesis · CTA · prioridad · validación necesaria.
- Efemérides genéricas (Día Mundial del Medio Ambiente sin ángulo propio): no entran.

---

## OUTPUT HTML — arquitectura exacta

Cuando Juan pida el calendario en HTML, replicar **exactamente** la arquitectura del `Calendario_Editorial_HS_interactivo.html`:

### Stack y persistencia
- HTML autocontenido, sin backend, sin dependencias externas salvo Google Fonts
- `localStorage` para persistir estados por pieza y métricas editables
- Claves de storage: `hs_cal_estados_v1` y `hs_cal_metricas_v1`
- Export a JSON y CSV desde botones en toolbar

### Paleta y tipografías (canónicas HS)
```css
:root {
  --verde-principal:#143C24; --verde-oscuro:#0C2A18; --verde-vivo:#2C8B4F;
  --terracota:#C75B33; --dorado:#D8A33A; --dorado-suave:#F1D27A;
  --crema:#F4F1E7; --tinta:#101813;
  --li:#0A66C2; --ig:#C13584; --web:#2C8B4F; --wa:#25785a;
  --font-display:'Bricolage Grotesque','Segoe UI',sans-serif;
  --font-body:'Hanken Grotesk','Segoe UI',system-ui,sans-serif;
}
```

### Estructura obligatoria del HTML

**Header:** gradiente verde oscuro→principal, KPIs dinámicos en grid (total piezas · LinkedIn · IG+Web · Pendientes · En proceso · Publicadas), tagline en dorado.

**Nav tabs sticky:** General · Por canal · Semanal · Prioridades · Métricas

**Vista General:** filtros (buscar texto, canal, estado, prioridad, línea, funnel, pilar) + grid de cards + toolbar de export.

**Card por pieza** con borde izquierdo coloreado por canal:
- Badges: canal (color canal) · funnel · prioridad (color semáforo) · pilar · línea de servicio
- Metadatos: día · semana
- Título del tema (h3)
- Formato
- Bloque de copy/idea principal (fondo crema suave)
- Objetivo · CTA (en terracota) · Métrica · Notas (⚠ si hay VERIFICAR)
- Footer de card: dot de color + selector de estado (Pendiente/En proceso/Publicado/Pausado)

**Vista Por canal:** panel por canal con stats (total/pendientes/en proceso/publicadas), frecuencia, objetivo, grid de cards propias, próximas acciones.

**Vista Semanal:** panel por semana con % de avance, temas, carga, canales activos, grid de cards de la semana.

**Vista Prioridades:** columnas (Alta prioridad · Pendientes · Conversión · Comercial/CTA fuerte) con mini-cards que muestran estado coloreado.

**Vista Métricas:** tabla editable con inputs para: Alcance · Impresiones · Interac. · Clics · Guard. · Coment. · Leads/Conv. · Reuniones · Observaciones. Todo persiste en localStorage.

**Footer:** verde oscuro, texto dorado suave, datos HS.

### Estructura de datos por pieza (objeto JS)
```javascript
{
  id: "s1-lun",           // semana-día único
  dia: "Lunes S1",
  semana: 1,
  canal: "LinkedIn",      // LinkedIn | Instagram | Web | WhatsApp
  formato: "Post texto · Trend-jacking regulatorio",
  pilar: "P1",            // P1-P5
  linea: "Consultoría ESG", // Consultoría ESG | Formación y Academia | Licencia Social para Operar | Marca general
  funnel: "Atracción",    // Atracción | Consideración | Conversión | Nutrición
  prio: "Alta",           // Alta | Media | Baja
  tema: "...",
  objetivo: "...",
  copy: "...",            // idea/gancho principal, no el post completo
  cta: "...",
  metrica: "...",
  notas: "..."            // incluir ⚠ VERIFICAR si hay datos sin confirmar
}
```

---

## OUTPUT GOOGLE SHEETS — estructura CSV

Cuando Juan pida en Sheets, entregar tabla con estas columnas exactas lista para importar:

`Semana | Día | Canal | Pilar | Línea de servicio | Funnel | Prioridad | Tema | Formato | Audiencia | Tesis/Ángulo | Copy/Idea principal | CTA | Métrica clave | Validación necesaria | Estado`

Una fila por pieza. Estado en blanco (Juan lo gestiona en Sheets).

---

## OUTPUT MARKDOWN — estructura compacta

Cuando el output sea MD, usar esta estructura:

```
## Semana N — [temas ancla]

| Día | Canal | Pilar | Tema | Formato | CTA | Prioridad |
|---|---|---|---|---|---|---|
| Lun | LinkedIn | P1 | ... | Post texto | ... | Alta |
...

**Racional estratégico:** [3-5 líneas sobre por qué esta distribución]
**⚠ Validaciones necesarias:** [lista de datos que Juan debe confirmar]
```

---

## Errores que no debes cometer

- Generar HTML sin tener claro el contenido del mes (primero el plan, luego el HTML)
- Llenar con efemérides genéricas sin ángulo de negocio
- Proponer más piezas de las que Juan puede sostener solo
- Distribuir esfuerzo en todos los canales por igual
- Medir éxito por número de publicaciones planeadas

---

## Cierre

Listo para revisión. / Necesita validación. / No recomiendo publicar todavía.
