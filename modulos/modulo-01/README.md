# Módulo 1: Frontend + sitio S3

[![Duración](https://img.shields.io/badge/Duración-~60min-blue)](#-objetivos)
[![Dificultad](https://img.shields.io/badge/Dificultad-Principiante-green)](#-objetivos)

> En este módulo crearás el sitio de Conecta Company y lo publicarás en Amazon S3. Comenzamos por el sitio porque necesitarás su URL para configurar el widget de Amazon Connect en los siguientes módulos.

## 🎯 Objetivos

Al finalizar este módulo tendrás:

- ✅ Sitio HTML de Conecta Company creado
- ✅ Bucket S3 configurado como sitio estático
- ✅ Sitio accesible mediante una URL pública

> El widget de chat se integrará en el **módulo 2**, después de crear la instancia Amazon Connect. Necesita una URL válida, por eso publicamos primero el sitio.

## 📚 Lectura recomendada

| Tema | Enlace |
|---|---|
| Alojamiento estático en S3 | [Alojar un sitio estático](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html) |
| Tutorial paso a paso | [Configurar un sitio estático](https://docs.aws.amazon.com/AmazonS3/latest/userguide/HostingWebsiteOnS3Setup.html) |
| Políticas de bucket | [Ejemplos de políticas](https://docs.aws.amazon.com/AmazonS3/latest/userguide/example-bucket-policies.html) |
| Block Public Access | [Bloquear el acceso público](https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-control-block-public-access.html) |

## 📖 Conceptos importantes

### S3 Static Website Hosting

Amazon S3 permite alojar archivos HTML/CSS/JS con una URL pública, sin administrar un servidor. Es una forma sencilla de publicar el sitio estático del laboratorio.

El endpoint de sitio web de S3 usa **HTTP, no HTTPS**. Conservamos esta arquitectura didáctica del original. Para producción se requiere una solución de entrega HTTPS, fuera del alcance del ejercicio; consulta [endpoints de sitios web S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteEndpoints.html).

### Block Public Access

S3 bloquea el acceso público de forma predeterminada. Para este sitio de laboratorio debes permitir lectura pública mediante una política del bucket. Usa un bucket dedicado que contenga únicamente los archivos públicos del ejemplo.

> Nunca coloques credenciales, claves, datos personales ni transcripciones en este bucket. El bloqueo también puede aplicarse a nivel de cuenta u organización: si impide el ejercicio, consulta al administrador o instructor; no retires protecciones generales de una cuenta compartida. [Referencia](https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-control-block-public-access.html).

## 🔨 Paso 1: crear los archivos del sitio

Crea dos archivos con tu editor de texto. También están disponibles en [`/exemplos/frontend/`](../../exemplos/frontend/).

### 1.1 index.html

Crea un archivo llamado `index.html`:

```html
<!DOCTYPE html>
<html lang="es-419">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Conecta Company: Atención al cliente</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <!-- Encabezado de la empresa -->
    <header>
        <h1>🏢 Conecta Company</h1>
        <p>Soluciones que marcan la diferencia</p>
    </header>

    <!-- Contenido principal -->
    <main>
        <section class="hero">
            <h2>¿Cómo podemos ayudarte?</h2>
            <p>
                Nuestro equipo está disponible para atenderte por chat.<br>
                Haz clic en el botón de la esquina inferior derecha para iniciar una conversación.
            </p>
        </section>

        <section class="cards">
            <div class="card">
                <h3>🎓 Inscripciones</h3>
                <p>Información sobre cursos e inscripciones</p>
            </div>
            <div class="card">
                <h3>🔧 Soporte técnico</h3>
                <p>Resuelve problemas con nuestra ayuda</p>
            </div>
            <div class="card">
                <h3>💬 Chat en vivo</h3>
                <p>Habla con un agente ahora</p>
            </div>
        </section>
    </main>

    <!-- Pie de página -->
    <footer>
        <p>&copy; 2024 Conecta Company. Todos los derechos reservados.</p>
    </footer>

    <!-- AGREGA AQUÍ EL SNIPPET DEL WIDGET DE CHAT EN EL MÓDULO 2 -->
</body>
</html>
```

### 1.2 styles.css

Crea un archivo llamado `styles.css`:

```css
* { margin: 0; padding: 0; box-sizing: border-box; }

body {
    font-family: 'Segoe UI', Arial, sans-serif;
    background-color: #f5f7fa;
    color: #333;
}

header {
    background-color: #1a73e8;
    color: white;
    padding: 30px 40px;
    text-align: center;
}

header h1 { font-size: 2rem; margin-bottom: 8px; }
header p { font-size: 1rem; opacity: 0.9; }

main {
    max-width: 900px;
    margin: 40px auto;
    padding: 0 20px;
}

.hero { text-align: center; margin-bottom: 40px; }
.hero h2 { font-size: 1.6rem; margin-bottom: 12px; color: #1a73e8; }
.hero p { font-size: 1rem; line-height: 1.6; color: #555; }

.cards { display: flex; gap: 20px; justify-content: center; flex-wrap: wrap; }

.card {
    background: white;
    border-radius: 8px;
    padding: 24px;
    width: 240px;
    text-align: center;
    box-shadow: 0 2px 8px rgba(0,0,0,0.08);
}

.card h3 { font-size: 1.1rem; margin-bottom: 8px; color: #1a73e8; }
.card p { font-size: 0.9rem; color: #666; }

footer {
    text-align: center;
    padding: 20px;
    margin-top: 60px;
    color: #999;
    font-size: 0.85rem;
    border-top: 1px solid #e0e0e0;
}
```

## 🔨 Paso 2: crear el bucket S3

📖 [Tutorial oficial](https://docs.aws.amazon.com/AmazonS3/latest/userguide/HostingWebsiteOnS3Setup.html).

> AWS actualiza la consola con frecuencia. Si cambia el nombre o la ubicación de una opción, usa el tutorial oficial como referencia.

### 2.1 Acceder a S3

1. Busca **S3** en AWS Console.
2. Haz clic en **Create bucket**.

### 2.2 Configurar el bucket

| Campo | Valor |
|---|---|
| Bucket name | `conecta-company-tu-nombre` (único globalmente y en minúsculas) |
| AWS Region | `us-east-1` (recomendada para este laboratorio) |

Si el nombre ya existe, agrega un sufijo único. Consulta las [reglas de nombres](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucketnamingrules.html).

En **Block Public Access settings**, para este bucket dedicado:

- Desmarca **Block all public access**.
- Confirma que comprendes el acceso público marcando la casilla correspondiente.

Haz clic en **Create bucket**.

### 2.3 Habilitar Static Website Hosting

1. Abre el bucket creado.
2. Ve a **Properties → Static website hosting**.
3. Haz clic en **Edit → Enable**.
4. En **Index document**, escribe `index.html`.
5. Haz clic en **Save changes**.

Anota el **Bucket website endpoint**. Lo necesitarás en el siguiente módulo.

```text
URL del sitio S3: ________________________________
```

### 2.4 Configurar la política del bucket

1. Ve a **Permissions → Bucket policy**.
2. Pega el siguiente JSON y reemplaza `TU-BUCKET` por el nombre real:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::TU-BUCKET/*"
    }
  ]
}
```

3. Haz clic en **Save changes**.

## 🔨 Paso 3: subir los archivos

1. En el bucket, abre **Objects → Upload**.
2. Selecciona `index.html` y `styles.css`.
3. Haz clic en **Upload**. Ambos archivos deben quedar en la raíz del bucket.

## 🔨 Paso 4: verificar el sitio

1. Abre el endpoint de sitio web que anotaste, con el protocolo `http://` que proporciona S3.
2. Comprueba que aparece el diseño de Conecta Company.
3. El comentario del widget todavía no muestra un botón: lo agregarás en el módulo 2.

## ✅ Validación del módulo 1

- [ ] Archivo `index.html` creado
- [ ] Archivo `styles.css` creado
- [ ] Bucket S3 creado con nombre único
- [ ] Static website hosting habilitado
- [ ] Block Public Access ajustado en el bucket del laboratorio
- [ ] Política del bucket configurada
- [ ] Archivos subidos
- [ ] Sitio accesible mediante URL pública
- [ ] URL del sitio anotada

## 📝 Anota estos datos

```text
Bucket Name:       ________________________________
URL del sitio S3:  ________________________________
```

## 📚 Documentación de referencia: módulo 1

| Documento | Enlace |
|---|---|
| Tutorial de sitio estático | [Tutorial S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/HostingWebsiteOnS3Setup.html) |
| Alojamiento estático | [Website hosting](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html) |
| Políticas de bucket | [Ejemplos](https://docs.aws.amazon.com/AmazonS3/latest/userguide/example-bucket-policies.html) |
| Block Public Access | [Configuración](https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-control-block-public-access.html) |
| Nombres de buckets | [Reglas](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucketnamingrules.html) |

## 🚨 Problemas comunes

**El sitio devuelve 403 (Forbidden)**
→ Verifica que el nombre del bucket en la política sea exacto y revisa los bloqueos de acceso público del bucket, la cuenta y la organización. [Diagnóstico de errores 403](https://docs.aws.amazon.com/AmazonS3/latest/userguide/troubleshoot-403-errors.html).

**El sitio devuelve 404**
→ Confirma que el archivo se llama exactamente `index.html`, está en la raíz y coincide con **Index document**.

¿Otros problemas? Consulta la [guía de solución de problemas](../../troubleshooting.md).

**¿Terminaste el módulo 1? Continúa con el [módulo 2 →](../modulo-02/README.md)**
