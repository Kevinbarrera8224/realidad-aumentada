# App WebAR de Información de Incidentes (Grupo Modelo)

**Objetivo:** al escanear un **QR** en la zona de trabajo, se abre esta web. Luego, al **apuntar a un marcador** (pegatina pequeña junto a la máquina), aparecen **cuadros flotantes** con la información del incidente y una **recomendación**, y **opcionalmente un video**. **No hay quizzes ni actividades**.

## ¿Cómo probar rápido?
1. Descarga el zip y descomprímelo.
2. Sube la carpeta a un hosting simple (por ejemplo GitHub Pages, Vercel o tu servidor interno). También puedes abrir `index.html` desde un servidor local (`npx serve` o con la extensión Live Server de VS Code).
3. Imprime un **marcador Hiro** (ya viene soportado) desde aquí:  
   https://raw.githubusercontent.com/AR-js-org/AR.js/master/three.js/examples/marker-training/examples/pattern-files/hiro.png  
   Pégalo discretamente junto a la máquina.
4. Abre la URL de tu despliegue con `?machine=empaquetadora-01` (ej.: `https://tu-dominio/ar/?machine=empaquetadora-01`).
5. Al **apuntar la cámara** al marcador, verás los cuadros flotantes con la información. Con los botones puedes cambiar de incidente, ocultar panel y silenciar.

> Nota: El **QR** únicamente debe **apuntar a esa URL** (por ejemplo, un QR distinto por máquina: `?machine=embotelladora-02`, etc.).

## Estructura
```
/
├─ index.html             # escena A‑Frame + AR.js (sin instalar app)
├─ data/
│  └─ incidents.json      # aquí están los incidentes por máquina
└─ assets/
   └─ demo-video.mp4      # video de ejemplo (opcional)
```

## Editar los incidentes
- Abre `data/incidents.json` y agrega tus máquinas (usa una clave por máquina, p. ej. `envasadora-03`).  
- Cada incidente tiene: `fecha`, `titulo`, `resumen`, `recomendacion`, y `video` (opcional).  
- El **QR** de cada máquina debe llevar a `index.html?machine=<id>` donde `<id>` es la clave que uses (ej. `envasadora-03`).

## Cambiar el marcador (anclaje AR)
Por sencillez, este prototipo usa el **marcador Hiro**. Opciones:
- **Dejar Hiro**: imprimirlo y pegarlo pequeño cerca de la máquina.
- **Barcode/ArUco** (sin imagen): cambia `<a-marker type="pattern" ...>` por `type="barcode" value="5"` (y luego imprime ese barcode).
- **Patrón propio**: genera un `.patt` con la herramienta de AR.js (marker-training) y usa su URL en `url="..."`.

> Si prefieres reconocer **la propia máquina sin marcador**, necesitas una solución con **targets de imagen/objeto** (e.g. 8th Wall o WebAR avanzada) y preparar un target por máquina. Es viable, pero es un paso adicional y suele ser de pago.

## Buenas prácticas de UX para no distraer al operador
- Máximo **3 bloques de texto** cortos; se muestran ya configurados en el panel.
- Botón **Ocultar panel** siempre visible.
- Videos **silenciados por defecto**; el operador decide si activar sonido.
- Altos contrastes y tipografía legible; panel translúcido.
- Nada de interacción adicional (sin quizzes, sin formularios).

## Despliegue
- **GitHub Pages**: sube este folder a un repo → Settings → Pages.
- **Vercel**: crea un proyecto “Static Site” apuntando a esta carpeta.
- **Servidor interno**: cualquier hosting de archivos estáticos.

## QR por máquina
Genera un QR por máquina con la URL respectiva, por ejemplo:
- `https://realidad-aumentada-mocha.vercel.app/?machine=empaquetadora-01`
- `https://tu-dominio/ar/?machine=embotelladora-02`

Puedes usar cualquier generador (ej. `qr-code-generator.com`) o desde la terminal:
```bash
# Ejemplo con qrencode (Linux/Mac)
qrencode -o qr-empaquetadora-01.png "https://tu-dominio/ar/?machine=empaquetadora-01"
```

## Notas técnicas
- WebAR con **AR.js + A‑Frame**, funciona en navegadores móviles modernos.
- El video usa `playsinline` y se silencia por política de autoplay; el usuario puede activar sonido.
- Si tu sitio usa HTTPS (recomendado), la cámara funcionará sin problemas.
- Si el video pesa mucho, conviene comprimir a H.264 (MP4) con ~720p.

---

Cualquier mejora que necesites (por ejemplo cambiar a tracking por imagen sin marcador o integrar un backend), se puede evolucionar sobre esta base.
