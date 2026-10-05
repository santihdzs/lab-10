# Bitácora — Práctica 10: Avisos con imágenes

Matrícula: A01787550

## Experimento A3 — Coil con su propio cliente

- Las imágenes ya vistas seguían saliendo: venían del caché de disco de Coil.
- Con el caché borrado: rectángulos grises y nada en el Logcat.
- En `docker compose logs api`: `GET /api/imagenes/… 401 Unauthorized`.
- Por qué: el cliente por omisión de Coil no tiene el interceptor (ni token ni log). Todo debe salir por el mismo `OkHttpClient`.

## Experimento D2 — Sin reducir

- Con `LADO_MAX = 8000` y `CALIDAD = 100` el JPEG creció (~2 MB → ~3.6 MB): recomprimir a 100 guarda ruido.
- El servidor respondió `413` con `{"error": "La imagen es demasiado grande", ...}`, pero la app solo decía "El servidor respondió 413.".
- Se arregló en D3: `mensajeDe()` muestra el `error` del servidor también para 413, 415, 500…

## Ejercicio D4 — Las imágenes de mi reto

**Tamaño y LADO_MAX:** se ve a lo ancho del teléfono (~1080 px). `LADO_MAX = 1280`.

**Quién la ve:** cualquiera con sesión. Si fuera solo el dueño, habría que guardar el dueño de cada clave y revisarlo en `GET /imagenes/{clave}`.

**Imagen subida sin aviso:** hoy nadie la borra, queda huérfana en el almacén. Solución: regla de ciclo de vida en el almacén (borrar las que no estén "en uso" tras un día).
