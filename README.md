# Helm Chart: sabana-api

Este Helm Chart despliega la aplicación `sabana-api` en un clúster de Kubernetes, incluyendo configuración de servicio, almacenamiento persistente, y recolección de logs.

## 📂 Funcionalidad específica de cada archivo del Helm Chart

- 📄 **Chart.yaml**	Metadatos básicos del Chart: nombre (sabana-api), versión del Chart, y descripción breve.


- 📄 **values.yaml**	Valores configurables del Chart: imagen Docker, réplicas, puertos, recursos asignados, variables de entorno y configuraciones específicas. 


- 📄 **deployment.yaml**	Define cómo se despliega tu aplicación (Deployment) en Kubernetes. Especifica contenedores, puertos, réplicas, configuraciones y probes de salud.


- 📄 **service.yaml**	Expone tu aplicación mediante un servicio interno (ClusterIP), haciendo accesible el servicio dentro del clúster Kubernetes.


- 📄 **hpa.yaml**	Escalado automático horizontal (HorizontalPodAutoscaler) basado en consumo de CPU o memoria, para ajustar el número de pods según demanda.


- 📄 **_helpers.tpl**	Plantillas auxiliares para nombrar recursos consistentemente y aplicar etiquetas estándar en todos los recursos generados por el Chart Helm.


## 📑 Archivos de ArgoCD (dentro de la carpeta argocd)

- 📄 **archivo-secret-github.yaml**	Guarda secretos en Kubernetes (token o clave SSH), permitiendo a ArgoCD acceder a repositorios privados GitHub.


- 📄 **sabana-argo-app.yaml**	Define la aplicación ArgoCD que apunta al repositorio de Helm Charts, permitiendo la gestión y despliegue continuo de la aplicación `sabana-api`.


## Estructura del Chart

```text
kubernetes-sabana/
├── argocd/
│   └── sabana-argo-app.yaml         # Aplicación ArgoCD que apunta al Helm Chart
│
├── charts/
│   └── sabana-api/
│       ├── Chart.yaml                 # Metadatos del Chart Helm
│       ├── values.yaml                # Valores predeterminados del Chart
│       └── templates/
│           ├── _helpers.tpl           # Funciones auxiliares para plantillas
│           ├── deployment.yaml        # Despliegue de la aplicación sabana-api
│           ├── service.yaml           # Servicio ClusterIP que expone la aplicación
│           └── hpa.yaml               # Escalado automático basado en métricas
│
└── README.md                          # Documentación del Chart Helm
```

## 🚀 Flujo de Despliegue GitOps con ArgoCD y Helm

1. Realizas un commit y push en el repositorio kubernetes-sabana.

2. ArgoCD detecta automáticamente cambios en Git (gracias al archivo sabana-argo-app.yaml).

3. ArgoCD usa el Chart Helm ubicado en charts/sabana-api.

4. Se despliegan automáticamente los manifiestos (deployment.yaml, service.yaml, hpa.yaml) en Kubernetes.

5. ArgoCD mantiene el despliegue sincronizado y reporta claramente su estado.


## 🧑‍💻 Ejemplo de comandos útiles con tu estructura actual
- Crea el namespace `api` si no existe.
```bash
kubectl create namespace api
```
- Para validar cambios (sincronización ArgoCD):
```bash
argocd app sync sabana-api
```

- Para ver el estado de la aplicación en ArgoCD:
```bash
argocd app get sabana-api
```

- Aplica el archivo de configuración de ArgoCD para crear el secreto que permite el acceso a tu repositorio privado de GitHub.
```bash
kubectl apply -f argocd/archivo-secret-github.yaml
```
- Aplica el archivo de configuración de ArgoCD para crear la aplicación que gestiona el despliegue del Helm chart `sabana-api`.
```bash
kubectl apply -f argocd/sabana-argo-app.yaml
```

- Instala el Helm chart `sabana-api` en el clúster por primera vez.
```bash
helm install sabana-api ./sabana-api
``` 

- Actualiza el release `sabana-api` en el namespace api con los últimos cambios del chart.
```bash
helm upgrade sabana-api charts/sabana-api -n api
``` 

- Reinicia el deployment para aplicar cambios recientes en la aplicación en el namespace `api`.
```bash
kubectl rollout restart deployment sabana-api-kubernetes-sabana -n api
``` 

- Expone el servicio `sabana-api-kubernetes-sabana` localmente en el puerto `9000` para acceso temporal.
```bash
kubectl port-forward svc/sabana-api-kubernetes-sabana 9000:9000 -n api
``` 


## Esta estructura:

Simplifica la gestión: Separación clara de recursos.

Reduce errores humanos: Automatización GitOps.

Aumenta seguridad y auditoría: Control completo vía Git.

Permite fácil escalabilidad: Helm y HPA facilitan ajustes dinámicos.


Es la mejor práctica actual para gestionar aplicaciones Kubernetes modernas con GitOps usando ArgoCD y Helm Charts.

# Arquitectura de Software — Universidad de La Sabana — Grupo 14 — 2025

