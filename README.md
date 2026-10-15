# Reto_Arquitectura_ArchiMate

## 8. Bounded Context Canvas

**Fase 6 · Vista `V11 - Bounded Context Canvas`**

![V11 - Bounded Context Canvas](vistas/V11_bounded_context_canvas.png)

### 8.1 Qué contexto documentamos y por qué

El canvas documenta **Control de lotes y vencimientos**. Es el contexto delimitado principal porque es el dominio central (V5): ataca la causa raíz de la pérdida por vencidos (las salidas no registran de qué lote salen) y es el único dueño del agregado Lote. Planeación de la demanda depende de él (cliente-proveedor, V8), y los dos genéricos, Catálogo y Usuarios, solo le prestan servicios.

### 8.2 Cómo se hizo en Archi (componente investigativo)

Evaluamos tres opciones:

| Opción | Cómo | A favor | En contra | Decisión |
|---|---|---|---|---|
| **A. Vista ArchiMate con grupos como celdas del canvas** | Un *Grupo* visual por sección, ubicado como el canvas de DDD Crew; dentro, elementos del modelo | Coherente con el modelo, reutiliza sin duplicar y todo es ArchiMate | Cuesta que quepa en una página | **Elegida** |
| **B. Canvas propio de Archi** (*File → New → Canvas*) | Bloques de texto e imagen con el diseño exacto de la plantilla | Se ve igual a la plantilla oficial | Sus bloques no son elementos ArchiMate y no se conectan al modelo | Descartada |
| **C. Solo notas en una vista** | Texto con la forma del canvas | Rápido | Es un documento pegado; modelado débil | Descartada |

Cómo se resolvió la opción A:

1. **Se reutilizan los elementos del modelo.** El componente, los requerimientos, los principios, los comandos y eventos de V6.1 y V6.2, los servicios A de la capa anticorrupción, los 14 términos del lenguaje ubicuo y los resultados de V1 son los mismos elementos del modelo, no copias. Si cambia un nombre, cambia también en el canvas.
2. **El canvas lee el modelo con etiquetas dinámicas.** Se agregaron al componente las propiedades `proposito`, `modelo_negocio`, `evolucion` y `rol_dominio` (ya tenía `tipo_dominio`). Las secciones 2, 3 y 4 muestran el mismo componente con una *label expression* (`${property:proposito}`, `${property:modelo_negocio}`…). El texto vive en el modelo y no en la vista.
3. **Especializaciones** para lo que ArchiMate no trae. Son el mecanismo de extensión de ArchiMate 3.2; en Archi se gestionan en *Tools → Specializations Manager*. Las nuevas son «Rol de dominio», «Decisión de negocio», «Supuesto» y «Pregunta abierta». Es el mismo mecanismo que usamos en la Fase 4 para *Raíz de agregado*, *Entidad* y *Objeto de valor*.
4. **Trazabilidad.** Cada elemento nuevo queda relacionado con el componente. En Archi, *Properties → Analysis → Model Relations* del componente muestra todo su canvas.

### 8.3 Sección por sección

| # | Sección | Cómo se modeló | Por qué |
|---|---|---|---|
| 1 | Nombre | Componente *Control de lotes y vencimientos* (V5) | El contexto se implementa como componente, igual que en V8 |
| 2 | Propósito | El componente con la etiqueta `${property:proposito}`, que **realiza** R1 y R2 de V1 | Une el canvas con la motivación; R1 y R2 ya llevan a los resultados y a la meta (V1 y V10) |
| 3 | Clasificación estratégica | *Dominio central* (agregación, V5), el componente con dominio, modelo de negocio y evolución, y dos elementos **Valor** | *Modelo de negocio:* reducción de costos por vencidos y cumplimiento de la norma sanitaria; se modelan como **Valor** porque ArchiMate define el valor como la utilidad que algo aporta. *Evolución:* desarrollo a la medida, porque ningún producto lleva el lote con estas reglas y se integra con sistemas que no se pueden modificar |
| 4 | Rol del dominio | Dos **funciones de aplicación** «Rol de dominio» **asignadas** al componente: *Execution* (principal) y *Enforcer* | La función agrupa comportamiento, que es lo que describe un rol de dominio. No es *Approver*, porque el sistema recomienda y la persona decide. Las alertas y los indicadores son parte de la ejecución, no un rol *Analysis* aparte |
| 5 | Comunicación entrante | Colaborador → mensaje (relación de **disparo**). Comandos del regente y del químico; eventos de los sistemas externos que llegan por A1, A2, A4 y A6 (anticorrupción); consultas a Catálogo (servicio abierto + lenguaje publicado), Usuarios (conformista) y código de barras E7 (conformista + lenguaje publicado); eventos de tiempo | Son los mensajes de V6.1 y V6.2 y los patrones de V8 y V9 |
| 6 | Comunicación saliente | Mensaje → intermediario → destino. *Lote dado de baja* → A5 → contabilidad; *Canje solicitado* → A4 → proveedores; *Existencia bajo el mínimo detectada* → Planeación; *Consultar existencias y lo que vencerá* **sirve a** Planeación (cliente-proveedor, somos el proveedor); *Lote entró en alerta* → químico y regente | Coherente con la capa anticorrupción de V9 y el mapa de V8 |
| 7 | Lenguaje ubicuo | La agrupación *Lenguaje de Control de lotes y vencimientos* con sus 14 términos (V7) y una nota con las precisiones de V7.1 | Definición y ejemplo en la documentación de cada término |
| 8 | Decisiones de negocio | Doce **restricciones** «Decisión de negocio» que el componente **realiza**; las que vienen de un principio de V1 lo **realizan** | Una restricción limita cómo puede comportarse el contexto. Son las 8 invariantes del Lote más las reglas de la alerta, el canje y la baja, y la del FEFO |
| 9 | Supuestos | Cinco **evaluaciones** «Supuesto» más las dos restricciones de partida de V1 | Un supuesto es un juicio sobre la situación que se da por cierto sin haberlo comprobado |
| 10 | Métricas de verificación | Los resultados de V1; el componente los **influye (+)** | Se consultan con el servicio S20 |
| 11 | Preguntas abiertas | Cuatro **evaluaciones** «Pregunta abierta» | Lo que falta confirmar antes de construir |

### 8.4 Decisiones de negocio

1. Nada sale sin recepción técnica aprobada, y quien la registra no la aprueba.
2. Nunca sale un lote vencido ni retenido. *(Principio: la seguridad del paciente va primero.)*
3. Vencido y retenido por el INVIMA aplican al lote completo, en todas sus ubicaciones. *(Principio: la seguridad del paciente va primero.)*
4. Lo apartado solo sale para la cirugía para la que se apartó.
5. Las existencias nunca son negativas, y lo recibido cuadra con lo que hay y lo que salió.
6. Todo movimiento lleva ubicación, responsable y hora.
7. Vida útil mínima al recibir: 12 meses, o el 75 % si es de vida corta.
8. La suma de los lotes cuadra cada día con el saldo oficial del sistema hospitalario.
9. Toda salida se descuenta del lote que vence primero (FEFO). *(Principio: sale primero lo que vence primero.)*
10. La alerta a 6, 3 y 1 mes no cambia el estado del lote; la acción la decide una persona. *(Principio: el sistema recomienda y la persona decide.)*
11. El canje solo se pide para un lote en alerta, y una solicitud respondida no cambia.
12. Sin acta de baja aprobada no hay baja, y quien elabora el acta no la aprueba.

### 8.5 Supuestos y preguntas abiertas

**Supuestos**

- Los sistemas hospitalario y contable entregan y reciben información por reportes, archivos o el paso nocturno.
- El lote y el vencimiento se registran en la recepción técnica, aunque el código de barras no los traiga.
- El saldo oficial por referencia sigue en el sistema hospitalario, y este contexto concilia cada día.
- Central, satélites, stock de piso y carros de paro son ubicaciones de una sola farmacia.
- La contingencia cuando un sistema se cae la sigue manejando el hospital como hoy.

**Preguntas abiertas**

- ¿Qué exigen hoy la Resolución 1403 de 2007, el Decreto 780 de 2016 y la Resolución 1732 de 2026?
- ¿Cuándo adoptará el hospital el identificador único de medicamentos (IUM) y cómo cambia la homologación?
- ¿Basta con el paso nocturno o hay que conocer las salidas durante el día?
- ¿Los proveedores pueden enviar lote y vencimiento en la factura electrónica?

### 8.6 Qué se agregó al modelo en la Fase 6

- **1 vista:** V11.
- **25 elementos nuevos:** 2 roles de dominio, 2 valores, 12 decisiones de negocio, 5 supuestos y 4 preguntas abiertas.
- **57 relaciones nuevas.** Todas se validaron contra las reglas de ArchiMate que trae Archi 5.10.
- **4 especializaciones nuevas.**
- **4 propiedades nuevas en el componente:** `proposito`, `modelo_negocio`, `evolucion` y `rol_dominio`.

No se modificó ni se borró nada de las fases anteriores.
