# Edición LATAM: origen, adaptación y verificación

## Origen y licencia

Esta edición deriva de [GabrielFMarinho/aws-connect-challenge](https://github.com/GabrielFMarinho/aws-connect-challenge), laboratorio brasileño de Amazon Connect. El repositorio de origen es privado; su enlace puede no estar disponible para todos los lectores.

- Revisión de referencia: `13baaf1e20627eedaf9754fae9e9ef0bb7da41c7`.
- Fecha de revisión de esta edición: **8 de noviembre de 2026**.
- Copyright original: **2024 Amazon Connect Lab Contributors**.
- La [licencia MIT](./LICENSE) se conserva sin modificaciones, incluido el aviso de copyright.
- Se publica una copia adaptada con historial nuevo; no se publica el historial Git del repositorio privado.

Los nombres de servicios y marcas de AWS se usan para identificar las tecnologías del laboratorio; este material no implica patrocinio ni certificación de AWS.

## Alcance conservado

Se mantienen Conecta Company, los cuatro módulos y su secuencia, la arquitectura S3 + Amazon Connect + Amazon Lex V2 + CCP, las seis opciones del negocio, los submenús, los objetivos y la duración aproximada de **cuatro horas**. El módulo 3 sigue siendo un desafío de investigación y diseño, sin una solución importable. Los nombres de las carpetas originales se conservan.

## Adaptaciones realizadas

1. Traducción al español latinoamericano del README, los cuatro módulos, la guía de contribución, los ejemplos de frontend, las plantillas y la guía de solución de problemas.
2. Mensajes, utterances y pruebas en español; **`es_419`** como locale de Lex V2 y **`es-419`** como idioma HTML.
3. Aclaraciones sobre compilación, versión, alias, idioma habilitado y asociación del bot con Connect.
4. Traducción del banner y los diagramas. Corrección de la secuencia antigua del diagrama de módulos y representación de Lex V2 en la arquitectura.
5. Sustitución de la imagen de negocio en portugués por un SVG editable en español, manteniendo sus seis opciones, dos submenús y destinos de atención. El archivo conserva el nombre base histórico `fluxo-negocio-modulo2`, aunque se utiliza en el módulo 3.
6. Resolución de la discrepancia de timeout: **un minuto**, como pedía el texto original y permite Chat timeout de Connect con Lex; la imagen antigua decía 8 segundos. Se mantienen hasta **dos repeticiones** por falta de reconocimiento.
7. Pruebas finales alineadas al menú del módulo 3: equipo comercial es la opción 5 y soporte técnico la 3; se prueban mediante lenguaje natural.
8. Alineación del frontend de ejemplo con las tarjetas del módulo 1 y corrección de las referencias de módulo en sus instrucciones.
9. Aclaraciones de URL/protocolo del widget, autenticación JWT fuera del alcance del sitio estático y canal Chat/cola del perfil de enrutamiento. No se agregan servidores, Lambda ni CloudFront al ejercicio.
10. Revisión de costos según Connect Customer/Customer Basic, solicitudes Lex incluidas o facturadas aparte, S3, logs y condiciones del Free Tier; se elimina la promesa de costo cero universal.
11. Crédito al origen, conservación de licencia, exclusión de contraseñas en hojas de anotaciones y orientación para evidencias y limpieza de recursos.

La rúbrica conserva **55 elementos y mínimo de 50**. El video se presenta como recomendado y opcional, coherente con el módulo 4 y la plantilla originales.

## Verificación antes de publicar

- Revisión de todos los archivos incluidos en la copia pública y búsqueda de patrones de claves AWS, tokens GitHub, claves privadas, JWT, identificadores de cuenta en ARN y direcciones de correo. No se encontraron coincidencias de esos patrones en el contenido preparado.
- Inspección del material original, incluidas las imágenes, antes de adaptar. No se copiaron archivos de configuración local, credenciales ni historial Git a la edición pública.
- Comparación de `LICENSE` con el original: contenido idéntico. No se incorporaron bibliotecas de terceros ni paquetes ejecutables al laboratorio; las figuras son SVG locales y el frontend es HTML/CSS.
- Validación de enlaces relativos, consistencia del HTML del módulo 1 con el ejemplo, cuatro módulos, rúbrica de 55 elementos y sintaxis XML de los cinco SVG.
- Comprobación de los enlaces HTTP de AWS utilizados; se corrigió el enlace de eliminación de instancias que redirigía al índice general.
- Revisión técnica de las páginas oficiales de idioma Lex V2, integración, timeout de chat, widget, endpoint HTTP de S3, costos y Free Tier.

La revisión de patrones es una comprobación preventiva y no una garantía de detección de cualquier secreto posible. El historial privado no forma parte del material publicado.

## Límites de la validación

**No se aprovisionaron recursos ni se ejecutó una prueba de extremo a extremo en una cuenta AWS.** Por tanto, la creación de la instancia, el reconocimiento real del bot, el enrutamiento, las métricas y la facturación deben confirmarse en la cuenta y región del curso. Las instrucciones y ejemplos se revisaron estáticamente contra la documentación disponible.

Los nombres de pantallas, funciones disponibles, precios y beneficios de cuenta pueden cambiar. Los ejemplos de costo son ilustrativos; no constituyen una promesa de gasto máximo. Se conserva el sitio S3 por HTTP como arquitectura didáctica del original; para producción se debe revisar HTTPS y autenticación del widget.

## Fuentes técnicas principales

- [Idiomas y locales de Amazon Lex V2](https://docs.aws.amazon.com/lexv2/latest/dg/how-languages.html)
- [Integración de Lex con Connect](https://docs.aws.amazon.com/connect/latest/adminguide/amazon-lex.html)
- [Get customer input: configuración y timeout](https://docs.aws.amazon.com/connect/latest/adminguide/get-customer-input.html)
- [Widget de chat y dominios permitidos](https://docs.aws.amazon.com/connect/latest/adminguide/add-chat-to-website.html)
- [Endpoints de sitios web S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteEndpoints.html)
- [Precios de Connect Customer](https://aws.amazon.com/products/connect/customer/pricing/)
- [Detalle de precios, Basic y solicitudes Lex incluidas](https://aws.amazon.com/products/connect/customer/pricing/appendix/)
- [Precios de Amazon Lex](https://aws.amazon.com/lex/pricing/)
- [AWS Free Tier](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/free-tier.html)
