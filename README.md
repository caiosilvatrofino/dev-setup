🚀 Guia de Configuração: Ambiente de Desenvolvimento Full-Stack

Este guia contém todos os passos e comandos necessários para configurar um ambiente de desenvolvimento com Java 17 (OpenJDK), Maven, Node.js 20, Angular CLI, PostgreSQL e VS Code com as extensões essenciais para projetos Spring Boot e Angular.

1. Atualizar o Sistema e Instalar Ferramentas Essenciais

Primeiro, vamos garantir que o sistema está atualizado e instalar ferramentas básicas como curl, wget e git, caso ainda não as tenha.
Bash

sudo apt update && sudo apt upgrade -y
sudo apt install curl wget git -y

2. Instalar Java 17 e Maven

Java OpenJDK 17 (LTS)

Instalaremos a versão 17 do OpenJDK, que é uma versão de suporte de longo prazo (LTS).
Bash

sudo apt install openjdk-17-jdk -y
java -version
javac -version

Configurar JAVA_HOME

Para que outras ferramentas encontrem o Java, é uma boa prática configurar a variável de ambiente JAVA_HOME.
Bash

echo 'export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64' >> ~/.bashrc
echo 'export PATH=$PATH:$JAVA_HOME/bin' >> ~/.bashrc
source ~/.bashrc
echo $JAVA_HOME

Maven

O Maven é a principal ferramenta de gerenciamento de projetos para Java.
Bash

sudo apt install maven -y
mvn -version

3. Instalar Node.js e Angular CLI

Node.js 20 LTS

Vamos instalar o Node.js 20, que é a versão LTS recomendada para desenvolvimento.
Bash

curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs
node -v
npm -v

Angular CLI

O Angular CLI é a interface de linha de comando para criar e gerenciar projetos Angular.
Bash

sudo npm install -g @angular/cli
ng version

Dica: Se tiver problemas de permissão com o sudo npm, você pode configurar o npm para instalar pacotes globalmente na sua pasta de usuário.
Bash

mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
npm install -g @angular/cli

4. Instalar e Configurar PostgreSQL

O PostgreSQL é um dos bancos de dados mais robustos e usados no mercado.
Bash

sudo apt install postgresql postgresql-contrib -y
sudo systemctl status postgresql
sudo systemctl start postgresql
sudo systemctl enable postgresql

Configuração do Banco de Dados

Agora, vamos criar um usuário e um banco de dados para os seus projetos.
Entre no terminal do psql com o usuário postgres:
Bash

sudo -u postgres psql

Dentro do psql, execute os seguintes comandos:
SQL

CREATE USER fitness_user WITH PASSWORD 'fitness123';
CREATE DATABASE fitness_db OWNER fitness_user;
GRANT ALL PRIVILEGES ON DATABASE fitness_db TO fitness_user;
\q

5. Instalar VS Code e Extensões Essenciais

Instalar o VS Code

Vamos instalar o editor de código mais popular para desenvolvimento web.
Bash

wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -o root -g root -m 644 packages.microsoft.gpg /etc/apt/trusted.gpg.d/
sudo sh -c 'echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/trusted.gpg.d/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list'
sudo apt update
sudo apt install code -y

Instalar Extensões

Essas extensões são cruciais para a produtividade em projetos Java/Spring e Angular.
Bash

code --install-extension vscjava.vscode-java-pack
code --install-extension vmware.vscode-spring-boot
code --install-extension vscjava.vscode-spring-initializr
code --install-extension vscjava.vscode-spring-boot-dashboard
code --install-extension redhat.java
code --install-extension vscjava.vscode-maven

code --install-extension angular.ng-template
code --install-extension ms-vscode.vscode-typescript-next
code --install-extension bradlc.vscode-tailwindcss
code --install-extension esbenp.prettier-vscode
code --install-extension ms-vscode.vscode-eslint
code --install-extension formulahendry.auto-rename-tag
code --install-extension christian-kohler.path-intellisense

code --install-extension ms-vscode.vscode-json
code --install-extension redhat.vscode-yaml
code --install-extension ms-python.python
code --install-extension ms-vscode.powershell
code --install-extension eamodio.gitlens
code --install-extension ritwickdey.liveserver
code --install-extension ms-vscode.remote-containers
code --install-extension ms-vscode-remote.remote-ssh

6. Configurar o VS Code

É importante garantir que o VS Code utilize o Java e o Maven que instalamos.
Abra o arquivo de configurações do VS Code:
Bash

code ~/.config/Code/User/settings.json

Adicione o seguinte JSON ao arquivo (você pode copiar e colar o bloco inteiro):
JSON

{
  "java.home": "/usr/lib/jvm/java-17-openjdk-amd64",
  "java.configuration.runtimes": [
    {
      "name": "JavaSE-17",
      "path": "/usr/lib/jvm/java-17-openjdk-amd64"
    }
  ],
  "spring-boot.ls.java.home": "/usr/lib/jvm/java-17-openjdk-amd64",
  "maven.executable.path": "/usr/bin/mvn",
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.organizeImports": true
  },
  "typescript.preferences.importModuleSpecifier": "relative",
  "angular.enable-strict-mode-prompt": false,
  "emmet.includeLanguages": {
    "typescript": "html"
  },
  "files.associations": {
    "*.html": "html"
  }
}

7. Testes e Verificação Final

Para garantir que tudo foi instalado corretamente, execute todos os comandos de verificação de uma vez.
Bash

echo "=== JAVA ==="
java -version
echo "JAVA_HOME: $JAVA_HOME"
echo "=== MAVEN ==="
mvn -version
echo "=== NODE ==="
node -v
npm -v
echo "=== ANGULAR ==="
ng version
echo "=== POSTGRESQL ==="
sudo systemctl status postgresql | grep Active
echo "=== VSCODE =="
code --version
echo "=== EXTENSÕES INSTALADAS ==="
code --list-extensions | grep -E "(java|spring|angular|typescript)"

Criar e Testar Projetos Exemplo

Para um teste completo, você pode criar um projeto Spring Boot e um Angular.

Teste do Spring Boot:
Bash

mkdir ~/teste && cd ~/teste
curl https://start.spring.io/starter.tgz \
  -d dependencies=web \
  -d type=maven-project \
  -d javaVersion=17 \
  -d groupId=com.teste \
  -d artifactId=teste-api \
  | tar -xzvf -

cd teste-api
./mvnw spring-boot:run

Teste do Angular:
Bash

cd ~/teste
ng new teste-app --routing --style=scss --skip-git
cd teste-app
ng serve
