# instalacion-configuracion-kubernetes-windows

INSTALACIÓN Y CONFIGURACIÓN DE KUBERNETES EN WINDOWS
Requerimiento necesarios:
- Windows 10
- Hyper V activado
- Docker 18.02 en adelante
- Conocimientos en PowerShell, Docker y Kubernetes


Primero instalar kubectl en windows:
- https://kubernetes.io/es/docs/tasks/tools/included/install-kubectl-windows/#install-kubectl-binary-with-curl-on-windows
- https://kubernetes.io/es/docs/tasks/tools/included/install-kubectl-windows/#install-on-windows-using-chocolatey-or-scoop

Instalar el binario de kubectl en Windows:
Kubectl.exe - situado en C:\Program Files\Docker\Docker\resources\bin -- ya lo tenemos instalado el ejecutable con Docker

O si tiene curl instalado, use este comando:
curl -LO https://dl.k8s.io/release//bin/windows/amd64/kubectl.exe

1. Validar el binario:
Descargue el archivo de comprobación de kubectl:
curl -LO https://dl.k8s.io//bin/windows/amd64/kubectl.exe.sha256

Valide el binario de kubectl con el archivo de comprobación:
Usando la consola del sistema para comparar manualmente la salida de CertUtil con el archivo de comprobación descargado:
CertUtil -hashfile kubectl.exe SHA256 type kubectl.exe.sha256

El archivo SHA256 lo movemos a la ruta C:\Windows\System32
Movemos el ejecutable kubectl.exe situado en C:\Program Files\Docker\Docker\resources\bin a la carpeta de system32
Usando PowerShell puede automatizar la verificación usando el operador –eq para obtener un resultado de True o False:
$($(CertUtil -hashfile .\kubectl.exe SHA256)[1] -replace " ", "") -eq $(type .\kubectl.exe.sha256)
Nos da un resultado de False.


2. Agregamos el binario a su path:
Path – Editar – Nuevo
Agregamos la ruta C:\Windows\System32
C:\Windows\system32\kubectl.exe.sha256


3. Para asegurar que la versión de kubectl es la misma que descargada, ejecute:
kubectl version –client
Kustomize Versión: v4.5.7

kubectl version
Client Version – v1.26.0 y v.1.25.2
Server Version – v1.25.3

Creamos y agregamos la variable de entorno Kubectl:
Nombre de la variable: Kubectl
Valor de la variable: C:\Program Files\Docker\Docker\resources\bin\kubectl.exe

Otra variable más:
Nombre de la variable: kubectl
Valor de la variable: C:\Users\alejandro\Downloads\kubectl.exe


Instalar en Windows usando Chocolatey o Scoop:
Paso 1. Para instalar kubectl en Windows, puede usar Chocolatey
Choco install kubernetes-cli


Paso 2. Use el comando para instalar un administrador de paquetes de chocolate. Una vez que haya copiado el comando, el paquete de chocolate se descargará automáticamente. Puede ver que choco.exe ya está listo. Esto significa que el comando se ha ejecutado con éxito.
IMPORTANTE: EJECUTAR POWERSHELL COMO ADMINISTRADOR PARA HACER LAS PRUEBAS
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))


Paso 3. Escribimos choco para verificar la versión


Paso 4. Ejecutar choco install minikube. Descargará Kubernetes-cli y el paquete minikube:
Choco install minikube


Paso 5: Kubernetes y minikube se ha instalado correctamente en la máquina local.


Paso 6: Una vez que ejecute el comando, llevará algún tiempo descargar minikube iso de Internet. Una vez que termine de descargarse, el clúster de Kubernetes aparecerá en su administrador de Hyper-V.
minikube start -- para iniciar minikube.
minikube stop -- para apagar minikube.


Paso 7: Si desea confirmar la información de control de minikube, puede usar el siguiente comando. También puede obtener los detalles de la URL del panel del panel:
kubectl cluster-info


Paso 8: Si lo desea, puede navegar directamente al tablero usando el siguiente comando. Ahora, mi Kubernetes se ha creado correctamente:
minikube dashboard
http://127.0.0.1:puerto



Comandos básicos de Kubernetes:
- kubectl get nodes -- visualiza los nodos existentes
- kubectl proxy -- conexión con el servidor mediante 127.0.0.1:puerto
- kubectl get services –- para enumerar los servicios en el espacio de nombres predeterminado
- kubectl config view –- para ver la configuración actual de kubectl
- kubectl config get-contexts –- para enumerar los contextos disponibles
- kubectl config current-context –- para obtener el contexto actual de kubectl
- kubectl get pods –all-namespaces –- para enumerar pods en todos los espacios de nombres
- kubectl get services –sort-by=.metadata.name -- permite ordenar la salida según un campo en particular y enumerar los servicios ordenados por nombre de servicio
- kubectl get events –sort-by=.metadata.creationTimestap -- para obtener una lista de eventos, pero que está ordenada por marca de tiempo, podemos usar -sort-by
- kubectl get service –A -- para listar los servicios
- kubectl get pods –A –- listar pods
- kubectl get deployments –A –- listar deployments
- kubectl get namespaces – listar namespaces (espacio de nombres)
- kubectl get deployments,pods,services,namespaces –o wide –- listar varios componentes en un solo comando
- kubectl get deploy,pod,svc,ns –- listar con abreviaturas
- kubectl config get-clusters
