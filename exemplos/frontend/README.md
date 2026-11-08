# Ejemplo de frontend — Conecta Company

Este directorio contiene el sitio que crearás en el **módulo 1**. Agregarás el widget en el **módulo 2**.

## Archivos

| Archivo | Descripción |
|---|---|
| `index.html` | Estructura HTML con espacio para el widget |
| `styles.css` | Estilos visuales del sitio |

## Cómo usarlo

1. Copia ambos archivos a una carpeta de tu equipo.
2. Publícalos en el bucket S3 siguiendo el módulo 1.
3. En el módulo 2, abre `index.html` en tu editor.
4. Pega el snippet del widget antes de `</body>`.
5. Guarda y vuelve a subir el `index.html` actualizado al bucket.

## Dónde pegar el snippet

Localiza este comentario en `index.html`:

```html
<!-- AGREGA AQUÍ EL SNIPPET DEL WIDGET DE CHAT EN EL MÓDULO 2 -->
```

Reemplázalo por el snippet completo generado por Amazon Connect. No agregues credenciales AWS ni la clave secreta de JWT al código público.

## Personalización

- **Color principal:** cambia `#1a73e8` en `styles.css`.
- **Fuente:** cambia `'Segoe UI', Arial, sans-serif` por otra fuente.
- **Textos:** edita `index.html`; conserva `lang="es-419"`.

## Referencias

- [Widget de Amazon Connect](https://docs.aws.amazon.com/connect/latest/adminguide/add-chat-to-website.html)
- [Alojamiento de sitios estáticos S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html)
