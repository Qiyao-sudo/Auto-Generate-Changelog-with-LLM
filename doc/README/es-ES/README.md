# 🚀 Generación Automática de Changelog con LLM

🌐 **Idiomas**: [English](doc/README/en-US/README.md) | [中文](doc/README/zh-CN/README.md) | [日本語](doc/README/ja-JP/README.md) | [Deutsch](doc/README/de-DE/README.md) | [Español](doc/README/es-ES/README.md) | [Русский](doc/README/ru-RU/README.md) | [العربية](doc/README/ar-SA/README.md)

Una herramienta automatizada basada en GitHub Actions que utiliza modelos de lenguaje grande (LLM) como DeepSeek para generar automáticamente registros de cambios (Changelog) estructurados y estandarizados. No es necesario escribir manualmente, solo tienes que activar el flujo de trabajo para obtener documentación de actualización de nivel profesional.

## ✨ Características

- 🤖 **Análisis inteligente**: Basado en LLM para analizar inteligentemente los registros de Git, categorizando automáticamente en nuevas funciones, optimizaciones de rendimiento, correcciones de errores, etc.
- 🏷️ **Gestión automática de etiquetas**: Soporta la creación automática de etiquetas Git y el cálculo inteligente de diferencias entre versiones.
- 📁 **Almacenamiento estructurado**: Almacena los documentos de Changelog generados de forma jerárquica por versión principal y subversión.
- 🎨 **Plantillas profesionales**: Proporciona plantillas Markdown estandarizadas con formato uniforme y atractivo.
- 🔧 **Altamente configurable**: Soporta API de LLM personalizada, modelos, plantillas de prompts y otros parámetros.
- ⚡ **Activación con un clic**: Activado manualmente a través de GitHub Actions, simplemente ingresa el número de versión para generar automáticamente.

## 🚀 Inicio Rápido

### Requisitos previos

1. **Repositorio de GitHub**: GitHub Actions habilitado
2. **Clave de API de LLM**: Clave de API para DeepSeek u otros servicios LLM compatibles con la API de OpenAI
3. **Entorno Python**: Entorno Ubuntu en GitHub Actions (configurado automáticamente)

### Pasos de instalación

1. **Copiar archivo de flujo de trabajo**: Copia `.github/workflows/generate-changelog.yml` en la misma ruta de tu repositorio
2. **Copiar archivos de script**: Copia el directorio `.github/workflows/scripts/` en tu repositorio
3. **Configurar Secrets**: Agrega los siguientes Secrets en Configuración del repositorio → Secrets and variables → Actions:
   - `LLM_API_KEY`: Tu clave de API de LLM
   - (Opcional) `LLM_API_ENDPOINT`: Punto de extremo de la API de LLM, por defecto DeepSeek
   - (Opcional) `LLM_API_MODEL`: Nombre del modelo a usar, por defecto `deepseek-chat`

## 📖 Uso

### Activar el flujo de trabajo manualmente

1. Ve a la página **Actions** de tu repositorio de GitHub
2. Selecciona el flujo de trabajo **Auto Generate Changelog with DeepSeek**
3. Haz clic en el botón **Run workflow**
4. Ingresa los siguientes parámetros:
   - **main_version**: Número de versión principal (por ejemplo, `1.X`)
   - **sub_version**: Número de versión completa (por ejemplo, `1.X.X`)
   - **current_tag**: Nombre de la etiqueta Git actual (por ejemplo, `v1.X.X`, se creará automáticamente si no existe)

### Proceso de ejecución del flujo de trabajo

1. **Extraer código**: Extraer el historial completo de Git y todas las etiquetas
2. **Procesamiento de etiquetas**: Verificar si la etiqueta especificada existe, crearla si no existe
3. **Análisis de diferencias de commits**: Calcular inteligentemente las diferencias de commits con la versión anterior
4. **Generación LLM**: Llamar a la API de DeepSeek para generar un Changelog estructurado
5. **Guardar archivo**: Guardar el documento generado en `doc/Changelogs/{versión principal}/{subversión}/App.md`
6. **Commit automático**: Commit del archivo de Changelog generado al repositorio
7. **Push de etiqueta**: Si es una etiqueta nueva, hacer push automático al repositorio remoto

## ⚙️ Instrucciones de configuración

### Configuración de variables de entorno

Configura las siguientes variables en GitHub Secrets:

| Nombre de variable | Requerido | Valor por defecto | Descripción |
|-------------------|-----------|-------------------|-------------|
| `LLM_API_KEY` | ✅ | Ninguno | Clave de API de LLM |
| `LLM_API_ENDPOINT` | ❌ | `https://api.deepseek.com/` | Dirección del punto de extremo de la API |
| `LLM_API_MODEL` | ❌ | `deepseek-chat` | Nombre del modelo a usar |

### Plantilla de prompt personalizada

Para modificar el formato de salida, edita el archivo `.github/workflows/scripts/template.txt`:

```txt
Eres un ingeniero de documentación de desarrollo de software. Por favor, genera un registro de cambios (Changelog) estructurado, legible y estandarizado basado en los siguientes registros de Git:
Requisitos:
1. Salida en formato Markdown, adaptado al estilo de documentación del proyecto App
2. Categorización: Nuevas funciones, Optimizaciones de rendimiento, Correcciones de errores, Refactorización de código, Actualizaciones de dependencias
3. Lenguaje conciso y formal, eliminando descripciones de commits de merge y wip inválidas
4. Encabezado con número de versión principal, número de versión completa, etiqueta actual y fecha de actualización
5. Solo salida de texto principal de Markdown, sin explicaciones adicionales, sin introducción
...
```

### Ruta del archivo de salida

La ruta del archivo de Changelog generado tiene el siguiente formato:
```
doc/Changelogs/{main_version}/{sub_version}/App.md
```

Por ejemplo, cuando `main_version=1`, `sub_version=1.2.3`, la ruta del archivo es:
```
doc/Changelogs/1/1.2.3/App.md
```

## 📁 Estructura del proyecto

```
.github/
├── workflows/
│   ├── generate-changelog.yml    # Definición del flujo de trabajo de GitHub Actions
│   └── scripts/
│       ├── gen_changelog.py      # Script principal de generación
│       └── template.txt          # Plantilla de prompt LLM
README.md                         # Documentación del proyecto
```

### Descripción de archivos clave

- **generate-changelog.yml**: Definición del flujo de trabajo de GitHub Actions con el proceso completo de generación automática
- **gen_changelog.py**: Script de Python que lee commits, llama a la API de LLM y guarda los resultados
- **template.txt**: Plantilla de prompt que controla el formato y el contenido de la salida LLM

## 🎯 Ejemplo de uso

### Ejemplo de activación del flujo de trabajo

1. **Parámetros de entrada**:
   - main_version: `1`
   - sub_version: `1.2.0`
   - current_tag: `v1.2.0`

2. **Resultado de ejecución**:
   - Creación automática de la etiqueta `v1.2.0` (si no existe)
   - Generación del archivo `doc/Changelogs/1/1.2.0/App.md`
   - Commit automático del archivo generado al repositorio

### Ejemplo de Changelog generado

```markdown
# 📝 Registro de cambios de versión
## [v1.2.0] - 2026-04-13

### ✨ Nuevas funciones
- 🌓 Nueva función de cambio entre temas claro/oscuro, mejora de la experiencia visual de la interfaz
- 🎨 Nueva lógica de cambio de color del lienzo, soporte para adaptación dinámica al tema

### 🐛 Correcciones de errores
- 🔧 Corrección de problemas de compatibilidad con comandos de inicio del backend multiplataforma
- 📂 Corrección de configuración de ruta incorrecta en app.py

### 🚀 Optimizaciones de funciones
- 🎭 Unificación de valores de color codificados en la interfaz a variables de tema, mejora de la consistencia del estilo visual
- 🎛️ Ajuste de la posición del icono de cambio de idioma, mejora de la lógica de interacción de operación
```

## ⚠️ Notas

1. **Costos de llamada a API**: El uso de la API de LLM puede generar costos. Asegúrate de entender el método de facturación del servicio que uses.
2. **Estabilidad de red**: Asegúrate de que GitHub Actions pueda acceder al punto de extremo de la API de LLM configurado.
3. **Calidad de los commits**: La calidad del Changelog generado depende de la claridad y completitud de los mensajes de commit.
4. **Convención de nombres de etiquetas**: Se recomienda usar nombres de versiones semánticas, como `v1.0.0`, `v2.1.3`, etc.
5. **Requisitos de permisos**: El flujo de trabajo necesita permisos de escritura en el repositorio. Asegúrate de que GitHub Actions tenga suficientes permisos.

## ❓ Preguntas frecuentes

### Q1: ¿Qué sucede si no hay una etiqueta de versión anterior?
A: El flujo de trabajo cuenta con un mecanismo de respaldo inteligente. Si no se encuentra una etiqueta válida anterior, automáticamente usa los últimos 20 registros de commit como base para la generación, asegurando que siempre se pueda generar un Changelog.

### Q2: ¿Puedo usar otros servicios LLM?
A: Sí. Este proyecto es compatible con cualquier servicio LLM que proporcione una API en formato OpenAI. Simplemente configura los correspondientes `LLM_API_ENDPOINT` y `LLM_API_MODEL` en los Secrets.

### Q3: ¿En qué rama se commitea el archivo de Changelog generado?
A: Por defecto, se commitea en la rama desde la que se activó el flujo de trabajo (normalmente la rama `main`). La configuración del flujo de trabajo especifica `ref: main` para asegurar que la operación se realice en la rama principal.

### Q4: ¿Cómo puedo modificar la forma de categorización del Changelog?
A: Edita la plantilla de prompt en el archivo `.github/workflows/scripts/template.txt` y ajusta los requisitos de categorización. Por ejemplo, puedes agregar categorías como "Actualizaciones de seguridad" o "Mejoras en la documentación".

### Q5: ¿Qué hacer si falla la llamada a la API?
A: GitHub Actions mostrará automáticamente registros de error. Las causas comunes incluyen: clave de API inválida, acceso de red al punto de extremo de la API, formato de respuesta de API incompatible, etc. Verifica la configuración de Secrets y la conectividad de red.

### Q6: ¿Puedo generar Changelogs para múltiples versiones al mismo tiempo?
A: Sí. Ingresa diferentes parámetros de número de versión cada vez que actives manualmente el flujo de trabajo para generar archivos de Changelog independientes para diferentes versiones.

### Q7: ¿Por qué se necesita el historial completo de Git?
A: El historial completo de Git (`fetch-depth: 0`) es necesario para calcular con precisión las diferencias de commits entre etiquetas. Esta es la base para crear registros de cambios de versión precisos.

## 🔄 Extensiones personalizadas

### Soporte para otros servicios LLM

Para cambiar a otro servicio LLM (como OpenAI, Claude, etc.), simplemente modifica las siguientes configuraciones:

1. Actualiza `LLM_API_ENDPOINT` a la dirección de API del servicio correspondiente
2. Actualiza `LLM_API_MODEL` al nombre del modelo correspondiente
3. Asegúrate de que el formato de respuesta de la API sea compatible con DeepSeek (devuelve `choices[0].message.content`)

### Modificación del formato de salida

Al editar el archivo `template.txt`, puedes personalizar completamente el formato de salida, por ejemplo:
- Ajustar la categorización
- Modificar los emojis
- Agregar secciones personalizadas
- Cambiar el estilo del documento

## 📄 Licencia

Este proyecto está bajo la licencia MIT. Consulta el archivo [LICENSE](LICENSE) para más detalles.

## 🤝 Guía de contribución

¡Las contribuciones en forma de Issues y Pull Requests para mejorar este proyecto son bienvenidas!

1. Haz un fork de este repositorio
2. Crea una rama de características (`git checkout -b feature/AmazingFeature`)
3. Commitea tus cambios (`git commit -m 'Add Changelog generation feature'`)
4. Haz push a la rama (`git push origin feature/AmazingFeature`)
5. Abre un Pull Request

## 🙏 Agradecimientos

- Gracias a [DeepSeek](https://www.deepseek.com/) por proporcionar un excelente servicio LLM
- Gracias a GitHub Actions por su poderosa capacidad de automatización
- Gracias a todos los colaboradores de la comunidad de código abierto

---

**¡Si este proyecto te resulta útil, por favor dale una ⭐ Star como apoyo!**