# Revisión antes de publicar

La documentación se ha organizado usando las capturas del proyecto Full. Verifica lo siguiente:

1. Confirmar que los nombres de archivos PNG coincidan exactamente con los de la carpeta `images/`; las capturas no forman parte del ZIP.
2. **Pendiente**: capturar un ejemplo real de marcado Rich Text y la propiedad `Text Style Set` del widget. Nombres propuestos: `richtext-ejemplo-etiquetas.png` y `richtext-widget-configuracion.png`.
3. **Pendiente**: una captura de `ST_DecisionLine` y `DT_Decisions_Test` guardadas con los nombres `estructura-decision-line.png` y `configuracion-decisiones-data-table.png`.
4. **Pendiente**: las capturas de audio con nombres `dialogo-componente-voz.png`, `dialogo-audio-data-table.png`, `dialogo-audio-localizacion.png`, `dialogo-procesar-linea.png` y `dialogo-audio-reproduccion.png`.
5. Verifica en `Dialogue_PlayAudio` el criterio exacto de duración con y sin audio.
6. Verifica que cada llamada a `BPI_DialogueEvents` en Full mantiene el receptor esperado.
7. Confirmar versiones UE5 soportadas según la ficha de Fab y que Common UI está habilitado.
8. Revisar la guía `08-migracion.md` con una instalación limpia de Free y Full.
9. Sustituir o archivar las guías antiguas `Installation.md` y `ARCHITECTURE.md` si siguen en el repositorio, para evitar contenido obsoleto.
