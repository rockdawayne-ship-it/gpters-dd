[English](README.md) | [한국어](README.ko.md) | [中文](README.zh.md) | [日本語](README.ja.md) | Español

# dd

<p align="center">
  <img src="assets/dd-hero-01.png" alt="dd" width="320">
</p>

> Pásale a Claude Code un log largo o una captura de pantalla sin pegarlos en el chat.

Trabajando en Claude Code pegas cosas todo el tiempo: logs de error cuando algo se rompe, una referencia cuando estás construyendo, una captura para mostrar lo que quieres. `dd` lo lee directamente del portapapeles, lo guarda en un archivo local y pasa a la conversación solo lo necesario — a veces analizando el material grande en segundo plano y trayendo de vuelta solo la conclusión.

[Inicio rápido](#inicio-rápido) • [¿Por qué dd?](#por-qué-dd) • [Cómo funciona](#cómo-funciona) • [Funciones](#funciones) • [Comandos](#comandos) • [Requisitos](#requisitos)

---

## Inicio rápido

### 1. Añade el marketplace

```
/plugin marketplace add https://github.com/fivetaku/gptaku_plugins.git
```

### 2. Instala

```
/plugin install dd
```

### 3. Reinicia Claude Code

(Los comandos nuevos solo se registran tras un reinicio.)

### 4. Úsalo

Pon algo en el portapapeles y escribe `/dd` (o `/ㅇㅇ`) con tu petición — sin pegar nada:

- **Imagen — Windows:** captura con `Win`+`Shift`+`S` y luego `/dd ¿por qué se ve así?` (la captura ya está en el portapapeles)
- **Imagen — macOS:** captura con `Shift`+`Cmd`+`4` y luego `/dd hazlo como esta referencia` (configuración única: haz que las capturas se guarden en el portapapeles — barra de capturas `Shift`+`Cmd`+`5` → **Opciones** → **Guardar en: Portapapeles**)
- **Texto largo / log de error:** cópialo y luego `/dd ¿qué error es?` — nunca aterriza en el chat como un muro de texto, así que la sesión se mantiene ligera
- **Sin petición:** solo `/dd` — lee el portapapeles y continúa a partir de la conversación

Con un IME coreano puedes escribir `/dd` tal cual — sale como `/ㅇㅇ`, que es lo mismo. Sin cambiar de idioma.

---

## ¿Por qué dd?

Trabajando en Claude Code pegas cosas constantemente. Un log de error cuando algo se rompe, una referencia cuando construyes, una captura para mostrar el aspecto que buscas. Pegarlo todo directamente en el chat choca con dos problemas.

El texto largo se come la conversación. Suelta un log grande una vez y ahí se queda, ocupando espacio y releyéndose en cada turno, así que la sesión se vuelve poco a poco más lenta y más cara. `dd` guarda el texto completo en un archivo, le entrega a Claude un resumen corto y lee más solo cuando la tarea lo necesita.

Las imágenes a menudo ni siquiera se dejan pegar. Según tu entorno, puede que Claude Code no acepte una imagen pegada. `dd` lee el portapapeles directamente, así que basta con capturar y escribir `/dd`. Un archivo de imagen copiado llega como la imagen real, no como su icono.

También dejas de repetirte: se acabó el "el error que acabo de copiar" o "la referencia de arriba". Y si `dd` agarra lo que no es (el portapapeles no lleva marca de tiempo), te muestra lo que capturó y pregunta primero.

El nombre `dd` no significa nada. Lo usas a menudo y escribir algo largo cada vez es un fastidio, así que son solo dos pulsaciones rápidas. (Con un IME coreano esas mismas teclas salen como `ㅇㅇ`, que funciona igual.)

---

## Cómo funciona

```
copiar / captura de pantalla
   → /dd [petición]
   → dd_clipboard.py captura el portapapeles actual en ~/dd/<fecha>/<id>/
       · texto → content.txt   imagen → image.png   + manifest.json
   → Claude lee el resumen del manifest (no el contenido completo)
   → muestra lo que capturó; confirma si algo no cuadra; si no, actúa
```

El portapapeles del sistema solo guarda la copia más reciente (sin historial). `dd` siempre captura ese elemento actual, oculta los secretos en la vista previa y limpia las capturas de más de 7 días.

---

## Funciones

| Función | Descripción |
|---------|-------------|
| Captura de texto e imagen | Portapapeles de macOS / Windows / WSL / Linux, texto o imagen |
| Inyección de patrón de instrucciones | Captura en cada `/dd` vía `${CLAUDE_PLUGIN_ROOT}` — funciona para todos |
| Puerta de confirmación | Pregunta antes de actuar solo cuando el portapapeles parece ajeno o ambiguo |
| Lectura perezosa | Lee según `size_class`, así un pegado enorme nunca inunda el contexto |
| Ocultación de secretos | `api_key`, `Bearer`, `sk-`, `ghp_`, `xoxb-`, etc. enmascarados en la vista previa |
| Limpieza automática | Las capturas de más de `DD_RETENTION_DAYS` (7 por defecto) se borran en cada ejecución |
| Análisis en segundo plano | Los logs o imágenes grandes se analizan en un subagente, y solo el resultado entra al chat |
| Responde en tu idioma | Contesta en el idioma de tu petición, o en el de la conversación si no hay ninguna |

---

## Comandos

| Comando | Descripción |
|---------|-------------|
| `/dd [petición]` | Captura el portapapeles actual y actúa sobre él |
| `/ㅇㅇ [petición]` | Igual que `/dd` (alias en modo hangul) |

### Disparadores en lenguaje natural

- "방금 캡처한 거 분석해줘", "이 레퍼런스로 만들어줘", "스크린샷 드롭"
- "drop clipboard", "use what I copied", "this screenshot"

---

## Requisitos

- [Claude Code](https://docs.anthropic.com/claude-code) CLI
- Python 3 (solo biblioteca estándar)
- **macOS**: funciona de fábrica (`pbpaste` / `osascript`)
- **Windows / WSL**: PowerShell (integrado) — verificado por usuarios
- **Linux**: una herramienta de portapapeles instalada (`xclip` / `wl-clipboard`) y una sesión gráfica

---

## Licencia

MIT

---

<div align="center">

Copia algo y luego `/dd`.

</div>
