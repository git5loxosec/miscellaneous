# COMBAT_SYSTEM.md — Combate por turnos

## Diseño base (estilo ATB simplificado)
- Cada combatiente tiene una barra de "velocidad de turno" que se llena
  según su `destreza`. Cuando se llena, le toca actuar (esto da la
  sensación Final Fantasy sin ser estrictamente por rondas fijas).
- Acciones: Atacar, Habilidad (según especialidad), Objeto, Defender, Huir.
- El daño se calcula con una fórmula simple y ajustable:
  `daño = (ataque_base + variación_random(0.9-1.1)) - defensa_objetivo * 0.5`
- Golpe crítico basado en `suerte` (probabilidad = suerte / 100 aprox).

## Estructura del módulo (server/combat/)
Debe ser una función pura tipo máquina de estados:
```
createBattle(participants) -> battleState
applyAction(battleState, action) -> newBattleState + log de eventos
isBattleOver(battleState) -> boolean/resultado
```
Nunca debe importar Express ni Socket.io — así se testea con Jest/node
directo, sin levantar servidor. Esto es lo que evita que el combate "se
rompa" cuando lo conectas a multijugador.

## Habilidades por especialidad (ejemplo mínimo viable)
- Guerrero: Golpe Poderoso (más daño, menos precisión)
- Mago: Bola de Fuego (daño mágico, cuesta "maná")
- Pícaro: Golpe Furtivo (crítico garantizado si ataca primero)
- Paladín: Escudo (reduce daño del equipo un turno)

Empieza con 1 habilidad por clase. Agregar más habilidades es fase de
pulido, no de MVP — no dejes que el agente se pierda balanceando 20
habilidades antes de tener el combate básico funcionando.

## Combate multijugador (grupo vs grupo/jefe)
El `battleState` soporta N participantes por bando desde el día 1, aunque
el MVP sea 1v1 — así no rediseñas el módulo entero cuando llegue la Fase 5.

## Visualización (Phaser)
- CombatScene separada del MapScene
- Sprites con animaciones simples: idle, ataque, recibir daño, victoria
- Barra de HP/maná animada (tween, no salto instantáneo)
- Log de texto de combate visible (esto es barato de hacer y agrega mucho
  a la sensación "de juego real")
