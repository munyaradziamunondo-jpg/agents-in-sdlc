---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido a Copilot cloud agent que realice tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque cloud agent trabaja de forma autónoma y asíncrona, es importante poder supervisar esas tareas.

Hay varias herramientas para gestionar las tareas asignadas a cloud agent, incluida la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de los agentes con solicitudes de incorporación de cambios (PR) abiertas, explorar las operaciones realizadas y orientar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas de cloud agent.
- orientarás una sesión en curso para solicitar una funcionalidad adicional.

## Escenario

Después de asignar al agente la implementación de un modo de alto contraste, el equipo se dio cuenta de que también sería un buen momento para añadir un modo claro. Como ya se estaba trabajando en actualizar los estilos del sitio y añadir la opción de cambiar de modo, parecía razonable incluir esta funcionalidad. Orientarás el trabajo del agente para que añada tanto el modo claro como el modo de alto contraste.

## Revisa las tareas de Copilot cloud agent

Consulta el estado de todas las tareas asignadas a Copilot cloud agent.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa las tareas del panel principal y del panel izquierdo. Deberías ver las tareas que has asignado a Copilot, como:
    - actualizar la documentación de la base de código.
    - generar API para modificar juegos.
    - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso y revisa las operaciones que ha realizado Copilot. Pueden incluir:
    - descargar el código del repositorio.
    - crear el entorno de trabajo de Copilot.
    - configurar servidores del Model Context Protocol (MCP).
    - realizar varios pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos concretos que aparecen dependen del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Observa también el panel de solicitudes de incorporación de cambios (PR) que aparece a la derecha. Desde allí puedes ver la PR y los archivos modificados para supervisar el trabajo.

## Orienta cloud agent

Ahora que has visto las tareas activas, pide a Copilot que añada la opción de modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que haya elegido Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Verás mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    Ya que estamos trabajando en el modo de alto contraste, añadamos también un modo claro. Debe haber un control para este modo que permita a los usuarios seleccionar el modo de visualización que prefieran.
    ```

    ![Tarea de cloud agent en la página de agentes con el cuadro de diálogo para orientar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma que lo ha recibido y lo incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot seguirá trabajando en la tarea actualizada. Incorporará la nueva solicitud al flujo de trabajo cuando termine el paso que estaba realizando al recibir el mensaje.

El proceso tardará varios minutos. Puedes aprovechar el tiempo para reflexionar sobre lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro de control para supervisar y guiar las tareas de GitHub Copilot cloud agent. Desde allí puedes seguir las tareas activas y completadas, revisar el trabajo y redirigir las tareas en curso para ajustar su alcance o aportar nuevas directrices.

Has explorado estos conceptos:

- has explorado mission control y la página de agentes para supervisar las tareas de cloud agent.
- has redirigido una sesión en curso para solicitar una funcionalidad adicional.

Mientras Copilot completa las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
