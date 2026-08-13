# ROADMAP.md — Orden de construcción para el agente

Este archivo es el que le das PRIMERO a tu agente en cada iteración del loop.
Regla de oro: el agente NO avanza a la siguiente fase sin que la anterior
compile, corra y pase una prueba manual mínima. Nada de "generar todo de
un tiro" — eso es lo que produce basura.

## Cómo usar esto con un loop tipo "gauntlet" (agente autónomo iterativo)

1. Cargar SIEMPRE estos docs como contexto fijo (no se regeneran, son la
   fuente de verdad): 00_ROADMAP, 01_ARCHITECTURE, 08_DATA_MODELS.
2. En cada ciclo, el agente solo trabaja UNA fase de la lista de abajo.
3. Al terminar una fase, el agente debe:
   - Correr el proyecto (`npm run dev` o equivalente)
   - Verificar contra el "Criterio de aceptación" de esa fase
   - Actualizar `PROGRESS.md` (log de qué se hizo, qué falló, qué falta)
   - NO tocar código de fases futuras
4. Si el agente falla una fase 2 veces seguidas, debe parar y pedir
   contexto humano en vez de improvisar sobre una base rota.

## Fases

### Fase 0 — Esqueleto del proyecto
- Repo con estructura de carpetas (ver 01_ARCHITECTURE.md)
- Servidor Node.js levantando un healthcheck (`/health`)
- Cliente Phaser 3 cargando una escena vacía con un rectángulo animado
- Criterio de aceptación: `npm run dev` levanta server + cliente sin errores,
  se ve un canvas en el navegador

### Fase 1 — Modelo de datos y persistencia
- Implementar esquemas de 08_DATA_MODELS.md (jugador, personaje, facción,
  misión, partida)
- Base de datos ligera (SQLite vía better-sqlite3, o lowdb si quieres cero
  dependencias nativas — importante en Termux sin root)
- Criterio de aceptación: crear/leer/actualizar un personaje de prueba
  desde un script standalone

### Fase 2 — Generación de personaje desde foto
- Ver 02_CHARACTER_SYSTEM.md
- Endpoint que recibe imagen, genera avatar estilizado + atributos random
  + especialidad + facción asignada
- Criterio de aceptación: subir una foto de prueba y obtener un JSON de
  personaje completo + un sprite/avatar renderizable

### Fase 3 — Combate por turnos
- Ver 03_COMBAT_SYSTEM.md
- Motor de combate como módulo puro (sin UI), testeable con inputs fijos
- Luego la escena de Phaser que lo visualiza
- Criterio de aceptación: dos personajes de prueba pelean hasta que uno
  llega a 0 HP, con log de turnos correcto

### Fase 4 — Mapa y exploración
- Ver 06_MAP_EXPLORATION.md
- Tilemap navegable, colisiones, zonas que disparan encuentros/misiones
- Criterio de aceptación: el personaje se mueve por el mapa con teclado/touch
  y entra en una zona que dispara un evento

### Fase 5 — Multijugador
- Ver 05_MULTIPLAYER.md
- WebSockets (Socket.io) para estado compartido: ver otros jugadores en el
  mapa, invitar a grupo, combate multijugador
- Criterio de aceptación: dos clientes conectados se ven entre sí en el
  mismo mapa en tiempo real

### Fase 6 — IA Game Master (misiones, eventos, torneos)
- Ver 04_AI_GAMEMASTER.md
- Servicio que usa un LLM para generar/coordinar contenido dinámico
- Criterio de aceptación: el sistema genera una misión nueva coherente con
  el estado del mundo, sin intervención manual

### Fase 7 — UI/UX pulido y animaciones
- Ver 07_UI_UX.md
- HUD, menús, transiciones, feedback visual de combate
- Criterio de aceptación: el juego se siente "de juego", no de prototipo

### Fase 8 — Pulido, balance, deploy
- Balance de stats, tutorial, onboarding
- Deploy (Render/Fly.io/VPS — evita depender de que el server viva en tu
  Termux)

## Anti-patrones que debes prohibirle explícitamente al agente
- Generar 3000 líneas de código en un solo mensaje sin poder correrlas
- Inventar dependencias que no existen o no están en package.json
- Mezclar lógica de servidor y cliente en el mismo archivo "para ir rápido"
- Hardcodear rutas de Termux o asumir root
- Saltarse el criterio de aceptación de una fase "porque ya casi"
