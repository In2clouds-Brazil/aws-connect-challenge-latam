# ✅ Lista de validación: Amazon Connect Lab LATAM

Usa esta lista para verificar que tu implementación esté completa antes de entregarla.

## Módulo 1: Frontend + sitio S3

- [ ] Archivo `index.html` creado
- [ ] Archivo `styles.css` creado
- [ ] Bucket S3 creado con nombre único
- [ ] Static website hosting habilitado
- [ ] Block Public Access ajustado en el bucket dedicado al sitio
- [ ] Política del bucket configurada correctamente
- [ ] Archivos subidos: `index.html` y `styles.css`
- [ ] Sitio accesible mediante URL pública
- [ ] URL del sitio anotada

## Módulo 2: Instancia Amazon Connect + widget

- [ ] Instancia creada con alias único
- [ ] Identity management: **Store users within Amazon Connect**, sin SSO
- [ ] Inicio de sesión del administrador funcional
- [ ] Usuario `agente01` creado
- [ ] Perfil de seguridad del agente: **Agent**
- [ ] Perfil de enrutamiento: **Basic Routing Profile**, con Chat y la cola elegida habilitados
- [ ] Communication Widget creado con el origen y protocolo exactos del sitio S3
- [ ] Snippet del widget agregado a `index.html`
- [ ] Archivo actualizado publicado nuevamente en S3
- [ ] Widget visible en el sitio
- [ ] Inicio de sesión del agente en el CCP funcional

## Módulo 3: Contact Flow con Amazon Lex V2

### Bot Amazon Lex V2

- [ ] Bot creado con locale **`es_419`** compilado
- [ ] Intents creadas para las seis opciones del menú principal
- [ ] Intents creadas para los submenús de las opciones 1 y 2
- [ ] Bot probado en el simulador de la consola Lex, en español
- [ ] Versión del bot asociada a un alias activo con `es_419` habilitado
- [ ] Alias del bot asociado a la instancia Amazon Connect

### Contact Flow

- [ ] Flujo creado con nombre descriptivo
- [ ] Mensaje de bienvenida configurado en español
- [ ] Integración con Lex V2 configurada para español LATAM
- [ ] Enrutamiento basado en las intents devueltas por Lex
- [ ] Submenús de las opciones 1 y 2 implementados
- [ ] Opciones de atención directa conectadas a la cola
- [ ] Timeout de chat configurado en **un minuto**
- [ ] Manejo de intent no reconocida con hasta **dos repeticiones**
- [ ] Flujo con estado **Published**
- [ ] Flujo asociado al Communication Widget
- [ ] Todas las rutas probadas con las herramientas disponibles y confirmadas en el chat

## Módulo 4: Pruebas y métricas

- [ ] Agente en estado **Available** durante las pruebas
- [ ] Al menos dos conversaciones por rutas diferentes
- [ ] Bot reconoció correctamente las intenciones durante las pruebas
- [ ] Agente recibió y respondió los chats en el CCP
- [ ] Historical Metrics consultado
- [ ] Datos de las conversaciones visibles en las métricas
- [ ] Contact Search utilizado
- [ ] Contactos de prueba localizados en Contact Search

## Evidencias recopiladas

- [ ] URL pública del sitio S3 anotada
- [ ] Captura del bot en la consola Lex V2
- [ ] Captura de las intents configuradas
- [ ] Captura de la prueba del bot en el simulador Lex
- [ ] Captura del sitio con el widget visible
- [ ] Captura de la conversación 1, ruta A
- [ ] Captura de la conversación 2, ruta B
- [ ] Captura de las métricas históricas
- [ ] Captura de Contact Search con los contactos
- [ ] Video de demostración (recomendado)

Revisa que las evidencias no contengan credenciales, claves ni datos personales reales. Coordina la limpieza de recursos después de la evaluación.

## Puntuación

| Módulo | Elementos | Completados |
|---|---|---|
| Módulo 1 | 9 | ___ / 9 |
| Módulo 2 | 11 | ___ / 11 |
| Módulo 3: Bot Lex | 6 | ___ / 6 |
| Módulo 3: Flujo | 11 | ___ / 11 |
| Módulo 4 | 8 | ___ / 8 |
| Evidencias | 10 | ___ / 10 |
| **Total** | **55** | **___ / 55** |

**Mínimo para aprobar: 50 / 55 (aproximadamente 90 %).** Se conserva la rúbrica original; el video recomendado aporta un elemento y puede omitirse sin impedir alcanzar el mínimo.
