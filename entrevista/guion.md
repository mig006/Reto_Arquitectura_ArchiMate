# Guion de entrevista — Experto del negocio

**Caso:** Gestión de medicamentos en la farmacia de un hospital
**Entrevistado (simulado con IA):** Químico farmacéutico, director del servicio farmacéutico del hospital
**Objetivo:** Obtener el conocimiento del negocio necesario para el diseño arquitectónico: motivación, capacidades, proceso E2E, servicios, dominios, agregados, lenguaje ubicuo e interoperabilidades.

**Tipos de pregunta:**

- **[A] Abierta:** explora el tema sin sesgar la respuesta.
- **[P] Profundización:** baja al detalle de una respuesta anterior.
- **[V] Validación:** confirma o corrige lo que el equipo ya entendió.

Las preguntas de profundización listadas son las mínimas; durante la entrevista se agregan repreguntas según las respuestas.

> **Regla de uso:** las preguntas buscan conocimiento del negocio. No se pide al experto que proponga capacidades, dominios, agregados, contextos ni vistas ArchiMate; esos productos los construye el equipo.

---

## 1. Contexto de la organización y motivación

*Alimenta: mapa de capacidades (V2), cadena de valor (V3) y Motivación (V1).*

1.1. **[A]** ¿Qué tipo de hospital es, cuáles son sus grandes áreas o macroprocesos y dónde se ubica la farmacia dentro de su cadena de valor?

1.2. **[A]** ¿Quiénes son los interesados en que la gestión de medicamentos funcione bien y qué le preocupa a cada uno? *(interesados)*

1.3. **[A]** ¿Qué está llevando al hospital a buscar este software ahora y cuál es la situación actual frente a vencimientos y desabastecimientos, con cifras o casos concretos si los hay? *(impulsores y evaluaciones)*

1.4. **[P]** ¿Qué metas tienen frente a este proceso y con qué resultados medibles sabrían que se cumplieron? *(metas y resultados)*

1.5. **[A]** ¿Qué principios no se negocian, qué debe cumplir obligatoriamente la solución y qué restricciones existen (normativas, presupuestales, tecnológicas)? *(principios, requerimientos y restricciones)*

1.6. **[V]** Si la meta principal es reducir pérdidas por vencimiento sin afectar la disponibilidad para la atención, ¿lo comparte o lo plantearía distinto?

## 2. Indicadores y normatividad

*Alimenta: resultados y restricciones de Motivación (V1), reglas de V6 e interoperabilidad con entes de vigilancia (V9).*

2.1. **[A]** ¿Qué indicadores miden hoy sobre la gestión de medicamentos (porcentaje o valor de vencidos, rotación, desabastecimientos, nivel de servicio) y cuáles les exige la gerencia o los entes de control?

2.2. **[A]** ¿Qué normas regulan el manejo de medicamentos en el hospital (recepción técnica, almacenamiento, control de vencimientos, destrucción, control especial) y qué obligaciones de registro o reporte imponen?

## 3. Proceso E2E y actores

*Alimenta: proceso E2E (V3) y descomposición de servicios (V4).*

3.1. **[A]** Descríbanos el recorrido completo de un medicamento: desde que se decide comprarlo hasta que se administra, se devuelve o se destruye. ¿Quién participa en cada etapa?

3.2. **[P]** ¿Qué eventos inician el proceso o cambian su curso (cirugía programada, ingreso a urgencias, stock bajo, lote próximo a vencer, alerta sanitaria)?

3.3. **[P]** ¿Qué etapas soporta hoy otro sistema o área, y cuáles no tienen ningún soporte?

3.4. **[V]** Lo resumimos así: planear demanda → solicitar compra → recibir → almacenar → dispensar → devolver o dar de baja. ¿Falta o sobra algún paso?

## 4. Reglas de negocio y excepciones

*Alimenta: agregados, invariantes, comandos y eventos (V6).*

4.1. **[A]** ¿Qué información describe un lote y por qué estados pasa desde que llega hasta que sale (cuarentena, disponible, reservado, próximo a vencer, vencido, dado de baja)?

4.2. **[P]** ¿Qué acción hace pasar un lote de un estado a otro, quién la ejecuta y cuáles requieren aprobación?

4.3. **[A]** ¿Qué reglas se deben cumplir siempre sobre el inventario (vida útil mínima al recibir, FEFO, bloqueo de vencidos, control especial, cadena de frío)?

4.4. **[A]** ¿Qué pasa en casos excepcionales: urgencia sin inventario, cirugía cancelada con medicamentos reservados, retiro de un lote por el INVIMA?

## 5. Alertas, demanda y compras

*Alimenta: servicios y alcance (V4), dominios (V5).*

5.1. **[A]** ¿Cómo se detecta hoy un lote próximo a vencer, con cuánta anticipación y qué acciones se pueden tomar con él (priorizarlo, reservarlo para una cirugía, devolverlo o canjearlo con el proveedor)?

5.2. **[A]** ¿Cómo estiman hoy la demanda? ¿Cada cirugía tiene un listado estándar de medicamentos y qué histórico existe para urgencias?

5.3. **[P]** ¿Cómo se genera un requerimiento de compra, con qué criterios de cantidad, y dónde termina la responsabilidad de la farmacia y empieza la de compras?

5.4. **[V]** ¿Quién debe recibir las alertas y quién debe aprobar la predicción y el requerimiento de compra antes de que salgan?

## 6. Interoperabilidad con sistemas y procesos

*Alimenta: capa anticorrupción (Fase 2), mapa de contextos (V8) e interoperabilidad (V9).*

6.1. **[A]** ¿Con qué áreas, sistemas y entidades externas intercambia información la farmacia? ¿Algún sistema es antiguo y no se puede modificar?

6.2. **[P]** Para cada uno, ¿qué información se intercambia, en qué dirección, con qué frecuencia y por qué medio? En particular: programación quirúrgica, historia clínica y prescripción, urgencias, compras y proveedores, contabilidad y entes de vigilancia.

6.3. **[P]** ¿Todos los sistemas identifican un medicamento con el mismo código (interno, CUM, del proveedor)? ¿Qué problemas tienen hoy esos intercambios?

6.4. **[V]** ¿Falta o sobra algún intercambio en la lista anterior?

## 7. Lenguaje del negocio

*Alimenta: lenguaje ubicuo (V7).*

7.1. **[A]** ¿Qué términos usan a diario en la farmacia que alguien externo no entendería? ¿Qué diferencia hay entre dispensar, despachar y administrar, y entre lote, referencia y presentación?

7.2. **[P]** ¿Hay palabras que signifiquen algo distinto para la farmacia y para otras áreas, como "pedido" para compras o "consumo" para contabilidad?

## 8. Cierre

8.1. **[V]** Le resumimos lo que entendimos del proceso, sus reglas y sus intercambios. ¿Qué corregiría o agregaría?

8.2. **[A]** Si el software solo pudiera resolver un problema, ¿cuál debería ser?
