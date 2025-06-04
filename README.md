### ☁️ Nextwork CI/CD Project - Java Web App Deployment with AWS DevOps Services

Este repositorio documenta cómo configuré un entorno de CI/CD en AWS para desplegar una aplicación web Java de forma automática. Utiliza CodeArtifact, CodePipeline, CodeBuild y CodeDeploy con una instancia EC2 como entorno de destino. Ideal para quienes deseen automatizar el flujo de integración y entrega continua en la nube de manera profesional y escalable.

---

⚙️ Stack Tecnológico
💻 Infraestructura y Lenguajes
•	🖥️ Instancia EC2 (Amazon Linux 2023)
•	☕ Java 8 (Amazon Corretto 8)
•	🔨 Apache Maven 
•	📂 Git y GitHub
•	🖋️ Visual Studio Code con extensión Remote - SSH
•	🌐 Aplicación web generada con maven-archetype-webapp (estructura estándar de proyecto web Java con Maven)
☁️ Servicios de AWS Utilizados
•	⚙️ Amazon EC2 – Servidor para desplegar y ejecutar la aplicación.
•	📦 AWS CodePipeline – Automatiza todo el flujo CI/CD.
•	🔧 AWS CodeBuild – Compila el código y genera artefactos.
•	🚀 AWS CodeDeploy – Despliega automáticamente los cambios a la instancia EC2.
•	🗃️ Amazon S3 (Artifact Store) – Almacena artefactos generados entre etapas del pipeline.
•	🔐 IAM (Roles y permisos) – Permite que los servicios interactúen entre sí de forma segura.
•	🔁 Webhooks de GitHub – Activan automáticamente el pipeline tras cada push.



## 📚 Índice de Contenidos

1. 🔐 [Configuración de un Usuario IAM](#configuración-de-un-usuario-iam)
2. ⚙️ [Instalar Apache Maven y Amazon Corretto 8 en tu instancia EC2](#instalar-apache-maven-y-amazon-corretto-8-en-tu-instancia-ec2)
3. 🔗 [Conectar VS Code con tu Instancia EC2](#conectar-vs-code-con-tu-instancia-ec2)
4. 🖥️ [Configuración de la instancia EC2](#configuración-de-la-instancia-ec2)
5. 🧰 [Instalación de Git en EC2](#instalación-de-git-en-ec2)
6. 🐙 [Crear repositorio en GitHub](#crear-repositorio-en-github)
7. 📁 [Inicializar repositorio Git en EC2](#inicializar-repositorio-git-en-ec2)
8. 💾 [Agregar, commitear y subir los cambios](#agregar-commitear-y-subir-los-cambios)
9. 🔑 [Autenticarse correctamente con GitHub](#autenticarse-correctamente-con-github)
10. 👤 [Configurar identidad de Git](#configurar-identidad-de-git)
11. ✅ [Confirmar archivos en GitHub](#confirmar-archivos-en-github)
12. 📜 [Historial de commits](#historial-de-commits)
13. ✍️ [Segundo commit: edición de `index.jsp`](#segundo-commit-edición-de-indexjsp)
14. 📤 [Subir los cambios al repositorio remoto](#subir-los-cambios-al-repositorio-remoto)
15. 💡 [Tip de productividad: No escribir el token cada vez](#tip-de-productividad-no-escribir-el-token-cada-vez)
16. 🔍 [Verifica tu cambio en GitHub](#verifica-tu-cambio-en-github)
17. 📦 [Crear y Configurar un Repositorio en AWS CodeArtifact](#crear-y-configurar-un-repositorio-en-aws-codeartifact)
18. 🔐 [Crear una política IAM para acceso a CodeArtifact](#crear-una-política-iam-para-acceso-a-codeartifact)
19. 🔗 [Adjuntar la política IAM y verificar la conexión a CodeArtifact](#adjuntar-la-política-iam-y-verificar-la-conexión-a-codeartifact)
20. 📦 [See Packages in CodeArtifact!](#see-packages-in-codeartifact)
21. 🔧 [Connect CodeBuild to your GitHub Repository (continuación)](#connect-codebuild-to-your-github-repository-continuación)
22. ⚙️ [Finish Setting Up Your CodeBuild Project](#finish-setting-up-your-codebuild-project)
23. 🐞 [Ejecutar la compilación y solucionar fallos](#ejecutar-la-compilación-y-solucionar-fallos)
24. ✅ [Verificar compilación exitosa y artefactos](#verificar-compilación-exitosa-y-artefactos)
25. 🚀 [Despliegue de la Infraestructura de Producción con CloudFormation](#despliegue-de-la-infraestructura-de-producción-con-cloudformation)
26. 🧪 [Prepare Deployment Scripts and AppSpec](#prepare-deployment-scripts-and-appspec)
27. 🛠️ [Set Up CodeDeploy](#set-up-codedeploy)
28. 🔍 [Crear y Verificar el Despliegue](#crear-y-verificar-el-despliegue)
29. 🧱 [Configura tu Pipeline](#configura-tu-pipeline)
30. 🧬 [Configuración de las etapas de Source, Build y Deploy](#configuración-de-las-etapas-de-source-build-y-deploy)
31. 🏗️ [Etapa Build (Construcción)](#etapa-build-construcción)
32. ▶️ [¡Ejecuta tu Pipeline!](#ejecuta-tu-pipeline)
33. 🧪 [¡Prueba tu Pipeline!](#prueba-tu-pipeline)


---

🪜 Paso a paso del setup

---
🔐 
## Configuración de un Usuario IAM
Desde la consola de AWS:
•	Inicio de sesión:
Inicia sesión en la Consola de AWS como usuario raíz (root user).
•	¿Qué es un usuario IAM y por qué crearlo?
En AWS, los usuarios IAM permiten controlar de forma segura quién puede acceder a qué recursos.
El usuario raíz es el acceso principal a la cuenta, pero por seguridad NO debes usarlo para tareas del día a día.
En su lugar, crea un usuario IAM con permisos administrativos.
Piensa en el usuario root como una llave maestra, y los usuarios IAM como copias con permisos definidos.
________________________________________
🧰 Pasos para crear un usuario IAM
1.	Abre la Consola de IAM.
2.	En el menú lateral izquierdo, haz clic en Users.
3.	Haz clic en Create user.
4.	En User name, escribe algo como:
TuNombre-IAM-Admin
(Reemplaza “TuNombre” por tu nombre real, por ejemplo Axel-IAM-Admin).
5.	Marca la casilla Provide user access to the AWS Management Console.
6.	Elige la opción Custom password, e ingresa una contraseña segura que puedas recordar.
🔒 Desactiva la casilla “Users must create a new password at next sign-in”.
7.	Haz clic en Next.
________________________________________
🛡️ Asignar permisos al usuario IAM
8.	En la sección Set permissions, selecciona:
Attach policies directly.
9.	Busca y marca la política:
AdministratorAccess
10.	Haz clic en Next y luego en Create user.
________________________________________
💾 Guardar credenciales
11.	En la página de confirmación, haz clic en Download .csv file
(Este archivo contiene tu nombre de usuario, contraseña y URL de inicio de sesión personalizada.)
12.	Copia el Console sign-in URL que aparece ahí.
________________________________________
🔄 Usar el nuevo usuario IAM
13.	Cierra sesión del usuario root.
14.	Abre el enlace de inicio de sesión (Console sign-in URL) que copiaste.
15.	Usa las credenciales del archivo .csv para iniciar sesión con tu nuevo usuario IAM.
16.	✅ ¡Listo! Ya estás usando tu usuario IAM con permisos de administrador, ideal para continuar con tus proyectos en AWS.

🧠 Instalación de Visual Studio Code (VS Code)
Ahora que tu instancia EC2 está en funcionamiento, usaremos Visual Studio Code (VS Code) para conectarnos a ella y configurar tu aplicación web.
________________________________________
🎨 ¿Qué es VS Code?
VS Code es uno de los editores de código más populares del mundo. Se le considera un IDE (Entorno de Desarrollo Integrado) que ayuda a escribir, depurar y administrar proyectos de código.
También se puede usar para conectarse a servidores remotos, como nuestras instancias EC2 en AWS.
________________________________________
💻 Pasos para instalar VS Code
1.	Dirígete al sitio oficial de VS Code:
👉 https://code.visualstudio.com/
2.	Descarga la versión correspondiente a tu sistema operativo (Windows, macOS, Linux).
💡 Si no sabes qué versión necesitas:
o	Mac: Ve al menú  → About This Mac → revisa si tu chip es Apple Silicon o Intel.
o	Windows: Pulsa el botón Inicio → escribe System Information → revisa si tu tipo de sistema es x64 o ARM.
o	Linux: Abre la terminal y ejecuta:
```bash
uname -m
```
Si ves x86_64, estás en una máquina de 64 bits. Si ves arm64 o aarch64, tienes una arquitectura ARM.
3.	Instala VS Code según las instrucciones del instalador.
4.	Abre VS Code desde tu computadora local. Si descargaste un .zip, descomprímelo primero.
________________________________________
🖥️ Configuración del terminal en VS Code
1.	Abre VS Code.
2.	Haz clic en la opción Terminal desde la barra superior.
3.	Selecciona New Terminal.
💡 ¿Qué es un terminal?
Es un lugar donde puedes escribir instrucciones para que tu sistema operativo las ejecute directamente.
4.	Navega a la carpeta donde guardaste tu archivo .pem (por ejemplo: DevOps en el escritorio):
o	En Linux/macOS:
```bash
cd ~/Desktop/DevOps
```
o	En Windows (Opción 1):
```cmd
cd %USERPROFILE%\Desktop\DevOps
```
o	En Windows (Opción 2):
```cmd
cd C:\Users\YourUserName\Desktop\DevOps
```
Reemplaza YourUserName con tu nombre de usuario real, el cual puedes consultar con:
```cmd
Whoami
```
5.	Lista los archivos para confirmar que tu archivo .pem está ahí:
o	En Linux/macOS:
```bash
ls
```
o	En Windows:
```cmd
dir
```
________________________________________
🔐 Cambiar permisos del archivo .pem
Dependiendo del sistema operativo, utiliza los siguientes comandos para asegurar el archivo de clave privada (.pem):
________________________________________
🔒 Linux/macOS
```bash
chmod 400 nextwork-keypair.pem
```
💡 Este comando restringe el acceso al archivo para que solo tú puedas leerlo.
________________________________________
🔒 Windows
```cmd
icacls "nextwork-keypair.pem" /reset
icacls "nextwork-keypair.pem" /grant:r "USERNAME:R"
icacls "nextwork-keypair.pem" /inheritance:r
```
💡 Reemplaza "USERNAME" por tu nombre de usuario real en Windows. Puedes obtenerlo ejecutando:
```cmd
whoami
```
________________________________________
🧩 ¿Errores comunes?
Si algo no funciona como esperas, aquí van algunas verificaciones:
•	¿Tu archivo .pem tiene el nombre correcto en el comando?
•	¿Estás en la carpeta correcta al ejecutar el comando?
•	¿La regla del grupo de seguridad de tu instancia EC2 permite tráfico SSH (puerto 22) desde tu IP?
•	Verifica tu IP actual con una herramienta externa y compárala con la que agregaste en AWS.


🔌 Conexión a tu instancia EC2 desde VS Code (SSH)
Ahora que tienes VS Code instalado y el archivo .pem configurado, es momento de conectarte a tu servidor EC2 usando SSH desde el terminal de VS Code.
________________________________________
🛰️ Obtener la dirección pública de tu EC2
1.	Abre la Consola de Administración de AWS.
2.	Haz clic en Instances en el menú lateral izquierdo.
3.	Marca la casilla junto a tu instancia EC2 para ver sus detalles.
4.	En la pestaña Details, busca el campo llamado:
Public IPv4 DNS
✨ ¡Deja esta pestaña abierta! Lo necesitarás en el siguiente paso.
________________________________________
💡 ¿Qué es un Public IPv4 DNS?
Es una dirección única en Internet que apunta a tu instancia EC2, como si fuera su "nombre público". Tu computadora usará esta dirección para conectarse al servidor remoto EC2.
________________________________________
🧑‍💻 Conectarse a tu instancia EC2 vía SSH desde VS Code
1.	Regresa a Visual Studio Code y abre de nuevo la terminal.
2.	Ejecuta el siguiente comando en la terminal, personalizándolo con tu información:
```bash
ssh -i [RUTA/A/TU/ARCHIVO.pem] ec2-user@[PUBLIC-DNS]
```
Reemplaza:
o	[RUTA/A/TU/ARCHIVO.pem] por la ruta real de tu archivo, por ejemplo:
```bash
~/Desktop/DevOps/nextwork-keypair.pem
```
o	[PUBLIC-DNS] por el valor del campo Public IPv4 DNS que obtuviste desde AWS.
Por ejemplo:
```bash
ec2-user@ec2-3-22-123-456.compute-1.amazonaws.com
```
________________________________________
🧠 ¿Qué hace este comando?
Parte del comando	Significado
ssh	Inicia una conexión segura a través del protocolo SSH
-i	Especifica el archivo de clave privada (.pem) que autentica la conexión
ec2-user@...	Define el usuario y la dirección DNS pública de la instancia EC2
________________________________________
🛠️ Problemas comunes y errores al conectarte
🙋‍♀️ ¿Te aparece un error? ¡No te preocupes! Algunos errores comunes incluyen:
•	❌ "Permission denied"
•	❌ "Cannot find path"
•	❌ "Connection timed out"
•	❌ "Could not establish connection"
•	🌀 SSH se queda cargando por más de 10 minutos
Verifica lo siguiente:
•	Asegúrate de no tener espacios en los nombres de tus carpetas.
Ejemplo: ❌ Dev Ops → ✅ DevOps
•	Confirma que el nombre y la ubicación del archivo .pem son correctos.
•	Verifica que tu grupo de seguridad EC2 permita tráfico SSH (puerto 22) desde tu dirección IP.
•	Usa whoami para asegurarte de estar usando tu nombre de usuario correcto en rutas personalizadas.
•	Asegúrate de que cambiaste los permisos del .pem (ver sección anterior).
________________________________________
🔐 Confirmación del servidor
Cuando conectes por primera vez, tu terminal preguntará si deseas continuar con la conexión:
```bash
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```
👉 Escribe yes y presiona Enter.
________________________________________
💡 ¿Qué es la huella digital del servidor (fingerprint)?
Es un código único que ayuda a verificar que el servidor es seguro y no ha sido alterado. Como es la primera vez que te conectas, tu equipo aún no ha guardado esa huella, así que debes confirmarla manualmente.
________________________________________
🚨 IMPORTANTE
Si no cambiaste los permisos de tu archivo .pem a chmod 400, la conexión será rechazada automáticamente por motivos de seguridad.
________________________________________
✅ ¡Felicidades! Si todo ha salido bien, ahora estás conectado a tu instancia EC2.
🧪 ¿Cómo saber si te conectaste correctamente?
Tu terminal debería cambiar y mostrar algo similar a:
```bash
ec2-user@ip-172-31-XX-XXX:~$
```


⚙️ 
## Instalar Apache Maven y Amazon Corretto 8 en tu instancia EC2
✅ ¡Conexión completada!
Tu terminal ahora controla tu instancia EC2 como si fuera una computadora frente a ti.
En esta etapa, vas a instalar dos herramientas clave para construir aplicaciones web en Java:
•	Apache Maven
•	Amazon Corretto 8 (versión de Java)
________________________________________
📦 ¿Qué es Apache Maven?
Apache Maven es una herramienta para compilar y gestionar proyectos Java. También actúa como un gestor de dependencias, descargando automáticamente bibliotecas externas necesarias.
Además, Maven facilita el inicio de proyectos web gracias a sus arquetipos (plantillas predefinidas). ¡Te ahorra mucho tiempo!
________________________________________
🔧 Instalar Apache Maven en tu instancia EC2
📥 Ejecuta todos estos comandos juntos en la terminal (puedes copiarlos y pegarlos directamente):
```bash
wget https://archive.apache.org/dist/maven/maven-3/3.5.2/binaries/apache-maven-3.5.2-bin.tar.gz
sudo tar -xzf apache-maven-3.5.2-bin.tar.gz -C /opt
echo "export PATH=/opt/apache-maven-3.5.2/bin:$PATH" >> ~/.bashrc
source ~/.bashrc
```
________________________________________
🙋‍♀️ ¿Error wget: command not found?
Esto significa que wget no está instalado por defecto. Soluciónalo así:
```bash
sudo yum install wget -y
```
Luego, vuelve a ejecutar los comandos de instalación de Maven.
________________________________________
💡 ¿Qué hacen esos comandos?
Comando	Descripción
wget ...	Descarga el archivo comprimido de Maven
tar -xzf ...	Extrae el archivo en el directorio /opt
echo "export PATH=..."	Añade Maven al PATH para poder usarlo desde cualquier ubicación
source ~/.bashrc	Aplica los cambios al entorno de la terminal
________________________________________
☕ Instalar Amazon Corretto 8 (Java 8)
Maven requiere Java para funcionar, así que ahora instalarás Amazon Corretto 8, una distribución gratuita y confiable de Java mantenida por AWS.
Ejecuta estos comandos en tu terminal EC2:
```bash
sudo dnf install -y java-1.8.0-amazon-corretto-devel
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-amazon-corretto.x86_64
export PATH=/usr/lib/jvm/java-1.8.0-amazon-corretto.x86_64/jre/bin/:$PATH
```
________________________________________
💡 ¿Qué hacen estos comandos?
Comando	Descripción
sudo dnf install ...	Instala Amazon Corretto 8
export JAVA_HOME=...	Define la variable de entorno para indicar dónde está instalado Java
export PATH=...	Permite ejecutar comandos Java desde cualquier carpeta
________________________________________
✅ Verificar las instalaciones
1.	Verifica Maven:
```bash
mvn -v
```
✅ Si ves una versión como Apache Maven 3.5.2, ¡todo está correcto!
🙋‍♀️ ¿No ves un número de versión?
•	Ejecuta también mvn -v en tu terminal local para comparar.
•	Si solo hay problema en EC2, revisa tu PATH o vuelve a instalar Maven.
•	Si tienes problemas en local, revisa tu JDK.
________________________________________
2.	Verifica Java:
```bash
java -version
```
✅ Deberías ver algo como:
```nginx
openjdk version "1.8.0_... Amazon Corretto"
```
🙋‍♀️ ¿No aparece Java 8?
Ejecuta este comando para seleccionar la versión correcta:
```bash
sudo alternatives --config java
```
________________________________________
🧠 Notas importantes
•	El texto que aparece tras ejecutar los comandos indica el progreso de la instalación.
•	Si encuentras errores durante la instalación de Maven o Java, compártelos con la comunidad NextWork.


🚀 Crear la Aplicación Web con Maven
Ahora que tienes Maven y Java instalados en tu instancia EC2, ¡es momento de generar tu aplicación web Java!
Vamos a usar Maven para crear un proyecto base usando un arquetipo (plantilla).
________________________________________
🛠️ Generar una aplicación web Java
Ejecuta el siguiente comando en tu terminal EC2 para generar el proyecto:
```bash
mvn archetype:generate \
  -DgroupId=com.nextwork.app \
  -DartifactId=nextwork-web-project \
  -DarchetypeArtifactId=maven-archetype-webapp \
  -DinteractiveMode=false
```
________________________________________
💡 ¿Qué hace este comando?
Este comando le dice a Maven que:
Parámetro	Significado
mvn archetype:generate	Inicia la generación de un nuevo proyecto desde un arquetipo (plantilla)
-DgroupId=com.nextwork.app	Define el grupo del proyecto, normalmente basado en el dominio de tu organización
-DartifactId=nextwork-web-project	Nombra tu proyecto; será también el nombre del directorio generado
-DarchetypeArtifactId=maven-archetype-webapp	Indica que deseas crear una aplicación web Java básica
-DinteractiveMode=false	Ejecuta sin pedir confirmación o datos adicionales
________________________________________
✅ Resultado esperado
Después de ejecutar el comando, deberías ver un mensaje como:
```csharp
[INFO] BUILD SUCCESS
```
Esto indica que Maven ha creado con éxito la estructura base de tu aplicación web.
________________________________________
📁 Nuevo directorio creado:
Al finalizar, deberías ver un nuevo directorio llamado nextwork-web-project, que contiene la estructura inicial de tu app web en Java.



🔗 
## Conectar VS Code con tu Instancia EC2
Ahora que creaste la estructura de tu aplicación web, es momento de conectarte desde Visual Studio Code (VS Code) a tu instancia EC2 para visualizar, editar y trabajar fácilmente con los archivos de tu aplicación Java.
________________________________________
🧭 ¿Por qué conectar VS Code?
Aunque ya tienes conexión SSH desde la terminal, conectarte directamente desde VS Code te permitirá:
•	Navegar por archivos como si estuvieran en tu computadora local
•	Editar archivos fácilmente con todas las ventajas del IDE
•	Ahorrar tiempo en tareas de desarrollo gracias a la integración con herramientas como Git, extensiones y resaltado de sintaxis
________________________________________
🧩 Paso 1: Instalar la extensión Remote - SSH
1.	Abre VS Code
2.	Haz clic en el ícono de Extensiones
3.	Busca Remote - SSH
4.	Haz clic en Instalar
________________________________________
🚀 Paso 2: Conectarte a la instancia EC2 desde VS Code
1.	Haz clic en el ícono de doble flecha (Remote Explorer) en la esquina inferior izquierda
2.	Selecciona Connect to Host...
3.	Luego selecciona + Add New SSH Host...
4.	Ingresa el siguiente comando, reemplazando los valores:
```bash
ssh -i ~/Desktop/DevOps/nextwork-keypair.pem ec2-user@<YOUR_PUBLIC_IPV4_DNS>
```
•	Reemplaza ~/Desktop/DevOps/nextwork-keypair.pem por la ruta de tu archivo .pem
•	Reemplaza <YOUR_PUBLIC_IPV4_DNS> por el DNS público de tu instancia EC2 (sin los <>)
5.	Selecciona el archivo de configuración SSH sugerido (ej. /Users/tu-usuario/.ssh/config)
6.	Haz clic en Open Config para verificar:
```ssh
Host ec2-nextwork
  HostName <YOUR_PUBLIC_IPV4_DNS>
  User ec2-user
  IdentityFile ~/Desktop/DevOps/nextwork-keypair.pem
```
7.	Guarda los cambios
8.	Haz clic nuevamente en el ícono de doble flecha y elige Connect to Host → Selecciona tu instancia EC2
📍 Verifica en la esquina inferior derecha de VS Code: deberías ver el DNS público de tu EC2 si todo salió bien.
________________________________________
📁 Paso 3: Abrir el proyecto Java
1.	Haz clic en el ícono de Explorador de Archivos (Explorer)
2.	Selecciona Open Folder
3.	Ingresa la ruta:
```bash
/home/ec2-user/nextwork-web-project
```
4.	Si aparece un popup preguntando si confías en los autores, selecciona Yes, I trust the authors
________________________________________
🧪 Explorar archivos del proyecto
•	src/: contiene todo el código fuente
o	webapp/: incluye archivos HTML, CSS, JS, y JSP
o	resources/: archivos de configuración
•	pom.xml: archivo clave para Maven, contiene la configuración del proyecto
________________________________________
✏️ Paso 4: Editar el archivo index.jsp
1.	Navega hasta src/main/webapp/index.jsp
2.	Reemplaza el contenido con el siguiente código, cambiando {YOUR NAME} por tu nombre:
```Html
<html>
  <body>
    <h2>Hello {YOUR NAME}!</h2>
    <p>This is my NextWork web application working!</p>
  </body>
</html>
```
3.	Guarda los cambios con Ctrl + S o Cmd + S
________________________________________
🎉 ¡Listo!
Has logrado:
•	Crear una app web en una instancia EC2
•	Conectarte a ella desde VS Code
•	Explorar y modificar archivos directamente desde el IDE
¡Todo está preparado para seguir construyendo sobre esta base durante el resto de la serie DevOps!



🖥️
## Configuración de la instancia EC2

Desde la consola de AWS:

* **Nombre de la instancia:**
  `nextwork-devops-axel`
  *(Reemplaza “axel” con tu nombre si estás replicando este tutorial).*

* **AMI:**
  Amazon Linux 2023 AMI

* **Tipo de instancia:**
  t2.micro (gratis en capa free tier)

* **Par de llaves (Key Pair):**

  * Si no tienes una:
    → Crea una llamada `nextwork-keypair` y descarga el `.pem`
  * Si ya tienes una:
    → Usa `nextwork-keypair.pem` existente.

* **Ruta recomendada del archivo PEM local:**
  `Escritorio/DevOps/nextwork-keypair.pem`

---

🧰  
## Instalación de Git en EC2

Abre la terminal en tu instancia EC2 y ejecuta:


```bash
sudo dnf update -y
sudo dnf install git -y
git --version
```


✅ Esto actualiza el sistema e instala Git correctamente.

---

🐙  
## Crear repositorio en GitHub

Desde [https://github.com](https://github.com):

* Repository name: `nextwork-web-project`
* Description: *Java web app set up on an EC2 instance*
* Visibility: Public
* Haz clic en **Create repository**

---

📁  
## Inicializar repositorio Git en EC2

Desde la terminal:


cd ~/nextwork-web-project
git init


Conecta el repositorio local con GitHub:


git remote add origin https://github.com/tuusuario/nextwork-web-project.git


---

💾  
## Agregar, commitear y subir los cambios


```bash
git add .
git diff --staged
git commit -m "Updated index.jsp with new content"
git push -u origin master
```


---
🔑  
## Autenticarse correctamente con GitHub
GitHub ya no acepta contraseñas normales por HTTPS. Debes usar un **Personal Access Token (PAT)**:

Desde GitHub:

* Settings → Developer settings → Personal access tokens → Tokens (classic)
* Generate token con permisos `repo`
* Guarda el token de forma segura (¡solo se muestra una vez!)

Al hacer `git push`, ingresa:

* Usuario: tu username de GitHub
* Contraseña: pega el token generado

---

👤  
## Configurar identidad de Git


```bash
git config --global user.name "Nombre"
git config --global user.email ejemplo@gmail.com
git config --global --list
```


---

✅  
## Confirmar archivos en GitHub

Visita tu repositorio y deberías ver:

* Todos los archivos del proyecto
* El mensaje del commit
* La rama `master` vinculada

---

📜  
## Historial de commits

```bash
git log
```

Muestra:

* ID del commit
* Autor
* Fecha
* Descripción del cambio

---

✍️  
## Segundo commit: edición de `index.jsp`

Abre `src/main/webapp/index.jsp` desde VSCode (conectado por Remote SSH) y agrega esta línea:
<!-- -------------------------------------------------- -->
```html
<p>If you see this line in Github, that means your latest changes are getting pushed to your cloud repo :o</p>
```
<!-- -------------------------------------------------- -->
<!-- -------------------------------------------------- -->
Guarda, y ejecuta:
<!-- -------------------------------------------------- -->
<!-- -------------------------------------------------- -->
```bash
git add .
git commit -m "Add new line to index.jsp"
git push
```
<!-- -------------------------------------------------- -->
<!-- -------------------------------------------------- -->
Verifica los cambios en GitHub.

📤  
## Subir los cambios al repositorio remoto
📦 Etapa 1: Staging
En la terminal de VSCode, ejecuta:
<!-- -------------------------------------------------- -->
```bash
git add .
```
<!-- -------------------------------------------------- -->
Esto prepara todos los archivos modificados para ser incluidos en el próximo commit.
<!-- -------------------------------------------------- -->
🔍 Etapa 2: Verifica los cambios que vas a guardar'
<!-- -------------------------------------------------- -->
```bash
git diff --staged
```
<!-- -------------------------------------------------- -->
Esto te muestra una comparación entre la última versión confirmada (commit anterior) y los cambios que están listos para confirmar.
💡 Si se queda “pegado”, presiona q para salir de la vista y volver a la terminal.
<!-- -------------------------------------------------- -->
📝 Etapa 3: Hacer commit
<!-- -------------------------------------------------- -->
```bash
git commit -m "Add new line to index.jsp"
```
<!-- -------------------------------------------------- -->
Esto crea un nuevo commit con un mensaje descriptivo.
<!-- -------------------------------------------------- -->
🚀 Etapa 4: Hacer push
<!-- -------------------------------------------------- -->
```bash
git push
```
<!-- -------------------------------------------------- -->
Git puede pedirte nuevamente:
• Tu usuario de GitHub
• Tu token personal de acceso (PAT) como contraseña


💡  
## Tip de productividad: No escribir el token cada vez
Ejecuta este comando después del push para que Git recuerde tus credenciales:
<!-- -------------------------------------------------- -->
```bash
git config --global credential.helper store
```
<!-- -------------------------------------------------- -->
Así no tendrás que pegar el token cada vez que haces push o pull.

🔍  
## Verifica tu cambio en GitHub
1.	Abre tu repositorio en GitHub.
2.	Navega a src/main/webapp/index.jsp
3.	Refresca la página.
✅ ¡Tu nueva línea HTML debería estar ahí!


📦  
## Crear y Configurar un Repositorio en AWS CodeArtifact
En este paso configuramos un repositorio privado para gestionar las dependencias de nuestro proyecto Java. Esto mejora la seguridad, consistencia y velocidad de los builds.
<!-- -------------------------------------------------- -->
🔐 ¿Qué es AWS CodeArtifact?
Es un servicio de AWS que proporciona un repositorio seguro para almacenar dependencias y paquetes utilizados en tus proyectos (como los .jar de Maven). Sustituye la descarga directa de Internet por un flujo controlado, seguro y replicable.
<!-- -------------------------------------------------- -->
🧱 Beneficios de usar CodeArtifact
1.	Seguridad: Evita descargar paquetes desde fuentes inseguras.
2.	Confiabilidad: Si Maven Central falla, tu repositorio privado sigue funcionando.
3.	Control: Puedes auditar y bloquear dependencias específicas.
<!-- -------------------------------------------------- -->
🔨 Crear el Repositorio
1.	Ve a la consola de AWS CodeArtifact.
2.	En el menú izquierdo, haz clic en Repositories.
3.	Haz clic en Create repository.
<!-- -------------------------------------------------- -->
Configuración del repositorio:
•	Repository name: nextwork-devops-cicd
<!-- -------------------------------------------------- -->
•	Repository description:
"This repository stores packages related to a Java web app created as a part of NextWork's CI/CD Pipeline series."
<!-- -------------------------------------------------- -->
•	Marca la casilla:
✅ maven-central-store (como upstream repository)
<!-- -------------------------------------------------- -->
🔁 ¿Qué son los Upstream Repositories?
Son repositorios públicos conectados a tu repositorio privado. Si falta una dependencia en tu repositorio, se buscará automáticamente en el upstream (como Maven Central) y se cacheará localmente para futuros usos.
<!-- -------------------------------------------------- -->
📁 Crear el Dominio
1.	En la sección Domain selection, elige:
✅ This AWS account
<!-- -------------------------------------------------- -->
3.	Domain name: nextwork
Esto agrupa múltiples repositorios bajo un mismo dominio, facilitando el manejo de permisos y seguridad.
<!-- -------------------------------------------------- -->
✅ Revisión final
Antes de hacer clic en Create repository, verifica:
•	✅ Repository name: nextwork-devops-cicd
•	✅ Domain name: nextwork
•	✅ Public upstream: maven-central-store
<!-- -------------------------------------------------- -->
🏁 Crear el repositorio
Haz clic en Create repository.
Deberías ver un mensaje de éxito confirmando que nextwork-devops-cicd fue creado correctamente.


🔐  
## Crear una política IAM para acceso a CodeArtifact
Para que Maven pueda trabajar con CodeArtifact, necesitamos crear un rol IAM que le dé permiso a nuestra instancia EC2 para acceder a CodeArtifact.
De lo contrario, Maven puede intentar todo lo que quiera, pero la instancia EC2 no podrá almacenar ni recuperar paquetes desde CodeArtifact porque no tiene permisos.
Recordemos que los roles IAM están compuestos por políticas, así que primero crearemos una política.
<!-- -------------------------------------------------- -->
En este paso harás:
• Intentar conectar Maven con CodeArtifact (¡y fallar!)
• Crear una nueva política IAM.
• Configurar la política para que la instancia EC2 tenga acceso a CodeArtifact.
• Consultar las instrucciones de conexión de CodeArtifact.
<!-- -------------------------------------------------- -->
🔗 1. Ver instrucciones de conexión
• En la página de tu repositorio CodeArtifact recién creado, haz clic en View connection instructions arriba a la derecha.
• Configura:
•	Sistema operativo: Mac y Linux
•	Cliente gestor de paquetes: mvn (Maven)
💡 Aunque estés usando Windows localmente, la instancia EC2 usa Amazon Linux 2023, por eso seleccionamos Mac y Linux.
• Asegúrate que el método de configuración esté en Pull from your repository.
<!-- -------------------------------------------------- -->
🔐 2. Exportar token de autorización CodeArtifact
• Busca el paso 3: Exportar un token de autorización.
• Copia y ejecuta el comando completo en la terminal de tu instancia EC2.
Este token es como una contraseña temporal que permite a Maven acceder a tu repositorio CodeArtifact.
<!-- -------------------------------------------------- -->
❌ 3. ¡Error! "Unable to locate credentials"
Este error indica que tu instancia EC2 no tiene permisos para acceder a CodeArtifact.
Es una medida de seguridad: por defecto, las instancias no pueden acceder a otros servicios AWS a menos que se les asigne un rol con permisos específicos.
<!-- -------------------------------------------------- -->
🛠️ 4. Crear una nueva política IAM
1.	Abre el servicio IAM en la consola de AWS.
2.	En el menú izquierdo, haz clic en Policies (Políticas).
3.	Haz clic en Create policy (Crear política).
4.	Ve a la pestaña JSON y reemplaza el contenido con el siguiente código:
<!-- -------------------------------------------------- -->
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "codeartifact:GetAuthorizationToken",
        "codeartifact:GetRepositoryEndpoint",
        "codeartifact:ReadFromRepository"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "sts:GetServiceBearerToken",
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "sts:AWSServiceName": "codeartifact.amazonaws.com"
        }
      }
    }
  ]
}
```
📌 Esta política:
• Permite obtener tokens y endpoints.
• Autoriza lectura desde repositorios.
• Usa sts:GetServiceBearerToken exclusivamente para CodeArtifact.
• Aplica a todos los recursos ("Resource": "*") por simplicidad (idealmente, deberías restringirlo en producción).
<!-- -------------------------------------------------- -->
🧾 5. Finalizar creación de la política
• Haz clic en Next.
• Asigna el nombre:
codeartifact-nextwork-consumer-policy
• En la descripción, puedes usar algo como:
"Permisos para leer desde CodeArtifact. Creado para la serie NextWork CI/CD Pipeline."
• Haz clic en Create policy.
<!-- -------------------------------------------------- -->
⚠️ 6. Problemas comunes al crear la política
• Asegúrate de no usar apóstrofes ('), barras diagonales o caracteres no válidos en la descripción.
• Intenta con descripciones simples como:
"Permisos CodeArtifact para NextWork"


🔗  
## Adjuntar la política IAM y verificar la conexión a CodeArtifact
Ahora que tienes la política creada, el siguiente paso es:
• Crear un rol IAM para EC2 con esta política.
• Asignar este rol a tu instancia EC2.
• Reintentar el comando de exportación del token.
<!-- -------------------------------------------------- -->
🔧 Crear un nuevo rol IAM para EC2
1.	Ve a IAM > Roles
2.	Haz clic en Create role
3.	Selecciona:
o	Trusted entity type: AWS service
o	Use case: EC2
4.	Haz clic en Next
5.	En Attach permissions policies, busca y selecciona:
codeartifact-nextwork-consumer-policy
6.	Haz clic en Next
7.	Nombre del rol:
EC2-instance-nextwork-cicd
8.	(Opcional) Descripción:
"Rol para que EC2 acceda a CodeArtifact. Proyecto NextWork CI/CD."
9.	Haz clic en Create role
<!-- -------------------------------------------------- -->
🖇️ Asociar el rol a tu instancia EC2
1.	Ve a EC2 > Instances
2.	Selecciona tu instancia (por ejemplo: nextwork-devops-yourname)
3.	Haz clic en:
Actions > Security > Modify IAM Role
4.	Haz clic en Update, selecciona:
EC2-instance-nextwork-cicd
5.	Haz clic en Update IAM role
6.	Verifica el banner verde de confirmación
<!-- -------------------------------------------------- -->
🧠 ¿Cómo funciona?
Cuando una instancia EC2 tiene un rol IAM, AWS le otorga credenciales temporales y rotadas automáticamente.
Tu instancia no necesita claves manuales, lo que es más seguro y escalable.
<!-- -------------------------------------------------- -->
🔁 Volver a ejecutar el token de conexión
• Conéctate de nuevo a tu instancia EC2 (por terminal o SSH).
• Ejecuta el comando de exportación del token desde las instrucciones de conexión de CodeArtifact.
📌 Esta vez, debería funcionar correctamente si el rol fue aplicado con éxito.
<!-- -------------------------------------------------- -->
📌 Recordatorio:
•	El token es temporal (12 horas aprox.).
•	Es seguro y no requiere que almacenes claves en tu instancia.
<!-- -------------------------------------------------- -->
❗ ¿Todavía ves "Unable to locate credentials"?
• Espera unos minutos: el cambio puede tardar en propagarse.
• Revisa que el rol está asignado en la consola EC2.
• Verifica que la política tenga los permisos correctos.
• Si nada funciona, prueba reiniciar la instancia EC2.


📦  
## See Packages in CodeArtifact!
Ahora que hemos creado la IAM Role, la hemos asociado a nuestra instancia EC2 y hemos podido obtener el token de autorización para CodeArtifact, es hora de verificar que todo funcione correctamente.
¿Qué vamos a hacer?
•	Terminar de configurar Maven para que use CodeArtifact.
•	Compilar nuestro proyecto Maven usando el archivo settings.xml.
•	Verificar que las dependencias se descarguen desde CodeArtifact.
•	Confirmar que las dependencias queden almacenadas en CodeArtifact para futuros builds.
<!-- -------------------------------------------------- -->
Paso 1: Crear y configurar el archivo settings.xml
1.	Dirígete al directorio raíz de tu proyecto Maven (nextwork-web-project) en VS Code.
2.	Crea un archivo nuevo llamado settings.xml en la raíz del proyecto.
💡 ¿Qué es settings.xml?
Es el archivo de configuración global para Maven. Aquí indicamos a Maven dónde buscar las dependencias y cómo autenticarse con los repositorios (en este caso, CodeArtifact).
3.	Abre settings.xml y agrega la estructura básica:
<settings>
</settings>
4.	Ve a la consola de AWS CodeArtifact en tu navegador, a la sección de instrucciones de conexión, y copia los fragmentos de código XML de los pasos 4, 5 y 6 (que contienen la configuración para <servers>, <profiles> y <mirrors>).
5.	Pega los fragmentos dentro de las etiquetas <settings> en este orden:
<!-- -------------------------------------------------- -->
<settings>
  <!-- Paso 4: Servidores -->
  <servers>
    <!-- Copia y pega aquí la sección servers que contiene los datos de autenticación -->
  </servers>

  <!-- Paso 5: Perfiles -->
  <profiles>
    <!-- Copia y pega aquí la configuración del perfil que indica el repositorio CodeArtifact -->
  </profiles>

  <!-- Paso 6: Espejos (mirrors) -->
  <mirrors>
    <!-- Copia y pega aquí la configuración de espejo para redirigir solicitudes al repositorio CodeArtifact -->
  </mirrors>
</settings>
<!-- -------------------------------------------------- -->
6.	Guarda el archivo.
<!-- -------------------------------------------------- -->
Paso 2: Compilar el proyecto con Maven y verificar la conexión
1.	En la terminal integrada de VS Code, asegúrate de estar en el directorio raíz de tu proyecto:
pwd
Si no estás, navega hasta él:
cd nextwork-web-project
2.	Ejecuta el comando para compilar tu proyecto usando el archivo settings.xml que acabas de crear:

```bash
mvn -s settings.xml compile
```

3.	Observa la salida en la terminal:
•	Deberías ver mensajes que indican que Maven está descargando paquetes desde el repositorio nextwork-devops-cicd (tu repositorio CodeArtifact).
•	Al final, si todo sale bien, deberías ver un mensaje como:
[INFO] BUILD SUCCESS
<!-- -------------------------------------------------- -->
Troubleshooting común: error "401 Unauthorized"
Si ves un error de autenticación, prueba estos pasos:
•	Confirma que estás usando las instrucciones de conexión para Linux/MacOS (aunque trabajes en Windows, si usas WSL o EC2).
•	Verifica que el token de autorización existe y es correcto:
<!-- -------------------------------------------------- -->
```bash
echo $CODEARTIFACT_AUTH_TOKEN
```
<!-- -------------------------------------------------- -->
•	Revisa que tu archivo settings.xml coincida exactamente con las instrucciones de CodeArtifact.
•	Asegúrate que la IAM Role con la política correcta está asociada a tu instancia EC2.
•	Como último recurso, limpia el caché de Maven y vuelve a compilar:
<!-- -------------------------------------------------- -->
```bash
rm -rf ~/.m2/repository
mvn -s settings.xml compile
```
<!-- -------------------------------------------------- -->
Paso 3: Verificar los paquetes en CodeArtifact
•	Vuelve al navegador y recarga la consola de CodeArtifact.
•	En la pestaña de Packages, haz clic en el botón de refrescar.
•	Ahora deberías ver las dependencias que Maven ha descargado y almacenado en tu repositorio CodeArtifact.
💡 ¿Por qué aparecen esos paquetes?
Cuando compilaste el proyecto, Maven pidió las dependencias indicadas en tu pom.xml. CodeArtifact, al ser el repositorio configurado, no las tenía almacenadas aún, así que las descargó desde Maven Central, las guardó en CodeArtifact y se las entregó a Maven.
Esto significa que ahora, otros desarrolladores o sistemas de CI/CD en tu organización podrán descargar esas dependencias directamente desde CodeArtifact, mejorando la velocidad y seguridad del proceso.


🔧  
## Connect CodeBuild to your GitHub Repository (continuación)
Próximos pasos para configurar CodeBuild con GitHub
1.	Selecciona tu repositorio GitHub en CodeBuild
o	Después de haber conectado exitosamente tu cuenta GitHub con AWS a través de CodeConnections, en la consola de CodeBuild, en la sección Source, ahora podrás seleccionar tu repositorio nextwork-web-project como fuente del código.
o	Esto permitirá que CodeBuild extraiga directamente el código desde tu repositorio privado cada vez que ejecutes un build o configures un pipeline.
2.	Configura los detalles del proyecto de CodeBuild
o	Build environment (Entorno de compilación):
	Selecciona una imagen de entorno adecuada para tu proyecto (por ejemplo, una imagen estándar de Amazon Linux con soporte para Maven si estás usando Java).
	Configura la versión de runtime que necesitas.
o	Buildspec:
	Puedes usar un archivo buildspec.yml en la raíz de tu repositorio para definir los comandos de compilación y pruebas.
	Alternativamente, puedes ingresar comandos directamente en la consola de CodeBuild.
o	Service Role (Rol de servicio):
	Asegúrate de que CodeBuild tenga un rol IAM con los permisos necesarios para acceder a CodeArtifact y a otros recursos que tu build necesite (por ejemplo, permisos para obtener paquetes, acceder a S3, etc.).
3.	Ejecuta un build para probar la integración
o	Ejecuta una compilación manual en CodeBuild para asegurarte que:
	El código se baja correctamente desde tu repositorio GitHub.
	Los paquetes Maven se descargan desde CodeArtifact (confirmado en el paso anterior).
	El proyecto compila sin errores.
o	Observa los logs de build para verificar que todo está funcionando como se espera.
<!-- -------------------------------------------------- -->
¿Por qué usar CodeConnections para conectar CodeBuild y GitHub?
•	Seguridad: AWS maneja la autenticación y el token de acceso, eliminando la necesidad de almacenar credenciales sensibles manualmente.
•	Facilidad: Configurar la conexión una sola vez y reutilizarla para varios proyectos o builds.
•	Confiabilidad: Reduce errores humanos y problemas por tokens caducados o mal configurados.
<!-- -------------------------------------------------- -->
Recapitulando
•	AWS CodeConnections actúa como puente seguro entre CodeBuild y tu repositorio GitHub privado.
•	Con esta conexión, CodeBuild puede extraer tu código fuente de forma automática y segura.
•	La integración completa te permite automatizar la compilación, pruebas y despliegue en AWS de forma confiable y controlada.


⚙️  
## Finish Setting Up Your CodeBuild Project
1. Configure Build Environment
•	Primary source webhook events:
o	Desmarca la casilla que dice:
"Rebuild every time a code change is pushed to this repository."
Esto evita builds automáticos. Por ahora usaremos ejecución manual.
•	Environment > Compute > Provisioning model:
o	Selecciona On-demand.
Crea los recursos solo cuando los necesites.
•	Environment > Environment image:
o	Selecciona Managed image.
•	Environment > Compute type:
o	Selecciona EC2.
Recomendado porque Java Corretto 8 no está soportado en Lambda.
•	Environment > Operating system:
o	Selecciona Amazon Linux.
•	Environment > Runtime(s):
o	Selecciona Standard.
•	Environment > Image:
o	Escoge aws/codebuild/amazonlinux-x86_64-standard:corretto8.
o	Mantén la opción:
“Always use the latest image for this runtime version”.
•	Service role:
o	Selecciona New service role para que AWS cree uno por ti.
<!-- -------------------------------------------------- -->
2. Specify Build Instructions (buildspec)
•	En la sección Buildspec:
o	Selecciona Use a buildspec file.
o	Deja el nombre como: buildspec.yml.
Este archivo debe estar en la raíz de tu repositorio.
<!-- -------------------------------------------------- -->
3. Batch Configuration (Opcional)
•	Puedes ignorar esta sección por ahora.
No usaremos builds en batch.
<!-- -------------------------------------------------- -->
4. Configure Build Artifacts
•	En la sección Artifacts:
o	Para Type, selecciona Amazon S3.
<!-- -------------------------------------------------- -->
5. Crear Bucket en Amazon S3 para los artefactos
•	Ve a la consola de S3 (busca “S3” en la barra superior de AWS).
•	Asegúrate de estar en la misma región que tu proyecto CodeBuild.
•	Haz clic en Create bucket.
•	En Bucket name, escribe:
nextwork-devops-cicd-yourname (reemplaza yourname).
•	Deja las demás opciones por defecto y crea el bucket.
<!-- -------------------------------------------------- -->
6. Volver a CodeBuild y elegir el bucket
•	Vuelve a la consola de CodeBuild.
•	Si no aparece el bucket, refresca la página.
•	En Artifacts > Bucket name, selecciona el bucket recién creado.
•	En Name, escribe: nextwork-devops-cicd-artifact.
•	En Artifacts packaging, selecciona Zip.
<!-- -------------------------------------------------- -->
7. Configurar CloudWatch Logs para monitorear el build
•	En la sección Logs:
o	Marca la opción CloudWatch logs.
o	En Group name, escribe:
/aws/codebuild/nextwork-devops-cicd
<!-- -------------------------------------------------- -->
8. Crear el proyecto
•	Desplázate hasta el final y haz clic en Create build project.
<!-- -------------------------------------------------- -->
¿Qué sigue?
•	Puedes iniciar tu primer build manual para validar la configuración.
•	Revisa los logs en CloudWatch para depuración.
•	Cuando el build termine, el archivo .war empaquetado estará disponible en tu bucket S3, listo para desplegar.



🐞  
## Ejecutar la compilación y solucionar fallos
Ahora que nuestro proyecto de CodeBuild está completamente configurado, ¡iniciemos nuestra primera compilación y veamos nuestra pipeline de CI en acción!
<!-- -------------------------------------------------- -->
En este paso, vas a:
• Iniciar tu primera compilación en CodeBuild.
• Solucionar un fallo de compilación añadiendo un archivo buildspec.yml a tu repositorio de la aplicación web. ¡Prepárate para correr tu primera compilación! Está bien si falla al principio — eso es parte del proceso de aprendizaje. Serás redirigido a la página de ejecución de la compilación.
<!-- -------------------------------------------------- -->
Deberías ver el estado de la compilación cambiar a In progress (En progreso).
Espera a que la compilación termine.
<!-- -------------------------------------------------- -->
¡Oh no, parece que la compilación falló!
<!-- -------------------------------------------------- -->
Al revisar los registros y los detalles de las fases, podemos identificar exactamente dónde y por qué nuestra compilación falló.
Selecciona la pestaña Phase details (Detalles de la fase) para entender el error.
<!-- -------------------------------------------------- -->
Navega a tu proyecto de CodeBuild recién creado llamado nextwork-devops-cicd.
Haz clic en el botón Start build (Iniciar compilación).
¡Ajá! La fase DOWNLOAD_SOURCE falló con el mensaje de error:
YAML_FILE_ERROR: YAML file does not exist 🙋‍♀️
¿Por qué veo este error?
¡Buenas noticias! Esto es exactamente lo que esperábamos que pasara. Este error simplemente significa que CodeBuild no pudo encontrar el archivo buildspec.yml en tu repositorio de GitHub, lo cual tiene sentido porque aún no lo hemos creado.
Este es un momento importante para aprender: sin este archivo, CodeBuild no sabe cómo construir tu proyecto. Crearemos este archivo en los próximos pasos.
<!-- -------------------------------------------------- -->
Haz clic en la pestaña Build details (Detalles de compilación).
Desplázate hacia abajo hasta la sección Buildspec.
Asegúrate que diga:
Using the buildspec.yml in the source code root directory
Esto confirma que CodeBuild está buscando el archivo buildspec.yml en el directorio raíz del repositorio de código de tu aplicación web nextwork-web-project... ¡y aún no hemos creado ese archivo!
<!-- -------------------------------------------------- -->
Como habrás notado, no hay archivo buildspec.yml — debería estar en la raíz de tu repositorio 🙈 Vamos a crearlo ahora.
<!-- -------------------------------------------------- -->
💡 Recuérdame: ¿para qué sirve buildspec.yml?
CodeBuild lee tu archivo buildspec.yml como un manual paso a paso. Va a través de cada fase en orden: install, pre_build, build y luego post_build, ejecutando los comandos que hayas especificado en cada sección.
Lo genial de usar buildspec.yml es que el proceso de construcción está definido como código, justo al lado de tu aplicación. Esto significa que tu proceso de build puede versionarse, revisarse y evolucionar como cualquier otra parte de tu base de código. Antes de la era de integración continua (CI), tenías que ejecutar muchos comandos manualmente para construir tu aplicación — ¡buildspec.yml automatiza todo este proceso!
<!-- -------------------------------------------------- -->
Vamos a revisar nuestro repositorio de GitHub. Selecciona el enlace del repositorio en tu proyecto CodeBuild. Abre tu proyecto en VS Code o en tu editor de código preferido.
Abre la carpeta nextwork-web-project.
Selecciona Ok.
En VS Code, en el panel del explorador, crea un nuevo archivo en la raíz del proyecto (nextwork-web-project).
Nómbralo buildspec.yml. Verifica cuidadosamente:
• ¿Está exactamente nombrado como buildspec.yml?
• ¿Está ubicado en la raíz de tu proyecto, no dentro de subcarpetas como src o target?
Pega el siguiente código para el archivo buildspec.yml:
<!-- -------------------------------------------------- -->
```yaml
version: 0.2  
phases:  
  install:  
    runtime-versions:  
      java: corretto8  
  pre_build:  
    commands:  
      - echo Initializing environment  
      - export CODEARTIFACT_AUTH_TOKEN=`aws codeartifact get-authorization-token --domain nextwork --domain-owner 123456789012 --region us-east-2 --query authorizationToken --output text`  

  build:  
    commands:  
      - echo Build started on `date`  
      - mvn -s settings.xml compile  
  post_build:  
    commands:  
      - echo Build completed on `date`  
      - mvn -s settings.xml package  
artifacts:  
  files:  
    - target/nextwork-web-project.war  
  discard-paths: no
```
<!-- -------------------------------------------------- -->
💡 ¿Qué contiene buildspec.yml?
Este archivo buildspec.yml es como una receta para construir tu aplicación web Java. Vamos a desglosarlo en términos simples:
• version: 0.2: Indica a AWS qué versión del formato buildspec estamos usando.
• phases: Son las distintas etapas que atraviesa la compilación:
o install: Fase de preparación, aquí le decimos a CodeBuild que use Java 8.
o pre_build: Tareas a hacer antes de empezar a compilar. Aquí obtenemos un token de seguridad para acceder a dependencias.
o build: Donde ocurre la compilación real usando Maven para compilar el código.
o post_build: Toques finales después de compilar, donde empaquetamos todo en un archivo WAR (formato para aplicaciones web).
• artifacts: Indica a CodeBuild qué archivos guardar como resultado final. En nuestro caso, el archivo WAR que creamos en la fase post_build.
<!-- -------------------------------------------------- -->
En el archivo buildspec.yml:
• Reemplaza el ID de cuenta AWS de ejemplo 123456789012 por tu ID real de cuenta AWS.
• Verifica que el código de región sea correcto, actualiza la sección --region us-east-2 a la región de AWS que estés usando.
<!-- -------------------------------------------------- -->
💡 ¿Dónde encuentro mi ID de cuenta?
Puedes encontrar tu ID de cuenta en la consola de AWS, en la esquina superior derecha, justo debajo de tu nombre de usuario.
¿Dónde encuentro mi código de región?
Despliega el selector de región en la esquina superior derecha de la consola de AWS y verás el código justo al lado de la región actual.
<!-- -------------------------------------------------- -->
Guarda el archivo buildspec.yml (presiona Ctrl+S o Cmd+S).
Confirma que el círculo pequeño en la pestaña del archivo desaparezca después de guardar, lo que indica que el archivo está guardado.
<!-- -------------------------------------------------- -->
Ahora, necesitamos hacer commit y push del archivo buildspec.yml a tu repositorio de GitHub para que CodeBuild pueda acceder a él.
<!-- -------------------------------------------------- -->
Abre la terminal en VS Code (Ctrl+ o Cmd+).
Ejecuta estos comandos Git:
```bash
git add .  
git commit -m "Añadiendo archivo buildspec.yml"  
git push
```
<!-- -------------------------------------------------- -->
🙋‍♀️ ¿Ves errores al hacer push?
Prueba a resetear tus credenciales Git y comienza de nuevo. Ejecuta estos comandos:
```bash
git config --global --unset credential.helper  
git config --local --unset credential.helper  
git remote set-url origin https://<TU_NOMBRE_DE_USUARIO>@github.com/<TU_NOMBRE_DE_USUARIO>/<NOMBRE_DE_TU_REPOSITORIO>.git
```
Asegúrate de reemplazar <TU_NOMBRE_DE_USUARIO> y <NOMBRE_DE_TU_REPOSITORIO> por los valores correctos.
Luego intenta hacer git push de nuevo e ingresa tu token PAT de GitHub cuando se te solicite.
<!-- -------------------------------------------------- -->
💡 ¿Por qué subir buildspec.yml a GitHub?
Debemos subir el archivo buildspec.yml a GitHub porque CodeBuild no busca instrucciones de compilación en tu computadora local, sino en tu repositorio GitHub.
Cuando CodeBuild se conecta a tu repositorio, primero busca este archivo especial. Sin él, CodeBuild no sabe qué hacer con tu código. Al subirlo a GitHub, nos aseguramos que nuestras instrucciones viajen junto con el código.
<!-- -------------------------------------------------- -->
Ve a tu repositorio GitHub nextwork-web-project en el navegador y actualiza la página para verificar que buildspec.yml está presente.
Revisa la salida en la terminal para confirmar que los cambios, incluyendo el nuevo archivo buildspec.yml, se han subido correctamente.


✅  
## Verificar compilación exitosa y artefactos
¡Perfecto! Ahora que hemos corregido la configuración de CodeBuild, es momento de ejecutar nuevamente el proceso de compilación y verificar que nuestro bucket S3 esté almacenando correctamente el artefacto generado.
<!-- -------------------------------------------------- -->
✅ En este paso vas a:
• Volver a ejecutar la compilación en CodeBuild.
• Solucionar un segundo fallo de compilación, esta vez otorgando permisos a CodeBuild para acceder a CodeArtifact.
• Ejecutar y verificar una compilación exitosa.
<!-- -------------------------------------------------- -->
🔁 Reintenta la compilación
1.	Abre la consola de CodeBuild en tu navegador.
2.	Haz clic en Retry build.
3.	El estado de la compilación cambiará a In progress.
4.	Espera a que la compilación termine...
😓 ¡Rayos! ¡Falló otra vez!
<!-- -------------------------------------------------- -->
🔎 Verifica la pestaña Phase details
Al revisar los detalles de esta segunda ejecución, notarás que la fase DOWNLOAD_SOURCE sí fue exitosa esta vez 🎉… pero otra fase falló.
❓ ¿Qué significa este error?
Este error indica que CodeBuild no pudo acceder al archivo settings.xml, probablemente porque no tiene permiso para acceder a CodeArtifact, que es donde se descargan las dependencias del proyecto.
<!-- -------------------------------------------------- -->
🔐 Soluciona el problema de permisos de CodeArtifact
Vamos a darle a CodeBuild los permisos necesarios para que pueda acceder a CodeArtifact.
1.	Dirígete a la consola de IAM.
2.	En el menú lateral izquierdo, haz clic en Roles.
3.	En el buscador, escribe codebuild para filtrar los roles disponibles.
4.	Selecciona el rol llamado algo como:
codebuild-nextwork-devops-cicd-service-role  
Este es el rol que se creó automáticamente para tu proyecto de CodeBuild.
5. Haz clic en Add permissions > Attach policies.
6. En el buscador escribe:
codeartifact-nextwork-consumer-policy  
7.	Marca la casilla de esa política. Puedes expandirla para ver los permisos otorgados.
8.	Haz clic en Add permissions.
✅ Verás un mensaje verde que dice "Policy was successfully attached to role".
<!-- -------------------------------------------------- -->
🔁 Reintenta la compilación nuevamente
1.	Regresa a la consola de CodeBuild.
2.	Ve a tu proyecto nextwork-devops-cicd.
3.	Haz clic en Retry build una vez más.
4.	Espera a que finalice la compilación...
🎉 ¡Ahora sí! El estado de la compilación debería cambiar a Succeeded ✅ con una marca verde, lo que indica que tu pipeline de CI ha funcionado correctamente.
<!-- -------------------------------------------------- -->
📦 Verifica el artefacto en Amazon S3
Vamos a comprobar si el artefacto generado se subió correctamente a tu bucket S3.
1.	Abre la consola de S3.
2.	Haz clic en Buckets desde el menú lateral izquierdo.
3.	Selecciona el bucket que creaste anteriormente, por ejemplo:
nextwork-devops-cicd  
4.	Si al principio está vacío, haz clic en Refresh para actualizar el contenido.
<!-- -------------------------------------------------- -->
💡 ¿Por qué es importante verificar el artefacto en S3?
Esto confirma que:
• Tu código se compiló y empaquetó correctamente.
• El proceso de compilación no presentó errores.
• El archivo .war fue generado y subido exitosamente.
• Ya tienes el artefacto listo para ser usado en una futura etapa de despliegue (CD).
Piensa en esto como la prueba final de que tu proceso de CI está funcionando como debe.
<!-- -------------------------------------------------- -->
🕵️‍♂️ ¿Lo encontraste?
Deberías ver un archivo llamado:
nextwork-devops-cicd-artifact.zip  
📥 Si lo descargas y lo inspeccionas, encontrarás dentro el archivo:
nextwork-web-project.war  
¡Esto confirma que la compilación generó el archivo esperado! 🙌


🚀  
## Despliegue de la Infraestructura de Producción con CloudFormation
En esta sección creamos una nueva instancia EC2 (ambiente de producción) utilizando AWS CloudFormation, implementando infraestructura como código (IaC).
🧩 ¿Por qué una nueva instancia EC2?
Ya habíamos creado una instancia EC2 para desarrollo. Ahora lanzamos una instancia separada para producción (entorno en vivo), evitando así probar cambios directamente frente a los usuarios.
<!-- -------------------------------------------------- -->
📌 Pasos importantes
1.	Accede a CloudFormation
• Ve a la consola de CloudFormation.
• Haz clic en Create stack > With new resources (standard).
2.	Sube la plantilla
• Selecciona: Template is ready.
• Elige Upload a template file. ---
<!-- -------------------------------------------------- -->
```yaml
AWSTemplateFormatVersion: 2010-09-09  
Parameters:  
  AmazonLinuxAMIID:  
    Type: AWS::SSM::Parameter::Value<AWS::EC2::Image::Id>  
    Default: /aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2  
  MyIP:  
    Type: String  
    Description: My IP address e.g. 1.2.3.4/32 for Security Group HTTP access rule. Get your IP from http://checkip.amazonaws.com/.  
    AllowedPattern: (\d{1,3})\.(\d{1,3})\.(\d{1,3})\.(\d{1,3})/32  
    ConstraintDescription: must be a valid IP address of the form x.x.x.x/32  
  
Resources:  
  VPC:  
    Type: AWS::EC2::VPC  
    Properties:  
      CidrBlock: 10.11.0.0/16  
      EnableDnsHostnames: true  
      EnableDnsSupport: true  
      Tags:  
        - Key: 'Name'  
          Value: !Join ['', [!Ref 'AWS::StackName', '::VPC'] ]  
  
  InternetGateway:  
    Type: AWS::EC2::InternetGateway  
    Properties:  
      Tags:  
        - Key: 'Name'  
          Value: !Join ['', [!Ref 'AWS::StackName', '::InternetGateway'] ]  
  
  VPCGatewayAttachment:  
    Type: AWS::EC2::VPCGatewayAttachment  
    Properties:  
      VpcId: !Ref VPC  
      InternetGatewayId: !Ref InternetGateway  
  
  PublicSubnetA:  
    Type: AWS::EC2::Subnet  
    Properties:  
      AvailabilityZone: !Select   
        - 0  
        - Fn::GetAZs: !Ref 'AWS::Region'  
      VpcId: !Ref VPC  
      CidrBlock: 10.11.0.0/20  
      MapPublicIpOnLaunch: true  
      Tags:  
        - Key: 'Name'  
          Value: !Join ['', [!Ref 'AWS::StackName', '::PublicSubnetA'] ]  
  
  PublicRouteTable:  
    Type: AWS::EC2::RouteTable  
    Properties:  
      VpcId: !Ref VPC  
      Tags:  
        - Key: 'Name'  
          Value: !Join ['', [!Ref 'AWS::StackName', '::PublicRouteTable'] ]  
  
  PublicInternetRoute:  
    Type: AWS::EC2::Route  
    DependsOn: VPCGatewayAttachment  
    Properties:  
      DestinationCidrBlock: 0.0.0.0/0  
      GatewayId: !Ref InternetGateway  
      RouteTableId: !Ref PublicRouteTable  
  
  PublicSubnetARouteTableAssociation:  
    Type: AWS::EC2::SubnetRouteTableAssociation  
    Properties:  
      RouteTableId: !Ref PublicRouteTable  
      SubnetId: !Ref PublicSubnetA  
  
  PublicSecurityGroup:  
    Type: AWS::EC2::SecurityGroup  
    Properties:  
      VpcId:  
        Ref: VPC  
      GroupDescription: Access to our Web server  
      SecurityGroupIngress:  
      - Description: Enable HTTP access via port 80 IPv4  
        IpProtocol: tcp  
        FromPort: '80'  
        ToPort: '80'  
        CidrIp: !Ref MyIP  
      SecurityGroupEgress:  
      - Description: Allow all traffic egress  
        IpProtocol: -1  
        CidrIp: 0.0.0.0/0  
      Tags:  
        - Key: 'Name'  
          Value: !Join ['', [!Ref 'AWS::StackName', '::PublicSecurityGroup'] ]  
  
  ServerRole:   
    Type: AWS::IAM::Role  
    Properties:   
      AssumeRolePolicyDocument:   
        Version: "2012-10-17"  
        Statement:   
          -   
            Effect: "Allow"  
            Principal:   
              Service:   
                - "ec2.amazonaws.com"  
            Action:   
              - "sts:AssumeRole"  
      Path: "/"  
      ManagedPolicyArns:  
        - "arn:aws:iam::aws:policy/AmazonSSMManagedInstanceCore"  
        - "arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess"  
  
  DeployRoleProfile:   
    Type: AWS::IAM::InstanceProfile  
    Properties:   
      Path: "/"  
      Roles:   
        -   
          Ref: ServerRole  
  
  WebServer:  
    Type: AWS::EC2::Instance  
    Properties:  
      ImageId: !Ref AmazonLinuxAMIID  
      InstanceType: t2.micro  
      IamInstanceProfile: !Ref DeployRoleProfile  
      NetworkInterfaces:   
        - AssociatePublicIpAddress: true  
          DeviceIndex: 0  
          GroupSet:   
            - Ref: PublicSecurityGroup  
          SubnetId:   
            Ref: PublicSubnetA  
      Tags:  
        - Key: 'Name'  
          Value: !Join ['', [!Ref 'AWS::StackName', '::WebServer'] ]  
        - Key: 'role'  
          Value: 'webserver'  
  
Outputs:  
  URL:  
    Value:  
      Fn::Join:  
      - ''  
      - - http://  
        - Fn::GetAtt:  
          - WebServer  
          - PublicIp  
    Description: NextWork web server
``` 
• Sube el archivo nextworkwebapp.yaml.
<!-- -------------------------------------------------- -->
3. Configura los detalles del stack
• Stack name: NextWorkCodeDeployEC2Stack.
• En el campo MyIP, pega tu IP desde https://checkip.amazonaws.com/ seguida de /32 (ej. 200.91.101.123/32).
4. Opciones del stack
• Marca: Roll back all stack resources.
• Marca: Delete all newly created resources.
• En Capabilities, selecciona:
I acknowledge that AWS CloudFormation might create IAM resources.
5. Revisar y lanzar
• Verifica que todo esté correcto y haz clic en Submit.
<!-- -------------------------------------------------- -->
👁️ Monitoreo del stack
• Ve a la pestaña Resources para ver qué recursos se están creando.
• Ejemplos:
o VPC
o Subnet
o Internet Gateway
o Route Table
o Security Group
o EC2 Instance
• En Events puedes seguir en tiempo real el progreso.
• Espera hasta ver el estado: CREATE_COMPLETE.
<!-- -------------------------------------------------- -->
❗ Si algo falla...
• Si el estado muestra ROLLBACK_IN_PROGRESS, revisa los errores en la pestaña Events.
• Verifica permisos, parámetros o errores en el template y vuelve a intentarlo.
<!-- -------------------------------------------------- -->
✅ ¡Listo! Ahora tienes una instancia EC2 configurada como ambiente de producción, con red segura, lista para desplegar tu aplicación.



🧪  
## Prepare Deployment Scripts and AppSpec

Antes de poder desplegar nuestra aplicación, debemos preparar un conjunto de scripts y un archivo de configuración llamado `appspec.yml` para que AWS CodeDeploy sepa exactamente cómo realizar el despliegue.
<!-- -------------------------------------------------- -->
### ✅ Objetivos

- Crear scripts para:
  - Instalar dependencias
  - Iniciar el servidor
  - Detener el servidor
- Crear el archivo `appspec.yml` que guíe a CodeDeploy en el proceso de despliegue.
- Modificar `buildspec.yml` para incluir todos los archivos necesarios en el build artifact.

---

### 📁 Crear carpeta `scripts`

En tu IDE (por ejemplo, Visual Studio Code), crea una nueva carpeta en la raíz del proyecto llamada:
<!-- -------------------------------------------------- -->
scripts
<!-- -------------------------------------------------- -->
---

### 🧠 ¿Qué son los scripts?
<!-- -------------------------------------------------- -->
Son archivos que contienen comandos ejecutables por terminal para automatizar tareas como instalar dependencias o iniciar servicios.
<!-- -------------------------------------------------- -->
---
<!-- -------------------------------------------------- -->
### 📄 install_dependencies.sh
<!-- -------------------------------------------------- -->
📍 Ubicación: `scripts/install_dependencies.sh`
<!-- -------------------------------------------------- -->
```bash
#!/bin/bash
sudo yum install tomcat -y
sudo yum -y install httpd
sudo cat << EOF > /etc/httpd/conf.d/tomcat_manager.conf
<VirtualHost *:80>
  ServerAdmin root@localhost
  ServerName app.nextwork.com
  DefaultType text/html
  ProxyRequests off
  ProxyPreserveHost On
  ProxyPass / http://localhost:8080/nextwork-web-project/
  ProxyPassReverse / http://localhost:8080/nextwork-web-project/
</VirtualHost>
EOF
```
💡 Este script instala Tomcat y Apache, y configura Apache como reverse proxy hacia Tomcat.
<!-- -------------------------------------------------- -->
📄 start_server.sh
📍 Ubicación: scripts/start_server.sh
<!-- -------------------------------------------------- -->
```bash
#!/bin/bash
sudo systemctl start tomcat.service
sudo systemctl enable tomcat.service
sudo systemctl start httpd.service
sudo systemctl enable httpd.service
```
💡 Inicia los servicios y configura su arranque automático.
<!-- -------------------------------------------------- -->
📄 stop_server.sh
📍 Ubicación: scripts/stop_server.sh
<!-- -------------------------------------------------- -->
```bash
#!/bin/bash
isExistApp="$(pgrep httpd)"
if [[ -n $isExistApp ]]; then
  sudo systemctl stop httpd.service
fi
isExistApp="$(pgrep tomcat)"
if [[ -n $isExistApp ]]; then
  sudo systemctl stop tomcat.service
fi
```
💡 Este script detiene Tomcat y Apache solo si están activos.
<!-- -------------------------------------------------- -->
📝 Crear archivo appspec.yml
📍 Ubicación: raíz del proyecto (no dentro de scripts)
<!-- -------------------------------------------------- -->
```yaml
version: 0.0
os: linux
files:
  - source: /target/nextwork-web-project.war
    destination: /usr/share/tomcat/webapps/
hooks:
  BeforeInstall:
    - location: scripts/install_dependencies.sh
      timeout: 300
      runas: root
  ApplicationStop:
    - location: scripts/stop_server.sh
      timeout: 300
      runas: root
  ApplicationStart:
    - location: scripts/start_server.sh
      timeout: 300
      runas: root
```
🧠 Define qué archivos copiar y cuándo ejecutar cada script.
<!-- -------------------------------------------------- -->
🛠️ Modificar buildspec.yml
Agrega esta sección para incluir todos los archivos necesarios en el artefacto:
<!-- -------------------------------------------------- -->
```yaml
artifacts:
  files:
    - target/nextwork-web-project.war
    - appspec.yml
    - scripts/**/*
  discard-paths: no
```
  <!-- -------------------------------------------------- -->
🔁 Hacer commit y push
<!-- -------------------------------------------------- -->
```bash
git add .
git commit -m "Adding CodeDeploy files"
git push
```
<!-- -------------------------------------------------- -->
💡 Verifica que scripts/ y appspec.yml estén en tu repositorio de GitHub.
<!-- -------------------------------------------------- -->
🛠️ ¿Problemas con git push?
Ejecuta esto si necesitas restablecer credenciales:
<!-- -------------------------------------------------- -->
```bash
git config --global --unset credential.helper
git config --local --unset credential.helper
git remote set-url origin https://<TU_USUARIO>@github.com/<TU_USUARIO>/<TU_REPO>.git
```
<!-- -------------------------------------------------- -->
Luego vuelve a ejecutar:
<!-- -------------------------------------------------- -->
```bash
git push
```
<!-- -------------------------------------------------- -->
e ingresa tu GitHub Personal Access Token cuando se te pida.
<!-- -------------------------------------------------- -->
✅ ¡Listo! Ya tienes tus scripts y archivos preparados para que AWS CodeDeploy realice el despliegue de forma automática y organizada.


🛠️  
## Set Up CodeDeploy
Now, let's get to know CodeDeploy and set it up to automate the deployment of our web app!
<!-- -------------------------------------------------- -->
📌 En este paso vas a:
• Crear una aplicación de CodeDeploy: es decir, una definición del software que deseas desplegar.
• Crear un deployment group, que agrupa configuraciones y recursos relacionados al despliegue.
• Darle a CodeDeploy los permisos necesarios para acceder a CodeArtifact y otros recursos.
<!-- -------------------------------------------------- -->
🚀 Crear una CodeDeploy Application
1.	Dirígete a la consola de CodeDeploy.
2.	En el menú izquierdo, selecciona Applications.
3.	Haz clic en Create application.
💡 ¿Qué es una aplicación de CodeDeploy?
Una aplicación en CodeDeploy es como una carpeta organizadora para tus despliegues. No hace mucho por sí sola, pero contiene todo lo necesario para manejar el proceso de despliegue de un sistema específico.
En términos más técnicos: una aplicación de CodeDeploy es un contenedor que agrupa configuraciones, grupos de despliegue y versiones.
<!-- -------------------------------------------------- -->
4.	Asigna el nombre: nextwork-devops-cicd.
5.	En Compute platform, elige: EC2/On-premises.
💡 ¿Qué son las Compute Platforms en CodeDeploy?
Son los tipos de entornos en los que puede vivir tu aplicación:
• EC2/On-premises: aplicaciones en servidores reales.
• AWS Lambda: serverless, sin servidores.
• Amazon ECS: aplicaciones en contenedores Docker.
✅ Finalmente, haz clic en Create application.
<!-- -------------------------------------------------- -->
🔁 Crear Deployment Group
1.	Selecciona Create deployment group.
💡 ¿Qué es un Deployment Group?
Un deployment group es un conjunto de instancias EC2 al que deseas desplegar. Aquí defines:
• A qué instancias va el despliegue.
• Cómo se lleva a cabo.
• Qué hacer en caso de error.
• Balanceo de carga, etc.
Puedes tener varios grupos dentro de una misma aplicación: uno para testing, otro para staging y otro para producción, todos con diferentes configuraciones.
<!-- -------------------------------------------------- -->
2.	Nombre del grupo de despliegue: nextwork-devops-cicd-deploymentgroup.
<!-- -------------------------------------------------- -->
👤 Crear un IAM Role para CodeDeploy
1.	Abre la consola de IAM.
2.	Ve a Roles > Create role.
3.	Elige AWS service como tipo de entidad.
4.	Servicio: selecciona CodeDeploy.
5.	Use case: CodeDeploy nuevamente.
6.	Haz clic en Next.
✅ Verás que ya se selecciona la política AWSCodeDeployRole.
Esta política le da a CodeDeploy permisos para trabajar con EC2, Auto Scaling, ELB, CloudWatch y S3.
7.	Haz clic en Next.
8.	Nombre del rol: NextWorkCodeDeployRole.
9.	Descripción:
vbnet
CopyEdit
Allows CodeDeploy to call AWS services such as Auto Scaling on your behalf.  
Created as a part of NextWork's CI/CD Pipeline series.
10.	Asegúrate de que la política AWSCodeDeployRole esté seleccionada.
11.	Haz clic en Create role.
<!-- -------------------------------------------------- -->
🔁 Asignar el IAM Role en CodeDeploy
1.	Regresa a la pestaña de configuración de tu Deployment Group.
2.	Refresca la página para que aparezca el nuevo rol.
3.	Reescribe el nombre del grupo: nextwork-devops-cicd-deploymentgroup.
4.	Selecciona el rol: NextWorkCodeDeployRole.
<!-- -------------------------------------------------- -->
⚙️ Configurar el tipo de Deployment
1.	Deployment type: selecciona In-place.
💡 ¿In-place vs Blue/Green?
• In-place: reemplaza la app directamente en los servidores existentes.
• Blue/Green: crea un entorno nuevo y cambia el tráfico cuando todo está listo.
En este proyecto usamos In-place por simplicidad y costo.
<!-- -------------------------------------------------- -->
Configurar las instancias de destino
1.	Environment configuration: elige Amazon EC2 instances.
2.	En Tag group 1, define:
o	Key: role
o	Value: webserver
💡 ¿Por qué usamos tags?
Tags ayudan a identificar las instancias de destino. En nuestra plantilla de CloudFormation, ya etiquetamos la instancia con role: webserver.
Beneficios:
• Flexibilidad: nuevas instancias con ese tag serán incluidas automáticamente.
• Documentación: etiquetas describen claramente el propósito de la instancia.
• Integración: todo encaja sin configuración adicional.
✅ Si ves el mensaje 1 unique matched instance is found, ¡todo va bien!
Si no:
• Revisa ortografía en las etiquetas.
• Asegúrate de que la instancia se lanzó correctamente con CloudFormation.
• Usa “Click here for details” para confirmar que la instancia NextWorkCodeDeployEC2Stack::WebServer es la detectada.
<!-- -------------------------------------------------- -->
🔧 Agent y Deployment Settings
✅ Agent Configuration
1.	En Agent configuration with AWS Systems Manager, selecciona:
o	Now and schedule updates
o	Basic scheduler with 14 Days
💡 ¿Qué es el CodeDeploy Agent?
Es el software que recibe las instrucciones de despliegue y las ejecuta en tu EC2. La actualización cada 14 días mantiene el agente al día con los últimos parches.
<!-- -------------------------------------------------- -->
✅ Deployment Settings
1.	En Deployment settings, mantén:
o	CodeDeployDefault.AllAtOnce
💡 ¿Qué significa AllAtOnce?
Despliega a todas las instancias al mismo tiempo. Es rápido pero arriesgado. Ideal para proyectos pequeños o de prueba.
En producción, se recomienda OneAtATime o HalfAtATime para reducir riesgos.
<!-- -------------------------------------------------- -->
❌ Desactiva Load Balancing
1.	Desmarca Enable load balancing.
💡 ¿Qué es un load balancer?
Un balanceador de carga distribuye el tráfico entre múltiples servidores para mejorar disponibilidad y rendimiento. No es necesario en este proyecto porque solo usamos una instancia.
<!-- -------------------------------------------------- -->
🎉 Finaliza
Haz clic en Create deployment group.
💪 ¡Excelente! Esta es la parte más detallada de CodeDeploy. Si llegaste hasta aquí, ya tienes una base sólida para manejar despliegues automatizados.


🔍  
## Crear y Verificar el Despliegue
¡Es momento de juntar todo y desplegar nuestra aplicación web en la instancia EC2!
En este paso, vas a:
✅ Crear un despliegue en CodeDeploy
✅ Monitorear el proceso de despliegue
✅ Acceder y verificar la aplicación web desplegada
<!-- -------------------------------------------------- -->
Crear el Despliegue
En la página de detalles del grupo de implementación, selecciona Crear despliegue.
<!-- -------------------------------------------------- -->
💡 ¿Qué es un despliegue de CodeDeploy?
Un despliegue en CodeDeploy representa una actualización única de tu aplicación, con su propio ID e historial. Al crear un despliegue, le estás diciendo a CodeDeploy:
• Qué versión de la aplicación desplegar (la revisión)
• Dónde desplegarla (el grupo de implementación)
• Cómo desplegarla (la configuración de implementación)
CodeDeploy orquesta todo el proceso: detener servicios, copiar archivos, ejecutar scripts, iniciar servicios... y registra si tuvo éxito o falló. Puedes monitorear todo en tiempo real y ver un registro detallado de cada paso. ¡Genial!
<!-- -------------------------------------------------- -->
Parte de la configuración ya viene completada automáticamente.
En el campo Tipo de revisión, asegúrate de seleccionar Mi aplicación está almacenada en Amazon S3, ya que nuestro artefacto de despliegue está dentro de un bucket de S3.
1.	Regresa a tu bucket de S3 llamado nextwork-devops-cicd.
2.	Entra al artefacto de compilación nextwork-devops-cicd-artifact.
3.	Copia el URI de S3 del archivo .zip.
4.	Pega ese URI en el campo Ubicación de la revisión en CodeDeploy.
5.	Selecciona .zip como el tipo de archivo de revisión.
<!-- -------------------------------------------------- -->
💡 ¿Qué es una ubicación de revisión? ¿Por qué usamos nuestro archivo WAR/ZIP?
La ubicación de revisión es donde CodeDeploy busca los artefactos de tu aplicación. Como estamos usando un archivo .zip almacenado en S3, CodeDeploy sabrá dónde encontrar la última versión de nuestra app para desplegarla en las instancias EC2.
<!-- -------------------------------------------------- -->
A continuación, deja las configuraciones en Comportamientos adicionales de despliegue con los valores predeterminados.
💡 ¿Qué son los comportamientos adicionales y los reemplazos del grupo de implementación?
• Comportamientos adicionales: Te permiten configurar cosas como permisos de archivos durante el despliegue o si se permite tráfico inmediatamente a nuevas instancias.
• Reemplazos del grupo de implementación: Permiten cambiar configuraciones para un despliegue específico. Por ejemplo, si normalmente despliegas una instancia a la vez (más seguro pero más lento), podrías cambiarlo para desplegar en todas las instancias al mismo tiempo (más rápido pero más riesgoso).
<!-- -------------------------------------------------- -->
Haz clic en Crear despliegue.
¡Y allá vamos! CodeDeploy iniciará el despliegue de tu aplicación web.
Desplázate hacia abajo a Eventos del ciclo de vida del despliegue y haz clic en Ver eventos para monitorear.
Verás eventos como BeforeInstall, ApplicationStart, etc. ¡Estos son los eventos que definiste en el archivo appspec.yml!
<!-- -------------------------------------------------- -->
¿Ves un error? 🛑
Si luego de unos minutos notas que ocurrió un error...
Piensa:
¿Cuándo fue la última vez que ejecutaste una compilación en CodeBuild?
💡 La última vez que ejecutaste CodeBuild fue antes de agregar el appspec.yml y los scripts de despliegue.
¡Ahí está el problema!
Tu instancia EC2 no está recibiendo esos scripts, lo cual causa el error que estás viendo.
➡️ Ve a tu proyecto de CodeBuild y vuelve a ejecutar la compilación.
Una vez que la segunda compilación sea exitosa, regresa a CodeDeploy y vuelve a intentar el despliegue.
<!-- -------------------------------------------------- -->
¡Verifica tu Aplicación Desplegada!
1.	Espera hasta que el estado del despliegue diga Éxito.
🙋‍♀️ ¿Tu despliegue sigue fallando?
No te preocupes, esto es común en el camino DevOps. Revisa lo siguiente:
• Mira los detalles del despliegue en CodeDeploy y los eventos del ciclo de vida para encontrar mensajes de error.
• Verifica las reglas del grupo de seguridad de tu instancia EC2. Asegúrate de que el puerto 80 (HTTP) esté abierto para tu dirección IP.
• Si cambiaste el código de tu aplicación web, haz commit/push de los cambios, vuelve a compilar y luego vuelve a desplegar.
<!-- -------------------------------------------------- -->
¡Accede a tu aplicación web!
1.	En el panel de eventos del despliegue, selecciona el ID de la instancia.
Esto te lleva a la instancia EC2 creada con CloudFormation.
2.	En la consola de EC2, copia la DNS pública IPv4 de tu instancia.
3.	Abre esa dirección en tu navegador (por ejemplo, http://ec2-3-123-45-67.compute-1.amazonaws.com).
🕒 Puede tardar un minuto o dos en estar disponible después del despliegue.
<!-- -------------------------------------------------- -->
🙋‍♀️ ¿No ves tu aplicación?
• Verifica si el navegador está usando https. Si es así, cámbialo a http, ya que tu grupo de seguridad solo permite conexiones por el puerto 80 (HTTP), no por el 443 (HTTPS).
• Si sigues sin verla, puede que tu dirección IP haya cambiado. Revisa tu IP visitando:
•	http://checkip.amazonaws.com/
•	https://whatismyipaddress.com/
Si tu IP cambió, actualiza el grupo de seguridad de tu instancia EC2 para permitir tu nueva IP.
<!-- -------------------------------------------------- -->
🎉 ¡WOOHOO! ¡Bienvenido a tu aplicación!
¡Felicidades!
Has automatizado exitosamente el despliegue de una aplicación web en EC2 usando AWS CodeDeploy.
¡Gran trabajo! 🚀



🧱  
## Configura tu Pipeline
<!-- -------------------------------------------------- -->
¡Vamos a comenzar con la creación de nuestro primer pipeline! Empezaremos configurando la estructura básica del pipeline y ajustando sus parámetros.
<!-- -------------------------------------------------- -->
En este paso vas a:
✅ Comenzar a crear un nuevo pipeline en CodePipeline.
<!-- -------------------------------------------------- -->
Crear un nuevo pipeline
Dirígete a la consola de CodePipeline.
Crea un pipeline personalizado en CodePipeline llamado:
nextwork-devops-cicd
<!-- -------------------------------------------------- -->
💡 ¿Qué es AWS CodePipeline y por qué lo usamos?
CodePipeline te permite crear un flujo de trabajo que automatiza el paso de los cambios en tu código desde la fase de construcción (build) hasta la fase de despliegue (deployment).
En nuestro caso, verás cómo un nuevo push a tu repositorio de GitHub automáticamente:
1.	Dispara una construcción en CodeBuild (Integración continua)
2.	Luego activa un despliegue en CodeDeploy (Despliegue continuo)
Esto garantiza que tus despliegues sean consistentes, confiables y automáticos cada vez que actualizas tu código, reduciendo errores humanos y ahorrándote tiempo.
<!-- -------------------------------------------------- -->
Selecciona el modo de ejecución:
Modo de ejecución: Superseded (Reemplazado)
💡 ¿Qué es el modo de ejecución?
Este parámetro controla cómo CodePipeline maneja múltiples ejecuciones simultáneas del mismo pipeline.
• Superseded (Reemplazado): Si se inicia una nueva ejecución mientras otra ya está en curso, la nueva ejecución cancela la anterior y toma el control.
✅ Ideal para asegurar que solo los cambios más recientes se procesen.
Otros modos de ejecución son:
• Queued (En cola): Las ejecuciones esperan su turno y se procesan una por una.
• Parallel (Paralelo): Permite ejecutar varias ejecuciones a la vez. Útil para manejar múltiples ramas o tareas en paralelo.
<!-- -------------------------------------------------- -->
Rol de servicio
Selecciona Nuevo rol de servicio (New service role)
Mantén el nombre de rol predeterminado.
💡 ¿Qué es un rol de servicio?
Es un tipo especial de rol IAM que servicios como CodePipeline usan para actuar en tu nombre. Le da a CodePipeline permisos para acceder a otros recursos de AWS como buckets S3 o CodeBuild.
<!-- -------------------------------------------------- -->
Almacenamiento de artefactos, clave de cifrado y variables
Mantén la configuración predeterminada para:
• Artifact store (Almacén de artefactos)
• Encryption key (Clave de cifrado)
• Variables
💡 ¿Qué significa esto?
• Artifact store: Es el bucket S3 donde CodePipeline guarda automáticamente los archivos generados en cada etapa (por ejemplo, el código fuente de GitHub o los archivos construidos por CodeBuild), para que estén disponibles en la siguiente etapa del pipeline.
• Encryption key: CodePipeline cifra automáticamente estos archivos usando claves administradas por AWS. Esto protege tu código y artefactos mientras se almacenan o transfieren.
• Variables: Aunque en este proyecto no las usaremos, las variables permiten compartir información dinámica (como números de versión o marcas de tiempo) entre etapas del pipeline. Son muy útiles en pipelines más complejos.
<!-- -------------------------------------------------- -->
🎉 ¡Listo! Has configurado la estructura básica de tu pipeline.



🧬  
## Configuración de las etapas de Source, Build y Deploy
¿Listo para juntar todas las piezas de tu arquitectura CI/CD?
<!-- -------------------------------------------------- -->
En este paso vas a:
• Conectar CodePipeline con tu repositorio y rama de GitHub.
• Conectar CodePipeline con tu proyecto de CodeBuild.
• Conectar CodePipeline con tu grupo de despliegue de CodeDeploy.
• Configurar eventos webhook para activar automáticamente el pipeline.
<!-- -------------------------------------------------- -->
Etapa de Source (Origen)
Ahora configuraremos la etapa de Source del pipeline. Aquí es donde indicaremos a CodePipeline de dónde obtener nuestro código fuente.
Usa tu repositorio de GitHub como origen.
<!-- -------------------------------------------------- -->
💡 ¿Qué es la etapa Source?
Es el primer paso en cualquier pipeline CI/CD. Su función es sencilla pero fundamental: obtener la última versión del código desde el repositorio seleccionado cada vez que hay cambios. Sin esta etapa, no habría nada que construir o desplegar.
CodePipeline soporta varios proveedores de origen, pero en este proyecto usamos GitHub, porque ahí está el código de nuestra aplicación web.
<!-- -------------------------------------------------- -->
En Output artifact format, déjalo en CodePipeline default.
💡 ¿Qué es Output artifact format?
Determina cómo CodePipeline empaqueta el código fuente que recibe de GitHub:
• CodePipeline default: Empaqueta el código como un archivo ZIP, eficiente para la mayoría de despliegues. No incluye metadata de Git.
• Full clone: Proporciona un clon completo del repositorio Git, con historial y metadata, útil si el proceso de build lo requiere. El tamaño del artefacto es mayor.
<!-- -------------------------------------------------- -->
Asegúrate de que la opción Webhook events esté marcada bajo Detect change events.
💡 ¿Qué son los Webhook events?
Son eventos que permiten a CodePipeline iniciar automáticamente el pipeline cada vez que se hace un push a la rama configurada en GitHub. Esto hace que el pipeline sea verdaderamente “continuo”, reaccionando en tiempo real a los cambios de código.
<!-- -------------------------------------------------- -->
💡 ¿Cómo funcionan los Webhooks?
Son notificaciones digitales. Al habilitar los Webhooks, CodePipeline crea un webhook en tu repositorio GitHub que escucha eventos específicos, como los push a la rama principal.
Cada vez que haces un push a esa rama, GitHub envía una notificación a CodePipeline, que inicia automáticamente una ejecución del pipeline. Esto automatiza completamente el proceso CI/CD.
<!-- -------------------------------------------------- -->
¡Muy bien! Has configurado la etapa Source. Ahora vamos a la etapa Build.


🏗️  
## Etapa Build (Construcción)
La etapa Build es donde el código fuente se transforma en un artefacto listo para desplegar.
Indicaremos a CodePipeline que use AWS CodeBuild para compilar y empaquetar nuestra aplicación web.
<!-- -------------------------------------------------- -->
Configura tu proyecto de CodeBuild como proveedor de Build.
Bajo Input artifacts, debería estar seleccionado por defecto SourceArtifact.
💡 ¿Qué son Input artifacts?
Son los archivos que provienen de la etapa anterior y se usan como entrada para la etapa actual. En nuestra etapa Build, usamos SourceArtifact, que es el archivo ZIP con el código fuente generado en la etapa Source.
<!-- -------------------------------------------------- -->
¡Vamos! Has configurado la etapa Build. Ahora pasamos a la siguiente etapa.
<!-- -------------------------------------------------- -->
Saltar la etapa de Test
En la página de agregar etapa de test, haz clic en Skip test stage.
<!-- -------------------------------------------------- -->
💡 ¿Qué es la etapa Test?
Aquí se automatizan las pruebas de la aplicación, que pueden ser:
• Pruebas unitarias: verificar componentes o funciones individuales.
• Pruebas de integración: validar la interacción entre partes del sistema.
• Pruebas de interfaz (UI): asegurar que la interfaz funciona correctamente.
Esta etapa ayuda a garantizar la calidad y detectar errores antes de producción. Aunque la saltamos para simplificar este proyecto, en escenarios reales es fundamental.
<!-- -------------------------------------------------- -->
Etapa Deploy (Despliegue)
Selecciona tu grupo de despliegue de CodeDeploy como proveedor de despliegue.
Activa la opción Configure automatic rollback on stage failure.
<!-- -------------------------------------------------- -->
💡 ¿Qué es el rollback automático?
Es un mecanismo de seguridad que, si la etapa de Deploy falla, vuelve automáticamente a la última versión que funcionó correctamente. Así minimizas el tiempo de caída y mantienes la estabilidad de tu aplicación.


▶️  
## Ejecuta tu Pipeline
¡Vamos a ver nuestro pipeline ejecutarse por primera vez! Esto nos ayudará a verificar que todo esté funcionando correctamente.
<!-- -------------------------------------------------- -->
En este paso vas a:
• Finalizar la creación de tu pipeline.
• Ver cómo tu pipeline se inicia y conecta GitHub, CodeBuild y CodeDeploy.
<!-- -------------------------------------------------- -->
Revisa tu Pipeline
Una vez que hayas revisado todos los ajustes y confirmado que están correctos, haz clic en Create pipeline (Crear pipeline).
<!-- -------------------------------------------------- -->
Ejecuta tu Pipeline
CodePipeline inicia automáticamente la ejecución del pipeline tan pronto como se crea.
Podrás ver el progreso de cada etapa en el diagrama del pipeline. Las etapas cambiarán de color:
• Gris: La etapa aún no ha comenzado.
• Azul: La etapa está en progreso.
• Verde: La etapa finalizó con éxito.
• Rojo: La etapa falló.
<!-- -------------------------------------------------- -->
💡 ¿Qué significan los diferentes estados?
• Gris: Etapa pendiente de iniciar.
• Azul: Etapa en ejecución.
• Verde: Etapa completada con éxito.
• Rojo: Etapa con error.
<!-- -------------------------------------------------- -->
Espera a que la ejecución del pipeline termine. Puedes monitorear el estado de cada etapa en el diagrama.
Para ver detalles específicos de una etapa, haz clic en el enlace del Stage ID en la pestaña Executions. Por ejemplo, haz clic en el Stage ID de la etapa Source para ver detalles sobre la obtención del código fuente.

🧪  
## Prueba tu Pipeline
Es hora de la prueba DEFINITIVA para este proyecto... ¡veamos cómo CodePipeline maneja un cambio en el código!
Probar con un cambio en el código confirmará que nuestro pipeline se activa automáticamente y despliega nuestras actualizaciones.
<!-- -------------------------------------------------- -->
En este paso vas a:
• Probar el pipeline haciendo un cambio en el código y subiéndolo a GitHub.
<!-- -------------------------------------------------- -->
Prueba el Pipeline con un Cambio en el Código
Agrega una nueva línea en el archivo index.jsp:
<!-- -------------------------------------------------- -->
```html
<p>If you see this line, that means your latest changes are automatically deployed</p>
```
<!-- -------------------------------------------------- -->
Luego, haz commit y push de los cambios a tu repositorio en GitHub usando estos comandos:
<!-- -------------------------------------------------- -->
```bash
git add .
git commit -m "Update index.jsp with a new line to test CodePipeline"
git push origin master
```
<!-- -------------------------------------------------- -->
🙋‍♀️ ¿Ves errores al hacer push?
Intenta restablecer tus credenciales de Git y empieza de nuevo. Ejecuta estos comandos en tu terminal:

<!-- -------------------------------------------------- -->

```bash
git config --global --unset credential.helper
```

<!-- ---------------------------------------------- -->

```bash
git config --local --unset credential.helper
```

<!-- ---------------------------------------------- -->

```bash
git remote set-url origin https://<TU_USUARIO_GITHUB>@github.com/<TU_USUARIO_GITHUB>/<TU_REPOSITORIO>.git
```
<!-- ---------------------------------------------- -->
Asegúrate de reemplazar <TU_USUARIO_GITHUB> con tu nombre de usuario de GitHub y <TU_REPOSITORIO> con el nombre de tu repositorio.
  <!-- -------------------------------------------------- -->
Después intenta hacer push de nuevo e ingresa tu PAT de GitHub cuando te pida la contraseña. Si no te lo pide en la terminal, revisa la parte superior de la ventana de VS Code.
<!-- -------------------------------------------------- -->
Deberías ver una nueva ejecución iniciarse automáticamente después de hacer push a GitHub.
Haz clic en el enlace del Commit ID en el panel de detalles de la etapa Source. Esto abrirá la página del commit en tu repositorio de GitHub en una nueva pestaña del navegador.
<!-- -------------------------------------------------- -->
Verifica que la página del commit muestre los cambios que acabas de subir (la nueva línea que agregaste en index.jsp).
Espera a que las etapas Build y Deploy terminen con éxito (se pongan verdes) en la consola de CodePipeline.
<!-- -------------------------------------------------- -->
Verifica el Despliegue Automático
Ahora deberías ver tu aplicación web con la nueva línea que agregaste.
<!-- -------------------------------------------------- -->
¡INCREÍBLE! Tu pipeline CI/CD ahora está construyendo y desplegando automáticamente tu aplicación web cada vez que haces cambios en GitHub.


✨ Autor
**Axel Andres Barrantes Anchia**
📍 Santa Ana, San José
📧 [axelbarrantesanchia@gmail.com](mailto:axelbarrantesanchia@gmail.com)
