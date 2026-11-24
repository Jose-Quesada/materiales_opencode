# 3. `/init`, `AGENTS.md` y patrones a ignorar

Cuando abres una sesión nueva, el modelo no sabe nada de tu proyecto a menos que explore archivos manualmente en cada turno (consumiendo tiempo y tokens). Para darle contexto instantáneo y controlar qué partes del repositorio puede o no puede buscar, OpenCode combina **`/init` (`AGENTS.md`)** con **`.gitignore` y `.ignore`**.

---

## 1. Preparar el `README.md` antes de ejecutar `/init`

Antes de pedirle a OpenCode que analice el proyecto, es muy recomendable contar con una sesión limpia (++ctrl+x++ ++n++) y un `README.md` que describa qué es el proyecto y qué tecnologías usa (o planea usar).

En el ejemplo del módulo se crea `README.md` en la raíz de `01-demo` (tomado de [este Gist](https://gist.github.com/Klerith/a4d015f6cd9c94bc0d14cfb8c5722a31)):

```markdown
# Mi Primera Página Web

Esta es una página web en la cual vamos a usarla para explorar y aprender un poco sobre OpenCode.

## Tecnologías Usadas

- HTML
- CSS
- Tailwind CSS
- JavaScript

## Contacto

- Email: juanpablo.m@outlook.com
```

---

## 2. El comando `/init` y la generación de `AGENTS.md`

Al ejecutar `/init` en la TUI, OpenCode no hace magia oculta: **inyecta un prompt predefinido** que le ordena al agente inspeccionar el proyecto (`README.md`, `git status`, `ls -la`, archivos clave como `index.html`) y redactar un archivo **`AGENTS.md`** en la raíz del repositorio.

### ¿Qué genera `/init` en `01-demo`?

Un ejemplo real de `AGENTS.md` generado por `/init` incluye secciones claras y cortas:

- `# 01-demo`
- `## Quick start`: Cómo abrir/probar el proyecto (ej. abrir `index.html` directamente en el navegador, sin servidor ni *build*).
- `## Stack`: Detalla lo que existe hoy y **aclara explícitamente las discrepancias**: por ejemplo, advierte que `Tailwind CSS` figura en el `README.md`, pero **todavía no está instalado ni configurado** en `index.html` (evitando que futuros agentes asuman un CDN o *build step* inexistente).
- `## Conventions`: Convenciones de estilo, idioma o estructura del código.

### Buenas prácticas con `AGENTS.md`

1. **Mantenlo conciso:** `AGENTS.md` se carga al inicio de las conversaciones; si lo llenas de texto innecesario, inflarás el contexto en cada turno.
2. **Usa referencias externas con `@`:** Si tienes guías largas (por ejemplo, reglas detalladas de diseño o estilo), guárdalas en un archivo aparte y refiérelo dentro de `AGENTS.md` como `@style-guide.md`. Así el agente solo lo leerá cuando realmente necesite consultarlo.
3. **Actualízalo periódicamente y haz commit:** Cada vez que cambie el *stack* (por ejemplo, al instalar Tailwind) o la arquitectura, actualiza `AGENTS.md` y súbelo a Git para que todo el equipo comparta la misma memoria de proyecto.

---

## 3. Patrones a ignorar: `.gitignore` vs `.ignore`

Por defecto, OpenCode utiliza herramientas internas basadas en `ripgrep` para buscar archivos con `@` y escanear el código. Saber distinguir entre `.gitignore` y `.ignore` te permite proteger secretos, evitar carpetas gigantes y afinar qué ve el agente.

| Archivo | ¿Lo ignora Git? | ¿Lo ignora OpenCode (`@` y búsquedas internas)? | Caso de uso ideal |
| :--- | :--- | :--- | :--- |
| **`.gitignore`** | **Sí** | **Sí** (por defecto) | Dependencias (`node_modules/`), builds (`dist/`), variables de entorno (`.env`) |
| **`.ignore`** | **No** | **Sí** | Carpetas sensibles o de código propio que **sí** quieres en Git (ej. `auth/`), pero no quieres que el agente indexe o toque por accidente |

### Ejemplo práctico 1: `.gitignore` con `node_modules` y `.env`

1. Crea `.gitignore` en la raíz con:

```gitignore
node_modules
.env
```

2. Crea el archivo de prueba `node_modules/no-tocar/no-tocar.js` con `console.log('No tocar');`.
3. En OpenCode, escribe `@no`: verás que **no aparece** en el autocompletado y Git tampoco lo rastreará.

### Ejemplo práctico 2: `.ignore` con `auth/auth.js`

¿Qué pasa si tienes una carpeta crítica de autenticación que **debe** versionarse en Git, pero no quieres que el agente la encuentre en búsquedas generales con `@` o `grep`?

1. Crea `auth/auth.js`.
2. Crea `.ignore` en la raíz del proyecto y añade:

```gitignore
auth/*
```

3. Prueba escribir `@auth` en OpenCode: **dejará de mostrarse** en las sugerencias y búsquedas internas de herramientas, mientras que en **Source Control (Git)** seguirá rastreándose con normalidad.

### Excepciones: incluir carpetas ignoradas (`!`)

Tal como indica la [documentación oficial de OpenCode](https://opencode.ai/docs/tools/#internals), si en algún proyecto necesitas que OpenCode **sí** pueda buscar dentro de carpetas que están en `.gitignore` (como dependencias o compilados), puedes usar el prefijo de negación `!` dentro de `.ignore`:

```gitignore
!node_modules/
!dist/
!build/
```

!!! note "Seguridad por capas"
    Recuerda que `.ignore` oculta archivos de las búsquedas automáticas (`@`, `glob`, `grep`), pero si le pasas una ruta explícita o tienes permisos abiertos, conviene complementarlo con reglas claras en `AGENTS.md` y revisiones en Git.
