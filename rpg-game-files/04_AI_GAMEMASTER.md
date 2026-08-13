# AI_GAMEMASTER.md — IA coordinando el mundo

## Rol
Un servicio server-side (NO en el cliente) que usa un LLM para:
- Generar misiones nuevas basadas en estado del mundo (facciones en
  conflicto, jugadores activos, eventos pasados)
- Programar eventos temporales (invasión de una facción, jefe de mundo)
- Organizar torneos: brackets, emparejamientos, narrativa de resultados

## Por qué esto necesita ser determinista donde importa
El LLM genera **contenido narrativo y decisiones de alto nivel** (qué
misión, qué enemigo, qué recompensa temática), pero NUNCA calcula
resultados de combate ni tira dados — eso lo hace tu motor de combate
puro (03_COMBAT_SYSTEM.md). Si dejas que el LLM "decida quién gana", el
juego deja de ser justo y no es reproducible. Regla dura: LLM = contenido,
motor de juego = reglas y números.

## Patrón de implementación
1. Cron/loop que corre cada X minutos (o al cumplirse condiciones: nueva
   facción ganó terreno, X jugadores nuevos, etc.)
2. Arma un prompt con contexto real del mundo (JSON del estado actual:
   facciones, misiones activas, nivel promedio de jugadores)
3. Le pide al LLM una salida en **JSON estructurado y validado con schema**
   (nunca texto libre que luego intentas parsear con regex) — misión con:
   título, descripción, facción origen, objetivo (tipo enum: matar X,
   explorar zona Y, derrotar jugador de facción rival), recompensa
4. El servidor valida el JSON contra el schema antes de insertarlo al
   mundo. Si falla validación, reintenta o descarta — nunca confía ciego.

## Torneos
- El bracket y el emparejamiento son lógica normal de servidor (no IA)
- El LLM genera el "flavor": nombre del torneo, comentario narrativo de
  cada ronda, título del campeón — la parte que hace que se sienta vivo
- Los combates reales corren por el motor de combate (simulado
  automáticamente si el jugador no está online, o jugado en vivo si sí)

## Ejemplo de contrato JSON para una misión generada
```json
{
  "title": "string",
  "description": "string",
  "faction_origin": "string",
  "objective_type": "kill_enemy | explore_zone | defeat_player | escort",
  "objective_target": "string",
  "reward": { "xp": 0, "gold": 0, "item_id": "string|null" },
  "expires_in_hours": 0
}
```
