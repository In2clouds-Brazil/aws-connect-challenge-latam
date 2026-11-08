# 🚨 Guía de solución de problemas

Consulta esta guía cuando encuentres problemas durante el laboratorio. Para otros casos, revisa el [diagnóstico oficial de Amazon Connect](https://docs.aws.amazon.com/connect/latest/adminguide/troubleshooting.html).

## Módulo 1: Frontend + sitio S3

### El sitio devuelve 403 (Forbidden)

**Problema:** el sitio no carga y muestra “Access Denied”.

**Solución:**

1. Revisa **Block Public Access** en el bucket dedicado al sitio.
2. Revisa la **Bucket policy**: el nombre debe coincidir exactamente con el bucket real.
3. Confirma que la política se guardó correctamente.
4. Si existe un bloqueo a nivel de cuenta u organización, consulta al administrador. No desactives protecciones generales de una cuenta compartida.

Ejemplo de política; reemplaza `TU-BUCKET` por el nombre real:

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

Referencias: [políticas de bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/example-bucket-policies.html), [Block Public Access](https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-control-block-public-access.html).

### El sitio devuelve 404

**Problema:** no se encuentra la página.

1. Confirma que el archivo se llama `index.html`, en minúsculas.
2. Comprueba que se subió correctamente a la raíz del bucket.
3. Verifica que **Index document** sea `index.html`.
4. Usa el **Bucket website endpoint**, no una URL inventada.

[Tutorial S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/HostingWebsiteOnS3Setup.html).

### El widget de chat no aparece

**Problema:** el sitio carga, pero falta el botón.

1. Recuerda que se agrega en el módulo 2; en el módulo 1 todavía no aparece.
2. Abre las herramientas del navegador con F12 y revisa **Console**.
3. Confirma que el snippet esté **antes de `</body>`**.
4. Comprueba que subiste el `index.html` actualizado, no el original.
5. Revisa el dominio permitido del widget y el protocolo exacto: el endpoint del sitio S3 es HTTP.
6. Recarga sin caché con Ctrl+Shift+R.

[Agregar chat al sitio web](https://docs.aws.amazon.com/connect/latest/adminguide/add-chat-to-website.html).

### El widget aparece, pero no inicia una conversación

1. Comprueba que el Contact Flow tenga estado **Published**.
2. Confirma que esté asociado al Communication Widget.
3. Verifica que la instancia Connect esté activa.
4. Si activaste autenticación JWT sin un servidor emisor de tokens, revisa el módulo 2: este laboratorio estático usa el widget sin esa opción. No pegues la clave secreta en el frontend.

[Guía del widget](https://docs.aws.amazon.com/connect/latest/adminguide/connect-widget-on-website.html).

## Módulo 2: Instancia Amazon Connect + widget

### “The access URL is already in use”

**Problema:** el alias ya está en uso.

Elige uno diferente, por ejemplo `conecta-company-ana-2026` o `conecta-lab-tu-nombre`. Usa minúsculas, números y guiones.

[Crear una instancia](https://docs.aws.amazon.com/connect/latest/adminguide/amazon-connect-instances.html).

### No puedo iniciar sesión en la instancia

1. Copia la URL real desde la consola; puede tener un formato como `https://TU-ALIAS.my.connect.aws`.
2. Verifica usuario y contraseña. Las credenciales del agente Connect no son las credenciales de AWS Console.
3. Utiliza **Forgot password**, si corresponde.
4. Comprueba que la instancia terminó de crearse.
5. Si una ventana privada bloquea cookies o ventanas necesarias, usa un perfil separado y revisa los requisitos del CCP.

[Inicio y cierre de sesión en el CCP](https://docs.aws.amazon.com/connect/latest/adminguide/ccp-login.html).

### El agente no aparece en User management

1. Espera uno o dos minutos y actualiza la página.
2. Comprueba que seleccionaste **Save** al crearlo.
3. Verifica que estés en la instancia correcta.

[Administrar usuarios](https://docs.aws.amazon.com/connect/latest/adminguide/manage-users.html).

### La consola exige una URL para configurar el widget

Usa el endpoint del sitio S3 del módulo 1. Cópialo desde **Properties → Static website hosting → Bucket website endpoint**. Configura el origen permitido con el protocolo exacto. No autorices todos los sitios mediante un comodín.

[Configuración del widget](https://docs.aws.amazon.com/connect/latest/adminguide/add-chat-to-website.html).

## Módulo 3: Amazon Lex V2 + Contact Flow

### El bot no reconoce la intención

1. Revisa las utterances y agrega variantes que usaría una persona real.
2. Usa frases en español y el locale **`es_419`**; `es-419` corresponde a HTML, no al identificador de Lex V2.
3. Vuelve a ejecutar **Build** después de modificar el bot.
4. Prueba en el simulador de Lex antes de integrar con Connect.
5. Si el simulador funciona pero Connect no, revisa la versión del alias y que el idioma esté habilitado en él.

Referencias: [frases de ejemplo](https://docs.aws.amazon.com/lexv2/latest/dg/sample-utterances.html), [pruebas](https://docs.aws.amazon.com/lexv2/latest/dg/test-bot.html), [locales](https://docs.aws.amazon.com/lexv2/latest/dg/how-languages.html).

### Connect no encuentra el bot

1. Comprueba que exista una versión compilada y un alias que apunte a ella.
2. Habilita `es_419` en el alias.
3. Asocia ese alias a tu instancia desde la configuración de Amazon Lex de Connect.
4. Para este laboratorio, utiliza la misma región que Connect.
5. Revisa los permisos de invocación si la asociación no se completa.

[Agregar un bot Lex a Connect](https://docs.aws.amazon.com/connect/latest/adminguide/amazon-lex.html).

### El flujo no decide correctamente según la intent

1. Revisa cómo **Get customer input** expone la intent.
2. Confirma que las condiciones usan el nombre exacto de la intent.
3. Revisa cómo termina la interacción del bot y cómo se manejan **Default** y **Error**.
4. Habilita logs si necesitas inspeccionar el comportamiento.

Referencias: [flujo con bot](https://docs.aws.amazon.com/connect/latest/adminguide/create-bot-flow.html), [logs](https://docs.aws.amazon.com/connect/latest/adminguide/contact-flow-logs.html).

### El timeout no funciona como se esperaba

En **Get customer input**, usa **Chat timeout** para la interacción con Lex. Configura **un minuto**, el requisito del laboratorio y el mínimo admitido; no copies los 8 segundos de la imagen antigua ni ajustes parámetros de silencio de voz. Prueba el caso sin enviar mensajes y verifica la salida de timeout.

[Referencia de Get customer input](https://docs.aws.amazon.com/connect/latest/adminguide/get-customer-input.html).

### No puedo conectar dos bloques

- Arrastra desde el punto de salida del bloque de origen.
- Suelta en el punto de entrada del bloque de destino.
- Conecta cada salida necesaria por separado.

[Crear flujos](https://docs.aws.amazon.com/connect/latest/adminguide/connect-contact-flows.html).

### El flujo no se publica: error de validación

1. Revisa que no haya bloques sin conectar.
2. Conecta **Entry point** al primer bloque.
3. Asegúrate de que cada ruta tenga un final válido, como una transferencia o desconexión, y maneja las salidas de error correspondientes.
4. Abre los bloques que la consola marque como incompletos y corrige su configuración.

[Buenas prácticas de flujos](https://docs.aws.amazon.com/connect/latest/adminguide/bp-contact-flows.html).

### Cómo depurar un flujo

1. Habilita los logs de flujo en la configuración de la instancia y verifica los permisos necesarios.
2. Agrega **Set logging behavior** al inicio del flujo y selecciona **Enable**.
3. Publica el flujo y ejecuta una prueba.
4. Consulta el grupo correspondiente en CloudWatch Logs.
5. Limita la retención y elimina los logs del laboratorio cuando ya no los necesites. No registres datos reales sensibles.

[Habilitar logs de flujo](https://docs.aws.amazon.com/connect/latest/adminguide/contact-flow-logs.html).

## Módulo 4: Pruebas y métricas

### El chat no llega al CCP del agente

1. Confirma que el agente esté **Available**, no Offline ni en trabajo posterior pendiente.
2. Comprueba que el flujo publicado esté asociado al widget.
3. Revisa el perfil de enrutamiento del agente, por ejemplo **Basic Routing Profile**.
4. Confirma que la cola usada por el flujo, por ejemplo **BasicQueue**, esté en ese perfil.
5. Habilita **Chat** con capacidad de al menos un contacto tanto en la configuración del perfil como donde corresponda a la cola.
6. Revisa que la transferencia a la cola sea alcanzable desde la intent probada.

Referencias: [estado del agente](https://docs.aws.amazon.com/connect/latest/adminguide/metrics-agent-status.html), [enrutamiento](https://docs.aws.amazon.com/connect/latest/adminguide/about-routing.html), [diagnóstico del CCP](https://docs.aws.amazon.com/connect/latest/adminguide/troubleshooting.html).

### Las métricas históricas no muestran datos

1. Espera unos minutos; prueba nuevamente después de 10 a 15 minutos si aún no aparecen. No es un plazo garantizado.
2. Comprueba fecha, zona horaria, cola y canal.
3. Confirma que finalizaste las conversaciones y el trabajo posterior.
4. Amplía el intervalo del informe.

[Informes históricos](https://docs.aws.amazon.com/connect/latest/adminguide/historical-metrics.html).

### Contact Search no encuentra los contactos

1. Comprueba fecha y zona horaria.
2. Selecciona **Chat** como canal.
3. Espera unos minutos para permitir la actualización.
4. Amplía el intervalo y retira filtros que puedan excluir tus pruebas.

[Buscar contactos](https://docs.aws.amazon.com/connect/latest/adminguide/contact-search.html).

### No aparece la transcripción

Comprueba el almacenamiento de transcripciones de chat y los permisos del usuario y del bucket privado de Connect. La ausencia de transcripción no significa por sí sola que el contacto no exista. Nunca hagas público ese bucket para resolver un problema de acceso.

[Buscar e inspeccionar contactos](https://docs.aws.amazon.com/connect/latest/adminguide/contact-search.html).

## Problemas generales

### Permiso denegado en AWS Console

1. Confirma que estás en la cuenta AWS correcta.
2. Revisa con el administrador los permisos IAM para crear y usar los recursos del laboratorio.
3. **AmazonConnect_FullAccess** es una política administrada amplia de Connect, pero no es la única forma de conceder acceso ni garantiza permisos sobre todos los servicios asociados. Pueden ser necesarios permisos adicionales para S3, Lex, roles o logs.
4. Revisa restricciones de la organización, límites del plan y cuotas. No uses la cuenta raíz como solución al problema.

[Permisos IAM de Connect](https://docs.aws.amazon.com/connect/latest/adminguide/security-iam-amazon-connect-permissions.html).

### La instancia no aparece en la lista

Comprueba la **región** seleccionada en AWS Console y la cuenta. La instancia pertenece a una región específica; para este laboratorio se recomienda `us-east-1`.

[Cuotas de Amazon Connect](https://docs.aws.amazon.com/connect/latest/adminguide/amazon-connect-service-limits.html).

### Aparecen cargos o el plan no permite crear recursos

Revisa el modelo Connect Customer/Customer Basic, el saldo de créditos, los servicios disponibles en el plan y el consumo en Billing. No asumas costo cero. Consulta el [apartado de costos](./README.md#-costo-estimado) y acuerda con el instructor una cuenta de formación adecuada si hace falta.

## 📚 Diagnóstico oficial

| Problema | Documentación |
|---|---|
| Problemas generales del CCP | [Diagnóstico](https://docs.aws.amazon.com/connect/latest/adminguide/troubleshooting.html) |
| Problemas comunes del CCP | [Casos frecuentes](https://docs.aws.amazon.com/connect/latest/adminguide/common-ccp-issues.html) |
| Identidad y acceso | [Diagnóstico IAM](https://docs.aws.amazon.com/connect/latest/adminguide/security_iam_troubleshoot.html) |
| Logs de flujo | [CloudWatch](https://docs.aws.amazon.com/connect/latest/adminguide/contact-flow-logs.html) |
| Errores 403 de S3 | [Acceso denegado](https://docs.aws.amazon.com/AmazonS3/latest/userguide/troubleshoot-403-errors.html) |
