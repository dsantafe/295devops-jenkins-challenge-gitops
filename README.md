# Despliegue de Aplicación en Kubernetes usando Minikube y Argo CD

Este repositorio contiene los archivos y comandos necesarios para desplegar una aplicación en un clúster de Kubernetes utilizando Minikube y Argo CD.

## Pasos para desplegar la aplicación

1. Clona este repositorio:
    ```shell
    git clone https://github.com/dsantafe/295devops-jenkins-challenge-gitops
    ```

2. Iniciar Minikube:

   Asegúrate de que Minikube esté iniciado antes de aplicar las configuraciones. Si no está iniciado, inicia Minikube con el siguiente comando:
   ```shell
   $ minikube start
   $ minikube dashboard
   ```

3. Instalar e iniciar Argo CD:

   Instalación del Chart
   ```shell
   $ kubectl create ns argo-cd
   $ helm repo add argo https://argoproj.github.io/argo-helm
   $ helm install argocd argo/argo-cd -n argo-cd
   ```

   Para acceder a la interfaz de usuario del servidor, tiene las siguientes opciones y luego abra el navegador en http://localhost:8080 y acepte el certificado.
   ```shell
   $ kubectl port-forward service/argocd-server -n argo-cd 8080:443
   ```

   Después de acceder a la interfaz de usuario por primera vez, puede iniciar sesión con el nombre de usuario: admin y la contraseña aleatoria generada durante la instalación. Puede encontrar la contraseña ejecutando:
   ```shell
   $ kubectl -n argo-cd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
   ```

4. Aplicar el archivo de ArgoCD:
Aplica el archivo de ArgoCD para desplegar la aplicación en Kubernetes. Este archivo referencia la carpeta /dev donde especifica el servicio para exponer la aplicación en un puerto específico en Minikube y cómo se deben ejecutar las réplicas de la aplicación.
    ```shell    
    $ kubectl apply -f application.yaml
    ```

5. Obtener la URL del servicio:
Obtén la URL para acceder a la aplicación a través de Minikube.
    ```shell
    $ kubectl port-forward service/vote -n 295devops 5000:80
    $ kubectl port-forward service/result -n 295devops 5001:80    
    ```

6. Acceder a la aplicación http://localhost:5000/ y http://localhost:5001/ desplegada.

## Links
- Config repo: https://github.com/dsantafe/295devops-jenkins-challenge-gitops
- Docker repo: https://github.com/dsantafe/295devops-jenkins-challenge
- Install ArgoCD: https://argo-cd.readthedocs.io/en/stable/getting_started/#1-install-argo-cd
- Login to ArgoCD: https://argo-cd.readthedocs.io/en/stable/getting_started/#4-login-using-the-cli
- ArgoCD Configuration: https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/