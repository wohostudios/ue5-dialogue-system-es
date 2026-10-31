# 10 — Arquitectura y extensión del sistema

[← Volver al índice](../README.md)

## Arquitectura Full confirmada

```text
Player Character Blueprint
├── AC_DialogueManager
│   ├── ST_DialogueLine / Data Tables de conversación
│   ├── Texto, idioma, audio, duración y eventos
│   └── Widgets de diálogo (subtítulos / centrado)
├── AC_DecisionManager
│   ├── ST_DecisionLine / Data Tables de opciones
│   └── Common UI y UIRoot Ref
└── DialogueVoice (Audio Component, demo)
```

![Componentes del personaje](../images/instalacion-actor-component.png)

## Flujo de decisiones

1. `Decisions Create Widgets` crea la interfaz raíz.
2. `Decision Play Choices` obtiene las opciones de una Data Table.
3. `BPI_PlayerDialogue` devuelve `Selected Result ID` al solicitante.
4. El juego procesa el resultado: una acción, idioma o siguiente conversación.

## Flujo de diálogo

1. `Dialogue Create Widgets` prepara el widget.
2. `Dialogue Play Conversation` recibe una Data Table y `Current Language`.
3. El manager consulta `ST_DialogueLine`, obtiene texto/audio/prompt localizado y actualiza el widget.
4. El manager reproduce voz, gestiona la temporización y despacha eventos.

## Blueprint Interfaces confirmadas

- `BPI_DialogueEvents`: `ExecuteDialogueEvent` y continuación de conversación.
- `BPI_PlayerDialogue`: `ExecuteDialogueFromDecision`, que devuelve `Selected Result ID`, `Requester` y `Active Decision Widget`.



**No se requiere `BP_DialogueGameInstance`.**

Siguiente: [Problemas frecuentes](09-problemas-frecuentes.md).
