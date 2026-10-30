# 08 — Migración desde Free y versiones anteriores

[← Volver al índice](../README.md)

## A. Migrar conversaciones del Starter Pack Free a Full

**Las Data Tables de conversación son compatibles.** No necesitas reescribir tus diálogos.

1. Crea una copia de seguridad de tu proyecto.
2. Instala el Framework Full desde Fab.
3. Conserva o transfiere tus Data Tables de conversaciones existentes.
4. Añade `AC_DialogueManager` Full al Blueprint de jugador que corresponda.
5. Inicializa con `Dialogue Create Widgets`.
6. Conecta las tablas a `Dialogue Play Conversation`.
7. Ejecuta tus conversaciones y verifica eventos, duraciones y nombres.
8. Activa Common UI si vas a utilizar el sistema de decisiones.

Para usar características nuevas (decisiones y Rich Text) deberás configurarlas adicionalmente; la migración de las conversaciones no crea decisiones por sí sola.

## B. Actualizar instalaciones antiguas basadas en GameInstance

Las versiones antiguas de la documentación utilizaban `BP_DialogueGameInstance`, una función de creación de widgets desde GameInstance y llamadas de reproducción basadas en esa clase.

La arquitectura actual utiliza `AC_DialogueManager` dentro del personaje. Para actualizar:

1. Haz una copia de seguridad del proyecto antes de importar la nueva versión.
2. Revisa y elimina únicamente las llamadas anteriores que dependen del antiguo GameInstance, después de confirmar sus reemplazos.
3. Añade `AC_DialogueManager` al jugador y cambia el inicio por `Dialogue Create Widgets` → `Dialogue Play Conversation`.
4. Conserva las Data Tables cuando sus estructuras sean compatibles.
5. Revisa las referencias a widgets, interfaces de audio, idioma y eventos; prueba cada función.
6. Si configuraste un GameInstance propio para el sistema anterior, no lo reemplaces sin evaluar otras dependencias de tu proyecto.

**Importante:** no se trata de una actualización automática de Blueprints. Las conversaciones pueden reutilizarse, pero las conexiones antiguas del proyecto pueden necesitar adaptación.

## C. Prueba de migración

- El primer diálogo reproduce el texto correcto.
- Los eventos llegan al receptor que implementa `BPI_DialogueEvents`.
- Audio y localización funcionan.
- Se muestra el widget apropiado.
- Las funciones de decisión y Rich Text aparecen al configurarlas en Full.

Siguiente: [Problemas frecuentes](09-problemas-frecuentes.md).
