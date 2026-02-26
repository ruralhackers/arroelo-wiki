# Plan: Web Espacio Arroelo con Astro

## 1. Resumen del contenido en `docs/`

El contenido está organizado como una wiki del **Espacio Arroelo** (coworking en Cobián Roffignac 6, Pontevedra). Estructura detectada:

### 1.1 Página principal (raíz)
- **Espacio Arroelo. Cobián Roffignac 6**
  - Valores: Pasión, Valentía, Compartir conocimiento, Inspiración, Personas
  - Misión (3 puntos)
  - Enlaces a: Coworking, Comunidad, Changemakers, Alianzas, Nuestro equipo

### 1.2 Secciones de primer nivel
| Sección | Contenido |
|--------|------------|
| **Coworking en harmonía** | Índice con: Dónde estamos, Cosas de casa, Seguriño, Plan Desconexión Digital |
| **Construyendo la comunidad** | Café a la fresca, Con Té de Tarde, PonteBlogs, Anceu, visibilidad, WhatsApp, acuerdos/promos, blog |
| **Changemakers** | Proyectos: Capitán Planeta, Creative Habitat, Rural Hackers, Stopphubbing.me, Creative Commons, empleo con apoyo Down, redes de innovación |
| **Alianzas** | Anceu, Creative Habitat, Rural Hackers, European Creative Hubs, Down Xuntos, Malayaka House, Pontevedra WordPress |
| **Nuestro equipo** | CSV con 12 personas + 12 fichas individuales en Markdown |

### 1.3 Subsecciones (Coworking → Cosas de casa)
- **Dónde estamos**: mapa, aparcar, tren, avión, bus
- **Cosas de casa**: índice → Guía Zen, Ayudando al Capitán Planeta
- **Guía Zen**: horarios, normas, reserva salas, limpieza, orden
- **Ayudando al Capitán Planeta**: política de sostenibilidad
- **Seguriño**: Nuki, alarma
- **Nuestro plan de Desconexión Digital**: Stopphubbing.me, casos urgente/no urgente, mejora continua

### 1.4 Datos del equipo
- **CSV**: columnas `Name`, `Role`, `Email`, `Phone`, `Talento`, `Talento 1`, etc.
- **Fichas .md**: una por persona con frontmatter/metadatos (Created, Role, Email, Phone, Talento, Talento 1)

---

## 2. Estructura objetivo del sitio Astro

```
arroelo-wiki/
├── src/
│   ├── pages/
│   │   ├── index.astro                    # Inicio (valores + misión + nav)
│   │   ├── coworking/
│   │   │   ├── index.astro                # Coworking en harmonía
│   │   │   ├── donde-estamos.astro
│   │   │   ├── cosas-de-casa.astro
│   │   │   │   # o desglosar en:
│   │   │   ├── guia-zen.astro
│   │   │   ├── capitán-planeta.astro
│   │   │   ├── segurino.astro
│   │   │   └── desconexion-digital.astro
│   │   ├── comunidad.astro                # Construyendo la comunidad
│   │   ├── changemakers.astro
│   │   ├── alianzas.astro
│   │   └── equipo/
│   │       ├── index.astro                # Listado equipo (desde CSV o MD)
│   │       └── [slug].astro               # Ficha por persona (MD)
│   ├── components/
│   │   ├── Layout.astro
│   │   ├── Nav.astro
│   │   ├── Card.astro
│   │   └── TeamMember.astro
│   ├── content/
│   │   ├── config.ts                      # content collections
│   │   ├── equipo/                        # MD por persona (opcional)
│   │   └── ...
│   └── styles/
│       └── global.css
├── public/
├── docs/                                  # Origen: sin tocar o solo como referencia
├── astro.config.mjs
├── package.json
└── README.md
```

---

## 3. Plan de implementación por fases

### Fase 0: Inicialización del proyecto
- [ ] Crear proyecto Astro (`npm create astro@latest`) en el repo actual
- [ ] Elegir: "Empty" o "Blog" (para content collections) y TypeScript si se desea
- [ ] Añadir integración `@astrojs/mdx` si se quiere renderizar MD/MDX desde `docs` o desde `src/content`

### Fase 1: Contenido y datos
- [ ] **Opción A – Contenido estático en .astro**: copiar el texto de cada .md a la página .astro correspondiente (más control, sin dependencia de `docs`).
- [ ] **Opción B – Content Collections**: definir colección(s) para páginas (y/o equipo), mover o enlazar el contenido desde `docs` y usar `getStaticPaths` + componente de render de Markdown.
- [ ] **Equipo**:  
  - Leer el CSV (en build o con script) y generar datos para la página listado.  
  - Por cada persona: usar la ficha .md existente (vía Content Collection o importación) o generar una página desde el CSV con campos Name, Role, Email, Phone, Talento, Talento 1.
- [ ] Normalizar enlaces internos: de rutas tipo `Espacio%20Arroelo%20...` a rutas Astro (`/`, `/coworking`, `/equipo/africa-rodriguez`, etc.).

### Fase 2: Layout y navegación
- [ ] `Layout.astro`: estructura común (html, head, body), meta, CSS global.
- [ ] `Nav.astro`: menú principal (Inicio, Coworking, Comunidad, Changemakers, Alianzas, Equipo) y submenú de Coworking (Dónde estamos, Cosas de casa, Guía Zen, Capitán Planeta, Seguriño, Desconexión digital).
- [ ] Footer opcional con contacto (email/teléfono de atención) y enlaces a redes/alianzas.

### Fase 3: Páginas principales
- [ ] **index.astro**: valores, misión, enlaces grandes a las 5 secciones.
- [ ] **coworking/index.astro**: texto intro + enlaces a subpáginas.
- [ ] **coworking/donde-estamos.astro**: mapa (iframe de Google Maps con el enlace que ya tienes), texto aparcar/tren/avión/bus.
- [ ] **coworking/cosas-de-casa.astro** (o **guia-zen** y **capitan-planeta** por separado): Guía Zen + Ayudando al Capitán Planeta.
- [ ] **coworking/segurino.astro** y **coworking/desconexion-digital.astro**.
- [ ] **comunidad.astro**, **changemakers.astro**, **alianzas.astro**: una página por sección con el texto actual de los .md.

### Fase 4: Equipo
- [ ] **equipo/index.astro**: grid/lista de personas (nombre, rol, foto si se añade después) enlazando a `/equipo/[slug]`.
- [ ] **equipo/[slug].astro**: ficha individual (nombre, rol, email, teléfono, talentos); slugs derivados del nombre (ej. `africa-rodriguez`).
- [ ] Datos: CSV → JSON o array en build, o Content Collection desde los .md del equipo.

### Fase 5: Estilos y UX
- [ ] Diseño coherente: tipografía, colores, espaciado (puede inspirarse en espacioarroelo.es si existe).
- [ ] Enlaces externos (Medium, Rural Hackers, Down Xuntos, etc.): abrir en nueva pestaña y accesibilidad.
- [ ] Responsive: menú móvil (hamburguesa o lista colapsable).
- [ ] Revisar contraste y jerarquía de títulos para accesibilidad.

### Fase 6: Contenido dinámico y enlaces
- [ ] Sustituir tablas de navegación de la wiki por el menú del layout.
- [ ] Enlaces rotos: comprobar que todos los enlaces internos apunten a rutas Astro.
- [ ] Mantener enlaces externos tal cual (PonteBlogs, stopphubbing.me, vídeos YouTube, etc.).

---

## 4. Decisiones técnicas recomendadas

| Tema | Recomendación |
|------|----------------|
| **Origen del contenido** | Content Collections con MD en `src/content` (copiando/adaptando desde `docs`) para mantener una sola fuente y poder reutilizar componentes. |
| **Equipo** | CSV → script de build que genere `src/data/equipo.json` o integrar en Content Collection; slugs por nombre normalizado (slugify). |
| **Markdown en páginas** | Usar `<Fragment set:html={...} />` con markdown compilado o componentes MDX para bloques concretos. |
| **Mapa** | Iframe con `https://goo.gl/maps/5NGwpxUtGAKinm9z9` en la página “Dónde estamos”. |
| **Idioma** | Todo en español; lang="es" en el HTML. |
| **Deploy** | Astro es estático por defecto; válido para Netlify, Vercel, GitHub Pages, etc. |

---

## 5. Orden sugerido de trabajo

1. Crear proyecto Astro y comprobar que `npm run build` y `npm run dev` funcionan.
2. Implementar `Layout.astro` y `Nav.astro` con rutas vacías o placeholder.
3. Migrar página de inicio y una sección completa (por ejemplo Coworking) para fijar el patrón de contenido.
4. Añadir el resto de páginas de Coworking, luego Comunidad, Changemakers, Alianzas.
5. Implementar listado y fichas del equipo (CSV + slugs).
6. Aplicar estilos y revisar enlaces y responsive.

---

## 6. Checklist rápido

- [ ] Proyecto Astro creado
- [ ] Layout + navegación
- [ ] Página principal (valores + misión)
- [ ] Todas las páginas de Coworking (incl. Guía Zen, Capitán Planeta, Seguriño, Desconexión)
- [ ] Páginas Comunidad, Changemakers, Alianzas
- [ ] Listado equipo + ficha por persona
- [ ] Enlaces internos y externos correctos
- [ ] Estilos y responsive
- [ ] Revisión de accesibilidad básica

Si quieres, el siguiente paso puede ser generar el esqueleto del proyecto Astro (comando de creación, `astro.config`, estructura de `src/pages` y `Layout.astro` + `Nav.astro`) en este mismo repositorio.
