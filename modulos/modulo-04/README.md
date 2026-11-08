# Módulo 4 — Pruebas, métricas y entrega

[![Duración](https://img.shields.io/badge/Duración-~60min-blue)](#-objetivos)
[![Requisito previo](https://img.shields.io/badge/Requisito%20previo-Módulo%203-orange)](../modulo-03/README.md)

> En este módulo probarás la solución completa, analizarás las métricas generadas y prepararás la entrega.

## 🎯 Objetivos

Al finalizar tendrás:

- ✅ Al menos dos conversaciones de prueba por rutas diferentes
- ✅ Métricas históricas consultadas y analizadas
- ✅ Contact Search utilizado para localizar tus contactos
- ✅ Evidencias recopiladas para entregar

## 📚 Lectura recomendada

| Tema | Enlace |
|---|---|
| Contact Control Panel | [Acceso al CCP](https://docs.aws.amazon.com/connect/latest/adminguide/amazon-connect-contact-control-panel.html) |
| Inicio y cierre de sesión | [Sesiones del CCP](https://docs.aws.amazon.com/connect/latest/adminguide/ccp-login.html) |
| Estado del agente | [Estados](https://docs.aws.amazon.com/connect/latest/adminguide/metrics-agent-status.html) |
| Métricas históricas | [Informes históricos](https://docs.aws.amazon.com/connect/latest/adminguide/historical-metrics.html) |
| Métricas en tiempo real | [Informes en tiempo real](https://docs.aws.amazon.com/connect/latest/adminguide/real-time-metrics-reports.html) |
| Contact Search | [Buscar contactos](https://docs.aws.amazon.com/connect/latest/adminguide/contact-search.html) |
| Visión general de métricas | [Métricas, paneles e informes](https://docs.aws.amazon.com/connect/latest/adminguide/amazon-connect-metrics.html) |
| Modelo de datos de contactos | [Campos de los contactos](https://docs.aws.amazon.com/connect/latest/adminguide/ctr-data-model.html) |

> Consejo de estudio: el modelo de datos explica los campos que verás al inspeccionar los contactos. Consúltalo para entender su significado.

## 📖 Conceptos importantes

### Contact Control Panel (CCP)

El **CCP** es la interfaz del agente. Allí `agente01` recibe los chats iniciados en el sitio. Debe estar en estado **Available** para recibir contactos. [Referencia](https://docs.aws.amazon.com/connect/latest/adminguide/amazon-connect-contact-control-panel.html).

### Estado del agente

| Estado | Significado |
|---|---|
| **Available** | Disponible para recibir contactos |
| **Offline** | Desconectado; no recibe nuevos contactos |
| **After Contact Work (ACW)** | Trabajo posterior a la atención |

[Referencia de estados](https://docs.aws.amazon.com/connect/latest/adminguide/metrics-agent-status.html).

### Historical Metrics

Son informes con datos agregados: contactos atendidos, tiempo promedio de atención y otros indicadores. La actualización no es inmediata; espera unos minutos después de finalizar las conversaciones. [Referencia](https://docs.aws.amazon.com/connect/latest/adminguide/historical-metrics.html).

### Contact Search

Permite buscar e inspeccionar contactos individuales: fecha, canal, agente y cola. La transcripción depende de que se haya habilitado su almacenamiento y de que tengas los permisos correspondientes. [Referencia](https://docs.aws.amazon.com/connect/latest/adminguide/contact-search.html).

## 🔨 Paso 1: preparar el entorno de prueba

> Si los nombres de los menús cambian, consulta la [documentación del CCP](https://docs.aws.amazon.com/connect/latest/adminguide/amazon-connect-contact-control-panel.html).

Necesitarás **dos ventanas o sesiones** abiertas al mismo tiempo.

**Ventana A: agente (CCP)**

1. Abre una ventana privada o un perfil de navegador separado.
2. Abre la URL de acceso real de la instancia del módulo 2.
3. Inicia sesión como `agente01`.
4. Abre el CCP o el espacio de trabajo del agente si no aparece automáticamente.
5. Selecciona **Available**.

Consulta [inicio y cierre de sesión](https://docs.aws.amazon.com/connect/latest/adminguide/ccp-login.html) si el navegador bloquea cookies o ventanas necesarias.

**Ventana B: cliente (sitio)**

1. Abre otra ventana normal.
2. Abre la URL del sitio S3.
3. Confirma que el widget es visible.

## 🔨 Paso 2: realizar las pruebas

Realiza **al menos dos conversaciones por rutas diferentes** del menú. Usa lenguaje natural en español. Estos casos corresponden al requisito de seis opciones del módulo 3; no se exige un menú numérico de dos opciones.

### Prueba 1: equipo comercial (opción 5 del negocio)

**En la ventana B (sitio):**

1. Haz clic en el botón de chat.
2. Comprueba que se abre la conversación.
3. Lee la bienvenida de tu Contact Flow.
4. Escribe `Quiero hablar con el equipo comercial`.
5. Comprueba el reconocimiento de la intent, el mensaje de respuesta y el enrutamiento esperado.

**En la ventana A (CCP):**

1. Comprueba que llega un contacto.
2. Acéptalo.
3. Envía: `¡Hola! Soy del equipo comercial. ¿Cómo puedo ayudarte?`
4. Finaliza con **End chat** y completa **After Contact Work**, si aparece, para quedar disponible nuevamente.

📸 Captura la conversación en el sitio y en el CCP, sin credenciales ni datos personales reales.

### Prueba 2: soporte técnico (opción 3 del negocio)

**En la ventana B (sitio):**

1. Inicia un **chat nuevo**; cierra el anterior y vuelve a abrir el widget.
2. Escribe `Necesito ayuda con un problema técnico`.
3. Comprueba la intent y el mensaje de respuesta diferente.

**En la ventana A (CCP):**

1. Acepta el contacto.
2. Envía: `¡Hola! Soy del equipo de soporte. ¿Qué problema tienes?`
3. Finaliza la conversación y completa el trabajo posterior.

📸 Captura esta segunda conversación.

Además de estas dos evidencias, conserva los resultados de las pruebas de todos los caminos exigidos en el módulo 3: ambos submenús, las otras rutas directas, el minuto de inactividad y las dos repeticiones por falta de reconocimiento.

## 🔨 Paso 3: analizar las métricas históricas

Espera inicialmente **5–10 minutos** después de las pruebas; la disponibilidad de datos puede tardar más. [Referencia](https://docs.aws.amazon.com/connect/latest/adminguide/historical-metrics.html).

### 3.1 Acceder a Historical Metrics

1. Entra al panel de administración de Connect como `admin`.
2. Ve a **Analytics and optimization**.
3. Abre **Historical metrics**.

### 3.2 Configurar el informe

1. Selecciona **Queue metrics** o el informe de contactos disponible.
2. En **Time range**, selecciona **Today** y comprueba la zona horaria.
3. Selecciona la cola utilizada y el canal Chat cuando corresponda.
4. Haz clic en **Generate report** o **Apply**.

### 3.3 Qué observar

| Métrica | Significado | Documentación |
|---|---|---|
| **Contacts handled** | Total de contactos atendidos | [Definición](https://docs.aws.amazon.com/connect/latest/adminguide/historical-metrics-definitions.html#contacts-handled-historical) |
| **Average handle time** | Tiempo promedio de atención | [Definición](https://docs.aws.amazon.com/connect/latest/adminguide/historical-metrics-definitions.html#average-handle-time-historical) |
| **Contacts queued** | Contactos que ingresaron a la cola | [Definición](https://docs.aws.amazon.com/connect/latest/adminguide/historical-metrics-definitions.html#contacts-queued-historical) |

📖 [Definiciones completas](https://docs.aws.amazon.com/connect/latest/adminguide/historical-metrics-definitions.html).

📸 Captura el informe con los datos visibles.

## 🔨 Paso 4: utilizar Contact Search

📖 [Buscar contactos](https://docs.aws.amazon.com/connect/latest/adminguide/contact-search.html).

### 4.1 Acceder

Ve a **Analytics and optimization → Contact search**.

### 4.2 Buscar tus contactos

1. En **Date range**, selecciona el día de las pruebas y verifica la zona horaria.
2. En **Channel**, selecciona **Chat**.
3. Haz clic en **Search**.

Deberías localizar al menos los dos contactos de prueba.

### 4.3 Inspeccionar un contacto

Abre un contacto y revisa su ID, hora, duración, cola, agente y transcripción, si está disponible. Anota los IDs para relacionarlos con las evidencias. Si no hay transcripción, verifica su almacenamiento y permisos; las capturas del chat siguen siendo necesarias.

📸 Captura la lista y los detalles de un contacto.

## 🔨 Paso 5: organizar las evidencias

Usa la [plantilla de entrega](../../templates/entrega.md).

| Evidencia | Descripción | Lista |
|---|---|---|
| URL del sitio S3 | URL pública del sitio | [ ] |
| Captura del sitio | Sitio cargado con widget visible | [ ] |
| Captura de prueba 1 | Conversación con el equipo comercial | [ ] |
| Captura de prueba 2 | Conversación con soporte técnico | [ ] |
| Captura de métricas | Informe de Historical Metrics | [ ] |
| Captura de Contact Search | Lista y detalles de contactos | [ ] |
| Video | Demostración completa, opcional y recomendada | [ ] |

Incluye también las evidencias del bot y sus intents que solicita la plantilla. Revisa las capturas antes de compartirlas: oculta claves, contraseñas, correos personales y datos ajenos al ejercicio.

## 🎬 Paso 6: grabar un video de demostración (recomendado)

Graba un video de **3–5 minutos** que muestre:

1. El sitio S3 cargándose en el navegador.
2. El widget visible.
3. Una conversación en lenguaje natural y navegación por el menú.
4. La recepción del chat por el agente en el CCP.
5. La finalización de la conversación.
6. Las métricas históricas con datos.

Puedes utilizar Loom, OBS o una herramienta de grabación de pantalla disponible en tu equipo.

## ✅ Lista final de verificación

### Módulo 1: frontend + S3

- [ ] Sitio accesible mediante URL pública
- [ ] URL del sitio anotada

### Módulo 2: instancia Amazon Connect

- [ ] Instancia creada sin SSO: **Store users within Amazon Connect**
- [ ] Usuario agente creado y funcional
- [ ] Communication Widget creado y asociado al sitio
- [ ] Snippet agregado y sitio publicado nuevamente en S3
- [ ] Widget visible

### Módulo 3: Contact Flow con Amazon Lex V2

- [ ] Bot Lex V2 con intents para las seis opciones y locale `es_419`
- [ ] Flujo publicado y asociado al widget
- [ ] Pruebas de todas las rutas completadas

### Módulo 4: pruebas y métricas

- [ ] Al menos dos conversaciones por rutas diferentes
- [ ] Métricas históricas consultadas
- [ ] Contact Search utilizado
- [ ] Todas las evidencias recopiladas

## 📚 Documentación de referencia — módulo 4

Además de las referencias de lectura y de métricas anteriores, consulta el [diagnóstico del CCP](https://docs.aws.amazon.com/connect/latest/adminguide/troubleshooting.html) para problemas de recepción de contactos.

## 🚨 Problemas comunes

**El chat no llega al CCP**
→ Comprueba **Available**, la asociación del flujo al widget y que la cola esté en el perfil del agente con Chat habilitado. [Diagnóstico](https://docs.aws.amazon.com/connect/latest/adminguide/troubleshooting.html).

**No aparecen métricas**
→ Espera más tiempo y revisa fecha, zona horaria, cola y cierre de las conversaciones. No hay una garantía de actualización inmediata. [Informes históricos](https://docs.aws.amazon.com/connect/latest/adminguide/historical-metrics.html).

**Contact Search no muestra contactos**
→ Comprueba el intervalo de fechas y el canal Chat. [Búsqueda](https://docs.aws.amazon.com/connect/latest/adminguide/contact-search.html).

¿Otros problemas? Consulta la [guía de solución de problemas](../../troubleshooting.md).

## 🧹 Limpieza después de la evaluación

Coordina con el instructor cuándo puede dejar de estar disponible la URL. Primero guarda las evidencias y cualquier exportación que quieras conservar. Después elimina **solo los recursos dedicados a este laboratorio**:

1. Desactiva o elimina el widget para evitar nuevas conversaciones.
2. Elimina la instancia de laboratorio siguiendo la [guía de eliminación](https://docs.aws.amazon.com/connect/latest/adminguide/delete-connect-instance.html).
3. Elimina el bot Lex V2 y sus alias/versiones si ya no se utilizan.
4. Vacía y elimina el bucket del sitio, y revisa por separado los buckets privados creados para datos de Connect. Si hay versionado, revisa versiones y marcadores de eliminación.
5. Revisa los grupos de logs de CloudWatch y otros recursos creados; eliminar la instancia no garantiza la eliminación de todos los recursos relacionados.
6. Comprueba Billing/Cost Explorer posteriormente. La información de facturación también puede tener demora.

No elimines recursos compartidos de una cuenta de formación. Para S3, consulta [eliminación de buckets](https://docs.aws.amazon.com/AmazonS3/latest/userguide/delete-bucket.html).

## 🎉 ¡Felicitaciones!

Si todo funciona, construiste desde cero una solución de atención por chat con Amazon Connect: una instancia de centro de contacto, un flujo de atención con menú conversacional, un sitio con chat integrado y análisis de métricas reales. Estos son fundamentos de una implementación de Amazon Connect.

**Completa la [plantilla de entrega →](../../templates/entrega.md)**
