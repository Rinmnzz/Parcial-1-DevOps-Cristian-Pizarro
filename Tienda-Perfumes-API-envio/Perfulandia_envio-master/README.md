# Parcial 1 - DevOps

micro servicio envio 

## Integrantes
* **Cristian Pizarro**

## Asignatura

**INGENIERIA DEVOPS_007V_OLS**

## Herramientas y Tecnologías
* **Java 17 & Maven**: Motor de desarrollo y gestión de dependencias.
* **GitHub Actions**: Automatización de integración continua.
* **Git**: Control de versiones.

## Modelo de Ramas (GitFlow)
adopte el modelo GitFlow para la gestión de este proyecto por las siguientes razones:
1. **Aislamiento de funciones**: Permite desarrollar nuevas características en ramas feature sin afectar el código estable.
2. **Estabilidad**: La rama main siempre contiene código listo para producción, mientras que develop sirve para la integración.
3. **Respuesta Rápida**: El uso de ramas hotfix permite corregir errores críticos en producción de forma inmediata y organizada.

### Ramas Utilizadas
* **main**: Contiene el código productivo y estable que ha pasado todas las pruebas.
* **develop**: Rama principal para la integración de nuevas funcionalidades.
* **feature/Cristian y Feature/documentacion**: Rama utilizada para el desarrollo de nuevas características y actualizaciones del proyecto.
* **hotfix/Cristian**: Rama de emergencia utilizada para corregir errores críticos directamente sobre la rama main.

### Mensajes de Commit
* feat: Para nuevas funcionalidades 
* fix: Para corrección de errores 
* ci: Para cambios en la configuración de GitHub Actions.

## Integración Continua (CI)
El proyecto cuenta con un flujo de trabajo automatizado mediante GitHub Actions. 
* **Trigger**: Se activa con cada push a la rama develop y con cada Pull Request hacia main.
* **Proceso**: El pipeline configura el entorno de Java 17, instala dependencias y compila el proyecto usando Maven para asegurar que el código sea íntegro antes de ser mezclado.

