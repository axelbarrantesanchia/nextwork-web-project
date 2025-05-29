# ☁️ Nextwork Web Project - Java Maven Web App on EC2

Este repositorio documenta cómo configuré una instancia EC2 para desarrollar y desplegar una aplicación web Java usando Maven, Git, GitHub y Visual Studio Code a través de conexión remota por SSH. Ideal para quienes quieran trabajar desde la nube de manera profesional y organizada.

---

## ⚙️ Stack Tecnológico

- 🖥️ **Instancia EC2 (Amazon Linux)**
- ☕ **Java 1.8.0 Amazon Corretto**
- 🔨 **Apache Maven 3.5.2**
- 📂 **Git y GitHub**
- 🖋️ **Visual Studio Code con extensión Remote - SSH**
- 🌐 **Maven Web App (`maven-archetype-webapp`)**

---

## 🪜 Paso a paso del setup

### 1. 🔐 Conexión SSH a la instancia EC2 desde VSCode con la Extension Remote - SSH

Conectarse a la instancia EC2 usando el archivo `.pem` proporcionado:
ssh -i /ruta/a/tu/clave.pem ec2-user@<ip-publica>

2. ☕ Instalación de Java (Amazon Corretto)
sudo dnf install -y java-1.8.0-amazon-corretto-devel

export JAVA_HOME=/usr/lib/jvm/java-1.8.0-amazon-corretto.x86_64
export PATH=/usr/lib/jvm/java-1.8.0-amazon-corretto.x86_64/jre/bin/:$PATH

3. ⚙️ Instalación de Maven 3.5.2
wget https://archive.apache.org/dist/maven/maven-3/3.5.2/binaries/apache-maven-3.5.2-bin.tar.gz
sudo tar -xzf apache-maven-3.5.2-bin.tar.gz -C /opt
echo "export PATH=/opt/apache-maven-3.5.2/bin:\$PATH" >> ~/.bashrc
source ~/.bashrc

Verifica que Maven esté instalado:
mvn -v

4. 💻 Instalación y configuración de Git
sudo dnf update -y
sudo dnf install git -y
git --version
Configuración del usuario global de Git:

git config --global user.name "Your Name"
git config --global user.email "you@gmail.com"

5. 📁 Inicializar proyecto Maven
Desde tu sesión SSH:

mvn archetype:generate \
  -DgroupId=com.nextwork.app \
  -DartifactId=nextwork-web-project \
  -DarchetypeArtifactId=maven-archetype-webapp \
  -DinteractiveMode=false
Esto creó el directorio nextwork-web-project.

6. 🧠 Edición remota con Visual Studio Code
Utilicé la extensión oficial de Microsoft Remote - SSH para conectarme directamente a la instancia EC2 desde VS Code y editar el proyecto remotamente.

7. 🐙 GitHub y control de versiones
Se creó un repositorio llamado nextwork-web-project en GitHub.

Se inicializó Git en la instancia:

cd nextwork-web-project
git init
git remote add origin https://github.com/usuario/nextwork-web-project.git
Se añadieron los archivos y se hizo el primer commit:

git add .
git commit -m "First Commit"
Se generó un token personal de acceso desde GitHub (con el scope repo) y se utilizó como contraseña al hacer git push:

git push -u origin master
⚠️ Git pide el nombre de usuario y el token se usa como contraseña cada vez que haces push si usas HTTPS.

💡 TIP: Ejecuta este comando para que Git recuerde tu token en futuras sesiones:

git config --global credential.helper store

8. 📝 Modificación de contenido
Se editaron las líneas del archivo index.jsp:

<p>This is a Nextwork web app working!</p>
<p>If you see this in GitHub, that means your latest changes are getting pushed to your cloud repository</p>
Luego se aplicaron los comandos:


git add .
git diff --staged
git commit -m "Add new line to index.jsp"
git push

✅ Confirmación
Puedes verificar los cambios en tu repositorio GitHub y ver el historial de commits con:
git log

📌 Conclusión
Este proceso dejó una instancia EC2 completamente funcional para desarrollar aplicaciones web en Java usando Maven, control de versiones con Git, integración con GitHub, y edición remota profesional con VS Code + Remote SSH.

Una solución ligera, profesional y portable para entornos de desarrollo en la nube. 🌩️

✨ Autor
Axel Andres Barrantes Anchia
📍 Santa Ana, San José
📧 axelbarrantesanchia@gmail.com






