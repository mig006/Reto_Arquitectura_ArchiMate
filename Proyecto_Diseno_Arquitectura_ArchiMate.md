# Diseño de la Arquitectura de una Aplicación Empresarial con ArchiMate y DDD

**Profesor:** César López
**Valor:** 25 % de la nota del curso

---

## Índice

1. [Propósito y objetivos de aprendizaje](#1-propósito-y-objetivos-de-aprendizaje)
2. [Enunciado del requerimiento](#2-enunciado-del-requerimiento)
   - [Fase 0. Entrevista al experto del negocio](#fase-0-entrevista-al-experto-del-negocio)
   - [Fase 1. Entendimiento del negocio y mapa de capacidades](#fase-1-entendimiento-del-negocio-y-mapa-de-capacidades)
   - [Fase 2. Descomposición de servicios del proceso E2E](#fase-2-descomposición-de-servicios-del-proceso-e2e)
   - [Fase 3. DDD estratégico](#fase-3-ddd-estratégico)
   - [Fase 4. DDD táctico](#fase-4-ddd-táctico)
   - [Fase 5. Mapa de contextos delimitados](#fase-5-mapa-de-contextos-delimitados)
   - [Fase 6. Canvas del contexto delimitado en ArchiMate](#fase-6-canvas-del-contexto-delimitado-en-archimate-componente-investigativo)
3. [Requisitos del modelo ArchiMate](#3-requisitos-del-modelo-archimate)
4. [Entregables y sustentación](#4-entregables-y-sustentación)
5. [Rúbrica de evaluación](#5-rúbrica-de-evaluación)
6. [Temas propuestos (solicitudes de usuario)](#6-temas-propuestos-solicitudes-de-usuario)

---

## 1. Propósito y objetivos de aprendizaje

Cada equipo actuará como el grupo de arquitectura que diseña una aplicación para una organización real, desde el entendimiento del negocio hasta el diseño de dominio, con alcance de arquitectura empresarial y modelado en ArchiMate.

Al finalizar, el equipo debe demostrar que es capaz de:

1. Levantar y validar conocimiento del negocio mediante una entrevista estructurada a un experto (simulado con IA).
2. Ubicar la aplicación dentro del mapa de capacidades y del proceso de negocio de extremo a extremo (E2E).
3. Descomponer el proceso en capas de servicio y decidir, con argumentos, qué entra y qué queda fuera del alcance.
4. Aplicar DDD estratégico y táctico: dominios, lenguaje ubicuo, agregados, comandos, eventos y mapa de contextos delimitados.
5. Diseñar las interoperabilidades con otros procesos y sistemas mediante una capa anticorrupción.
6. Representar todo lo anterior en vistas ArchiMate coherentes y trazables, y documentarlo en un repositorio del equipo.

---

## 2. Enunciado del requerimiento

El equipo recibirá una solicitud de usuario (ver [sección 6](#6-temas-propuestos-solicitudes-de-usuario)) y deberá diseñar la arquitectura de la aplicación que la atiende, simulando el acercamiento al negocio con las técnicas vistas en clase. **El diseño lo hace el equipo; la IA solo aporta conocimiento del negocio.**

### Fase 0. Entrevista al experto del negocio

El equipo configurará una IA para que asuma el rol de experto del negocio de la organización asignada y la entrevistará para complementar su entendimiento del dominio.

- **Guion previo:** preguntas organizadas por tema (contexto de la organización, proceso E2E, actores, reglas de negocio, excepciones, indicadores, normatividad, sistemas y procesos con los que interactúa). Debe incluir preguntas abiertas, de profundización y de validación.
- **Interoperabilidad:** el guion debe tener un bloque específico para descubrir con qué otros procesos, áreas, sistemas y entidades externas intercambia información el proceso.
- **Evidencia:** la instrucción de rol usada, la conversación completa exportada y un análisis del equipo que indique qué hallazgos se aceptaron, cuáles se descartaron o contrastaron con otras fuentes y cómo influyeron en el diseño.
- **Regla de uso:** no se permite pedir a la IA que proponga el mapa de capacidades, los dominios, los agregados, el mapa de contextos ni las vistas ArchiMate.

> ⚠️ **Esos productos son la evaluación del equipo.**

### Fase 1. Entendimiento del negocio y mapa de capacidades

*(Diapositiva 35 de 7_SOA)*

Construir el mapa de capacidades completo del negocio donde se ubica la aplicación, identificar la cadena de valor y el proceso E2E en el que opera, y señalar en qué capacidades empresariales impacta la solución.

Este mapa de capacidades se debe diseñar en ArchiMate, complementándolo con los **elementos de motivación** que lo justifican. Estos deben investigarse con la IA haciendo preguntas alrededor de los elementos estratégicos de un negocio: interesados, impulsores, evaluaciones, metas, resultados, principios, requerimientos y restricciones.

### Fase 2. Descomposición de servicios del proceso E2E

*(Diapositivas 20 y 28 de 7_SOA)*

Partiendo del proceso E2E, descomponer en **mínimo cuatro capas de servicio** hasta llegar a definir servicios con el tamaño adecuado sobre los cuales se construirá la aplicación.

- Identificar los servicios que se implementarán y los que no, con una **justificación explícita para cada decisión** (tabla de inclusión y exclusión).
- En la **capa anticorrupción (interoperabilidad)**, identificar los servicios externos de todo tipo (otros procesos internos, sistemas legados, proveedores, entidades regulatorias, dispositivos) con los que se debe construir interoperabilidad, indicando qué información se intercambia y en qué dirección.
- Los servicios deben diseñarse en ArchiMate con el elemento **servicio de la capa de negocio** (amarillos), complementados con roles y actores, funciones y elementos pasivos.
- Una vez identificados los servicios, aplicar el concepto **Contract First**, diseñando con objetos pasivos del negocio (*contract*) el contrato de cada servicio.

> A partir de aquí el diseño va en la **capa de aplicaciones**.

### Fase 3. DDD estratégico

*(Diapositivas 7 a 15 de 9_DDD_Diseño)*

1. **Análisis del ecosistema** donde operará el software.
2. **Extracción de dominios (clústers)** a partir del entendimiento detallado del proceso, y su **clasificación** en dominio central, de soporte y genérico, con argumentos. Deben diseñarse en ArchiMate con elementos de la capa de aplicaciones.
3. **Lenguaje ubicuo:** glosario de términos del proceso y su entorno, con definición, dominio al que pertenece y ejemplos. Un mismo término con significados distintos en dos contextos debe quedar explícito. Se recomienda usar elementos pasivos del negocio (*representation*) para representarlos.

### Fase 4. DDD táctico

*(Diapositivas 16 a 24 de 9_DDD_Diseño)*

Para cada dominio, identificar las entidades y conceptos de dominio (entidades y objetos de valor) y diseñar los **agregados**: raíz, límites e invariantes que protege. Cada agregado se acompaña de su **gráfico de comandos y eventos**. El equipo debe argumentar con claridad el alcance de cada entidad y cada agregado.

### Fase 5. Mapa de contextos delimitados

*(Diapositivas 25 a 28 de 9_DDD_Diseño)*

Diseñar el mapa de contextos delimitados (*bounded contexts*) con los patrones de relación entre ellos y con los sistemas externos: por ejemplo cliente-proveedor, conformista, núcleo compartido, servicio abierto, lenguaje publicado o capa anticorrupción. Debe ser coherente con la capa anticorrupción de la Fase 2.

### Fase 6. Canvas del contexto delimitado en ArchiMate (componente investigativo)

*(Diapositivas 34 y 35 de 9_DDD_Diseño)*

Documentar el contexto delimitado principal con un **Bounded Context Canvas** construido en ArchiMate. El canvas debe mostrar en una sola página el diseño del software: propósito, clasificación estratégica, rol del dominio, lenguaje ubicuo, comunicación entrante y saliente, decisiones de negocio y supuestos. **Investigar cómo hacerlo con la herramienta es parte de la evaluación.**

---

## 3. Requisitos del modelo ArchiMate

Todo el diseño se modela en ArchiMate, en **un único modelo con varias vistas**. Las vistas deben usar elementos de las capas de **Estrategia, Motivación, Negocio y Aplicación**, con **relaciones horizontales** (dentro de una capa) y **verticales** (entre capas) que permitan trazar cada componente de aplicación hasta la meta y la capacidad que lo justifican.

| # | Vista mínima | Capa ArchiMate | Qué debe mostrar | Elementos ArchiMate de referencia |
|---|---|---|---|---|
| V1 | Motivación | Motivación | Por qué existe la aplicación y cómo se conecta con los elementos motivacionales del negocio (se preguntan a la IA en la entrevista) | Interesado, impulsor, evaluación, meta, resultado, principio, requerimiento, restricción |
| V2 | Mapa de capacidades | Estrategia | Capacidades del negocio y las impactadas | Capacidad, recurso, curso de acción |
| V3 | Cadena de valor y proceso E2E | Negocio | Dónde se ubica la aplicación | Flujo de valor, proceso, evento y actor de negocio |
| V4 | Descomposición de servicios | Negocio | Las cuatro o más capas y lo que entra o no | Servicio de negocio, proceso, función, agrupación |
| V5 | Dominios | Aplicaciones | Clústers y su clasificación | Agrupación, objeto de negocio |
| V6 | Agregados, comandos y eventos | Aplicaciones | Un diagrama por dominio | Objeto de negocio y de datos, evento de negocio y de aplicación, proceso de aplicación |
| V7 | Lenguaje ubicuo | Negocio (*representation*) | Términos y su significado | Significado, representación, objeto de negocio |
| V8 | Mapa de contextos | Aplicaciones | Contextos y patrones de relación | Componente, interfaz y servicio de aplicación, flujo, servida por |
| V9 | Interoperabilidad y anticorrupción | Aplicaciones | Integraciones con procesos y sistemas externos | Interfaz de aplicación, flujo, servicio, componente externo |
| V10 | Trazabilidad vertical | Transversal | De la meta al componente de aplicación | Relaciones de realización, influencia, servicio y asignación |
| V11 | Bounded Context Canvas | Canvas ArchiMate | El diseño del software en una página | Libre elección del equipo, justificada |

> La columna de elementos es orientativa. El equipo puede usar otros elementos siempre que justifique por qué representan mejor el concepto.

---

## 4. Entregables y sustentación

El equipo entrega un **repositorio en el GitHub del grupo** y sustenta el trabajo en una **reunión presencial que se grabará en Teams** como evidencia.

### Contenido del repositorio

- [ ] `README.md` con el documento principal: cada vista ArchiMate exportada como imagen, seguida de su análisis y la argumentación de las decisiones.
- [ ] Índice al inicio del `.md` con enlaces internos a cada fase y cada vista, para navegar rápidamente.
- [ ] Modelo ArchiMate fuente (por ejemplo el archivo `.archimate`) en la carpeta `/modelo`.
- [ ] Imágenes de las vistas en `/vistas`.
- [ ] Carpeta `/entrevista` con el guion, la instrucción de rol, la conversación exportada y el análisis de hallazgos.
- [ ] Tabla de aportes: qué construyó cada integrante, verificable en el historial de commits.

**Estructura sugerida del índice:**

1. Contexto y entrevista
2. Motivación
3. Mapa de capacidades y proceso E2E
4. Descomposición de servicios
5. Dominios y lenguaje ubicuo
6. Agregados, comandos y eventos
7. Mapa de contextos e interoperabilidad
8. Bounded Context Canvas
9. Decisiones y supuestos

### Sustentación presencial

- **Duración:** 40 minutos por equipo (30 de presentación y 10 de preguntas), grabados en Teams por el equipo.
- Todos los integrantes deben estar presentes y conocer el diseño completo.
- **El docente elige al inicio, por sorteo, quién presenta cada sección.** Así se evidencia que el producto es de todo el equipo y no una suma de partes.
- Las preguntas se dirigen a cualquier integrante sobre cualquier parte, con énfasis en el porqué de las decisiones y en las interoperabilidades.

---

## 5. Rúbrica de evaluación

La nota es **grupal**: todos los integrantes reciben la misma calificación, salvo evidencia de que alguno no participó. Escala de 0,0 a 5,0.

| Criterio | Peso | Sobresaliente (4,5 – 5,0) | Aceptable (3,5 – 4,4) | Básico (3,0 – 3,4) | Insuficiente (< 3,0) |
|---|---|---|---|---|---|
| 1. Entrevista al experto | 10 % | Guion completo por temas, con bloque de interoperabilidad; hallazgos analizados, contrastados y trazados al diseño | Guion completo; análisis de hallazgos parcial | Guion superficial o sin análisis de hallazgos | Sin evidencia, o la IA hizo el diseño |
| 2. Motivación, capacidades y proceso E2E | 10 % | Mapa de capacidades completo; motivación y proceso E2E coherentes con la aplicación | Mapa completo con vínculos débiles a la motivación | Mapa parcial centrado solo en la aplicación | Ausente o incorrecto |
| 3. Descomposición de servicios y anticorrupción | 15 % | Cuatro o más capas bien definidas; cada inclusión y exclusión justificada; servicios externos completos | Cuatro capas; justificación incompleta | Menos de cuatro capas o sin justificación | Ausente |
| 4. Dominios y lenguaje ubicuo | 10 % | Dominios bien extraídos y clasificados con argumentos; glosario preciso y usado en todas las vistas | Clasificación correcta; glosario incompleto | Dominios poco argumentados | Ausente o confuso |
| 5. Entidades, agregados, comandos y eventos | 15 % | Agregados con raíz, límites e invariantes claros; comandos y eventos consistentes por dominio | Agregados correctos con alcance poco argumentado | Entidades sin diseño de agregados | Ausente |
| 6. Mapa de contextos e interoperabilidad | 10 % | Patrones de relación correctos y consistentes con la capa anticorrupción | Mapa correcto con patrones imprecisos | Mapa sin patrones de relación | Ausente |
| 7. Canvas en ArchiMate (investigativo) | 10 % | Canvas completo en una página, bien resuelto en la herramienta y coherente con el modelo | Canvas completo con decisiones de modelado débiles | Canvas incompleto | Ausente |
| 8. Calidad del modelo ArchiMate | 5 % | Cuatro capas, relaciones horizontales y verticales correctas; trazabilidad de meta a componente | Errores menores de notación | Errores de notación que afectan la lectura | No usa ArchiMate |
| 9. Documento en GitHub | 5 % | Índice navegable; cada vista con su análisis y argumentación | Documento completo con argumentación breve | Vistas sin análisis | Ausente |
| 10. Trabajo en equipo y sustentación | 10 % | Cualquier integrante sortea y explica cualquier sección con solvencia; commits distribuidos; producto integrado y coherente | La mayoría responde bien; aportes algo desiguales | Respuestas dependientes de uno o dos integrantes | Integrantes no conocen el trabajo |

### Ajuste individual

Todos los integrantes deben llegar a tiempo a la sustentación. Si alguno llega tarde, se descontará **0,1 por cada minuto de retraso** (por ejemplo, 5 minutos de retraso = 0,5 de descuento).

---

## 6. Temas propuestos (solicitudes de usuario)

Cada equipo recibe uno de estos temas. Las interoperabilidades listadas son un punto de partida: el equipo debe confirmarlas, ampliarlas o descartarlas en la entrevista.

### Tema 1. Mantenimiento preventivo de drones de fumigación agrícola

> *«Somos una empresa que presta servicios de fumigación de cultivos con una flota de drones. Necesitamos un software que apoye nuestro proceso de mantenimiento preventivo para mantener la flota disponible, segura y dentro de la normatividad aeronáutica».*

**Alcance:**

- Registro de horas de vuelo y ciclos de cada dron, a partir de las operaciones realizadas.
- Despiece de cada dron (subsistemas y componentes como motores, hélices, baterías, bombas y boquillas) con las condiciones de mantenimiento preventivo de cada pieza: por horas, ciclos, calendario o estado.
- Repuestos y consumibles requeridos para cada cambio.
- Plan de mantenimiento que programe las intervenciones según esas condiciones.
- Indicadores del proceso, por ejemplo disponibilidad de la flota, cumplimiento del plan, tiempo medio entre fallas y costo de mantenimiento por hora de vuelo.

**Interoperabilidades a explorar:** programación de servicios a clientes, inventario y compras de repuestos, telemetría de los drones, técnicos certificados y autoridad aeronáutica.

### Tema 2. Gobierno de datos en una inmobiliaria

> *«Somos una empresa que vende y arrienda inmuebles. Necesitamos un software que apoye nuestro proceso de gobierno de datos, para conocer, clasificar y describir los datos que manejamos y cumplir la ley de protección de datos personales».*

**Alcance:**

- Identificación y clasificación de los atributos del negocio según la Ley 1581 de 2012 (datos públicos, semiprivados, privados y sensibles).
- Registro de metadatos de negocio por atributo, identificados por los analistas: definición, responsable, políticas de calidad y expresiones regulares de validación.
- Importación de metadatos técnicos (tipo de dato, longitud) desde los diccionarios de los motores de las bases de datos.
- Gestión del diccionario de datos, que integra para cada atributo sus metadatos técnicos y de negocio.
- Gestión del catálogo de términos de negocio, donde cada término se asocia a los atributos que lo describen.

**Interoperabilidades a explorar:** sistemas de ventas, arriendos y CRM como fuentes de datos, oficial de protección de datos, seguridad de la información y reporte ante la Superintendencia de Industria y Comercio.

### Tema 3. Gestión de medicamentos en la farmacia de un hospital

> *«Nuestra farmacia hospitalaria debe atender la demanda programada (cirugías, procedimientos y hospitalización) y la no programada (urgencias). Necesitamos un software que apoye la gestión de medicamentos para estas operaciones y reduzca las pérdidas por vencimiento».*

**Alcance:**

- Control de inventarios de medicamentos por lote y fecha de vencimiento.
- Alertas de medicamentos próximos a vencer, priorizando su uso en la demanda próxima para disminuir lo que se destruye.
- Predicción de la demanda a partir de la programación y de modelos de análisis histórico.
- Generación de requerimientos de compra de medicamentos.

**Interoperabilidades a explorar:** programación quirúrgica, historia clínica y prescripción, urgencias, compras y proveedores, contabilidad y entes de vigilancia de medicamentos.

### Tema 4. Recaudo del sistema integrado de transporte de una ciudad mediana

> *«Operamos el sistema integrado de transporte de la ciudad, con tres modos: buses alimentadores (satélites), buses articulados en carriles exclusivos y metro. Necesitamos un software que apoye el proceso de recaudo, que está ligado directamente a la operación, y que permita a residentes y visitantes pagar su viaje de forma ágil y con la tarifa que les corresponde».*

**Alcance:**

- Recaudo en los tres modos de transporte con tres medios de pago: la tarjeta propia de la ciudad, **VAMOS**, y las tarjetas débito y crédito, estas últimas pensadas para que los visitantes puedan usar el sistema sin adquirir una tarjeta.
- Gestión de la tarjeta VAMOS: emisión, recargas, consulta de saldo y bloqueo por pérdida.
- Gestión de tarifas y perfiles: tarifa plena y tarifas especiales para estudiantes de colegio, universitarios, adultos mayores y personas con movilidad reducida, incluida la validación de que el usuario cumple las condiciones de su perfil.
- Aplicación de la tarifa correcta en cada validación, considerando la integración entre modos (por ejemplo, transbordos).
- Consolidación del recaudo por modo, ruta y operador, como base para la distribución de ingresos y la conciliación.

**Fuera del alcance:** los validadores, lectores y demás dispositivos, y todo lo relacionado con IoT. El software recibe las transacciones que estos generan, pero no los gestiona.

**Interoperabilidades a explorar:** experiencia y servicio al cliente (peticiones, quejas, reclamos y devoluciones), financiero (registro contable de recaudos e ingresos), planeación de rutas y de la operación, redes de pago y entidades financieras, entidades que certifican los perfiles especiales (instituciones educativas, por ejemplo) y la autoridad de transporte de la ciudad. El análisis de capacidades empresariales permitirá descubrir otras.

### Tema 5. Gestión del arrendamiento de yates de terceros

> *«Somos una empresa que arrienda yates que pertenecen a terceros y que están almacenados en diferentes marinas. Necesitamos un software que apoye toda la gestión del arrendamiento, desde la reserva hasta la devolución, con control del riesgo que implica entregar bienes que no son nuestros».*

**Alcance:**

- Inventario de yates disponibles por marina, para que el cliente consulte los de la marina más cercana. Los propietarios deciden si su yate está habilitado o bloqueado para arriendo.
- Reserva y notificación al propietario del yate y a la marina, para que realicen la preparación y la entrega.
- Pagos asociados al arriendo: valor del alquiler, depósito para daños, seguro y combustible consumido.
- Seguimiento en tiempo real de la ubicación y el nivel de combustible del yate, visible para la marina y para el cliente.
- Cierre del arriendo: devolución y recepción del yate, inspección y ajustes de dinero (cobro de combustible, devolución o retención del depósito).
- Sistema de calificaciones mutuas entre clientes y propietarios.

**Interoperabilidades a explorar:** CRM para la gestión de clientes en los procesos comerciales, gestión de contratos con propietarios y clientes (manejar bienes de terceros implica una gran responsabilidad), aseguradoras, empresas que evalúan el nivel de riesgo de cada posible arriendo, marinas, pasarelas de pago y la fuente de datos de ubicación y combustible del yate. El análisis de capacidades empresariales permitirá descubrir otras.
