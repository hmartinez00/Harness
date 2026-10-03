---
description: Orquesta las etapas de Spec Kit para dejar una iniciativa lista antes de implementarla.
agent: build
---

# Configurar una iniciativa con Spec Kit

Completa el flujo de Spec Kit de este proyecto para la iniciativa indicada por
el usuario. El objetivo es dejar sus artefactos de preimplementación completos,
coherentes y revisados. **No implementes la iniciativa.**

## Flujo

1. Lee las instrucciones vigentes del repositorio y detecta cómo está instalado
   y configurado Spec Kit: comandos, handoffs, scripts, templates y artefactos
   requeridos. Sigue ese flujo local; no presupongas stack, rutas, nombres de
   archivos ni convenciones que no estén configurados en el proyecto.
2. Recopila el objetivo de la iniciativa. Pregunta al usuario solo por la
   información esencial que falte y que no pueda obtenerse del repositorio.
3. Completa, en orden, todas las etapas locales de Spec Kit previas a
   implementación. Como mínimo, cuando estén disponibles:
   `specify → clarify → plan → tasks → analyze`.
   Usa los comandos o handoffs configurados y espera a que cada etapa termine
   antes de iniciar la siguiente. Ejecuta otras etapas (por ejemplo, checklist)
   cuando el flujo local las requiera.
4. Si una etapa detecta ambigüedades, inconsistencias o tareas incompletas,
   resuélvelas mediante la etapa Spec Kit correspondiente y vuelve a ejecutar
   las revisiones necesarias. No inventes decisiones: deja explícitos los
   bloqueos que requieran información o aprobación del usuario.
5. Verifica al final que los artefactos requeridos por el flujo existan, se
   correspondan entre sí y que el análisis final no deje problemas pendientes
   ocultos.

## Límites

- Deduce el stack y el contexto técnico de la iniciativa a partir del
  repositorio y de sus fuentes vigentes; distingue evidencia de supuestos.
- Respeta las instrucciones, plantillas, Constitution y convenciones existentes
  del proyecto. No crees una estructura documental paralela.
- No implementes tareas ni modifiques código de aplicación como parte de este
  flujo.
- No ejecutes operaciones destructivas ni cambies datos, dependencias o
  configuración de runtime. No hagas commits.
- Crea únicamente los artefactos que correspondan a las etapas de
  preimplementación configuradas en Spec Kit.
- La creación de un plan o tasks no constituye autorización para implementar.

## Cierre

Detente cuando el flujo previo a implementación esté completo o bloqueado.
Resume los artefactos creados o actualizados, las etapas completadas, el
resultado del análisis final y cualquier decisión, aprobación o información
pendiente. No declares el flujo completo si una etapa requerida no se ejecutó o
si su resultado no pudo verificarse.
