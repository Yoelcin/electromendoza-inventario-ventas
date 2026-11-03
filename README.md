# ⚡ ElectroMendoza S.A. - Módulo Integrado de Gestión de Inventario y Ventas (B2B/B2C)

**Asignatura:** Organización Empresarial  
**Carrera:** Tecnicatura Universitaria en Programación a Distancia (TUPAD) - Universidad Tecnológica Nacional (UTN)  
**Docente Titular:** Prof. Gabriela Martínez | **Docente Tutor:** Prof. Carolina Bruno  
**Autor / Desarrollador:** Yoel Samir Axel Ramirez  

---

## 📌 Descripción General del Proyecto
**ElectroMendoza S.A.** es una organización dedicada a la comercialización y distribución mayorista (B2B) y minorista (B2C) de materiales eléctricos, componentes de automatización e insumos electromecánicos en la región de Cuyo.

El presente repositorio aloja la arquitectura documental, el modelo de datos relacional y el modelado de procesos de negocio (BPMN 2.0) del **Módulo Integrado de Gestión de Inventario y Ventas**, diseñado para:
- Soportar un catálogo inicial de al menos **1.500 SKU** (conductores unipolares, contactores, protecciones, módulos Jeluz/Kalop e iluminación LED) con validación de certificación **IRAM**.
- Garantizar tiempos de consulta de stock inferiores a **2 segundos**.
- Automatizar el descuento de inventario y disparar alertas críticas de reposición a la **Sección Compras**.
- Integrar la validación fiscal ante **AFIP/ATM** con la preparación logística (*Picking* y *Packing*).

---

## 🗂️ Estructura del Repositorio
- `/docs`: Entregas progresivas del Trabajo Práctico Integrador (Unidades 1 a 5).
- `/diagrams`: Esquema del Sistema Abierto (TGS), Organigrama Piramidal Normalizado y Flujo BPMN 2.0.
- `/sql`: Estructura preliminar del Diccionario de Datos (`PRODUCTO`, `CLIENTE`, `PEDIDO_VENTA`, `DETALLE_PEDIDO`).

---

## 📋 Historias de Usuario (Backlog Ágil - GitHub Projects)
1. **HU-01 (Ventas B2B/B2C):** *Como* vendedor de las secciones Mayorista y Minorista, *quiero* consultar en tiempo real el stock disponible y el precio por código SKU, *para* confirmar presupuestos en el acto sin depender de consultas telefónicas al depósito. *(Estado: Listo / Done)*
2. **HU-02 (Almacén y Logística):** *Como* encargado de Almacén, *quiero* que el sistema descuente automáticamente las unidades vendidas y emita una alerta cuando un insumo llegue al stock mínimo, *para* evitar quiebres de stock en materiales de alta rotación. *(Estado: En Progreso / In Progress)*
3. **HU-03 (Gerencia General):** *Como* titular de la Gerencia General, *quiero* visualizar un tablero de control con el monto total facturado y los artículos eléctricos más vendidos por período, *para* tomar decisiones fundadas sobre compras por volumen y políticas de precios. *(Estado: Por Hacer / To Do)*

---

## 📊 Indicadores de Control Técnico (KPIs)
| Indicador (KPI) | Estándar / Meta | Frecuencia | Acción Correctiva |
| :--- | :--- | :--- | :--- |
| **Latencia de Consulta SKU** | `<= 2,0 seg` | Semanal | Optimización de índices en `id_producto` y `codigo_sku`. |
| **Exactitud de Inventario (ERO)** | `>= 98,5%` | Mensual | Auditoría de bandera `stock_descontado` y arqueo físico. |
| **Tasa de Quiebre de Stock** | `< 3,0%` | Quincenal | Ajuste del umbral `stock_minimo_alerta` en tabla `PRODUCTO`. |
| **Ciclo Facturación-Despacho** | `<= 15 min` (B2C) | Semanal | Balanceo operativo en sector de Picking y Packing. |

---

## 🤖 Declaración de Uso Crítico de Herramientas de IA
En cumplimiento con las pautas de transparencia académica de la cátedra, se declara que el caso de estudio, la definición de reglas de negocio del sector eléctrico y la estructura organizacional fueron desarrollados por el alumno. Se empleó Inteligencia Artificial Generativa como soporte puntual bajo auditoría crítica humana:

1. **Prompt utilizado para estructuración de datos:** *"Generar plantilla tabular para diccionario de datos relacional de 4 entidades aplicado a inventario y facturación"*.
   - **Auditoría y corrección humana:** El modelo sugirió inicialmente tipos de datos `Float` para importes monetarios y carecía de controles propios de seguridad eléctrica e impuestos locales. Se reemplazó `Float` por `Decimal(10,2)` para evitar errores de punto flotante en facturación, y se añadieron manualmente los campos `certificacion_iram (Boolean)`, `cuit_dni`, `condicion_iva` y `stock_descontado`.
2. **Prompt utilizado para maquetación de diagrama BPMN:** *"Estructurar plantilla gráfica SVG de carriles BPMN para proceso de ventas y almacén"*.
   - **Auditoría y corrección humana:** El resultado preliminar incluía al cliente como un carril interno dentro de la empresa y planteaba un flujo lineal sin manejo de errores. Se corrigió separando al **Cliente** en un *Pool* externo independiente (comunicado por flujos de mensaje discontinuos) e incorporando dos compuertas exclusivas (`XOR`) para modelar el rechazo por quiebre de stock y el disparo automático de órdenes de reposición.
