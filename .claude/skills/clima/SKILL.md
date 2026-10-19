---
name: clima
description: Obtiene el clima actual y el pronóstico de la ubicación local del usuario (o de una ciudad indicada). Úsalo cuando pida el clima, la temperatura, si va a llover o el pronóstico.
---

# Clima

Usa `wttr.in` (sin API key). Ciudad por defecto: `Villa del Rosario, Cordoba, Argentina`.

## Pasos

1. Si el usuario dio otra ciudad, úsala; si no, usa la ciudad por defecto.
2. Ejecuta (reemplaza `CIUDAD`; espacios como `+`, ej. `Villa+del+Rosario,Cordoba,Argentina`):

   ```bash
   curl -s "wttr.in/CIUDAD?format=j1&lang=es"
   ```

3. Del JSON, responde en español y breve:
   - Ubicación (`nearest_area`)
   - Ahora (`current_condition[0]`): `temp_C`, `FeelsLikeC`, `lang_es[0].value`, `humidity`, `windspeedKmph`
   - Próximos días (`weather[]`): `mintempC`/`maxtempC` y probabilidad de lluvia (`hourly[].chanceofrain`, máximo del día)

## Notas

- Si `nearest_area` no corresponde a la ciudad pedida, avísalo.
- Si `curl` falla o devuelve algo que no es JSON, informa el error; no inventes datos.
