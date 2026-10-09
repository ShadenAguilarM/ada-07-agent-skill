# AI_USAGE_LOG.md

## Registro del uso de IA — ADA-07: Build an Agent Skill

Durante el desarrollo de ADA-07 utilicé **Antigravity como apoyo para analizar el procedimiento, proponer contenido y preparar los archivos de la Skill**. No tomé sus respuestas como definitivas: revisé los archivos generados, ajusté la redacción para que las instrucciones fueran más claras y comprobé que la estructura y el propósito de cada archivo correspondieran con la actividad. 

---

## Parte I — Construcción y evaluación de mi Skill

### 1. Workflow observation: identificar los pasos de la revisión

- **Objetivo/prompt:** Pedí a Antigravity apoyo para organizar el procedimiento de revisión de un Pull Request y documentar sus pasos.
- **Aporte de la IA:** Propuso revisar los requisitos o la especificación disponible, inspeccionar los archivos modificados, identificar pruebas pertinentes, clasificar hallazgos y preparar la información para revisión humana.
- **Decisión humana:** Revisé la secuencia y conservé los pasos que correspondían al objetivo de evaluar si los cambios estaban listos para revisión. Evité tratar las sugerencias de la IA como evidencia de que una prueba se había ejecutado si no había comprobado su resultado.
- **Impacto:** La observación del workflow sirvió como base para definir un procedimiento ordenado y reutilizable, en lugar de escribir la Skill únicamente a partir de una idea general.

### 2. Skill authoring: mejorar la descripción y los límites de uso

- **Objetivo/prompt:** Solicité ayuda para redactar el `SKILL.md`, en especial la descripción y las instrucciones del workflow.
- **Aporte de la IA:** Antigravity propuso una descripción centrada en la revisión de Pull Requests, junto con ejemplos de solicitudes que deberían activar la Skill y tareas que quedan fuera de su alcance.
- **Decisión humana:** Revisé la redacción y prioricé que la Skill tuviera una sola responsabilidad: evaluar la preparación de los cambios para una revisión humana. Mantuve límites explícitos para no implementar cambios, escribir comentarios en GitHub, aprobar ni combinar un Pull Request.
- **Impacto:** La descripción quedó orientada a facilitar una activación más precisa y a evitar que la Skill se utilice para tareas distintas de una revisión de preparación.

### 3. Workflow: ordenar el procedimiento y las condiciones de parada

- **Objetivo/prompt:** Pedí estructurar las instrucciones de ejecución de la Skill.
- **Aporte de la IA:** Sugirió una secuencia que parte de identificar el Pull Request o diff, consultar requisitos, inspeccionar cambios y pruebas, aplicar criterios de revisión y preparar la información para evaluación humana.
- **Decisión humana:** Revisé el orden para que cada paso tuviera una finalidad clara. Conservé la necesidad de detenerse o solicitar intervención humana cuando existan conflictos relevantes entre documentos normativos o cuando una decisión exceda el alcance de la revisión.
- **Impacto:** El workflow quedó más fácil de seguir y con límites explícitos, en vez de depender de instrucciones generales difíciles de verificar.

### 4. Script/reference design: separar criterios y verificaciones deterministas

- **Objetivo/prompt:** Solicité ayuda para organizar los archivos auxiliares y definir qué responsabilidades debía tener cada uno.
- **Aporte de la IA:** Antigravity ayudó a preparar el checklist de revisión y la guía de severidad en `references/`, además de plantear un script para comprobar la estructura del artefacto.
- **Decisión humana:** Conservé en `SKILL.md` las instrucciones principales del procedimiento y separé el conocimiento detallado en archivos de referencia. La validación mecánica de secciones obligatorias corresponde a `scripts/validate_review_report.py`; el script no debe decidir si el código es correcto ni sustituir el criterio de revisión.
- **Impacto:** Esta separación aplica el principio de *Progressive Disclosure*: mantener las instrucciones principales compactas y consultar los criterios detallados cuando sean necesarios, mientras que las comprobaciones repetibles se realizan de forma determinista.

### 5. Evaluation/refinement: revisar la validación del artefacto

- **Objetivo/prompt:** Pedí a Antigravity apoyo para definir cómo comprobar que el artefacto de revisión incluyera sus secciones obligatorias.
- **Aporte de la IA:** Propuso verificar elementos estructurales como `Review Context`, `Requirements / Acceptance Criteria Reviewed`, `Test Evidence`, las categorías de hallazgos y `Final Review Summary`.
- **Decisión humana:** Revisé el alcance de esas comprobaciones y mantuve la distinción entre validar la estructura y evaluar la calidad del contenido. Una validación estructural correcta no demuestra, por sí sola, que el análisis técnico sea correcto.
- **Impacto:** La validación se planteó como una comprobación objetiva y repetible, con un código de salida que permita distinguir entre una estructura válida y otra incompleta. La evaluación técnica permanece bajo revisión humana.

### 6. Evaluation/refinement: definir casos positivos, negativos y de límites

- **Objetivo/prompt:** Solicité apoyo para preparar casos que permitieran evaluar el comportamiento esperado de la Skill.
- **Aporte de la IA:** Propuso ejemplos que deberían activar la revisión, solicitudes que no deberían activarla y un caso en el que se solicita una acción fuera del alcance, como combinar un Pull Request.
- **Decisión humana:** Revisé los casos para que no se evaluara únicamente si la Skill se activa, sino también si respeta sus límites y deja las decisiones finales a una persona. No consideré suficiente una respuesta convincente sin evidencia del comportamiento observado.
- **Impacto:** La evaluación contempla activación, no activación, ejecución y límites de seguridad. Los resultados reales deben registrarse como `PASS` o `FAIL` de acuerdo con lo observado, no con lo que se esperaba que ocurriera.

---

## Parte II — Inspección y comparación con una Skill profesional

### 7. Third-party audit: inspeccionar antes de adoptar una Skill externa

- **Objetivo/prompt:** Utilicé Antigravity como apoyo para organizar la inspección previa de la Skill `code-review-and-quality` del repositorio `addyosmani/agent-skills`.
- **Aporte de la IA:** Ayudó a identificar los aspectos que debían revisarse antes de instalar una dependencia: propósito y descripción, condiciones de activación, referencias auxiliares, scripts o comandos, permisos y versión o commit.
- **Decisión humana:** Consideré que una Skill externa debe tratarse como una dependencia y no instalarse a ciegas. La inspección debe permitir identificar instrucciones inesperadas, comandos con efectos secundarios, permisos excesivos o falta de una versión reproducible. Estos son riesgos que deben comprobarse en la fuente; no se deben presentar como vulnerabilidades confirmadas sin evidencia.
- **Impacto:** La auditoría previa se orientó a tomar una decisión informada sobre su uso y a conservar límites de solo lectura. 

### 8. Professional skill comparison: ampliar el criterio de diseño

- **Objetivo/prompt:** Pedí ayuda para organizar una comparación entre mi Skill y `code-review-and-quality`, usando el mismo cambio o Pull Request como referencia.
- **Aporte de la IA:** Sugirió comparar aspectos como activación, alcance, profundidad del workflow, criterios de calidad, severidad, evidencia, verificación, seguridad y decisiones reservadas a una persona.
- **Decisión humana:** Tomé la comparación como una oportunidad para identificar diferencias de diseño, no para copiar automáticamente la solución de terceros. Una Skill más extensa puede aportar controles adicionales, pero también debe evaluarse si esos controles son pertinentes para el objetivo y el contexto del proyecto.
- **Impacto:** Mi criterio de diseño pasó a considerar no solo que la Skill describa pasos, sino que también delimite su autoridad, solicite evidencia para los hallazgos y permita verificar su comportamiento.

### 9. Revisión humana de los archivos generados

- **Objetivo/prompt:** Usé Antigravity para acelerar la creación y organización de los archivos de la actividad.
- **Aporte de la IA:** Generó borradores y propuso estructuras, listas y redacción para los documentos de la Skill y su evaluación.
- **Decisión humana:** Revisé los archivos antes de considerarlos terminados, corregí frases demasiado generales, mejoré la claridad y comprobé que las responsabilidades de `SKILL.md`, `references/`, `scripts/`, `assets/` y `evals/` no se confundieran. Cuando una afirmación requería evidencia de ejecución, la traté como algo que debía verificarse y no como un resultado confirmado por haber sido propuesto por la IA.
- **Impacto:** La IA funcionó como herramienta de apoyo para redactar y organizar, mientras que la revisión, selección de cambios y responsabilidad por el contenido final permanecieron de mi lado.

---

## Reflexión final

El uso de Antigravity facilitó la elaboración de los archivos y la exploración de alternativas, pero también me permitió distinguir entre **generar instrucciones**, **verificar condiciones deterministas** y **tomar decisiones de ingeniería**. Una Skill robusta no depende únicamente de un `SKILL.md` bien redactado: necesita un alcance claro, criterios de referencia, límites de seguridad y casos de evaluación. La IA puede apoyar el proceso, pero las afirmaciones sobre resultados, pruebas y riesgos deben estar respaldadas por comprobaciones reales y la decisión final corresponde al estudiante.