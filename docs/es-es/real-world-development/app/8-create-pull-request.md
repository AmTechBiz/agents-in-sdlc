---
title: "Lección 8: Crear y combinar la PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil QA y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones y de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Es el momento de revisarlos juntos y abrir una solicitud de incorporación de cambios. Tú combinaste la solicitud de incorporación de cambios (PR) de las valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las instrucciones, la actualización de la habilidad y el agente QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una sesión y una rama, y lo incluirás en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el proceso de combinación.
- inspeccionarás la PR completa de la funcionalidad y las pruebas de verificación.
- autorizarás Agent Merge solo después de la revisión y confirmarás que la PR se ha combinado.

## Escenario

Durante el flujo de trabajo de filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener la autorización para combinar bajo el control del desarrollador.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para integrar una solicitud de incorporación de cambios en la aplicación GitHub Copilot. Al activarlo, la sesión de la aplicación lee la solicitud y resuelve los impedimentos: corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y reorganiza la base cuando hace falta. Después la combina en cuanto GitHub lo permite. Se ejecuta en segundo plano, continúa aunque reinicies la aplicación y se desactiva cuando se combina la solicitud.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de esa tarea, pero su capacidad para editar código y combinar todavía requiere tu autorización explícita. Revisa las acciones que puede realizar y el trabajo antes de conceder permiso para combinar.

## Usar Agent Merge para gestionar la PR

Ya has creado y revisado todo el código. Ahora dejaremos que Agent Merge gestione el proceso de la PR.

1. Utiliza el selector de agentes para seleccionar **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción en el menú desplegable no lo inicia.

Se inicia el proceso de Agent Merge. El agente:

- crea la solicitud de incorporación de cambios con un título y una descripción.
- si iniciaste la sesión desde una incidencia, la incluye en el cuerpo de la descripción.
- reorganiza la base o resuelve los conflictos de combinación con la rama de destino.
- supervisa el proceso de CI para comprobar que pasan todas las comprobaciones.
- supervisa los comentarios de otros desarrolladores o de la revisión de código de Copilot y realiza cambios para resolverlos.
- puede combinar automáticamente la PR cuando todo termina correctamente.

Permitiremos que Agent Merge también combine la PR cuando se superen todas las comprobaciones.

5. Revisa las diferencias de la PR y las pruebas de verificación antes de conceder permiso para combinar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Confirma que la opción **Merge pull request** está marcada.
8. Pide explícitamente al agente que combine la PR:

   ```plaintext
   Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
   ```

9. Supervisa el progreso y resuelve cualquier impedimento que se indique. Antes de continuar, confirma que el estado de la PR es **Merged**. Tener habilitado el permiso de combinación o un agente en ejecución no confirma que la PR se haya combinado.

> [!IMPORTANT]
> Agent Merge no omite las protecciones del repositorio ni los permisos que falten. Resuelve esos impedimentos antes de continuar.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la verificación, y ahora también el proceso de la solicitud de incorporación de cambios. En esta lección:

- aprendiste qué es Agent Merge y cómo automatiza el proceso de combinación.
- inspeccionaste la PR completa de la funcionalidad y las pruebas de verificación.
- autorizaste Agent Merge solo después de la revisión y confirmaste que la PR se había combinado.

A continuación, [utilizarás un lienzo existente y crearás uno de triaje][next-lesson] para explorar una forma más completa de examinar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
