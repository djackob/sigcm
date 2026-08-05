# Prototipo navegable SIGCM

Prototipo web local, sin backend, elaborado a partir de los 10 diagramas Bizagi ubicados en `Analisis/Flujo de Proceso - Contratacion menor a 8 UIT v 5.0` y de la Directiva N.° 002-2026-ANIN (versión 1, 52 páginas).

## Cómo ejecutar

Opción directa:

1. Abrir la carpeta `prototipado`.
2. Hacer doble clic en `index.html`.
3. En la pantalla SSO, elegir cualquiera de los 10 perfiles y pulsar **Continuar con SSO**.

No requiere instalación, servidor, conexión a internet ni credenciales reales. Para una prueba más cercana a producción también puede servirse la carpeta con cualquier servidor HTTP estático.

## Alcance funcional

- Simulación de ingreso por SSO y cierre/cambio de perfil.
- Menú lateral dinámico calculado a partir de la matriz de acceso.
- Tablero, bandeja de tareas, buscador y consulta de expedientes.
- Detalle de expediente, documentos, trazabilidad, auditoría y plazos.
- Alertas, notificaciones y preferencias de aviso.
- Reportes e indicadores con exportación CSV compatible con Excel.
- Exportación a PDF mediante la vista de impresión del navegador.
- Formularios simulados, validaciones básicas y mensajes de confirmación.
- Registro, modificación y eliminación temporal de datos en cada módulo. Las tablas, bandejas, expedientes y exportaciones se actualizan inmediatamente.
- Diseño responsive para escritorio, tableta y móvil; navegación por teclado y etiquetas accesibles.

Los datos son demostrativos y se administran mediante arreglos temporales en memoria. Los cambios no se guardan al recargar la página.

## Perfiles representados

1. Proveedor
2. Mesa de Partes
3. Área usuaria
4. Oficina de administración
5. DAI
6. Planeamiento y Presupuesto
7. Unidad de Abastecimiento
8. Unidad de Contabilidad
9. Unidad de Tesorería

## Matriz de acceso implementada

La normalización de mayúsculas en los nombres no cambia el contenido de la matriz.

| Módulo / subproceso | Perfiles con acceso |
|---|---|
| Gestión CMN | Unidad de Abastecimiento; Oficina de administración; Área usuaria |
| Requerimiento a Notificación | Área usuaria; Oficina de administración; DAI; Unidad de Abastecimiento; Planeamiento y Presupuesto |
| Pago | Proveedor; Mesa de Partes; Área usuaria; Unidad de Abastecimiento; Unidad de Contabilidad; Unidad de Tesorería |
| Ejecución | Proveedor; Mesa de Partes; Área usuaria; Unidad de Abastecimiento |
| Modificación-Ampliación | Proveedor; Mesa de Partes; Área usuaria; Unidad de Abastecimiento |
| Resolución - Acumulación máxima de penalidad | Proveedor; Área usuaria; Unidad de Abastecimiento |
| Resolución - Hecho sobresaliente | Proveedor; Área usuaria; Unidad de Abastecimiento |
| Resolución - Incumplimiento | Proveedor; Área usuaria; Unidad de Abastecimiento |
| Resolución - Mutuo acuerdo | Proveedor; Área usuaria; Unidad de Abastecimiento |
| Resolución - Resolución unilateral | Área usuaria; Unidad de Abastecimiento |

Después del ingreso SSO, el sistema abre directamente **Mi bandeja**. El menú lateral contiene únicamente Mi bandeja y los módulos de proceso autorizados para el perfil. Se retiraron Inicio, Expedientes, Alertas y notificaciones, Reportes e indicadores y Auditoría de la navegación, junto con la búsqueda global y el centro superior de notificaciones.

Las pantallas de los módulos muestran directamente su encabezado y bandeja de trabajo. Se retiraron los bloques informativos “Flujo por perfil”, “Responsabilidades del perfil”, acciones rápidas y variantes equivalentes en todos los módulos.

Los formularios de registro utilizan cuatro tipos de objeto o prestación: **Bien, Servicio, Consultoría y Locación**. En Ejecución, Consultoría y Locación siguen la ruta de presentación y evaluación de entregables; únicamente Bien activa la ruta de ingreso y recepción física.

## Flujos representados

### Gestión CMN

Gestión de la modificación del Cuadro Multianual de Necesidades conforme a la Directiva N.° 0007-2025-EF/54.01: generación del **Anexo 3 — Solicitud de modificación del CMN** y del **Anexo 4 — Aprobación de modificaciones al CMN**.

La bandeja CMN aplica acciones específicas por perfil y etapa:

- **Área usuaria:** registra el nuevo CMN, firma y envía el Anexo 3; recepciona observaciones, subsana y reenvía; finalmente recepciona el Anexo 4 y cierra el flujo.
- **Oficina de administración:** no registra solicitudes; recepciona el Anexo 3 y lo deriva a la Unidad de Abastecimiento.
- **Unidad de Abastecimiento:** recepciona y valida el Anexo 3; si tiene observaciones las notifica al Área usuaria; si está conforme genera, firma y envía el Anexo 4.

El Anexo 3 registra Área usuaria, fecha, código, descripción, unidad de medida, cantidades y valores de exclusión/inclusión, sustento y años afectados. Su tabla de ítems es dinámica: inicia con una fila y permite agregar o retirar todas las filas necesarias. Tras validarlo, Abastecimiento completa el Anexo 4 conservando todos los ítems, además de entidad, identificación, solicitud y los dos firmantes previstos en el formato. Ambos documentos tienen visor, huella y firma digital simulada; no pueden enviarse antes de firmarse.

Para probar el circuito completo sin recargar la página, cierre la sesión y cambie de perfil en cada transferencia. El expediente conserva temporalmente su etapa e historial mientras la pestaña permanezca abierta.

### Requerimiento a Notificación

El registro pregunta si la necesidad ya está incluida en el CMN. Si la respuesta es sí, solicita adjuntar el Anexo 1 firmado; si es no, muestra un acceso a Gestión CMN y exige seleccionar o adjuntar el Anexo 4 aprobado. En este mismo formulario se consultan datos temporales del SIGA y se acumulan uno o más pedidos, que luego se reflejan en los anexos sin volver a capturarlos. Después se genera el documento técnico según el objeto:

- **Bien:** EETT, Anexo 1 (páginas 21–26).
- **Servicio:** TDR, Anexo 2 (páginas 27–31).
- **Locación:** primero se genera y firma la propuesta del Área usuaria, Anexo 5 (página 42), y luego se elabora y firma el TDR, Anexo 3 (páginas 32–37). Los pedidos SIGA vinculados en el requerimiento se muestran como referencia no editable y se reflejan en ambos documentos. El Anexo 5 admite múltiples proveedores, cada anexo dispone de su propio icono y visor independiente, y la indagación requiere una sola cotización válida.
- **Consultoría:** TDR, Anexo 4 (páginas 38–41).

Cada formato se completa en un formulario específico, se muestra en un visor tipo PDF y debe firmarse digitalmente antes de permitir la remisión. El formulario inicial del requerimiento también conserva un visor permanente con todos los pedidos SIGA. Sólo el Área usuaria puede editarlo mientras está en borrador o después de recepcionar observaciones; al guardar la subsanación vuelve a modo de solo lectura. Después continúan las autorizaciones, revisión y subsanación, no objeción, indagación de mercado, consultas, validación técnica, CCP/previsión presupuestal, perfeccionamiento y notificación de OS/OC o contrato.

La bandeja implementa acciones específicas por perfil y dependencia competente:

- **Área usuaria:** registra la necesidad, disponibilidad, inclusión CMN, pedido SIGA, EETT/TDR y anexos; subsana observaciones, responde no objeción y consultas de mercado, y valida ofertas técnicas.
- **Oficina de administración:** cuando la DEC es la Unidad de Abastecimiento, recepciona el requerimiento del Área usuaria y lo deriva a la UA el mismo día. No crea requerimientos.
- **DAI:** recibe directamente los requerimientos asignados a DAI y ejecuta las actuaciones de revisión, indagación, CCP, perfeccionamiento y notificación.
- **Unidad de Abastecimiento:** actúa como DEC para bienes, servicios y consultorías ordinarias; recibe por derivación de OA, revisa, observa o solicita no objeción, gestiona cotizaciones, cuadro comparativo, CCP, OC/OS y notificación.
- **Planeamiento y Presupuesto:** recepciona, aprueba y remite la CCP o previsión presupuestal.

Se simulan las ramas de observación y reformulación, mejoras/no objeción, consultas de mercado, falta o reiteración de cotizaciones, validación técnica, certificación presupuestal y notificación final. El expediente conserva la etapa al cambiar de perfil mientras no se recargue la página.

### Ejecución

Inicio posterior a la notificación; entrega de bienes o presentación de entregables; recepción por Mesa de Partes; verificación por el Área usuaria y, para bienes, Almacén; observación/retiro; subsanación; guía de remisión; Acta de Conformidad (Anexo 11); inicio del pago.

La bandeja de Ejecución distribuye el flujo por perfil:

- **Proveedor:** registra el inicio de la OS/OC, presenta entregables o transporta bienes, subsana servicios observados y retira bienes no conformes.
- **Mesa de Partes:** recepciona y deriva los entregables de servicios, incluidas las subsanaciones.
- **Área usuaria:** evalúa entregables, informa observaciones, emite conformidad; para bienes designa al verificador y recepciona los bienes o la guía firmada.
- **Unidad de Abastecimiento:** notifica observaciones de servicios; en Sede Central autoriza el ingreso al Almacén, coordina el acompañamiento, verifica y entrega los bienes al Área usuaria.

Se implementaron tres rutas: servicios/consultorías, bienes en Sede Central y bienes en sede desconcentrada. Cuando la ejecución finaliza conforme, el prototipo crea automáticamente el trámite correspondiente en el módulo Pago, evitando duplicados.

### Pago

Conformidad; expediente y Check list de control de pagos (Anexo 9); Determinación de penalidades (Anexo 10); control previo; devolución de observaciones al responsable; devengado; autorización; solicitud y registro del giro.

La bandeja de Pago aplica las tareas del diagrama por perfil:

- **Proveedor:** subsana el entregable según las observaciones notificadas.
- **Mesa de Partes:** notifica la carta al proveedor y recepciona el entregable subsanado.
- **Área usuaria:** inicia el trámite, evalúa la prestación, emite el Acta de Conformidad (Anexo 11) o el informe de observaciones y subsana devoluciones documentales.
- **Unidad de Abastecimiento:** elabora la carta de observaciones, verifica documentos, realiza el control de pagos, incorpora el Check list (Anexo 9) y la Determinación de penalidades (Anexo 10), y deriva a Contabilidad.
- **Unidad de Contabilidad:** ejecuta el control previo; deriva observaciones al Área usuaria o Abastecimiento; registra y aprueba el devengado y solicita el giro.
- **Unidad de Tesorería:** recepciona la solicitud y registra el giro en SIAF-SP, cerrando el flujo.

Se pueden probar tanto el ciclo de observación/subsanación del entregable como las devoluciones del control previo. El expediente mantiene temporalmente su etapa al cambiar de perfil sin recargar la página.

### Modificación-Ampliación

Se implementaron las dos rutas y sus decisiones del diagrama `4. MODIFICACION-AMPLIACION.png`:

- **Proveedor:** presenta una modificación contractual o ampliación respecto de la OS/OC, adjunta sustento y medios probatorios, y recepciona la respuesta para continuar la ejecución.
- **Mesa de Partes:** recepciona la solicitud, la deriva según su tipo y notifica el pronunciamiento final al proveedor.
- **Área usuaria:** puede iniciar internamente una modificación; evalúa las solicitudes contractuales, elabora el sustento y justificación o el informe de rechazo, y emite opinión técnica cuando Abastecimiento la solicita para una ampliación.
- **Unidad de Abastecimiento:** revisa la modificación, emite el informe y el Acta de Modificación cuando resulta conforme; para ampliaciones verifica plazo y sustento, solicita opinión técnica, aprueba o deniega y envía la respuesta a Mesa de Partes.

La ampliación de plazo no genera Acta de Modificación, conforme a la nota del diagrama. Todos los cambios de estado y documentos se simulan en memoria y aparecen de inmediato en la bandeja, expediente y trazabilidad.

### Resolución

Resolución se presenta como **un único menú, sin submenús**, con una bandeja común. Al crear el expediente, el combo **Tipo de resolución** adapta los campos, el estado inicial y las acciones posteriores. La tabla sólo muestra los tipos permitidos por la matriz de acceso.

- **Acumulación máxima de penalidad:** la inicia el Área usuaria cuando existe causal; informa la acumulación y emite el informe de incumplimiento. Abastecimiento emite la carta notarial de resolución total o parcial. Si no existe causal, el flujo termina sin resolución.
- **Hecho sobresaliente:** puede iniciarlo el Proveedor mediante solicitud o el Área usuaria mediante informe técnico. El Área usuaria determina la procedencia; Abastecimiento emite la resolución total/parcial o la carta que deniega la solicitud.
- **Incumplimiento:** el Área usuaria informa los hechos y solicita el apercibimiento; Abastecimiento notifica la carta notarial; el Proveedor la recepciona y responde si subsana. El Área usuaria evalúa la respuesta y, si no se subsana, Abastecimiento emite la carta de resolución.
- **Mutuo acuerdo:** lo inicia el Proveedor. El Área usuaria emite pronunciamiento favorable o desfavorable y Abastecimiento formaliza la aceptación total/parcial o la denegatoria.
- **Resolución unilateral:** la inicia exclusivamente el Área usuaria con sustento técnico y legal; Abastecimiento emite la carta de resolución total o parcial.

Tipos disponibles al crear según el perfil iniciador:

- **Proveedor:** Hecho sobresaliente y Mutuo acuerdo.
- **Área usuaria:** Acumulación máxima de penalidad, Hecho sobresaliente, Incumplimiento y Resolución unilateral.
- **Unidad de Abastecimiento:** atiende y formaliza expedientes, pero no inicia una nueva resolución en los diagramas.

## Reglas de la Directiva incorporadas

- Requerimiento presentado al menos 10 días hábiles antes del inicio previsto.
- Revisión y subsanación del requerimiento: hasta 2 días hábiles por etapa.
- Cotización: hasta 3 días hábiles, ampliable según la naturaleza de la contratación.
- Al menos 2 cotizaciones, salvo los supuestos de excepción descritos por la Directiva.
- Consultas de mercado al Área usuaria: pronunciamiento en 1 día hábil; validación técnica en 2 días hábiles.
- CCP/previsión presupuestal: atención en 1 día hábil.
- Ampliación: solicitud dentro de los 10 días hábiles siguientes al hecho generador; evaluación y notificación dentro de los plazos normativos.
- Conformidad: 7 días calendario; 20 días calendario para consultorías o cuando se requieran pruebas.
- Subsanación del entregable: plazo no mayor al 30% del plazo del entregable.
- Penalidades por mora y otras penalidades: tope conjunto de 10%.
- Pago: remisión a Contabilidad en 3 días hábiles; control previo y subsanación en 2 días hábiles; devengado y giro en SIAF-SP.
- Resolución total/parcial, apercibimiento y notificación notarial o por PLADICOP según causal.

## Archivos

- `index.html`: estructura y vistas de la aplicación.
- `styles.css`: sistema visual institucional y comportamiento responsive.
- `app.js`: perfiles, matriz de acceso, navegación, datos demostrativos, formularios y exportaciones.
