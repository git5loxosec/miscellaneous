# ARCHITECTURE.md — Stack y estructura

## Por qué este stack (pensado para Termux/proot sin root)

- **Node.js + JavaScript/TypeScript** en todo el stack: evita compilar
  binarios nativos pesados. Ya tienes experiencia en Node/JS.
- **Phaser 3** para el cliente: motor 2D con animación, tilemaps, sprites,
  físicas de arcade — todo lo que pide "juego animado con mapa". Corre en
  el navegador, así que el "juego" real vive en un servidor (puede ser tu
  proot-Ubuntu) y se juega desde cualquier navegador, móvil incluido.
- **Socket.io** para multijugador en tiempo real sobre WebSockets.
- **SQLite (better-sqlite3) o lowdb** para persistencia. Si better-sqlite3
  da problemas de compilación nativa en Termux, usa lowdb (JSON plano) o
  Postgres remoto (ej. Supabase/Neon) — cero compilación local.
- **LLM vía API** (Anthropic API, la que ya usas en DEM con OpenRouter)
  para el Game Master de IA.
- **Generación de imagen** vía API externa (no corre localmente) para el
  avatar del personaje a partir de la foto.

## Estructura de carpetas

```
rpg-game/
├── server/
│   ├── index.js              # entrypoint, healthcheck
│   ├── db/                   # esquemas y acceso a datos
│   ├── character/            # generación de personaje (Fase 2)
│   ├── combat/                # motor de combate puro (Fase 3)
│   ├── world/                 # estado del mapa, zonas, eventos
│   ├── gamemaster/            # servicio de IA para misiones/torneos
│   ├── multiplayer/           # sockets, salas, sincronización
│   └── routes/                # API REST (auth, upload de foto, etc.)
├── client/
│   ├── src/
│   │   ├── scenes/            # BootScene, MapScene, CombatScene, UIScene
│   │   ├── entities/          # Player, NPC, Enemy
│   │   ├── ui/                # HUD, menús, diálogos
│   │   └── net/                # cliente de sockets
│   └── assets/                 # sprites, tilesets, sonidos
├── shared/
│   └── types/                  # tipos/esquemas compartidos server-client
├── PROGRESS.md                 # log que el agente actualiza cada fase
└── package.json
```

## Reglas de arquitectura para el agente
- El motor de combate y el motor de generación de personaje son módulos
  **puros** (sin dependencia de Express, Socket.io ni Phaser) — así se
  testean con `node script.js` sin levantar nada.
- La UI nunca decide reglas de juego; solo pinta el estado que le manda
  el servidor. Esto es lo que evita el "juego roto" cuando hay multijugador.
- Nada de lógica de negocio dentro de los `.on('connection')` de sockets —
  esos solo traducen eventos de red a llamadas a los módulos puros.
