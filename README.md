# Dialogue System Framework — Documentación en español

**Sistema completo de diálogos y decisiones para Unreal Engine 5.** Implementado en Blueprints, basado en Data Tables y diseñado para integrarse mediante los Actor Components `AC_DialogueManager` y `AC_DecisionManager`.

> Crea conversaciones, decisiones interactivas, texto enriquecido, voces y eventos de juego sin programar C++.

**[Ver el producto Full en Fab](https://www.fab.com/listings/e6c1f535-2990-4551-8303-c3405ebd7aad)** · **[Ver video de demostración](https://youtu.be/sOUxLFjuRq0)** · **[Descargar demo jugable Full](https://mega.nz/file/BqxC2CZa#zAplOAOMxhFl9x-6w8IYzfUxBjKvCPLndjz7heS21YU)**

## Primeros pasos

1. [Instalación e inicio rápido](docs/01-instalacion.md)
2. [Conversaciones y Data Tables](docs/02-conversaciones.md)
3. [Eventos mediante Blueprint Interfaces](docs/03-eventos.md)
4. [Sistema de decisiones con Common UI](docs/04-decisiones.md)
5. [Rich Text: palabras con colores](docs/05-rich-text.md)
6. [Audio, duración y controles](docs/06-audio-y-controles.md)
7. [Localización e interfaces de diálogo](docs/07-localizacion-e-interfaces.md)
8. [Migración desde Free y versiones anteriores](docs/08-migracion.md)
9. [Problemas frecuentes](docs/09-problemas-frecuentes.md)
10. [Arquitectura y extensión del sistema](docs/10-arquitectura.md)

## Características principales

- Conversaciones mediante Data Tables y estructura `ST_DialogueLine`.
- `AC_DialogueManager` agregado al jugador; **no reemplaza GameInstance**.
- Decisiones con botones de **Common UI**, configuradas mediante Data Tables.
- Rich Text para resaltar palabras con estilos de color.
- Interfaces de subtítulos y diálogo centrado.
- Localización de textos, mensajes y voces en español e inglés.
- Audio sincronizado, duración y pausas entre líneas.
- Control de omisión por línea (`AllowSkip`) y mensajes contextuales.
- Eventos mediante `BPI_DialogueEvents` y avisos de inicio/fin de conversación.

## Requisitos

- Unreal Engine 5 y proyecto con Blueprints.
- **Plugin Common UI**, incluido con Unreal Engine: activarlo en `Edit > Plugins`, reiniciando el editor cuando sea necesario.
- Paquete Full instalado desde Fab y su contenido disponible en el proyecto.
- Las versiones concretas de UE5 y plataformas soportadas deben corresponder a las indicadas en la ficha de Fab.

## Vista rápida

![Actor Components del paquete Full](images/instalacion-actor-component.png)

## Primera conversación

1. Agrega `AC_DialogueManager` al Blueprint de tu jugador.
2. En `BeginPlay`, llama a `Dialogue Create Widgets` con `AC_DialogueManager` como `Target`.
3. Llama a `Dialogue Play Conversation` y conecta tu Data Table al pin `Conversation`.
4. Prueba el nivel. Puedes trasladar el inicio a una interacción con NPC cuando termines la prueba.

```text
Event BeginPlay
    → Dialogue Create Widgets (Target: AC_DialogueManager)
    → Dialogue Play Conversation (Target: AC_DialogueManager,
                                  Conversation: DT_MiConversacion)
```

Consulta [la instalación paso a paso](docs/01-instalacion.md).

## Compatibilidad con Starter Pack Free

Las Data Tables de **conversaciones** creadas con Free se pueden reutilizar en Full: consérvalas y conéctalas al componente Full. No necesitas volver a escribir las líneas. Las decisiones y el Rich Text son funciones adicionales de Full.

**[Descargar Starter Pack Free](https://www.fab.com/listings/1d39c10d-b6b0-4dd1-b704-8b3d60658e23)** · [Guía de migración](docs/08-migracion.md)

## Documentación y soporte

- [Documentación Full en inglés](https://github.com/wohostudios/ue5-dialogue-system-en) — se actualizará tras revisar esta versión española.
- [Comunidad en Discord (español)](https://discord.gg/JY5JTQaWqn)
- [Documentación del Starter Pack Free (ES)](https://github.com/wohostudios/ue5-dialogue-system-starter-pack-es)


