# 09 — Preguntas frecuentes y solución de problemas

[← Volver al índice](../README.md)

### No aparece el diálogo

Comprueba que `AC_DialogueManager` esté en el personaje correcto, que se llame `Dialogue Create Widgets` antes de `Dialogue Play Conversation`, y que el pin `Conversation` tenga una Data Table válida.

### No aparecen las decisiones

Verifica que **Common UI** esté habilitado, reinicia el editor si hace falta y confirma que la tabla y los widgets de decisiones de Full se estén utilizando. Si aparecen pero no reciben foco, revisa la configuración de enfoque/navegación del widget.

### Los eventos no hacen nada

El receptor debe implementar `BPI_DialogueEvents`. Verifica `TriggerEvent`, el `Event Name` y la rama correspondiente de `Switch on Name`. Recuerda que el sistema puede enviar el evento a varios actores con la interfaz.

### El audio no termina o la línea no avanza

Revisa las referencias `Audio_ES` / `Audio_EN`, `Duration`, `DelayAfter` y los eventos de finalización de audio en el componente. La configuración de interfaces de la arquitectura anterior debe validarse en Full actual.

### No se muestran palabras de color

Comprueba que estés utilizando un Rich Text Block y que sus estilos y marcas correspondan con los definidos en el widget. Verifica que la Data Table conserva el marcado correcto.

### El idioma es incorrecto

Comprueba qué idioma está seleccionado y qué columnas `Text_ES`, `Text_EN`, `Audio_ES`, `Audio_EN` y `PromptText` tienen contenido.

### Tengo conversaciones de Free: ¿debo rehacerlas?

No. Puedes reutilizar las Data Tables de conversaciones y conectarlas a Full. Las decisiones y etiquetas Rich Text se configuran aparte.

### ¿Debo usar `BP_DialogueGameInstance`?

No. La edición Full actual utiliza `AC_DialogueManager` y `AC_DecisionManager` como Actor Components.

## Si necesitas ayuda

Adjunta una captura de los nodos, el nombre exacto del asset, la versión de Unreal Engine y el mensaje de error. [Comunidad de Discord](https://discord.gg/JY5JTQaWqn).
