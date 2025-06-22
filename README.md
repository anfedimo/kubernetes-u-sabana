# Helm Chart: nequidev-api

Este Helm Chart despliega la aplicación `nequidev-api` en un clúster de Kubernetes, incluyendo configuración de servicio, almacenamiento persistente, y recolección de logs.

## Estructura de Componentes

- **Chart.yaml**: Define el nombre del chart, versión y descripción.
- **values.yaml**: Contiene los valores por defecto del despliegue, como nombre de imagen, puertos, recursos y configuración del volumen.
- **deployment.yaml**: Manifiesto de Kubernetes que define el Deployment de la aplicación.
- **service.yaml**: Expone la aplicación mediante un Service de tipo `ClusterIP`.
- **persistentVolume.yaml**: Define un `PersistentVolume` para almacenamiento local.
- **promtail.yaml**: Configura Promtail como DaemonSet para recolección de logs hacia Loki.
- **_helpers.tpl**: Contiene plantillas auxiliares usadas en los manifiestos para nombramiento y etiquetas consistentes.

## Estructura del Chart

    ```text
    kubernetes-nequi/
    ├── Chart.yaml
    ├── values.yaml
    └── templates/
        ├── deployment.yaml
        ├── service.yaml
        ├── _helpers.tpl
        └── (otros opcionales: ingress.yaml, configmap.yaml, secret.yaml, hpa.yaml)
    ```

## Despliegue

    ```bash
    helm install nequidev-api ./nequidev-api
    ``` 
