# Análisis de hallazgos de la entrevista

**Caso:** Gestión de medicamentos en la farmacia de un hospital
**Entrevistado:** Jorge Iván Salazar, director del servicio farmacéutico (simulado con IA)

Este documento resume lo que dijo el experto, qué tomamos para el diseño, qué recortamos y qué dejamos por fuera. El criterio fue quedarnos con lo que pide el enunciado del tema (inventario por lote, alertas de vencimiento, predicción de demanda y requerimientos de compra) y no complicar el diseño con todo lo demás que hace una farmacia.

---

## 1. Lo esencial en pocas líneas

- **El problema:** el hospital pierde unos $310 millones al año por medicamentos vencidos (1,1 % de lo que compra), y al mismo tiempo se le agotan medicamentos y hace compras urgentes.
- **La causa de fondo:** nadie sabe con certeza qué lote hay, dónde está y cuándo vence. El lote se anota al recibir, pero cuando el medicamento sale no se registra de qué lote salió.
- **Por qué se vence:** se compra de más porque se pide con promedios (40 %), hay producto quieto en los servicios (25 %), llega con poca vida útil (20 %) y cambian los protocolos (15 %).
- **Lo que más pide el experto:** saber en cualquier momento qué lote hay, cuánto, dónde y con qué fecha de vencimiento. Según él, todo lo demás depende de eso.
- **Las condiciones que pone:** el sistema recomienda y una persona decide. No hay personal nuevo y no se pueden modificar los sistemas que ya existen.

---

## 2. Hallazgos que aceptamos

| Hallazgo | Cómo lo usamos | Dónde |
|---|---|---|
| Meta: garantizar la disponibilidad de medicamentos y reducir los vencidos a menos del 0,4 % de las compras en 12 meses | Es la meta principal del modelo | V1, V10 |
| Impulsores: la pérdida pasó de $190 M a $310 M en dos años, la Secretaría de Salud encontró vencidos en un carro de paro, hay presión de caja por la cartera con las EPS y la gerencia busca la acreditación | Son los impulsores de la vista de Motivación | V1 |
| Interesados principales: farmacia, gerencia y financiera, médicos, enfermería y compras | Son los interesados de V1. Al resto los dejamos por fuera para no recargar la vista | V1 |
| Principios: la seguridad del paciente va primero, la disponibilidad pesa más que el ahorro, sale primero lo que vence primero, el sistema recomienda y la persona decide | Son los principios de V1 y justifican las reglas | V1, V6 |
| Resultados medibles: vencidos ≤ 0,4 %, 100 % de las salidas con lote, ≥ 80 % de lo que entra en alerta se usa antes de vencer, máximo 6 compras urgentes al mes | Son los resultados de V1. Escogimos solo los que se relacionan directamente con el alcance | V1 |
| Macroprocesos del hospital: estratégicos, misionales, de apoyo diagnóstico y terapéutico (ahí está la farmacia) y de apoyo administrativo | Son la base del mapa de capacidades completo | V2 |
| El proceso tiene dos ciclos: abastecimiento (planear, pedir, recibir, almacenar) y atención (distribuir, dispensar, devolver). La vigilancia de vencimientos va en paralelo a los dos | Es la estructura del proceso E2E | V3 |
| Eventos que disparan el proceso: programación quirúrgica, existencia bajo el mínimo, prescripción, lote que entra en alerta, cirugía cancelada y retiro de un lote por el INVIMA | Son los eventos del E2E y del diagrama de comandos y eventos | V3, V6 |
| Estados del lote: cuarentena, disponible, apartado, retenido, vencido y dado de baja | Son el ciclo de vida del lote | V6 |
| "Próximo a vencer" no es un estado: el lote sigue disponible y gana prioridad de salida | Lo modelamos como una alerta, no como un estado | V6, V7 |
| Reglas que nunca se rompen: nada sale sin recepción técnica, nunca sale un vencido ni un retenido, las existencias nunca son negativas | Son los invariantes del agregado Lote | V6 |
| Alertas a 6, 3 y 1 mes del vencimiento. La de 6 meses existe porque los proveedores piden ese aviso para aceptar un canje | Son las reglas fijas de las alertas | V4, V6 |
| Acciones ante un lote próximo a vencer: priorizarlo, trasladarlo, apartarlo para una cirugía, frenar la compra o pedir canje | Son los servicios del dominio de vencimientos | V4 |
| Existen unos 180 paquetes estándar de medicamentos por cirugía y 5 años de histórico de urgencias | Son los insumos de la predicción: lo programado sale de los paquetes y lo no programado del histórico | V4, V5 |
| Cálculo actual del requerimiento: consumo × tiempo a cubrir − existencias − lo que viene en camino | Lo mantenemos y le agregamos descontar lo que se va a vencer antes de usarse | V4, V6 |
| Frontera con compras: farmacia dice qué y cuánto, compras dice a quién y a qué precio | Comprar queda fuera del alcance | V4, V9 |
| Los sistemas hospitalario y contable no se pueden modificar, y cada sistema identifica los medicamentos con un código distinto | Justifica la capa anticorrupción | V8, V9 |
| Palabras confusas que pidió distinguir: dispensar, despachar y administrar; requerimiento y orden de compra; "consumo", "pedido" y "devolución" | Son la base del lenguaje ubicuo | V7 |

---

## 3. Hallazgos que aceptamos pero simplificamos

| Lo que dijo el experto | Cómo lo simplificamos y por qué |
|---|---|
| Hay farmacia central, satélites, stocks de piso, carros de paro y farmacia ambulatoria | La tratamos como **una sola farmacia con varias ubicaciones**. El lote se controla por ubicación, sin modelar cada punto como algo aparte. |
| Las alertas deberían depender de la rotación de cada medicamento | Usamos las alertas fijas de 6, 3 y 1 mes que ya aplica el hospital. Son suficientes para la meta. |
| La predicción debería considerar estacionalidad, censo de camas, tipo de anestesia y preferencias del cirujano | Lo programado se calcula con los paquetes multiplicados por las cirugías de la agenda. Lo no programado, con el promedio histórico de urgencias. Cumple el enunciado ("programación y análisis histórico") sin meternos en modelos complejos. |
| Hay varios niveles de aprobación según el monto, el tipo de medicamento y el horario | Dejamos una sola regla: el químico farmacéutico aprueba la predicción y el requerimiento, y quien elabora no aprueba. |
| La dispensación ya está en el sistema hospitalario, pero sale sin lote | La dispensación sigue en el sistema hospitalario. Nuestro software recibe cada salida y la descuenta del lote que vence primero. |
| Ante un retiro del INVIMA se deben ubicar los lotes, rastrear a los pacientes, devolver al proveedor y responder a la autoridad | Solo tomamos **retener el lote en todas sus ubicaciones** y ver dónde está. El resto lo sigue haciendo farmacovigilancia. |
| Canje con el proveedor: la farmacia lo solicita, compras negocia y contabilidad registra la nota crédito | Solo modelamos la **solicitud de canje**. La negociación y la nota crédito quedan fuera. |
| La baja exige acta, aprobación, entrega al gestor de residuos y certificado de destrucción | Modelamos la baja con acta aprobada. La destrucción queda fuera. |

---

## 4. Hallazgos que descartamos

| Hallazgo | Por qué lo dejamos fuera |
|---|---|
| Libro de control especial, informe al Fondo Rotatorio y compras de monopolio del Estado | Es un proceso regulado con su propio registro. No hace parte del alcance del enunciado y agregaría un dominio completo. |
| Monitoreo de temperatura y planillas de cadena de frío | Es control de calidad del almacenamiento, no gestión de vencimientos. Una falla de nevera solo nos interesa como causa para retener un lote. |
| Gestión de residuos, gestor ambiental y certificados de destrucción | Ocurre después de la baja y la maneja gestión ambiental. |
| Reportes de farmacovigilancia al INVIMA | Los hace calidad en su propio aplicativo. |
| Reporte de precios al SISMED | Lo arma compras. |
| Facturación al paciente y glosas | No cambia la gestión de vencimientos ni la de demanda. |
| Préstamos con otras clínicas | Es informal y ocasional, sin impacto real en la meta. |
| Admisiones y censo de camas | Solo serviría para afinar la predicción de hospitalización. Lo dejamos como mejora futura. |
| Reempaque en dosis unitaria | Es una operación técnica de la farmacia que no cambia el control por lote. |
| Operación sin sistema durante caídas (contingencia en papel) | Es un requisito de infraestructura. Lo dejamos como supuesto, no como parte del diseño. |
| Identificador único de medicamentos (IUM) del Ministerio | El hospital no lo usa y no se sabe en qué va. |
| Conflictos con compras por descuentos por volumen | Es un tema de gestión entre áreas. El software solo genera el requerimiento técnico. |

---

## 5. Interoperabilidades: cuáles confirmamos y cuáles descartamos

El enunciado propone seis interoperabilidades. Así quedaron después de la entrevista:

| Interoperabilidad del enunciado | Decisión | Qué se intercambia |
|---|---|---|
| Programación quirúrgica | **Confirmada** | Entra a nuestro software: cirugías programadas, canceladas y reprogramadas |
| Historia clínica y prescripción | **Confirmada, a través del sistema hospitalario** | Entran: las salidas dispensadas y las devoluciones |
| Urgencias | **Confirmada, dentro del sistema hospitalario** | Entran: los consumos de urgencias, que alimentan el histórico. No es un sistema aparte |
| Compras y proveedores | **Confirmada y separada en dos** | Compras: sale el requerimiento aprobado y entra lo que viene en camino. Proveedores: entran el lote y el vencimiento en la recepción; sale la solicitud de canje |
| Contabilidad | **Confirmada** | Salen las bajas por vencimiento |
| Entes de vigilancia | **Solo el INVIMA** | Entran las alertas sanitarias y los retiros de lotes. El Fondo Rotatorio y el SISMED quedan fuera (ver sección 4) |

---

## 6. Contraste con otras fuentes

- **Enunciado del tema:** comparamos cada hallazgo contra los cuatro puntos del alcance. Lo que no apoya ninguno quedó en la sección 4.
- **Normatividad:** el experto pidió verificar varias normas. Revisamos en fuentes oficiales las que usamos en el diseño (consulta del 15 de octubre de 2026):
  - [x] **Resolución 1403 de 2007** (Modelo de Gestión del Servicio Farmacéutico y Manual de Condiciones Esenciales y Procedimientos). **Confirmada y vigente.** Respalda el principio FEFO: el almacenamiento debe garantizar "que el lote más próximo a vencerse sea lo primero en dispensarse" (Manual, Título I, Capítulo II, numeral 1.1, literal i). También exige un área para los medicamentos que se deben destruir por vencimiento o deterioro. Fuente: [normograma del INVIMA](https://normograma.invima.gov.co/compilacion/docs/resolucion_minproteccion_1403_2007.htm).
  - [x] **Decreto 780 de 2016.** **Confirmado:** compila el Decreto 2200 de 2005 a partir del artículo 2.5.3.10.1, y la recepción y el almacenamiento están entre los procesos del servicio farmacéutico (artículo 2.5.3.10.14). Fuente: [Decreto 2200 de 2005 con notas de compilación (normograma del INVIMA)](https://normograma.invima.gov.co/compilacion/docs/decreto_2200_2005.htm).
  - [x] **Resolución 1732 de 2026.** **El dato del experto no es correcto.** Se firmó el 5 de agosto de 2026 para reemplazar la Resolución 3100 de 2019, pero la **Resolución 2080 del 8 de septiembre de 2026** la revocó en su integridad porque no se publicó en el Diario Oficial. La norma de habilitación vigente sigue siendo la **Resolución 3100 de 2019** con sus modificaciones. La revocatoria es anterior a la entrevista, así que la IA dio un dato desactualizado. Fuentes: [Resolución 2080 de 2026 (Régimen Legal de Bogotá)](https://www.alcaldiabogota.gov.co/sisjur/normas/Norma1.jsp?dt=S&i=194230) y [Consultorsalud](https://consultorsalud.com/minsalud-revoca-la-resolucion-1732-de-2026-asi-quedan-las-reglas-de-habilitacion-para-prestadores-de-servicios-de-salud/).

  **Cómo influyó en el diseño:** FEFO deja de ser solo una práctica del hospital y queda respaldado por la norma (principio de la V1 y servicio de dominio *Elegir el lote que vence primero* de la V6.1). La revocatoria de la 1732 no cambia el diseño: la habilitación no está en el alcance y solo era el contexto del hallazgo de la Secretaría de Salud.

  Las normas que no usamos (control especial, residuos, farmacovigilancia) no se verificaron porque quedaron fuera del alcance.
- **Datos que el experto marcó como estimados:** las cirugías aplazadas, el presupuesto de $450 M y el 93 % de entregas completas. Los usamos solo como referencia en la vista de Motivación, sin tratarlos como datos confirmados.
- **Inconsistencia detectada:** al principio entendimos que era una sola farmacia, pero el experto describió una central con satélites. Lo resolvimos como una farmacia con varias ubicaciones (ver sección 3).

---

## 7. Supuestos del diseño

1. Los sistemas hospitalario y contable pueden entregar y recibir información de alguna forma (reportes, archivos o el paso nocturno). Cómo se hace técnicamente lo define el área de sistemas.
2. El lote y la fecha de vencimiento se registran en la recepción técnica, aunque el código de barras del empaque no los traiga.
3. Los paquetes por cirugía se cargan una vez desde el Excel actual y los mantiene farmacia.
4. La contingencia cuando el sistema se cae la sigue manejando el hospital como lo hace hoy.
