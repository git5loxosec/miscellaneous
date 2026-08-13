# UI_UX.md — Interfaz de juego

## Pantallas necesarias
1. **Login/Registro** — simple, con opción de subir foto para crear personaje
2. **Creación de personaje** — muestra el resultado generado (avatar,
   atributos, especialidad, facción) con una animación de "revelado"
   (esto vende la sensación de gacha/sorpresa)
3. **Mapa/Exploración** — HUD con: retrato + barra HP/maná, minimapa,
   botón de misiones activas, chat de grupo/facción
4. **Combate** — retratos de ambos bandos, barras de HP/turno animadas,
   menú de acciones (Atacar/Habilidad/Objeto/Defender/Huir), log de texto
5. **Misiones/Torneos** — lista generada por la IA Game Master, con
   estado (disponible, en progreso, completada)
6. **Perfil de facción** — lore, miembros, progreso de conflicto actual

## Principios de diseño
- Contraste alto entre HUD y mundo (el HUD nunca debe competir visualmente
  con el mapa — usa paneles semi-transparentes oscuros con texto claro)
- Cada facción con paleta de color distintiva que se repite en su UI,
  bordes de ventana, iconos — refuerza identidad
- Todo cambio de valor numérico (HP, XP) se anima con tween, nunca salto
  instantáneo — es la diferencia #1 entre "se siente barato" y "se siente pulido"
- Iconografía consistente: usa un solo set de iconos (Kenney UI pack es
  gratuito y consistente) en vez de mezclar estilos

## Para el agente
Dale explícitamente esta lista de pantallas y dile que construya UNA a la
vez, con datos de prueba (mock) antes de conectarlas al backend real —
así puedes evaluar visualmente cada pantalla sin depender de que todo el
backend ya funcione.
