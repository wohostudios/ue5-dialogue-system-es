# Dialogue System

Dialogue System es un framework de diálogos modular, desacoplado y basado en Data Tables para Unreal Engine 5, diseñado para ser reutilizable en cualquier proyecto desarrollado completamente con Blueprints.

Su objetivo es proporcionar un sistema de diálogos reutilizable, desacoplado y basado en datos (*Data Driven*), que pueda integrarse fácilmente en cualquier proyecto sin depender de la lógica específica del juego.

---

# Características

- ✅ Conversaciones mediante Data Tables.
- ✅ Localización de texto y audio.
- ✅ Sincronización automática con la duración del audio.
- ✅ Sistema de Delay entre líneas.
- ✅ Permite omitir líneas (Skip).
- ✅ Líneas que requieren interacción del jugador.
- ✅ Ejecución de eventos personalizados.
- ✅ Múltiples interfaces de diálogo.
- ✅ Mensajes contextuales (Prompt Text).
- ✅ Mostrar u ocultar el nombre del personaje.
- ✅ Colores para subtítulos.
- ✅ Arquitectura completamente desacoplada mediante Interfaces.
- ✅ Eventos de inicio y fin de conversación.

---

# Arquitectura

Toda la lógica del sistema se encuentra centralizada en:

```text
BP_GameInstance
```

El GameInstance es responsable de:

- Controlar el flujo de las conversaciones.
- Seleccionar el idioma.
- Gestionar la interfaz activa.
- Reproducir el audio.
- Esperar tiempos y entradas del jugador.
- Ejecutar eventos.
- Controlar el estado de la conversación.

---

# Arquitectura basada en Interfaces

El sistema utiliza Interfaces para desacoplar completamente sus componentes.

Actualmente incluye las siguientes Interfaces:

## BPI_DialogueAudio

Responsable de reproducir el audio del diálogo.

Funciones:

```text
PlayDialogueAudio()
```

---

## BPI_DialogueAudioFinished

Notifica al sistema cuando un audio ha terminado.

Funciones:

```text
AudioFinished()
```

---

## BPI_DialogueWidget

Implementada por todos los Widgets del sistema.

Funciones:

```text
UpdateDialogue()

ClearDialogue()

ShowWidget()

HideWidget()
```

---

## BPI_DialogueEvents

Permite ejecutar eventos del juego desde una línea de diálogo.

Funciones:

```text
ExecuteDialogueEvent(EventName)
```

---

## BPI_DialogueConversation

Permite notificar cuándo inicia y cuándo termina una conversación.

Funciones:

```text
ConversationStarted()

ConversationFinished()
```

---

# Sistema Data Driven

Toda la información del diálogo se encuentra almacenada en una Data Table.

Cada fila define completamente el comportamiento de una línea de diálogo.

Entre sus propiedades principales se encuentran:

| Propiedad | Descripción |
|-----------|-------------|
| Type | Tipo de línea |
| Speaker | Personaje que habla |
| Text | Texto localizado |
| Audio | Audio localizado |
| DisplayType | Tipo de interfaz |
| PromptText | Texto contextual |
| SubtitleColor | Color del subtítulo |
| AllowSkip | Permite omitir la línea |
| DelayAfter | Pausa antes de la siguiente línea |
| TriggerEvent | Evento personalizado |

---

# Localización

Actualmente el sistema soporta:

- Español
- Inglés

Cada línea contiene:

```text
Text_ES
Text_EN

Audio_ES
Audio_EN

PromptText_ES
PromptText_EN
```

El idioma se selecciona automáticamente mediante:

```text
CurrentLanguage
```

---

# Tipos de diálogo

Actualmente existen los siguientes tipos:

```text
Dialogue
Input
Event
Cinematic
```

Cada tipo controla su propio comportamiento mediante un:

```text
Switch on EDialogueType
```

---

# Tipos de interfaz

Actualmente existen dos presentaciones visuales:

```text
Subtitle
```

Muestra el diálogo como subtítulos en la parte inferior de la pantalla.

```text
Center
```

Muestra el diálogo como un mensaje centrado.

El sistema activa automáticamente el Widget correspondiente.

---

# Prompt Text

Cada línea puede mostrar un mensaje adicional.

Ejemplos:

```text
Presiona Q para omitir

Presiona E para continuar

Mantén presionado Shift para correr
```

Si el PromptText está vacío, el Widget lo oculta automáticamente.

---

# Sistema de Audio

El sistema soporta:

- Audio localizado.
- Líneas sin audio.
- Espera automática hasta finalizar el audio.
- Sincronización entre texto y voz.

La conversación solamente continúa cuando:

```text
Audio Finalizado

Y

Duration Finalizado
```

---

# Delay After

Cada línea puede definir un tiempo de espera antes de mostrar la siguiente.

Flujo:

```text
Mostrar diálogo

↓

Limpiar interfaz

↓

DelayAfter

↓

Mostrar siguiente diálogo
```

Esto permite generar pausas naturales entre las conversaciones.

---

# Sistema Skip

Cada línea controla si puede omitirse.

```text
AllowSkip = true
```

Permite avanzar inmediatamente.

```text
AllowSkip = false
```

Ignora la entrada del jugador.

---

# Show Speaker

Cada línea decide si debe mostrarse el nombre del personaje.

Controlado mediante:

```text
ShowSpeaker
```

---

# Subtitle Color

Cada línea puede definir su color utilizando:

```text
EDialogueSubtitleColor
```

Ejemplos:

```text
White
Gray
Yellow
Blue
Red
Green
```

---

# Trigger Event

Una línea puede ejecutar un evento personalizado.

Ejemplo:

```text
TriggerEvent

↓

OpenDoor
```

El sistema envía:

```text
ExecuteDialogueEvent()
```

mediante Interface.

El objeto receptor decide qué acción realizar.

El sistema de diálogos nunca contiene lógica específica del juego.

---

# Inicio y fin de conversación

El framework expone dos eventos globales:

```text
ConversationStarted()
```

```text
ConversationFinished()
```

Estos permiten reaccionar al inicio o al final de una conversación.

Algunos ejemplos:

- Bloquear movimiento.
- Mostrar u ocultar HUD.
- Iniciar música.
- Guardar progreso.
- Lanzar cinemáticas.

---

# Flujo del sistema

```text
PlayConversation()

↓

ConversationStarted()

↓

Crear Widgets

↓

Cargar conversación

↓

Leer línea actual

↓

Inicializar variables

↓

Seleccionar idioma

↓

Seleccionar interfaz

↓

Actualizar Widget

↓

Reproducir audio

↓

Esperar

↓

Comprobar continuación

↓

Ejecutar TriggerEvent

↓

Limpiar interfaz

↓

DelayAfter

↓

Siguiente línea

↓

...

↓

ConversationFinished()

↓

Ocultar Widgets

↓

Reiniciar variables
```

---

# Principios del proyecto

El framework fue diseñado siguiendo cinco principios fundamentales.

## 1. Data Driven

Toda la información del diálogo se define mediante Data Tables.

No existen conversaciones programadas directamente en Blueprints.

---

## 2. Desacoplado

La lógica del juego nunca conoce cómo funciona el sistema de diálogos.

El sistema de diálogos tampoco conoce la lógica del juego.

Toda la comunicación se realiza mediante Interfaces.

---

## 3. Modular

Cada componente del sistema es independiente.

- Audio
- UI
- Eventos
- Localización
- Flujo del diálogo

---

## 4. Reutilizable

El framework puede integrarse fácilmente en diferentes proyectos desarrollados con Unreal Engine.

---

## 5. Escalable

Agregar nuevos Widgets, idiomas, eventos o funcionalidades no requiere modificar el núcleo del sistema.

---

# Versión actual

## Dialogue System v1.0

Implementado actualmente:

- ✅ Flujo completo de conversaciones.
- ✅ Localización.
- ✅ Sincronización de audio.
- ✅ Múltiples interfaces.
- ✅ Prompt Text.
- ✅ Trigger Events.
- ✅ Eventos de conversación.
- ✅ Sistema Skip.
- ✅ Delay After.
- ✅ Colores para subtítulos.
- ✅ Mostrar/Ocultar Speaker.
- ✅ Arquitectura mediante Interfaces.
- ✅ Sistema completamente Data Driven.


