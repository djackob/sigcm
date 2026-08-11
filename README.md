# Prototipo navegable SIGCM

Prototipo web local, sin backend, elaborado a partir de los diagramas Bizagi ubicados en `Analisis/Flujo de Proceso - Contratacion menor a 8 UIT v 5.0`, la Directiva N.° 002-2026-ANIN (versión 1, 52 páginas) y la Directiva N.° 0007-2025-EF/54.01 para la gestión del CMN. Este README documenta el comportamiento actualmente implementado y sirve como línea base para validar los flujos antes de redactar los casos de uso de implementación.

## Cómo ejecutar

Opción directa:

1. Abrir la carpeta `mockup/sigcm`.
2. Hacer doble clic en `index.html`.
3. En la pantalla SSO, elegir uno de los perfiles disponibles y pulsar **Continuar con SSO**.

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

El acceso SSO presenta 12 opciones, correspondientes a nueve grupos funcionales. El Área usuaria y la Unidad de Abastecimiento se desagregan porque sus tareas, firmas y autorizaciones no son intercambiables.

1. Proveedor.
2. Mesa de Partes.
3. Área usuaria · Jefe.
4. Área usuaria · Especialista.
5. Oficina de Administración (OA).
6. DAI.
7. Planeamiento y Presupuesto (OPP / Unidad de Presupuesto).
8. Unidad de Abastecimiento · Jefe.
9. Unidad de Abastecimiento · Coordinador.
10. Unidad de Abastecimiento · Especialista.
11. Unidad de Contabilidad.
12. Unidad de Tesorería.

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

#### Actores y responsabilidades

- **Área usuaria · Especialista:** verifica la disponibilidad en SIAF, registra y sustenta el Anexo 3, administra sus ítems, atiende las observaciones y deriva el documento al Jefe cuando se requiere firma o envío externo.
- **Área usuaria · Jefe:** firma digitalmente el Anexo 3, aprueba su envío o reenvío, recepciona formalmente las observaciones y recepciona el Anexo 4 individual o consolidado para cerrar el flujo.
- **Oficina de Administración:** revisa el Anexo 3 firmado. Puede observarlo y devolverlo al Área usuaria o, si está conforme, derivarlo a la Unidad de Abastecimiento.
- **Unidad de Abastecimiento:** revisa el Anexo 3, registra si la inclusión es ordinaria o urgente, formula observaciones o aprueba la solicitud. Con solicitudes conformes genera, firma y envía el Anexo 4, de manera individual o consolidada.

#### Flujo principal implementado

1. El Área usuaria registra el **Anexo 3** con su sustento y al menos un ítem.
2. Si lo prepara el Especialista, lo deriva al Jefe. El Jefe revisa y firma digitalmente el documento.
3. El Jefe envía el Anexo 3 a OA; el expediente queda **En evaluación OA**.
4. OA revisa la integridad del documento:
   - Si observa, la notificación llega al Jefe del Área usuaria.
   - Si está conforme, deriva el expediente a la Unidad de Abastecimiento.
5. Abastecimiento revisa el Anexo 3 y selecciona el tipo de inclusión: **Ordinario** o **Urgente**.
6. Si Abastecimiento observa, notifica al Área usuaria. Si aprueba, el expediente queda listo para generar el Anexo 4.
7. Abastecimiento genera y firma el **Anexo 4**. Puede procesarlo individualmente o seleccionar dos o más solicitudes conformes para generar un único Anexo 4 consolidado.
8. Abastecimiento envía el Anexo 4 al Área usuaria. En el envío consolidado se crea una sola entrada de recepción con la referencia de todos los expedientes incluidos.
9. El Jefe del Área usuaria recepciona el Anexo 4. La recepción finaliza la entrada consolidada y todos los expedientes relacionados.

#### Observación y subsanación

- OA y Abastecimiento pueden registrar el motivo de la observación.
- El Área usuaria debe recepcionar la notificación antes de corregir.
- La subsanación permite modificar el sustento y el contenido completo del Anexo 3.
- Si se actualiza un Anexo 3 ya firmado, la firma anterior se invalida y el Jefe debe firmar nuevamente.
- Una observación de OA retorna a OA después de la subsanación. Una observación de Abastecimiento retorna directamente a Abastecimiento.
- El documento corregido no puede reenviarse hasta contar con la nueva firma del Jefe.

#### Reglas y documentos CMN

- Cada ítem contiene código, descripción, unidad de medida, cantidad y valor.
- Por cada ítem se registra **exclusión o inclusión, nunca ambas**; el grupo seleccionado exige cantidad y valor completos.
- La tabla es dinámica, pero debe conservar al menos un ítem.
- El Anexo 3 muestra el responsable del Área usuaria y el Anexo 4 representa los dos firmantes previstos: responsable de Abastecimiento y máxima autoridad administrativa.
- Ambos anexos disponen de visor, impresión/PDF, huella y firma digital simulada.
- Estados principales: `Registrar Anexo 3` → `Por firmar AU` → `Firmado Anexo 3` → `En evaluación OA` → `Derivado a UA` / `En evaluación UA` → `Generar Anexo 4` → `Firmar digitalmente Anexo 4` → `Enviar Anexo 4` → `Recepcionar Anexo 4` → `Fin`.

Para probar el circuito completo sin recargar la página, cierre la sesión y cambie de perfil en cada transferencia. El expediente conserva temporalmente su etapa e historial mientras la pestaña permanezca abierta.

### Requerimiento a Notificación

El módulo cubre desde el registro de la necesidad hasta la emisión y notificación de la orden. El registro inicial captura denominación, objeto, DEC, disponibilidad presupuestal, condición CMN, monto, ATE, RUC, plazo, sustento y uno o más pedidos SIGA. Los datos presupuestales y del pedido se precargan desde un catálogo simulado; los campos derivados de SIGA son de solo lectura.

Si la necesidad está incluida en el CMN se solicita el Anexo 1 firmado. Si no está incluida, se habilita el acceso a Gestión CMN y se exige seleccionar un Anexo 4 finalizado o adjuntar un Anexo 4 firmado. También se registra la evidencia del saldo disponible o de la habilitación aprobada. El monto debe ser mayor que cero y no superar ocho UIT; para 2026 el prototipo usa S/ 44 000.

#### Documentos según el objeto

- **Bien:** EETT, Anexo 1 (páginas 21–26).
- **Servicio:** TDR, Anexo 2 (páginas 27–31).
- **Locación:** primero se genera y firma la propuesta del Área usuaria, Anexo 5 (página 42), y después se elabora y firma el TDR, Anexo 3 (páginas 32–37). El Anexo 5 admite entre una y cinco propuestas; no exige una terna. Los pedidos SIGA vinculados se reflejan en ambos documentos y no se vuelven a capturar.
- **Consultoría:** TDR, Anexo 4 (páginas 38–41).

Cada formato tiene un formulario y un visor independiente. La firma digital corresponde al **Jefe o titular del Área usuaria**; el Especialista elabora y deriva. El requerimiento inicial puede editarse mientras está en borrador o durante una subsanación formalmente recepcionada. Fuera de esas etapas funciona como visor de solo lectura.

#### Actores y responsabilidades

- **Área usuaria · Especialista:** registra la necesidad y los pedidos SIGA, elabora anexos y documentos técnicos, prepara subsanaciones, responde consultas y realiza la validación técnica.
- **Área usuaria · Jefe:** firma los anexos y el documento técnico, aprueba el envío y reenvío, responde actuaciones que requieren la decisión del titular y valida técnicamente cuando corresponde.
- **Oficina de Administración:** para expedientes cuya DEC es Abastecimiento, revisa el formulario y los documentos impresos. Puede observar y devolver al Área usuaria o derivar a Abastecimiento. No crea requerimientos.
- **DAI:** recibe directamente los requerimientos que le corresponden y actúa como DEC durante la revisión, indagación, CCP, perfeccionamiento y notificación.
- **Unidad de Abastecimiento · Especialista:** recepciona el expediente derivado por OA, efectúa la revisión inicial, conduce la indagación, recepciona la CCP aprobada y notifica la orden.
- **Unidad de Abastecimiento · Coordinador:** decide el resultado de la revisión, gestiona observaciones/no objeción, valida la selección, elabora el Anexo 8 cuando corresponde, registra la solicitud de CCP y verifica el expediente para perfeccionamiento.
- **Unidad de Abastecimiento · Jefe:** autoriza el perfeccionamiento y emite la OC u OS.
- **OPP / Unidad de Presupuesto:** recepciona la solicitud, aprueba u observa la CCP o previsión presupuestal y la remite nuevamente a la DEC. OPP no aprueba el cuadro de cotizaciones.

#### Flujo principal implementado

1. El Especialista del Área usuaria registra el requerimiento y vincula uno o más pedidos SIGA.
2. Se elaboran los documentos según el objeto. En Locación el orden obligatorio es **Anexo 5 → firma del Jefe → TDR Anexo 3 → firma del Jefe**.
3. El Jefe aprueba y remite el expediente:
   - Si la DEC es la Unidad de Abastecimiento: Área usuaria → OA → Unidad de Abastecimiento.
   - Si la DEC es DAI: Área usuaria → DAI, sin pasar por OA.
4. OA revisa el expediente completo, incluyendo los visores impresos. Puede observarlo o derivarlo a la Unidad de Abastecimiento.
5. En Abastecimiento, el Especialista recepciona y revisa; el Coordinador registra el resultado: conforme, observado o con mejoras sujetas a no objeción.
6. Si está conforme, el Especialista inicia la indagación de mercado. Locación requiere una cotización válida; los demás objetos requieren dos o más, salvo excepción validada.
7. Las consultas u observaciones de mercado retornan al Área usuaria. Si la respuesta modifica el TDR, se genera una nueva versión, el Jefe vuelve a firmarla y se remite a la DEC mediante SGD.
8. El Área usuaria valida técnicamente la cotización u ofertas. Si no cumplen, la indagación se reinicia.
9. El Coordinador confirma al proveedor seleccionado:
   - Con una sola cotización de Locación no se genera el Anexo 8.
   - Con dos o más cotizaciones válidas se genera y suscribe el **Anexo 8 — Cuadro de Cotizaciones**.
10. El Coordinador registra la solicitud de CCP o previsión en SIAF WEB y la remite por SGD a OPP / Unidad de Presupuesto.
11. OPP puede aprobar o observar. Si observa, devuelve a la DEC para corregir y solicitar nuevamente. Si aprueba, remite la CCP o previsión a la DEC.
12. El Especialista recepciona la CCP; el Coordinador verifica la integridad del expediente y lo deja listo para perfeccionamiento.
13. El Jefe de Abastecimiento emite la orden: OC para bienes y OS para servicios, consultorías o Locación.
14. El Especialista notifica la orden al proveedor por correo institucional, con copia al Jefe del Área usuaria, y finaliza el flujo.

#### Observación y subsanación

- OA o la DEC pueden observar el expediente.
- El Área usuaria primero recepciona la observación y después abre la subsanación integral.
- La subsanación puede corregir los datos generales, pedidos SIGA, Anexo 5 y documento técnico; no se limita al pedido inicialmente seleccionado.
- Si se modifica un anexo firmado, se invalida la firma anterior y se exige una nueva firma del Jefe.
- Para la ruta de Abastecimiento, la subsanación conserva el circuito Área usuaria → Jefe → OA → Unidad de Abastecimiento.
- La falta de cotizaciones produce reiteraciones; después de los intentos simulados, el expediente puede retornar al Área usuaria para reformulación.

El expediente conserva la etapa, los documentos y la trazabilidad al cambiar de perfil mientras la pestaña no se recargue.

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
- Al menos 2 cotizaciones, salvo los supuestos de excepción descritos por la Directiva. Para Locación de servicios de persona natural, el flujo implementado acepta una cotización válida.
- El Anexo 8 se genera con dos o más cotizaciones válidas; no corresponde cuando la indagación concluye con una única cotización.
- Consultas de mercado al Área usuaria: pronunciamiento en 1 día hábil; validación técnica en 2 días hábiles.
- CCP/previsión presupuestal: atención en 1 día hábil.
- Ampliación: solicitud dentro de los 10 días hábiles siguientes al hecho generador; evaluación y notificación dentro de los plazos normativos.
- Conformidad: 7 días calendario; 20 días calendario para consultorías o cuando se requieran pruebas.
- Subsanación del entregable: plazo no mayor al 30% del plazo del entregable.
- Penalidades por mora y otras penalidades: tope conjunto de 10%.
- Pago: remisión a Contabilidad en 3 días hábiles; control previo y subsanación en 2 días hábiles; devengado y giro en SIAF-SP.
- Resolución total/parcial, apercibimiento y notificación notarial o por PLADICOP según causal.

## Línea base para elaborar casos de uso

Una vez que los responsables funcionales aprueben esta versión de los flujos, los casos de uso deben derivarse de las acciones visibles en cada bandeja. Como mínimo, cada caso deberá identificar actor y subrol, precondiciones, estado de entrada, formulario o visor utilizado, validaciones, documentos generados, firma requerida, resultado, estado de salida, destinatario y rutas alternativas de observación o devolución.

Los bloques funcionales previstos para esa siguiente etapa son:

1. Autenticación y selección de perfil.
2. Registro, firma, envío, observación y subsanación del Anexo 3 CMN.
3. Revisión de OA y validación de Abastecimiento en CMN.
4. Generación, firma, envío y recepción del Anexo 4 individual o consolidado.
5. Registro y edición controlada del requerimiento con pedidos SIGA y evidencia CMN/presupuestal.
6. Elaboración, versionado y firma de EETT/TDR y del Anexo 5.
7. Revisión de OA y de la DEC, observación, subsanación y no objeción.
8. Indagación, consultas, validación técnica y selección del proveedor.
9. Generación o exclusión justificada del Anexo 8.
10. Solicitud, observación, aprobación y recepción de CCP o previsión.
11. Perfeccionamiento, emisión y notificación de OC/OS.

### Decisiones que deben quedar aprobadas

- Confirmar si el Anexo 4 CMN individual continuará permitido o si todo envío deberá ser consolidado desde dos solicitudes.
- Confirmar qué subrol de Abastecimiento firma el Anexo 4 CMN y cómo se representa la segunda firma de la máxima autoridad administrativa.
- Confirmar la desagregación interna de roles de DAI, actualmente representada como una sola DEC.
- Confirmar el número máximo de propuestas del Anexo 5; el mockup usa el límite físico de cinco filas del formato.
- Confirmar que la excepción de una cotización y la no generación del Anexo 8 aplican a toda Locación de persona natural bajo este flujo.
- Confirmar si la notificación final debe copiar únicamente al Jefe del Área usuaria o también al Especialista responsable.

## Archivos

- `index.html`: estructura y vistas de la aplicación.
- `styles.css`: sistema visual institucional y comportamiento responsive.
- `app.js`: perfiles, matriz de acceso, navegación, datos demostrativos, formularios y exportaciones.
