# MULTIPLAYER.md — Solo o en grupo

## Modelo de salas (Socket.io rooms)
- `world:<zona>` — todos los jugadores en la misma zona del mapa se ven
  moverse entre sí en tiempo real
- `party:<id>` — grupo de jugadores que exploran/pelean juntos
- `faction:<id>` — canal de facción para eventos/chat de facción
- `battle:<id>` — sala efímera solo mientras dura un combate

## Flujo jugar solo vs en grupo
- Por defecto el jugador está en modo solo: ve a otros en el mapa pero
  sus encuentros de combate son individuales (o contra IA)
- Puede mandar invitación a otro jugador cercano → si acepta, se crea
  `party:<id>` y desde ahí los encuentros son de grupo
- Buscar grupo abierto: lista simple de parties con espacio, filtrable
  por facción/nivel — no necesitas matchmaking complejo para el MVP

## Sincronización de estado
- El servidor es la única fuente de verdad (autoridad de servidor). El
  cliente manda inputs ("moverme a X,Y", "atacar con habilidad Y"), nunca
  manda "mi HP ahora es 50" — eso lo calcula y devuelve el servidor.
- Esto evita que sea trivial hacer trampa y evita que el estado se
  desincronice entre jugadores.

## Reconexión
Guarda el estado de sesión en el servidor por `userId`, no por
`socketId` — así si el jugador pierde conexión (típico en móvil) puede
reconectar y seguir donde estaba en vez de perder la partida.
