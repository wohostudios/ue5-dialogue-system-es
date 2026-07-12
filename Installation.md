# Installation Guide

Esta guía explica cómo instalar y configurar **Dialogue System** en un proyecto de Unreal Engine 5.

---

# Requisitos

Antes de comenzar asegúrate de cumplir con los siguientes requisitos:

- Unreal Engine 5
- Proyecto basado en Blueprints
- Sistema de Audio configurado
- Enhanced Input (Opcional)

---

# Paso 1. Migrar el contenido

Migrar la carpeta **DialogueSystem** al proyecto o Instalar desde Fab.

Se recomienda mantener la estructura original de carpetas para evitar referencias rotas.

---

# Paso 2. Configurar el GameInstance

Ir a:

```text
Project Settings

↓

Maps & Modes

↓

Game Instance Class
```

Seleccionar:

```text
BP_DialogueGameInstance
```

A partir de este momento todo el framework será administrado por el GameInstance.

---

# Paso 3. Crear los Widgets

Dialogue System utiliza dos Widgets principales.

```text
WBP_Subtitles
```

Muestra subtítulos en la parte inferior de la pantalla.

---

```text
WBP_CenterDialogue
```

Muestra mensajes centrados.

---

Ambos Widgets implementan:

```text
BPI_DialogueWidget
```

No deben eliminarse.

---

# Paso 4. Crear los Widgets al iniciar el juego

En el BeginPlay del Level Blueprint (o cualquier otro punto de inicialización) llamar:

```text
Dialogue_CreateWidgets()
```

Esta función crea ambos Widgets y los mantiene en memoria durante toda la conversación.

---

# Paso 5. Configurar el sistema de Audio

El Blueprint responsable de reproducir el audio (normalmente el Character) debe implementar:

```text
BPI_DialogueAudio
```

Implementar la función:

```text
PlayDialogueAudio()
```

Cuando el Audio termine deberá ejecutar:

```text
AudioFinished()
```

mediante:

```text
BPI_DialogueAudioFinished
```

Esto permite al sistema saber cuándo puede continuar con la siguiente línea.

---

# Paso 6. Configurar Trigger Events

Cualquier Blueprint que deba responder a eventos del diálogo debe implementar:

```text
BPI_DialogueEvents
```

Implementar:

```text
ExecuteDialogueEvent(EventName)
```

El parámetro **EventName** contiene el nombre del evento definido en la Data Table.

Ejemplo:

```text
OpenDoor

PlayMusic

SpawnEnemy
```

---

# Paso 7. Configurar eventos de conversación

Si el juego necesita reaccionar al inicio o final de una conversación implementar:

```text
BPI_DialogueConversation
```

Funciones:

```text
ConversationStarted()

ConversationFinished()
```

Ejemplos de uso:

- Desactivar movimiento.
- Mostrar barras negras.
- Ocultar HUD.
- Reproducir música.
- Activar una cinemática.

---

# Paso 8. Configurar la Localización

Dialogue System soporta múltiples idiomas.

Actualmente incluye:

```text
ES
EN
```

Cada línea de diálogo debe contener:

```text
Text_ES
Text_EN

Audio_ES
Audio_EN

PromptText_ES
PromptText_EN
```

El idioma activo es seleccionado automáticamente por el GameInstance con la variable.

```text
CurrentLanguage
```
---

# Paso 9. Crear una Data Table

Crear una Data Table utilizando la estructura:

```text
ST_DialogueLine
```

Cada fila representa una línea del diálogo.

---

# Paso 10. Completar la información del diálogo

Cada fila permite configurar los siguientes campos.

## Speaker

Nombre del personaje.

---

## Text

Texto localizado.

---

## Audio

Audio localizado.

---

## DisplayType

Tipo de interfaz utilizada.

Valores disponibles:

```text
Subtitle

Center
```

---

## Type

Tipo de línea.

Valores disponibles:

```text
Dialogue

Input

Event

Cinematic
```

---

## ShowSpeaker

Determina si el nombre del personaje debe mostrarse.

```text
true

false
```

---

## PromptText

Texto contextual mostrado debajo del diálogo.

Ejemplo:

```text
Presiona E para continuar.
```

Si está vacío el Widget permanecerá oculto.

---

## SubtitleColor

Color utilizado por el texto.

---

## AllowSkip

Permite que el jugador omita la línea.

```text
true

false
```

---

## DelayAfter

Tiempo de espera antes de mostrar la siguiente línea.

---

## TriggerEvent

Nombre del evento personalizado.

Ejemplo:

```text
OpenDoor

PlayMusic

SpawnEnemy
```

---

# Paso 11. Iniciar una conversación

Desde cualquier Blueprint obtener el GameInstance.

Llamar:

```text
PlayConversation()
```

Parámetros:

- Data Table

El framework iniciará automáticamente toda la conversación.

---

# Flujo de una conversación

```text
PlayConversation()

↓

ConversationStarted()

↓

CreateWidgets()

↓

Load Conversation

↓

Initialize Variables

↓

Select Language

↓

Select Widget

↓

Update Widget

↓

Play Audio

↓

Wait

↓

TriggerEvent

↓

Clear Dialogue

↓

DelayAfter

↓

Next Dialogue

↓

ConversationFinished()

↓

Hide Widgets
```

---

# Agregar un nuevo Widget

Para crear un nuevo estilo de interfaz:

1.

Crear un Widget.

2.

Implementar:

```text
BPI_DialogueWidget
```

3.

Agregar un nuevo valor al Enum:

```text
EDialogueDisplay
```

4.

Actualizar:

```text
Dialogue_UpdateActiveWidget()
```

No es necesario modificar ninguna otra parte del framework.

---

# Agregar un nuevo idioma

Agregar nuevos campos a la estructura.

Ejemplo:

```text
Text_FR

Audio_FR

PromptText_FR
```

Actualizar la selección de idioma en el GameInstance.

Los Widgets no requieren modificaciones.

---

# Agregar un nuevo Trigger Event

Escribir el nombre del evento dentro de la Data Table.

Ejemplo:

```text
OpenDoor
```

Implementar la lógica correspondiente dentro de:

```text
ExecuteDialogueEvent()
```

No es necesario modificar el framework.

---

# Solución de problemas

## No aparece la interfaz

Verificar:

- GameInstance configurado.
- Widgets creados.
- Dialogue_CreateWidgets() ejecutado.

---

## El diálogo nunca continúa

Verificar que el Blueprint encargado del audio llame:

```text
AudioFinished()
```

---

## TriggerEvent no funciona

Verificar que el Blueprint implemente:

```text
BPI_DialogueEvents
```

---

## PromptText no aparece

Verificar:

- Que el texto no esté vacío.
- Que el Widget implemente UpdateDialogue().
- Que PromptText sea enviado desde el GameInstance.

---

## El Widget no cambia

Verificar:

```text
DisplayType
```

y

```text
Dialogue_UpdateActiveWidget()
```

---

# Recomendaciones

- Mantener toda la lógica del diálogo dentro del GameInstance.
- Evitar agregar lógica del juego dentro del framework.
- Utilizar Blueprint Interfaces para toda comunicación.
- Mantener las conversaciones completamente Data Driven.
- Crear nuevos Widgets implementando siempre BPI_DialogueWidget.
- Utilizar TriggerEvents para conectar el diálogo con la lógica del juego.


# Checklist de instalación

Antes de utilizar Dialogue System verifica que:

- [ ] BP_GameInstance configurado.
- [ ] Dialogue_CreateWidgets() ejecutado.
- [ ] Character implementa BPI_DialogueAudio.
- [ ] Character implementa BPI_DialogueAudioFinished.
- [ ] Widgets implementan BPI_DialogueWidget.
- [ ] Blueprint receptor implementa BPI_DialogueEvents.
- [ ] DataTable creada con ST_DialogueLine.
- [ ] Idiomas configurados.
- [ ] TriggerEvents implementados.
