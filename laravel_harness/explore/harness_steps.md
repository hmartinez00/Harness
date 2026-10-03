# Pasos de implementacion de Harness

### Instalacion de speckit

```
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
```

### Inicializar
```
specify init . --integration opencode
```

### Crear AGENTS.md
```
/init (Esto deberia crear el AGENTS.md)
```

### Crear primera constitucion
```
/speckit.constitution
```

### Implementar Agentes

- Revisor documental
- Implementador

### Implantar gobierno documental

### Conectar MCP y SKILLS

```
https://mcp.context7.com/mcp
```

```
npx skills add https://github.com/anthropics/skills --skill frontend-design
```

```
laravel-blade-tailwind-crud
```






Tomar el control de un proyecto legacy utilizando OpenCode como arnés de IA y GitHub Spec Kit como la metodología de Desarrollo Dirigido por Especificaciones (SDD) es una excelente estrategia. Evita que la IA actúe a ciegas o rompa dependencias críticas del código antiguo. [1, 2, 3, 4, 5] 
A continuación se detalla el flujo de pasos secuenciales para configurarlo todo desde cero:
------------------------------
## Paso 1: Instalación de las Herramientas
Primero debes preparar el entorno de tu máquina instalando el CLI de Spec Kit y el runtime de OpenCode. [2, 3] 

   1. Instalar Spec Kit (CLI global): Se recomienda su instalación moderna a través del gestor de paquetes de Python rápido uv:
   
   uv tool install specify-cli
   
   2. Instalar OpenCode: Dependiendo de tu preferencia, instala su CLI nativo de terminal o su aplicación de escritorio: [2, 3, 6] 
   
   # En macOS/Linux vía script oficial de OpenCode o usando npm para su SDK
   npm install -g @opencode-ai/sdk
   
   
------------------------------
## Paso 2: Inicialización e Integración en el Proyecto Legacy
Navega a la raíz de tu proyecto heredado para acoplar ambas herramientas. [7] 

   1. Inicializar Spec Kit apuntando a OpenCode: Ejecuta el comando de inicialización indicando el adaptador correcto:
   
   specify init --integration opencode
   
   Esto creará la estructura base del entorno de especificaciones, incluyendo la carpeta .specify/. [3, 7] 
   2. Arrancar el entorno OpenCode: Abre el proyecto y ejecuta el comando nativo para que el arnés escanee el stack tecnológico antiguo y genere las bases operativas:
   
   opencode /init
   
   Esto generará de forma automática el archivo AGENTS.md (o actualizará tu opencode.json) detectando comandos heredados de compilación y testing. [8, 9] 

------------------------------
## Paso 3: Configuración del Arnés de Control (opencode.json)
Crea o edita el archivo opencode.json en la raíz para orquestar cómo OpenCode consumirá las directivas de Spec Kit: [8] 

{
  "$schema": "https://opencode.ai/config.json",
  "instructions": ["AGENTS.md", ".specify/memory/constitution.md"],
  "default_agent": "plan",
  "permissions": {
    "bash": "ask",
    "edit": "ask",
    "webfetch": "allow"
  }
}


* 
* Importante: Establecer el default_agent en "plan" y los permisos en "ask" garantiza que la IA no intente reescribir archivos ni ejecutar comandos aleatorios en tu base de código legacy sin que tú apruebes el paso primero. [1, 8] 
* 

------------------------------
## Paso 4: Definición de Líneas Rojas (constitution.md)
Antes de pedirle a la IA que toque el código legacy, debes establecer la "constitución" del proyecto. Este archivo le prohíbe explícitamente a la IA cometer errores comunes del software antiguo. [3] 
Ejecuta el comando:

/speckit.constitution

Dentro de .specify/memory/constitution.md, asegúrate de que se listen restricciones estrictas para el código heredado, por ejemplo:

* 
* No actualizar versiones de librerías globales sin previo aviso.
* Mantener compatibilidad estricta con la versión actual de entorno (ej. Java 8, Node 14, Python 3.8).
* Prohibido refactorizar clases masivas; los cambios deben ser modulares y aislados.
* 

------------------------------
## Paso 5: El Flujo Operativo para Intervenciones (SDD)
Con el arnés blindado, cada vez que requieras resolver un bug, añadir una función o modernizar una sección del proyecto legacy, seguirás estrictamente el flujo secuencial de Spec Kit directamente en la interfaz de OpenCode: [3, 10] 

   1. /speckit.specify → Describe el cambio que necesitas. La IA creará una especificación clara en Markdown dentro de specs/ validando el impacto técnico en el software antiguo. [3] 
   2. /speckit.plan → La IA diseñará la estrategia técnica paso a paso basándose en la especificación. Aquí evaluará efectos secundarios o regresiones en el código legacy. [3, 11] 
   3. /speckit.tasks → Divide el plan en micro-tareas atómicas y accionables (tasks/). [3, 12] 
   4. /speckit.implement → OpenCode empezará a escribir el código de manera controlada y secuencial, pidiéndote confirmación en cada bloque. [3] 
   5. /speckit.converge → Fase de cierre. OpenCode ejecutará la suite de pruebas heredada para garantizar que nada se rompió y cerrará el ciclo del cambio. [3] 

Si estás listo para empezar, dime:

* 
* ¿Cuál es el lenguaje o framework principal de tu proyecto legacy?
* ¿Tiene actualmente una suite de pruebas automatizadas (unitarias, integración, etc.) que podamos mapear en el archivo de configuración?
* 


[1] [https://innfactory.ai](https://innfactory.ai/en/ai-harness/opencode/)
[2] [https://www.teamday.ai](https://www.teamday.ai/harness/opencode)
[3] [https://www.webreactiva.com](https://www.webreactiva.com/blog/spec-kit)
[4] [https://harn.app](https://harn.app/kb/specs)
[5] [https://www.rifty.ai](https://www.rifty.ai/labs/bmad-vs-spec-kit)
[6] [https://pub.towardsai.net](https://pub.towardsai.net/why-opencode-beat-out-every-other-ai-coding-harness-i-tried-4f1d60922303)
[7] [https://www.youtube.com](https://www.youtube.com/watch?v=2kXr9PvOoPc&t=467)
[8] [https://news.ycombinator.com](https://news.ycombinator.com/item?id=47941721)
[9] [https://www.youtube.com](https://www.youtube.com/watch?v=5jCF5KG2xOk&t=185)
[10] [https://opencode.ai](https://opencode.ai/docs/ecosystem/)
[11] [https://github.com](https://github.com/formin/spec-kit-harness)
[12] [https://github.com](https://github.com/deepklarity/harness-kit)







Al integrar TDD (Test-Driven Development) como el motor del flujo operativo en un proyecto legacy controlado por OpenCode y Spec Kit, la estructura cambia drásticamente. En TDD, las pruebas automatizadas se escriben antes que el código de producción, por lo que el arnés de OpenCode debe configurarse para validar el ciclo Red-Green-Refactor (Rojo-Verde-Refactorizar).
Así se modifica el flujo completo desde el inicio:
------------------------------
## 1. Modificación en la Configuración Inicial (AGENTS.md y opencode.json)
Para que TDD funcione con código heredado, el arnés de OpenCode debe tener la capacidad de ejecutar micro-pruebas de forma ultrarrápida y continua.

* En el AGENTS.md, debes definir obligatoriamente el comando para correr un test específico y aislado (no la suite completa, que en proyectos legacy suele tardar horas):

## TDD Core Commands- Run single test file: `npm test -- {test_file_path}` # O el equivalente en tu framework
- Watch mode: `npm test -- --watch`

* En el opencode.json, se añade un disparador automático (hook) para que OpenCode valide el estado del test tras cada cambio de línea:

{
  "tdd_mode": true,
  "auto_verify_command": "npm test -- {current_spec_test}"
}


------------------------------
## 2. Modificación en la Constitución (constitution.md)
Debes añadir la regla de oro de TDD en las directivas innegociables de la IA:

* "Ninguna línea de código de producción puede ser escrita o modificada por la IA a menos que exista una prueba unitaria fallando previamente que lo justifique."
* "El código heredado que no tenga cobertura debe ser envuelto en un test de caracterización antes de aplicar cambios."

------------------------------
## 3. El Nuevo Flujo Operativo Secuencial (SDD + TDD)
El flujo secuencial de Spec Kit se altera a partir del paso de planificación para dar prioridad absoluta a los archivos de pruebas (*.test.js, *_test.py, etc.):

| Paso Tradicional | Paso Adaptado a TDD | Qué hace OpenCode en este punto |
|---|---|---|
| 1. /speckit.specify | 1. /speckit.specify | Define los requisitos de la nueva funcionalidad o el comportamiento esperado del bug a corregir. |
| 2. /speckit.plan | 2. /speckit.plan_tdd | La IA no diseña la arquitectura final. Diseña la estrategia de pruebas: qué aserciones, mocks de dependencias legacy y casos de borde se van a probar primero. |
| 3. /speckit.tasks | 3. /speckit.tasks_tdd | Las tareas se dividen estrictamente en el ciclo TDD: 1. Escribir Test X. 2. Hacer pasar Test X con el código mínimo. 3. Refactorizar. |
| 4. /speckit.implement | 4.1 /speckit.test_red (¡Nuevo!) | Fase Roja: La IA genera únicamente el archivo de pruebas y OpenCode lo ejecuta. El flujo se detiene y solo avanza si el test falla con un error esperado (ej. Method not found). |
| | 4.2 /speckit.implement_green | Fase Verde: La IA escribe el código de producción estrictamente necesario (puede ser "sucio" o hardcodeado) para que el test pase. OpenCode ejecuta el test y confirma el color verde. |
| | 4.3 /speckit.refactor (¡Nuevo!) | Fase de Refactorización: OpenCode analiza el código recién escrito para limpiarlo, eliminar duplicados o adaptarlo a los estándares del proyecto legacy, asegurando que el test siga en verde. |
| 5. /speckit.converge | 5. /speckit.converge | Corre la suite completa de tests de regresión del sistema legacy para asegurar que el nuevo código bajo TDD no rompió partes antiguas no cubiertas. |

## El gran beneficio en proyectos Legacy
Al obligar a OpenCode a ejecutar 4.1 /speckit.test_red, creas una red de seguridad automática. Si la IA intenta programar a ciegas en el código antiguo, el arnés de OpenCode bloqueará la ejecución del código de producción hasta que demuestre que el entorno de pruebas entiende el problema.
¿Qué framework de testing utiliza tu proyecto legacy (Jest, PyTest, JUnit, Mocha)? Si me lo compartes, te puedo escribir el comando de ejecución exacta para el bucle test_red / implement_green que leerá OpenCode.

