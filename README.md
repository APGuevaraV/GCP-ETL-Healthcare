# 🏥 Real-Time Patient Vital Signs ETL Pipeline (GCP)

Este proyecto implementa un pipeline ETL en streaming para el monitoreo en tiempo real de signos vitales de pacientes. Utiliza Google Cloud Platform (GCP) con una arquitectura basada en Pub/Sub, Dataflow (Apache Beam), Cloud Storage, BigQuery y Power BI.
El flujo sigue un enfoque Bronze → Silver → Gold, permitiendo manejar datos crudos, limpios, enriquecidos y finalmente analíticos para dashboards en tiempo real.

---

## 📌 Arquitectura General

![Architecture](docs/GCP-architecture.jpg)

# 🩺 ETL en GCP con Arquitectura Medallón (Bronze → Silver → Gold)

Este proyecto implementa un pipeline de procesamiento en tiempo real para monitorear signos vitales de pacientes utilizando Pub/Sub, Dataflow (Apache Beam), Cloud Storage y BigQuery. La arquitectura sigue el estándar Medallion Architecture.

---

## 📌 Arquitectura General

### **Descripción del flujo**

### **Data Source**
- Un simulador genera signos vitales sintéticos de pacientes (heart rate, SpO₂, temperatura, presión arterial).
- Se inyectan errores de forma controlada para simular escenarios reales.

### **Data Streaming – Pub/Sub**
- Los registros son enviados en tiempo real a un **topic de Pub/Sub**.
- Un **subscriber** procesa cada mensaje individualmente.

### **Data Processing – Dataflow + Apache Beam**
Pipeline que:
- Captura datos desde Pub/Sub  
- Escribe datos crudos en la capa **Bronze (GCS)**  
- Limpia y valida registros → **Silver**  
- Enriquece la data y genera métricas → **Gold**  
- Carga data analítica en **BigQuery**

### **Data Warehouse – BigQuery**
- Tabla final `patient_risk_analytics` usada para análisis y BI.

### **Dashboard – Power BI**
- Conecta BigQuery para visualizar métricas de riesgo por paciente en tiempo real.

---

## 🧪 1. Simulador de Signos Vitales (Python)

El proyecto incluye un simulador que genera y publica registros hacia Pub/Sub en formato JSON.

### **Características**
- Produce data de: `heart_rate`, `SpO₂`, `temperature`, `systolic/diastolic_pressure`.
- Permite configurar: número de pacientes, intervalo y `error_rate`.
- Inserta errores reales:
  - Campos nulos
  - Valores negativos
  - SpO₂ fuera de rango
- Envía mensajes a un topic Pub/Sub.

