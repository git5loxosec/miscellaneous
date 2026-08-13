# CHARACTER_SYSTEM.md — Foto → Personaje

## Flujo
1. Usuario sube foto → `POST /character/generate` (multipart)
2. Servidor manda la foto a un modelo de visión (ej. API de Claude o GPT-4V)
   con un prompt fijo: extraer rasgos generales estilizables (NO datos
   biométricos identificables, solo cosas como "cabello corto oscuro,
   complexión atlética") para usarlos como guía de un avatar estilo
   pixel-art/anime, no una réplica realista de la persona.
3. Con esos rasgos, se llama a una API de generación de imagen para crear
   un sprite/avatar estilizado (32x32 o 64x64 para pixel-art, o ilustración
   más grande para el retrato del HUD).
4. En paralelo, se generan atributos aleatorios con una semilla basada en
   el usuario (no puro random.random() — usa un seed determinista por
   userId+timestamp para poder reproducir/debuggear).

## Esquema de atributos (ejemplo, ajústalo a tu balance)
```
{
  "fuerza": 1-20,
  "destreza": 1-20,
  "inteligencia": 1-20,
  "vitalidad": 1-20,
  "suerte": 1-20
}
```
Distribución sugerida: no uniforme puro — usa una curva (ej. suma de dos
d10) para que la mayoría caiga en un rango medio y los extremos sean raros.
Esto hace que sacar un personaje muy fuerte se sienta especial.

## Especialidades (clases)
Genera la especialidad basada en cuál atributo salió más alto, con algo
de aleatoriedad para que no sea 100% predecible:
- Fuerza alta → Guerrero / Berserker
- Destreza alta → Pícaro / Arquero
- Inteligencia alta → Mago / Invocador
- Vitalidad alta → Paladín / Tanque
- Suerte alta → Comodín (clase híbrida rara, baja probabilidad)

## Facciones
3-4 facciones con identidad visual y de lore propia (colores, símbolo,
filosofía). Asignación:
- Puede ser aleatoria ponderada, o el jugador elige entre 2 opciones que
  el sistema le ofrece según su especialidad (mejor UX: sensación de
  elección dentro de aleatoriedad controlada)
- Guarda la facción en el modelo de personaje; el Game Master de IA
  (04_AI_GAMEMASTER.md) la usa para generar conflictos entre facciones

## Importante — privacidad
No almacenes la foto original más tiempo del necesario para generar el
avatar. Guarda solo el avatar resultante. Si vas a lanzar esto para otras
personas, necesitas un consentimiento explícito de subida de foto y borrar
el original tras procesar.
