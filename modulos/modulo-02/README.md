# Módulo 2 — Instancia Amazon Connect + Communication Widget

[![Duración](https://img.shields.io/badge/Duración-~60min-blue)](#-objetivos)
[![Requisito previo](https://img.shields.io/badge/Requisito%20previo-Módulo%201-orange)](../modulo-01/README.md)

> En este módulo crearás la instancia Amazon Connect, configurarás el usuario agente y crearás el Communication Widget. Como ya publicaste el sitio en S3, puedes configurar el widget con la URL real desde el inicio.

## 🎯 Objetivos

Al finalizar este módulo tendrás:

- ✅ Instancia Amazon Connect creada
- ✅ Usuario agente configurado
- ✅ Communication Widget creado y asociado al sitio
- ✅ Snippet del widget agregado al sitio y publicado nuevamente en S3
- ✅ Acceso funcional al Contact Control Panel (CCP)

## 📚 Lectura recomendada

| Tema | Enlace |
|---|---|
| Crear una instancia | [Crear una instancia Amazon Connect](https://docs.aws.amazon.com/connect/latest/adminguide/amazon-connect-instances.html) |
| Administración de identidades | [Identity management](https://docs.aws.amazon.com/connect/latest/adminguide/connect-identity-management.html) |
| Modelo Connect Customer | [Connect Customer](https://docs.aws.amazon.com/connect/latest/adminguide/enable-nextgeneration-amazonconnect.html) |
| Communication Widgets | [Agregar chat al sitio web](https://docs.aws.amazon.com/connect/latest/adminguide/add-chat-to-website.html) |

## 📖 Conceptos importantes

### Amazon Connect y el modelo Connect Customer

La oferta Connect Customer incluye capacidades de IA y cobra por canal. Customer Basic ofrece otra combinación de funciones y precios. Verifica el modelo de la instancia que vas a crear y consulta las [diferencias y el procedimiento de cambio](https://docs.aws.amazon.com/connect/latest/adminguide/enable-nextgeneration-amazonconnect.html).

El laboratorio utiliza chat, flujos, colas y Lex V2. No requiere agregar funciones generativas ni cambiar de arquitectura. **No supongas que el laboratorio queda cubierto por el Free Tier**: revisa los [costos y condiciones](../../README.md#-costo-estimado) antes de continuar.

### Identity Management

Define cómo se administran los usuarios:

| Opción | Cuándo usarla |
|---|---|
| **Store users within Amazon Connect** | Usuarios administrados directamente en Connect; opción sencilla para laboratorios y pruebas de concepto. |
| **Link to an existing directory** | Integración con AWS Directory Service (Active Directory). |
| **SAML 2.0-based authentication** | Federación con un proveedor de identidad externo (Okta, Microsoft Entra ID, etc.). |

> La opción de administración de identidades **no puede cambiarse** después de crear la instancia.

## 🔨 Paso 1: crear la instancia

### 1.1 Acceder al servicio

1. Abre [AWS Console](https://console.aws.amazon.com).
2. Selecciona `us-east-1`, la región recomendada para este laboratorio.
3. Busca **Connect** y abre **Amazon Connect**.
4. Haz clic en **Create instance** o **Add an instance**.

### 1.2 Configurar Identity Management

Selecciona **Store users within Amazon Connect**.

En **Access URL**, define un alias único, por ejemplo `conecta-company-tu-nombre`. Usa letras minúsculas, números y guiones. Anota el alias y copia la **URL real que muestre la consola** (por ejemplo, `https://TU-ALIAS.my.connect.aws`).

Haz clic en **Next**.

### 1.3 Crear el administrador

| Campo | Valor sugerido |
|---|---|
| First name | Admin |
| Last name | Connect |
| Username | `admin` |
| Email | Una dirección válida a la que tengas acceso |
| Password | Una contraseña segura y exclusiva |

El correo permite la recuperación de acceso en esta configuración sin SSO. Guarda la contraseña en un administrador de contraseñas, nunca en el repositorio o en la entrega. Haz clic en **Next**.

### 1.4 Configurar Telephony

Este ejercicio usa **solo chat**. Si el asistente permite desactivar llamadas entrantes y salientes, déjalas desactivadas. Si aparecen habilitadas por defecto, no reclames números telefónicos ni realices llamadas: no son necesarios para completar el laboratorio.

Haz clic en **Next**.

### 1.5 Data Storage y revisión

- Mantén la configuración de almacenamiento propuesta para el laboratorio y anota los recursos creados.
- El bucket de datos o transcripciones de Connect debe permanecer **privado**, separado del bucket público del sitio.
- Haz clic en **Next**, revisa y selecciona **Create instance**.

Espera a que la instancia esté activa; puede tardar varios minutos. Si cambia la secuencia de pantallas, consulta la [documentación oficial](https://docs.aws.amazon.com/connect/latest/adminguide/amazon-connect-instances.html).

## 🔨 Paso 2: acceder a la instancia

1. Abre el alias de tu instancia en la lista de instancias.
2. Copia la URL de acceso y abre el sitio de administración.
3. Inicia sesión con el usuario `admin` que creaste.

**Log in for emergency access** es una vía alternativa desde AWS Console para un usuario IAM autorizado; no es el inicio de sesión habitual del agente. Usa la URL normal de la instancia con `agente01` en las pruebas.

## 🔨 Paso 3: crear el usuario agente

📖 [Administrar usuarios](https://docs.aws.amazon.com/connect/latest/adminguide/manage-users.html).

1. Ve a **Users → User management**.
2. Haz clic en **Add new users**.

| Campo | Valor |
|---|---|
| First name | Agente |
| Last name | Prueba |
| Login name | `agente01` |
| Email | Una dirección válida a la que tengas acceso |
| Password | Una contraseña segura |
| Security profile | **Agent** |
| Routing profile | **Basic Routing Profile** |

Haz clic en **Save**. Puedes usar un alias de tu correo, si tu proveedor lo admite; no necesitas crear otra cuenta de correo.

Revisa **Users → Routing profiles → Basic Routing Profile**: habilita el canal **Chat** con capacidad para al menos un contacto y confirma que la cola utilizada (por ejemplo, **BasicQueue**) admite chat en ese perfil. Deberá coincidir con la cola elegida en el módulo 3. [Referencia de enrutamiento](https://docs.aws.amazon.com/connect/latest/adminguide/about-routing.html).

## 🔨 Paso 4: crear el Communication Widget

Usa la URL del sitio S3 creado en el módulo 1. [Guía oficial](https://docs.aws.amazon.com/connect/latest/adminguide/add-chat-to-website.html).

### 4.1 Acceder a los widgets

En el sitio de administración de Connect, busca **Channels → Communication widgets**. La ubicación puede variar: consulta [el widget en tu sitio web](https://docs.aws.amazon.com/connect/latest/adminguide/connect-widget-on-website.html).

### 4.2 Crear el widget

1. Selecciona la opción para crear un widget y habilita el canal de chat.
2. Completa y personaliza los campos disponibles:

| Campo | Valor |
|---|---|
| Widget name | `Conecta Support` |
| Website domain | Origen exacto del sitio S3, por ejemplo `http://conecta-company-tu-nombre.s3-website-us-east-1.amazonaws.com` |
| Welcome message | `¡Hola! ¿Cómo podemos ayudarte hoy?` |
| Text Input Placeholder | `Escribe un mensaje` |
| End Chat Button Text | `Finalizar chat` |

El protocolo (`http://` o `https://`) debe coincidir con el sitio. No uses un comodín para todos los sitios. El endpoint de sitio web S3 del laboratorio es HTTP; para producción AWS recomienda HTTPS.

Si debes seleccionar un flujo ahora, usa un flujo de entrada de ejemplo compatible con chat y publicado. En el módulo 3 lo reemplazarás por tu flujo. Ver el widget no demuestra todavía el funcionamiento completo del enrutamiento.

Para este laboratorio estático, configura **No** en la opción de seguridad adicional mediante JWT, si aparece. Activarla requiere un servidor que emita tokens, ausente en esta arquitectura. **Nunca pegues la clave secreta del widget en HTML, JavaScript público o GitHub.** Para producción, consulta la autenticación recomendada en la guía oficial.

3. Guarda el widget.

### 4.3 Copiar el snippet

Después de guardar, copia el **snippet completo** que genera la consola. Es el código que agregarás al sitio; no lo confundas con una clave secreta para JWT.

## 🔨 Paso 5: agregar el widget al sitio y volver a publicarlo

### 5.1 Editar index.html

Abre el archivo del módulo 1 y localiza:

```html
<!-- AGREGA AQUÍ EL SNIPPET DEL WIDGET DE CHAT EN EL MÓDULO 2 -->
```

Reemplaza el comentario por el snippet copiado, antes de `</body>`.

### 5.2 Subir el archivo actualizado a S3

Solo debes volver a subir `index.html`; `styles.css` no cambió.

1. Abre el bucket del módulo 1.
2. Haz clic en **Upload**.
3. Selecciona el `index.html` **actualizado**.
4. Haz clic en **Upload** y reemplaza el archivo anterior.

### 5.3 Verificar el widget

1. Abre la URL del sitio S3.
2. Comprueba que aparece el botón de chat en la esquina inferior derecha.
3. Haz clic y confirma que se abre la ventana de chat.

## ✅ Validación del módulo 2

- [ ] Instancia creada y alias anotado
- [ ] Identity management: **Store users within Amazon Connect**, sin SSO
- [ ] Inicio de sesión del administrador funcional
- [ ] Usuario `agente01` creado con perfil de seguridad **Agent**
- [ ] Communication Widget creado con la URL del sitio S3
- [ ] Snippet copiado y agregado a `index.html`
- [ ] Archivo actualizado subido a S3
- [ ] Widget visible en el sitio

### Prueba rápida del CCP

1. Abre una ventana privada o un perfil de navegador separado.
2. Abre la URL real de la instancia.
3. Inicia sesión como `agente01`.
4. Abre **Contact Control Panel (CCP)** o el espacio de trabajo del agente si no se abre automáticamente. Permite las ventanas emergentes y cookies necesarias según la documentación.

## 📝 Anota estos datos

```text
Instance Alias:     ________________________________
Instance URL:       ________________________________
Admin Username:     admin
Agent Username:     agente01
AWS Region:         us-east-1
```

Guarda las contraseñas únicamente en un administrador de contraseñas.

## 📚 Documentación de referencia — módulo 2

| Documento | Enlace |
|---|---|
| Crear una instancia | [Instancias](https://docs.aws.amazon.com/connect/latest/adminguide/amazon-connect-instances.html) |
| Identidades | [Identity management](https://docs.aws.amazon.com/connect/latest/adminguide/connect-identity-management.html) |
| Connect Customer | [Modelo y cambio a Basic](https://docs.aws.amazon.com/connect/latest/adminguide/enable-nextgeneration-amazonconnect.html) |
| Usuarios | [Administración](https://docs.aws.amazon.com/connect/latest/adminguide/manage-users.html) |
| Perfiles de seguridad | [Security profiles](https://docs.aws.amazon.com/connect/latest/adminguide/default-security-profiles.html) |
| Communication Widget | [Agregar chat](https://docs.aws.amazon.com/connect/latest/adminguide/add-chat-to-website.html) |
| Widget en el sitio | [Guía completa](https://docs.aws.amazon.com/connect/latest/adminguide/connect-widget-on-website.html) |
| CCP | [Acceso al panel](https://docs.aws.amazon.com/connect/latest/adminguide/amazon-connect-contact-control-panel.html) |

## 🚨 Problemas comunes

**“The access URL is already in use”**
→ Prueba otro alias, por ejemplo `conecta-company-ana-2026`.

**No puedo iniciar sesión**
→ Comprueba la URL real de la instancia y utiliza **Forgot password** si corresponde. [Ayuda de acceso](https://docs.aws.amazon.com/connect/latest/adminguide/ccp-login.html).

**La consola exige una URL para guardar el widget**
→ Copia el **Bucket website endpoint** desde **Properties → Static website hosting** del bucket del módulo 1.

**El widget no aparece después de publicar**
→ Revisa el snippet antes de `</body>`, la URL permitida y que hayas subido el archivo actualizado. Recarga sin caché con Ctrl+Shift+R.

¿Otros problemas? Consulta la [guía de solución de problemas](../../troubleshooting.md).

**¿Terminaste el módulo 2? Continúa con el [módulo 3 →](../modulo-03/README.md)**
