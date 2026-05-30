# Parcial 2 - DevOps 
## Microservicio de Envío - Perfulandia

[![Java CI/CD Pipeline con Maven y Docker](https://github.com/Rinmnzz/Parcial-1-DevOps-Cristian-Pizarro/actions/workflows/ci.yml/badge.badge.svg)](https://github.com/Rinmnzz/Parcial-1-DevOps-Cristian-Pizarro/actions)

##  Integrantes
* **Cristian Pizarro**

##  Asignatura
* **INGENIERIA DEVOPS_007V_OLS**

---

##  Herramientas y Tecnologías
* **Java 17 & Maven**: Motor de desarrollo y gestión de dependencias.
* **Undertow Server**: Servidor web integrado de alto rendimiento elegido para mitigar vulnerabilidades críticas de seguridad.
* **Snyk CLI**: Herramienta de análisis estático de seguridad (SCA) para auditoría de dependencias.
* **Docker & Buildx**: Containerización avanzada de la aplicación.
* **GitHub Container Registry (GHCR)**: Registro privado/público de artefactos para el almacenamiento de imágenes Docker.
* **GitHub Actions**: Automatización y orquestación del ciclo de vida del software (CI/CD).
* **Git**: Control de versiones.

---

##  Modelo de Ramas (GitFlow)
Adopté el modelo GitFlow para la gestión de este proyecto por las siguientes razones:
1. **Aislamiento de funciones**: Permite desarrollar nuevas características en ramas feature sin afectar el código estable.
2. **Estabilidad**: La rama `main` siempre contiene código listo para producción, mientras que `develop` sirve para la integración.
3. **Respuesta Rápida**: El uso de ramas `hotfix` permite corregir errores críticos en producción de forma inmediata y organizada.

### Ramas Utilizadas
* **main**: Contiene el código productivo y estable que ha pasado todas las pruebas y auditorías de seguridad.
* **develop**: Rama principal para la integración de nuevas funcionalidades.
* **feature/Cristian** y **feature/documentacion**: Ramas utilizadas para el desarrollo de nuevas características y actualizaciones del proyecto.
* **hotfix/Cristian**: Rama de emergencia utilizada para corregir errores críticos directamente sobre la rama main.

### Mensajes de Commit
* `feat:` Para nuevas funcionalidades.
* `fix:` Para corrección de errores (bugs y parches de seguridad).
* `ci:` Para cambios en la configuración de GitHub Actions y el pipeline.

---

##  Decisiones Arquitectónicas y Seguridad Avanzada
Durante la fase de análisis estático de dependencias con **Snyk**, se detectaron múltiples vulnerabilidades de severidad Alta y Crítica en el servidor nativo *Tomcat*. 

Como estrategia avanzada de ingeniería DevOps para mitigar riesgos sin detener el ciclo de entrega (debido a la falta de parches oficiales por parte de Tomcat compatibles con Spring Boot 3.5.x), se tomó la decisión arquitectónica de **reemplazar Tomcat por JBoss Undertow** mediante la exclusión de dependencias en el archivo `pom.xml`. Esto eliminó el vector de ataque principal, redujo la superficie de vulnerabilidad y mejoró el rendimiento en el manejo de peticiones concurrentes de la API.

---

##  Pipeline de Integración y Despliegue Continuo (CI/CD)
El proyecto cuenta con un flujo de trabajo 100% automatizado mediante **GitHub Actions** configurado en el archivo `ci.yml`.

###  Triggers (Disparadores)
* Se activa de manera automatizada ante cualquier evento de `push` en la rama `main`.

###  Fases del Pipeline (Jobs)

#### 1. Build and Test (`build-and-test`)
* **Checkout**: Descarga el código fuente del repositorio.
* **JDK Setup**: Configura el entorno con Java 17 (Distribución Temurin) optimizando la velocidad mediante caché de Maven.
* **Testing Obligatorio**: Ejecuta las pruebas unitarias del proyecto (`mvn test`) asegurando la integridad lógica.
* **Compilación**: Empaqueta la aplicación generando el archivo JAR ejecutable.
* **Auditoría Snyk**: Instala Snyk CLI y realiza un escaneo de seguridad con un umbral estricto para bloquear el pipeline ante vulnerabilidades críticas de código o dependencias.

#### 2. Build Image (`build-image`)
* **Dependencia**: Requiere que la fase anterior finalice con éxito.
* **Autenticación**: Inicia sesión de forma segura en **GitHub Container Registry (ghcr.io)** utilizando el token nativo del sistema (`GITHUB_TOKEN`).
* **Normalización**: Procesa y transforma el nombre del repositorio a minúsculas mediante Bash para cumplir estrictamente con los estándares de nomenclatura de Docker.
* **Construcción y Push**: Compila la imagen Docker de la API de Perfumes basándose en el Dockerfile y la publica automáticamente en GHCR bajo la etiqueta `:latest`.

#### 3. Deploy (`deploy`)
* **Dependencia**: Requiere que la imagen Docker se haya subido correctamente.
* **Simulación en la Nube**: Simula el despliegue del contenedor en un entorno Cloud.
* **Trazabilidad Absoluta (IL 2.4)**: El pipeline inyecta dinámicamente metadatos del ciclo de vida en los registros de salida, imprimiendo el **ID único del Commit (`github.sha`)** y el **Autor del cambio (`github.actor`)**, asegurando auditoría completa sobre qué código se está desplegando y quién lo autorizó.
