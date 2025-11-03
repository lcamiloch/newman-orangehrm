# Pruebas Automatizadas de API para OrangeHRM

## 📜 Descripción

Este proyecto contiene una suite de pruebas de API automatizadas para la plataforma de demostración de OrangeHRM. Su propósito es validar de forma continua la funcionalidad del endpoint de creación de empleados, asegurando la estabilidad y fiabilidad del contrato de la API.

La suite está diseñada para ser ejecutada tanto localmente como en un pipeline de Integración Continua (CI/CD) utilizando Newman y GitHub Actions.

## 🔄 Flujo de Automatización

El flujo simula un ciclo completo de interacción de un usuario para garantizar la independencia de cada ejecución:

1.  **Logout:** Cierra la sesión para asegurar que ejecución comience desde un estado limpio.
2.  **Obtener Token CSRF:** Inicia una sesión anónima para extraer un token de seguridad (`_token`) de la página de login.
3.  **Login de Usuario:** Utiliza el `_token` y las credenciales para autenticarse, recibiendo a cambio una cookie de sesión (`orangehrm`).
4.  **Creación de Empleado (Data-Driven):** Ejecuta la creación de usuarios con datos de un archivo `.csv`, autenticándose con la cookie de sesión.

## 🚀 Tecnologías Utilizadas

| Herramienta / Lenguaje | Versión (Ejemplo) | Sitio Oficial                                     |
| :--------------------- | :---------------- | :------------------------------------------------ |
| **Postman**            | 11.x              | [postman.com](https://www.postman.com/)           |
| **Newman**             | 6.x               | [npmjs.com/package/newman](https://www.npmjs.com/package/newman) |
| **Git**                | 2.x               | [git-scm.com](https://git-scm.com/)               |
| **GitHub Actions**     | v4                | [github.com/features/actions](https://github.com/features/actions) |
| **JavaScript (Node.js)** | 18.x LTS          | [nodejs.org](https://nodejs.org/)                 |

## 🛠️ Guía de Instalación y Ejecución

Sigue estos pasos para configurar y ejecutar el proyecto en tu máquina local.

### Prerrequisitos

Asegúrate de tener instalado lo siguiente:
-   [Git](https://git-scm.com/downloads)
-   [Node.js](https://nodejs.org/) (que incluye npm, el gestor de paquetes de Node)

### Paso 1: Clonar el Repositorio

Abre tu terminal y clona este repositorio en tu máquina local.

```bash
git clone https://github.com/tu-usuario/tu-repositorio.git
```

### Paso 2: Navegar al Directorio del Proyecto

```bash
cd tu-repositorio
```

### Paso 3: Instalar Newman

Newman es la herramienta que nos permitirá ejecutar las pruebas desde la terminal. Instálalo de forma global en tu sistema.

```bash
npm install -g newman
```

### Paso 4: Ejecutar las Pruebas

Ejecuta el siguiente comando desde la raíz del proyecto. Este comando ejecutará la colección completa, utilizando el archivo de entorno y el archivo de datos CSV.

**Importante:** Reemplaza `Admin` y `admin123` con las credenciales correctas si son diferentes.

```bash
newman run "Prueba_Davivienda.postman_collection.json" \
  -e "Davivienda_Env.postman_environment.json" \
  -d "data.csv" \
  --env-var "admin_username=Admin" \
  --env-var "admin_password=admin123"
```

**Desglose del comando:**
-   `run ...`: Especifica el archivo de la colección de Postman a ejecutar.
-   `-e ...`: Especifica el archivo de entorno que contiene variables como `base_url`.
-   `-d ...`: Especifica el archivo de datos (`.csv`) para las pruebas Data-Driven.
-   `--env-var "key=value"`: Inyecta o sobrescribe variables de entorno en tiempo de ejecución. Es el método más seguro para manejar credenciales.