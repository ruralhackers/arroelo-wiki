# Espacio Arroelo — Web en Astro

Web estática del **Espacio Arroelo** (coworking en Cobián Roffignac 6, Pontevedra) generada con [Astro](https://astro.build). El contenido proviene de la wiki en `docs/`.

## Estructura

- **Inicio**: valores, misión y enlaces a las secciones.
- **Coworking**: dónde estamos (mapa), cosas de casa, Guía Zen, Capitán Planeta, Seguriño, Desconexión digital.
- **Comunidad**: Café a la fresca, Con Té de Tarde, PonteBlogs, visibilidad, acuerdos.
- **Changemakers**: proyectos (Creative Habitat, Rural Hackers, Stopphubbing.me, etc.).
- **Alianzas**: Anceu, Creative Habitat, Rural Hackers, Down Xuntos, Malayaka House, Pontevedra WordPress.
- **Equipo**: listado y ficha por persona (datos en `src/data/equipo.json`).
- **Fotos de equipo**: coloca las fotos en `public/equipo/` con el nombre del slug (ej. `africa-rodriguez.jpg`) y añade en cada persona del JSON el campo `"photo": "africa-rodriguez.jpg"`. Si no hay foto se muestra un placeholder.

## Comandos

| Comando         | Acción                          |
|----------------|----------------------------------|
| `npm install`  | Instalar dependencias           |
| `npm run dev`  | Servidor de desarrollo (puerto 4321) |
| `npm run build`| Generar sitio estático en `dist/` |
| `npm run preview` | Previsualizar el build        |

## Deploy

El build es estático. Puedes desplegar la carpeta `dist/` en Netlify, Vercel, GitHub Pages o cualquier hosting estático.
