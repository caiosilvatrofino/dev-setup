# Setup Ambiente de Desenvolvimento Fullstack

Este guia configura um ambiente completo para desenvolvimento Java Spring Boot + Angular no Linux.

## Pré-requisitos

Sistema operacional Ubuntu/Debian/Linux Mint com acesso root.

## Instalação Base

### Atualizando o sistema
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install curl wget git build-essential -y
```

## Java Development Kit

### OpenJDK 17 LTS
```bash
sudo apt install openjdk-17-jdk -y
```

Verificar instalação:
```bash
java -version
javac -version
```

### Configuração JAVA_HOME
Adicionar ao arquivo `~/.bashrc`:
```bash
export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
export PATH=$PATH:$JAVA_HOME/bin
```

Aplicar mudanças:
```bash
source ~/.bashrc
echo $JAVA_HOME
```

## Apache Maven

### Instalação
```bash
sudo apt install maven -y
mvn -version
```

## Node.js e npm

### Via NodeSource Repository
```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs
```

Verificar versões:
```bash
node -v
npm -v
```

### Configurar npm global sem sudo (opcional)
```bash
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```

## Angular CLI

### Instalação global
```bash
npm install -g @angular/cli
ng version
```

## PostgreSQL

### Instalação e configuração
```bash
sudo apt install postgresql postgresql-contrib -y
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

### Criação de usuário e database
```bash
sudo -u postgres psql
```

No console do PostgreSQL:
```sql
CREATE USER app_user WITH PASSWORD 'dev_password';
CREATE DATABASE app_database OWNER app_user;
GRANT ALL PRIVILEGES ON DATABASE app_database TO app_user;
\q
```

## Docker e Docker Compose

### Instalação Docker
```bash
# Remover versões antigas
sudo apt-get remove docker docker-engine docker.io containerd runc

# Instalar dependências
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg lsb-release

# Adicionar chave GPG oficial
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# Configurar repositório
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Instalar Docker Engine
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

### Configurar usuário Docker
```bash
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker
```

### Testar instalação
```bash
docker run hello-world
docker compose version
```

## Visual Studio Code

### Instalação via repositório oficial
```bash
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -o root -g root -m 644 packages.microsoft.gpg /etc/apt/trusted.gpg.d/
sudo sh -c 'echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/trusted.gpg.d/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list'

sudo apt update
sudo apt install code -y
```

### Extensões recomendadas

#### Para Java/Spring Boot
```bash
code --install-extension vscjava.vscode-java-pack
code --install-extension vmware.vscode-spring-boot
code --install-extension vscjava.vscode-spring-initializr
code --install-extension vscjava.vscode-spring-boot-dashboard
code --install-extension vscjava.vscode-maven
```

#### Para Angular/TypeScript
```bash
code --install-extension angular.ng-template
code --install-extension ms-vscode.vscode-typescript-next
code --install-extension esbenp.prettier-vscode
code --install-extension ms-vscode.vscode-eslint
code --install-extension bradlc.vscode-tailwindcss
code --install-extension formulahendry.auto-rename-tag
```

#### Ferramentas gerais
```bash
code --install-extension ms-vscode.vscode-json
code --install-extension redhat.vscode-yaml
code --install-extension eamodio.gitlens
code --install-extension ms-azuretools.vscode-docker
code --install-extension humao.rest-client
```

## Configuração VSCode

### Settings.json recomendado
Caminho: `~/.config/Code/User/settings.json`

```json
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
  }
}
```

## Ferramentas adicionais

### Git configuração inicial
```bash
git config --global user.name "Seu Nome"
git config --global user.email "seu.email@exemplo.com"
git config --global init.defaultBranch main
```

### Postman (opcional)
```bash
sudo snap install postman
```

### DBeaver (cliente banco de dados)
```bash
sudo snap install dbeaver-ce
```

## Verificação da instalação

### Script de verificação
```bash
#!/bin/bash

echo "=== Verificação do Ambiente ==="

echo "Java:"
java -version
echo "JAVA_HOME: $JAVA_HOME"

echo -e "\nMaven:"
mvn -version

echo -e "\nNode.js:"
node -v

echo -e "\nnpm:"
npm -v

echo -e "\nAngular CLI:"
ng version --skip-git

echo -e "\nPostgreSQL:"
sudo systemctl is-active postgresql

echo -e "\nDocker:"
docker --version
docker compose version

echo -e "\nVSCode:"
code --version

echo -e "\n=== Instalação concluída ==="
```

## Estrutura de projeto recomendada

```
projeto-fitness/
├── backend/                # Spring Boot API
│   ├── src/
│   ├── pom.xml
│   └── Dockerfile
├── frontend/               # Angular App
│   ├── src/
│   ├── package.json
│   └── Dockerfile
├── docker-compose.yml      # Orquestração containers
├── docs/                   # Documentação
└── README.md
```

## Docker Compose básico

### docker-compose.yml
```yaml
version: '3.8'

services:
  database:
    image: postgres:15
    environment:
      POSTGRES_DB: app_database
      POSTGRES_USER: app_user
      POSTGRES_PASSWORD: dev_password
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

  backend:
    build: ./backend
    ports:
      - "8080:8080"
    depends_on:
      - database
    environment:
      SPRING_DATASOURCE_URL: jdbc:postgresql://database:5432/app_database
      SPRING_DATASOURCE_USERNAME: app_user
      SPRING_DATASOURCE_PASSWORD: dev_password

  frontend:
    build: ./frontend
    ports:
      - "4200:4200"
    depends_on:
      - backend
    volumes:
      - ./frontend:/app
      - /app/node_modules

volumes:
  postgres_data:
```

## Comandos úteis

### Desenvolvimento
```bash
# Iniciar banco local
sudo systemctl start postgresql

# Criar projeto Spring Boot
curl https://start.spring.io/starter.tgz \
  -d dependencies=web,data-jpa,security,postgresql \
  -d type=maven-project \
  -d javaVersion=17 \
  -d groupId=com.example \
  -d artifactId=app-backend \
  | tar -xzvf -

# Criar projeto Angular
ng new app-frontend --routing --style=scss

# Subir ambiente com Docker
docker compose up -d

# Parar ambiente
docker compose down
```

### Manutenção
```bash
# Atualizar npm packages
npm update -g

# Limpar cache Maven
mvn clean

# Atualizar Angular CLI
npm update -g @angular/cli

# Limpar containers Docker
docker system prune -f
```

## Troubleshooting

### Erro JAVA_HOME não encontrado
```bash
sudo update-alternatives --config java
# Copiar o caminho e configurar JAVA_HOME
```

### Erro permissão npm global
```bash
sudo chown -R $(whoami) ~/.npm
```

### PostgreSQL não conecta
```bash
sudo systemctl restart postgresql
sudo -u postgres psql -c "SELECT version();"
```

### Docker permissão negada
```bash
# Relogar após adicionar usuário ao grupo docker
su - $USER
```

---

**Ambiente configurado com sucesso!** Agora você pode desenvolver aplicações fullstack usando Java Spring Boot + Angular com suporte completo a containers Docker.
