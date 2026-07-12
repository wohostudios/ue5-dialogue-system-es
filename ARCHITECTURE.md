# Dialogue System Architecture

Este documento describe la arquitectura interna de **Dialogue System** y las decisiones de diseño utilizadas durante su desarrollo.

Su objetivo es facilitar el mantenimiento del framework y servir como guía para futuras ampliaciones.

---

# Filosofía

Dialogue System fue diseñado siguiendo cuatro principios fundamentales.

## 1. Responsabilidad única

Cada componente del sistema debe tener una única responsabilidad.

Ejemplos:

- GameInstance controla el flujo del diálogo.
- Los Widgets únicamente muestran información.
- El Character reproduce el audio.
- Los eventos del juego se ejecutan mediante Interfaces.

---

## 2. Data Driven

Toda la información de las conversaciones se encuentra almacenada en Data Tables.

El comportamiento de una conversación nunca debe programarse directamente dentro de Blueprints.

Las conversaciones solamente describen datos.

Nunca contienen lógica.

---

## 3. Desacoplamiento

Ningún sistema conoce la implementación interna de otro.

Toda la comunicación se realiza mediante Blueprint Interfaces.

Esto permite reemplazar componentes sin modificar el resto del framework.

---

## 4. Reutilización

El objetivo principal del proyecto es poder reutilizar el framework en cualquier videojuego desarrollado con Unreal Engine.

Por esta razón el framework nunca contiene lógica específica del juego.

---

# Arquitectura General

```text
                    Data Table
                         │
                         ▼
                 BP_GameInstance
                         │
        ┌────────────────┼────────────────┐
        │                │                │
        ▼                ▼                ▼
     Widgets          Audio          Trigger Events
        │                │                │
        ▼                ▼                ▼
 Interfaces        Interfaces       Interfaces
```

BP_GameInstance actúa como el núcleo del framework.

Todos los sistemas reciben instrucciones desde este Blueprint.

---

# Responsabilidad de cada componente

## BP_GameInstance

Es el núcleo del framework.

Responsabilidades:

- Iniciar conversaciones.
- Finalizar conversaciones.
- Controlar el flujo.
- Seleccionar el idioma.
- Seleccionar el Widget activo.
- Controlar los tiempos.
- Gestionar Skip.
- Ejecutar Trigger Events.
- Mantener el estado actual del sistema.

BP_GameInstance nunca contiene lógica específica del juego.

---

## Widgets

Los Widgets únicamente muestran información.

Nunca toman decisiones.

Nunca controlan el flujo.

Nunca ejecutan eventos del juego.

Su única responsabilidad consiste en representar visualmente la información recibida.

Actualmente existen:

```text
WBP_Subtitles

WBP_CenterDialogue
```

Ambos implementan exactamente la misma Interface.

---

## Character

Actualmente el Character solamente reproduce el audio del diálogo.

No controla conversaciones.

No conoce Data Tables.

No conoce Widgets.

No conoce Trigger Events.

---

# Blueprint Interfaces

Todas las comunicaciones entre sistemas utilizan Interfaces.

Actualmente existen cinco Interfaces.

---

## BPI_DialogueAudio

Responsable de reproducir audio.

```text
PlayDialogueAudio()
```

---

## BPI_DialogueAudioFinished

Notifica cuando un audio termina.

```text
AudioFinished()
```

---

## BPI_DialogueWidget

Implementada por todos los Widgets.

Funciones:

```text
UpdateDialogue()

ClearDialogue()

ShowWidget()

HideWidget()
```

---

## BPI_DialogueEvents

Permite ejecutar eventos del juego.

```text
ExecuteDialogueEvent(EventName)
```

---

## BPI_DialogueConversation

Notifica el inicio y fin de una conversación.

```text
ConversationStarted()

ConversationFinished()
```

---

# Flujo interno

El framework sigue siempre el mismo flujo.

```text
PlayConversation()

↓

ConversationStarted()

↓

Load Conversation

↓

Current Line

↓

Initialize Variables

↓

Select Language

↓

Select Display Type

↓

Update Widget

↓

Play Audio

↓

Wait

↓

Dialogue_CheckContinue()

↓

Trigger Event

↓

Clear Dialogue

↓

DelayAfter

↓

Next Dialogue

↓

ConversationFinished()
```

Todo el framework gira alrededor de este flujo.

---

# Sistema de Estados

Actualmente el sistema distingue varios estados internos.

Ejemplo:

```text
None

Playing

WaitingForInput
```

Cada estado controla cómo debe responder el framework.

---

# Sistema de Localización

Toda la selección de idioma ocurre dentro del GameInstance.

Los Widgets nunca conocen qué idioma está utilizando el juego.

Ellos únicamente reciben el texto ya localizado.

---

# Sistema de Audio

El sistema permite dos comportamientos.

## Línea con audio

Espera simultáneamente:

- Duration
- AudioFinished

Solo cuando ambos terminan la conversación continúa.

---

## Línea sin audio

Únicamente espera Duration.

---

# Sistema de Widgets

Los Widgets se crean una única vez al iniciar la conversación.

Nunca se destruyen mientras ésta permanece activa.

El sistema únicamente cambia su visibilidad.

```text
Visible

Hidden
```

El Widget activo siempre se almacena en:

```text
Dialogue_CurrentWidget
```

El resto del framework nunca conoce cuál Widget está mostrando la información.

---

# Trigger Events

Los Trigger Events permiten que el diálogo notifique al juego cuando ocurre un evento.

Ejemplo:

```text
OpenDoor

PlayMusic

SpawnEnemy
```

El framework solamente envía el nombre del evento.

El Blueprint receptor decide qué hacer.

Esta decisión mantiene completamente desacoplados ambos sistemas.

---

# Prompt Text

PromptText es un mensaje contextual.

Puede utilizarse para:

- Tutoriales.
- Indicaciones.
- Mensajes de interacción.
- Ayuda al jugador.

El Prompt nunca forma parte del diálogo principal.

---

# Display Type

DisplayType permite cambiar la presentación visual de una línea.

Actualmente existen:

```text
Subtitle

Center
```

Nuevos tipos podrán añadirse sin modificar el flujo del framework.

---

# Cómo agregar un nuevo Widget

1. Crear el Widget.
2. Implementar BPI_DialogueWidget.
3. Agregar un nuevo valor al Enum DisplayType.
4. Actualizar Dialogue_UpdateActiveWidget().

No es necesario modificar el resto del sistema.

---

# Cómo agregar un nuevo idioma

1. Agregar los nuevos campos a la Data Table.

```text
Text_FR

Audio_FR

PromptText_FR
```

2. Agregar el idioma al Enum correspondiente.

3. Actualizar la selección de idioma dentro del GameInstance.

Los Widgets no requieren modificaciones.

---

# Cómo agregar un nuevo Trigger Event

1. Escribir el nombre del evento en la Data Table.

```text
TriggerEvent

OpenDoor
```

2. Implementar la lógica dentro del Blueprint receptor.

No es necesario modificar BP_GameInstance.

---

# Buenas prácticas

✔ Mantener toda la lógica dentro del GameInstance.

✔ Mantener los Widgets libres de lógica.

✔ Utilizar Interfaces para toda comunicación.

✔ Mantener las conversaciones completamente Data Driven.

✔ Evitar referencias directas entre sistemas.

✔ Evitar lógica específica del juego dentro del framework.

---

# Objetivo del proyecto

Dialogue System no pretende ser un sistema de diálogos para un único videojuego.

Su objetivo es convertirse en un framework reutilizable, modular y escalable que pueda utilizarse como base para cualquier proyecto desarrollado con Unreal Engine.
