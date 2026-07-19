# Diseño: página `/checklist-2`

## Contexto

El sitio (`sistemamaestrocapacitaciones`) es una SPA de Vite + React con una única
landing (`src/pages/Landing.tsx`, sin router). Se despliega a GitHub Pages con
dominio propio (`sistemamaestrohys.tupuntodigital.shop`) vía
`.github/workflows/deploy.yml`, que corre `npm run build` y publica `dist/`.

El usuario pidió agregar una página de checklist (proporcionada como HTML
standalone: React, Babel y jsPDF cargados por CDN, sin paso de build propio)
accesible en `/checklist-2`.

## Enfoque

Archivo estático independiente, sin integrarlo al build de React del sitio:

- Crear `public/checklist-2/index.html` con el contenido provisto por el
  usuario.
- Vite copia todo `public/` tal cual a la raíz de `dist/` (comportamiento por
  defecto, `base: "./"` en `vite.config.ts` no lo afecta), así que
  `public/checklist-2/index.html` queda servido en `/checklist-2/` sin tocar
  `App.tsx`, sin instalar router ni reescribir el checklist a JSX.
- El archivo corrige mojibake introducido al pegar el HTML en el chat
  (`Ã³` → `ó`, `Â¿` → `¿`, emojis rotos, etc.) revirtiendo la doble
  codificación UTF-8/Latin-1 byte a byte — no cambia ninguna palabra del
  contenido ni de los checklists.
- No se modifica ninguna otra parte del sitio.

## Verificación

- `npm run build && npm run preview`, confirmar que `/checklist-2/` sirve la
  página, que los botones ✓/✗/○ cambian de color, que "Descargar PDF" genera
  el PDF, y que los datos persisten en `localStorage` al recargar.
- El deploy a producción es automático al pushear a `main` (workflow
  existente, sin cambios).

## Fuera de alcance

- No se toca el contenido/copy del checklist.
- No se integra a React Router ni se convierte a componente del sitio.
