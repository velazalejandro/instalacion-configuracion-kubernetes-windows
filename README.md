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
<img width="691" height="334" alt="image" src="https://github.com/user-attachments/assets/378b9bfb-ee9b-4298-a58f-2829124f2e43" />
Kubectl.exe - situado en C:\Program Files\Docker\Docker\resources\bin -- ya lo tenemos instalado el ejecutable con Docker

O si tiene curl instalado, use este comando:
<img width="786" height="97" alt="image" src="https://github.com/user-attachments/assets/bb852ea5-f4ea-46a7-b838-3b1e9083d59b" />
curl -LO https://dl.k8s.io/release//bin/windows/amd64/kubectl.exe

1. Validar el binario:
Descargue el archivo de comprobación de kubectl:
<img width="742" height="96" alt="image" src="https://github.com/user-attachments/assets/215fe3d9-4878-41cf-995c-cae12d01b527" />
curl -LO https://dl.k8s.io//bin/windows/amd64/kubectl.exe.sha256

Valide el binario de kubectl con el archivo de comprobación:
Usando la consola del sistema para comparar manualmente la salida de CertUtil con el archivo de comprobación descargado:
CertUtil -hashfile kubectl.exe SHA256 type kubectl.exe.sha256
<img width="567" height="91" alt="image" src="https://github.com/user-attachments/assets/02309651-a8ec-4592-8fe9-3a8392017385" />
<img width="691" height="353" alt="image" src="https://github.com/user-attachments/assets/500d2417-601d-47c0-b890-24a0e79e93c8" />

El archivo SHA256 lo movemos a la ruta C:\Windows\System32
<img width="949" height="201" alt="image" src="https://github.com/user-attachments/assets/a869c766-4ba0-4902-97c8-192a9825dcdd" />
Movemos el ejecutable kubectl.exe situado en C:\Program Files\Docker\Docker\resources\bin a la carpeta de system32
Usando PowerShell puede automatizar la verificación usando el operador –eq para obtener un resultado de True o False:
$($(CertUtil -hashfile .\kubectl.exe SHA256)[1] -replace " ", "") -eq $(type .\kubectl.exe.sha256)
<img width="845" height="68" alt="image" src="https://github.com/user-attachments/assets/d7ac7561-6cb4-44e9-aacc-9f3f62961811" />
Nos da un resultado de False.


2. Agregamos el binario a su path:
<img width="514" height="492" alt="image" src="https://github.com/user-attachments/assets/189b295d-723b-42d2-8896-fe8c35183ce4" />
Path – Editar – Nuevo
Agregamos la ruta C:\Windows\System32
<img width="237" height="35" alt="image" src="https://github.com/user-attachments/assets/f3e4cabd-9c2d-478a-bdc1-c1759608781f" />
C:\Windows\system32\kubectl.exe.sha256

3. Para asegurar que la versión de kubectl es la misma que descargada, ejecute:
kubectl version –client
<img width="960" height="126" alt="image" src="https://github.com/user-attachments/assets/47e9b863-3bed-4b88-ad85-9f49bda4afc3" />
Kustomize Versión: v4.5.7

kubectl version
<img width="964" height="158" alt="image" src="https://github.com/user-attachments/assets/7f30aea2-c0a2-44b9-b93d-7e29a1429149" />
Client Version – v1.26.0 y v.1.25.2
Server Version – v1.25.3

Creamos y agregamos la variable de entorno Kubectl:
<img width="659" height="167" alt="image" src="https://github.com/user-attachments/assets/18d06b62-42e6-477d-9290-f78f4d533fee" />
Nombre de la variable: Kubectl
Valor de la variable: C:\Program Files\Docker\Docker\resources\bin\kubectl.exe

Otra variable más:
<img width="644" height="162" alt="image" src="https://github.com/user-attachments/assets/c2d67ad8-c2b7-4c8b-a825-b20460e76a6c" />
<img width="531" height="48" alt="image" src="https://github.com/user-attachments/assets/2b9217ee-6379-4ad2-a5a8-b78a7e03c629" />
Nombre de la variable: kubectl
Valor de la variable: C:\Users\alejandro\Downloads\kubectl.exe


Instalar en Windows usando Chocolatey o Scoop:
Paso 1. Para instalar kubectl en Windows, puede usar Chocolatey
Choco install kubernetes-cli


Paso 2. Use el comando para instalar un administrador de paquetes de chocolate. Una vez que haya copiado el comando, el paquete de chocolate se descargará automáticamente. Puede ver que choco.exe ya está listo. Esto significa que el comando se ha ejecutado con éxito.
IMPORTANTE: EJECUTAR POWERSHELL COMO ADMINISTRADOR PARA HACER LAS PRUEBAS
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
<img width="832" height="460" alt="image" src="https://github.com/user-attachments/assets/f9aabb55-790f-4450-b70c-9be6b834b8bf" />


Paso 3. Escribimos choco para verificar la versión
<img width="453" height="73" alt="image" src="https://github.com/user-attachments/assets/43e4fa94-b6cc-41df-a5b9-21581c3e0dd9" />


Paso 4. Ejecutar choco install minikube. Descargará Kubernetes-cli y el paquete minikube:
Choco install minikube
<img width="843" height="623" alt="image" src="https://github.com/user-attachments/assets/77b7682c-d529-4bfd-8000-1b66228ae5f1" />


Paso 5: Kubernetes y minikube se ha instalado correctamente en la máquina local.
<img width="838" height="485" alt="image" src="https://github.com/user-attachments/assets/dc26d937-9810-45f7-9b97-a22ffcf1273c" />


Paso 6: Una vez que ejecute el comando, llevará algún tiempo descargar minikube iso de Internet. Una vez que termine de descargarse, el clúster de Kubernetes aparecerá en su administrador de Hyper-V.
minikube start -- para iniciar minikube.
<img width="759" height="271" alt="image" src="https://github.com/user-attachments/assets/d33c6938-81f3-4754-ae28-0c97408d49d3" />
<img width="939" height="680" alt="image" src="https://github.com/user-attachments/assets/23c021a9-5f26-4c4b-ad59-4ce9ae52d393" />
minikube stop -- para apagar minikube.
<img width="287" height="62" alt="image" src="https://github.com/user-attachments/assets/bdc12f8a-ed50-41c5-bcd1-b009db702eff" />


Paso 7: Si desea confirmar la información de control de minikube, puede usar el siguiente comando. También puede obtener los detalles de la URL del panel del panel:
kubectl cluster-info
<img width="782" height="111" alt="image" src="https://github.com/user-attachments/assets/baf651b6-a6c7-4a38-9f3a-64901c98d188" />


Paso 8: Si lo desea, puede navegar directamente al tablero usando el siguiente comando. Ahora, mi Kubernetes se ha creado correctamente:
minikube dashboard
<img width="838" height="88" alt="image" src="https://github.com/user-attachments/assets/85123d91-339b-4223-bb12-2e0e75733bd2" />
<img width="953" height="514" alt="image" src="https://github.com/user-attachments/assets/99b37f92-d04f-4b45-8623-1a8df36e5075" />
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
