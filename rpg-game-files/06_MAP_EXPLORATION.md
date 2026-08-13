# MAP_EXPLORATION.md — Mapa animado y exploración

## Herramientas
- **Tiled** (editor gratuito) para diseñar el tilemap y exportarlo como
  JSON — Phaser lo carga nativo con `this.load.tilemapTiledJSON`
- Tilesets: puedes usar sets libres (itch.io tiene muchísimos gratuitos
  con licencia CC0, ej. Kenney.nl) mientras generas los tuyos con IA

## Estructura de escena
- `MapScene`: tilemap + capa de colisiones + sprite del jugador con
  físicas Arcade
- Movimiento con teclado (WASD/flechas) y con joystick virtual táctil
  para móvil (plugin `rexvirtualjoystickplugin` de Phaser)
- Cámara sigue al jugador (`this.cameras.main.startFollow(player)`)

## Animación de personaje
- Spritesheet con 4 direcciones x 3-4 frames de caminata (estándar RPG
  2D). Si generas el avatar por IA (Fase 2), necesitas post-procesarlo a
  un spritesheet — o usar un avatar estático solo para el retrato/HUD y
  un sprite genérico animado por clase para el mapa (más realista de
  lograr bien que animar cada foto única)

## Zonas y encuentros
- Objetos de tipo "trigger" en Tiled (zonas invisibles) que al pisarlas
  disparan: encuentro random de combate, evento de facción, inicio de
  misión, entrada a otra escena/mapa
- Esto conecta directo con el Game Master de IA: las zonas pueden pedir
  al servidor "qué evento corresponde aquí ahora" en vez de ser 100%
  estático

## Transiciones y feel "de juego"
- Fade in/out entre escenas (`this.cameras.main.fadeOut/fadeIn`)
- Partículas simples en encuentros (polvo al moverse, brillo en items)
- Esto es barato en Phaser y es lo que más diferencia un "prototipo" de
  un "juego" a nivel de sensación
