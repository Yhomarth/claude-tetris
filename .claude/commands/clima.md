# Skill: clima

Consulta el clima local del usuario usando `wttr.in` (no requiere API key).

## Pasos

1. Ejecuta el siguiente comando para obtener el clima actual basado en IP:

```bash
curl -s "https://wttr.in/?format=j1"
```

2. Parsea el JSON y presenta la información de forma clara y legible en español:
   - Ciudad detectada (campo `nearest_area[0].areaName[0].value`)
   - País (`nearest_area[0].country[0].value`)
   - Temperatura actual en °C (`current_condition[0].temp_C`)
   - Sensación térmica en °C (`current_condition[0].FeelsLikeC`)
   - Humedad % (`current_condition[0].humidity`)
   - Descripción del clima (`current_condition[0].weatherDesc[0].value`)
   - Viento en km/h (`current_condition[0].windspeedKmph`)
   - Pronóstico de los próximos días (`weather[0..2]`): fecha, máx/mín en °C, descripción.

3. Si el usuario pasa una ciudad como argumento (ej. `/clima Madrid`), úsala en lugar de detección por IP:

```bash
curl -s "https://wttr.in/CIUDAD?format=j1"
```

Sustituye `CIUDAD` con el argumento recibido (reemplaza espacios por `+`).

4. Presenta todo en un bloque de texto limpio y organizado, sin código ni JSON crudo.
