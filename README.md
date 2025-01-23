# 🏗️ Descripción de la Arquitectura

---

## 1. 🧱 Estilo Arquitectónico  
**Sistema monolítico modular con API Gateway**  
Diseñado para:  

- 🔒 **Centralizar el manejo de peticiones**  
  Unificación de políticas de seguridad y logging.  
- 📦 **Simplificar la escalabilidad horizontal**  
  Réplicas independientes de módulos funcionales.  
- 🧩 **Desacoplamiento relativo**  
  Módulos independientes bajo un mismo despliegue.  

---

## 2. 🧰 Componentes Principales  

| **Capa**       | **Componente**         | **Responsabilidad**                                                                 | **Tecnologías**                              |
|-----------------|------------------------|-------------------------------------------------------------------------------------|----------------------------------------------|
| **🌐 Clientes** | Web App                | Interfaz para navegadores                                                           | React.js/Vue.js                              |
|                 | Mobile App             | Versión móvil nativa/híbrida                                                       | React Native/Flutter                         |
| **⚙️ Backend**  | API Gateway            | Enrutamiento inteligente + Autenticación inicial + Rate limiting                   | Spring Cloud Gateway/Kong                    |
|                 | Auth Module            | Gestión de identidad (JWT, OAuth2, roles)                                           | Spring Security/Keycloak                     |
|                 | Movies Module          | CRUD de catálogo + Búsquedas complejas                                              | Node.js + Elasticsearch                      |
|                 | Bookings Module        | Procesamiento transaccional (pagos, disponibilidad)                                 | Java + Spring Boot                           |
|                 | Notifications Module   | Notificaciones push/email                                                           | Nodemailer + Firebase Cloud Messaging        |
| **🗃️ Datos**    | PostgreSQL             | Almacenamiento transaccional ACID                                                   | PostgreSQL v15                               |

---

## 3. 🔑 Decisiones Clave de Diseño  

| **Aspecto**       | **Decisión**                            | **Justificación**                                                                 |
|--------------------|-----------------------------------------|-----------------------------------------------------------------------------------|
| **🔐 Seguridad**   | HTTPS obligatorio                       | Previene ataques MITM y asegura integridad de datos.                              |
| **🚪 API Gateway** | Único punto de entrada                  | Centraliza políticas CORS, autenticación y métricas.                              |
| **💾 Base de Datos**| PostgreSQL v15                          | Garantiza consistencia en transacciones críticas (ej: pagos).                     |
| **🧩 Modularidad** | Separación por dominio (DDD)            | Permite escalar equipos/módulos independientemente.                               |

---
## 4. 📊 Diagrama de Flujo  
```mermaid
graph TD
    subgraph Clientes["Clientes"]
        A[Web App]
        A2[Mobile App]
    end

    subgraph Backend["Backend"]
        B[API Gateway]
        C[Auth Module]
        D[Movies Module]
        E[Bookings Module]
        F[Notifications Module]
    end

    subgraph Datos["Base de Datos"]
        G[(PostgreSQL)]
    end

    A -->|HTTPS| B
    A2 -->|HTTPS| B
    B --> C
    C -->|JWT Válido| D
    C -->|JWT Válido| E
    D --> G
    E --> G
    E --> F
    F --> H[Servicios Externos: Email/SMS]

    style Clientes fill:#e1f5fe,stroke:#039be5
    style Backend fill:#f0f4c3,stroke:#c0ca33
    style Datos fill:#c8e6c9,stroke:#43a047
