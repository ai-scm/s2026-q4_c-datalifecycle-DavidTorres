# Ciclo de Vida de los Datos en AWS — Mapas Mentales (Etapas 1 y 2)

Documentación visual y síntesis arquitectónica basada en la guía oficial de referencia de Amazon Web Services (*AWS Data Lifecycle*), enfocada en las dos primeras etapas fundamentales para construir soluciones de datos y analítica en la nube: **Ingesta de Datos** y **Montaje de Datos**.

---

## Contenido del Proyecto

El trabajo consiste en el diseño de mapas mentales radiales de **3 niveles de profundidad** desarrollados en código XML nativo para **draw.io (diagrams.net)**, estructurados para facilitar tanto el estudio técnico como la exposición ejecutiva de los servicios de datos de AWS:

1. **Etapa 1 – Ingesta de Datos (`etapa-1-ingesta-datos.drawio`):** Criterios de selección, transmisión en tiempo real, transporte físico a gran escala, conectividad híbrida y migración de bases de datos.
2. **Etapa 2 – Montaje de Datos (`etapa-2-montaje-datos.drawio`):** Almacenamiento de objetos, gobernanza mediante metadatos y etiquetas, arquitectura de Data Lakes y selección de motores de bases de datos relacionales, NoSQL y especializados.

---

## Arquitectura de los Mapas Mentales

### 1. Etapa 1 – Ingesta de Datos en AWS
Aborda el movimiento, validación e importación de datos desde fuentes externas (ERP, *clickstream*, registros, sensores IoT y bases de datos) hacia destinos de almacenamiento o análisis en la nube, evaluando siempre **volumen, velocidad, fuente y formato**.

| Categoría (Nivel 1) | Servicios / Conceptos (Nivel 2) | Detalles Técnicos y Casos de Uso (Nivel 3) |
| :--- | :--- | :--- |
| **Fundamentos y Criterios** | Propósito, Fuentes y Preguntas Clave | • Traslado de centros locales/discos físicos a la nube.<br>• Selección según patrón: *streaming* (Kinesis), bases de datos (DMS) o microlotes satelitales (S3 Transfer Acceleration). |
| **Transmisión en Tiempo Real (Amazon Kinesis)** | • Kinesis Video Streams<br>• Kinesis Data Streams (KDS)<br>• Kinesis Data Firehose | • **Video Streams:** Ingesta segura desde millones de dispositivos + Rekognition Video / ML.<br>• **KDS:** Captura de GB/seg con disponibilidad en **milisegundos**.<br>• **Firehose:** Carga gestionada casi en tiempo real hacia S3, Redshift, Elasticsearch y Splunk.<br>• *Caso de estudio:* **Brain Power** (análisis de lenguaje corporal en ensayos clínicos). |
| **Transporte Físico (Familia Snow)** | • AWS Snowball<br>• AWS Snowball Edge<br>• AWS Snowmobile | • **Snowball:** Ahorro de hasta 1/5 frente a transferencia por internet; copia directa desde **HDFS**.<br>• **Snowball Edge:** Cómputo perimetral (EC2/Lambda); *Storage Optimized* (100 TB / 24 vCPUs) y *Compute Optimized* (52 vCPUs + GPU).<br>• **Snowmobile:** Contenedor de 45 pies en camión con capacidad de hasta **100 PB**.<br>• *Caso de estudio:* **DigitalGlobe** (migración de 70 PB de imágenes terrestres). |
| **Redes y Transferencia Híbrida** | • AWS Direct Connect<br>• S3 Transfer Acceleration<br>• AWS DataSync<br>• AWS Storage Gateway | • **Direct Connect:** Conexión privada dedicada que omite proveedores de internet (ISP).<br>• **Transfer Acceleration:** Transferencias a larga distancia (+100 millas) usando las **155 ubicaciones de borde** de CloudFront.<br>• **DataSync:** Automatización in situ a S3/EFS vía NFS hasta **10x más rápido** que herramientas *open-source*.<br>• **Storage Gateway:** Portal híbrido (NFS, SMB, iSCSI) hacia S3, Glacier, EBS y AWS Backup. |
| **Bases de Datos y Orquestación** | • AWS DMS<br>• AWS Data Pipeline<br>• AWS Glue | • **DMS:** Migraciones homogéneas y heterogéneas sin tiempo de inactividad.<br>• **Data Pipeline:** Movimiento y transformación programada entre fuentes locales y S3, RDS, DynamoDB o EMR.<br>• **Glue:** Preparación y transferencia hacia depósitos de datos (profundizado en la Etapa 3).<br>• *Caso de estudio:* **US Department of Veterans Affairs** (replicación de 20M registros de VACOLS/Oracle 11g a RDS Multi-AZ). |

---

### 2. Etapa 2 – Montaje de Datos en AWS
Se centra en la preparación ("tarea doméstica"), limpieza, gobierno y organización de los datos en múltiples formatos antes de habilitarlos para el consumo analítico.

| Categoría (Nivel 1) | Servicios / Conceptos (Nivel 2) | Detalles Técnicos y Casos de Uso (Nivel 3) |
| :--- | :--- | :--- |
| **Fundamentos y Criterios** | Desafíos y Preguntas Clave de Diseño | • Unificación de silos (depósitos, hojas de cálculo, BD, texto) frente al crecimiento de volumen y variedad.<br>• Decisión OLTP vs. Data Warehouse; uso de **Amazon S3** como destino base o **Kinesis Firehose** como búfer para conjuntos muy pequeños. |
| **Almacenamiento de Objetos (Amazon S3)** | • Capacidades Core<br>• Metadatos de Usuario<br>• S3 Object Tags + IAM | • Durabilidad extrema, alta disponibilidad, escalabilidad infinita y pago por uso.<br>• **Taxonomía de Metadatos:** Técnicos (formato, *checksum*, filas), Operacionales (frecuencia, linaje, propietario) y de Negocio (PII/PHI, llaves, dominio).<br>• **Object Tags:** Pares Clave/Valor externos al objeto que, junto con **IAM**, aplican control de acceso granular. |
| **Creación de Lago de Datos (Data Lake)** | • Arquitectura Desacoplada<br>• AWS Lake Formation | • Almacenamiento de datos estructurados y no estructurados en su formato nativo (*schema-on-read*), desacoplando almacenamiento y cómputo.<br>• **5 Pasos de Lake Formation:** 1) Configurar almacenamiento, 2) Mover datos, 3) Limpiar, preparar y catalogar, 4) Aplicar seguridad/cumplimiento, 5) Habilitar para análisis. |
| **Bases de Datos Relacionales y Caché** | • Amazon Aurora y Serverless<br>• Amazon RDS y RDS on VMware<br>• Amazon ElastiCache | • **Aurora:** Compatible con MySQL/PostgreSQL, hasta **64 TB**, 15 réplicas de lectura y replicación en 3 AZ. Versión **Serverless** con escalado automático y cobro por segundo.<br>• **RDS:** Gestión automatizada de 6 motores (Aurora, PostgreSQL, MySQL, MariaDB, Oracle, SQL Server) y extensión local vía **VMware vSphere**.<br>• **ElastiCache:** Caché en memoria de latencia ultrabaja para *Hot Data*. |
| **Bases de Datos NoSQL y Especializadas** | • DynamoDB y DocumentDB<br>• Amazon Neptune<br>• Amazon QLDB<br>• Amazon Timestream | • **DynamoDB:** Clave-valor y documentos con latencia de milisegundos de un dígito (+10 billones de solicitudes/día, aceleración con **DAX**). **DocumentDB** ofrece compatibilidad con MongoDB.<br>• **Neptune:** Base de datos de grafos (*Property Graph* y *RDF*, consultas con **Gremlin** y **SPARQL**).<br>• **QLDB:** Libro contable inmutable y verificable criptográficamente.<br>• **Timestream:** Series de tiempo (*append-only* para IoT) a **1/10 del costo** de una BD relacional.<br>• *Caso de estudio:* **Healthdirect Australia** (arquitectura dual de lectura/escritura con DynamoDB, S3, Lambda, Kinesis y Athena). |

---

## Cómo Importar y Editar los Diagramas en draw.io

Los diagramas fueron construidos utilizando el motor XML nativo de **draw.io** para garantizar una distribución radial limpia, nodos estilizados y conectores curvos (`entityRelationEdgeStyle`).

1. Ingresa a [app.diagrams.net](https://app.diagrams.net/) y abre un lienzo en blanco.
2. En el menú superior, navega a **Extras > Editar diagrama...** (*Extras > Edit Diagram...*).
3. Selecciona y elimina todo el contenido XML existente en la ventana emergente.
4. Pega el bloque XML correspondiente al mapa mental deseado (**Etapa 1** o **Etapa 2**) y haz clic en **Aceptar**.
5. Para exportar el resultado final, ve a **Archivo > Exportar como** y selecciona `PNG`, `PDF` o `SVG`.

---

## Referencia

* **Fuente base:** *AWS – Guía de referencia del ciclo de vida de los datos* (Etapa 1: Ingesta de datos, págs. 17–23; Etapa 2: Montaje de datos, págs. 25–30). © Amazon Web Services, Inc.
