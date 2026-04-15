# 🛡️ Guia de Configuração: SSH & UFW no Ubuntu 24.04 (Docker)

![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![SSH](https://img.shields.io/badge/SSH-000000?style=for-the-badge&logo=ssh&logoColor=white)
![Firewall](https://img.shields.io/badge/UFW-Firewall-green?style=for-the-badge)

## 📖 Propósito
Este documento fornece um guia passo a passo para instalar e configurar acesso remoto seguro (SSH) e um firewall básico (UFW) em uma imagem minimalista do Ubuntu 24.04 rodando em um container Docker.

Este guia é voltado para fins acadêmicos na disciplina de **Gerência de Configuração e Segurança de Infraestrutura (GCSI)**.

---

## 🛠️ Requisitos e Preparação

Devido à natureza minimalista dos containers Docker, a imagem padrão não possui os serviços de rede pré-instalados. É necessário prepará-lo.

### Passo 1: Iniciar o Container com Permissões
Para testar o firewall corretamente, o container precisa de acesso à stack de rede do host.

```bash
docker run -it --name GerenciaConfiguracao --privileged -p 2222:22 ubuntu:24.04 /bin/bash

Passo 2: Atualizar o Repositório de Pacotes
Dentro do container, antes de instalar qualquer coisa:


apt update

🔑 Configuração de Acesso Remoto (SSH)
O OpenSSH Server permitirá conexões remotas para administração.

1. Instalação

apt install openssh-server -y


2. Configuração de Segurança e Ambiente

Crie o diretório necessário para o funcionamento do daemon e altere a senha do usuário root (ou crie um novo usuário).

# Criar diretório do processo
mkdir /var/run/sshd

# Definir senha para o Root (Substitua 'sua_senha_segura')
echo 'root:defina sua senha' | chpasswd

⚙️ Configuração Avançada (Opcional):

Para permitir acesso direto do root via SSH (em ambientes de teste), edite o arquivo /etc/ssh/sshd_config.

# Permitir Login do Root
sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

3. Iniciar o Serviço SSH


service ssh start




Configuração do Firewall (UFW)

O UFW (Uncomplicated Firewall) gerencia o tráfego de rede de forma simplificada.

1. Instalação

apt install ufw -y

Entendi perfeitamente! Você quer um visual profissional, com títulos organizados, ícones, e blocos de código bem destacados, exatamente como o GitHub faz.

Para o seu arquivo ficar nesse estilo, você não deve salvar o texto "puro", mas sim usar a sintaxe Markdown.

Vou remontar todo o guia de configuração para você. Copie todo o conteúdo abaixo e cole dentro do seu arquivo README.md (pode apagar o que estiver lá e colocar este novo).

Markdown
# 🛡️ Guia de Configuração: SSH & UFW no Ubuntu 24.04 (Docker)

![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![SSH](https://img.shields.io/badge/SSH-000000?style=for-the-badge&logo=ssh&logoColor=white)
![Firewall](https://img.shields.io/badge/UFW-Firewall-green?style=for-the-badge)

## 📖 Propósito
Este documento fornece um guia passo a passo para instalar e configurar acesso remoto seguro (SSH) e um firewall básico (UFW) em uma imagem minimalista do Ubuntu 24.04 rodando em um container Docker.

Este guia é voltado para fins acadêmicos na disciplina de **Gerência de Configuração e Segurança de Infraestrutura (GCSI)**.

---

## 🛠️ Requisitos e Preparação

Devido à natureza minimalista dos containers Docker, a imagem padrão não possui os serviços de rede pré-instalados. É necessário prepará-lo.

### Passo 1: Iniciar o Container com Permissões
Para testar o firewall corretamente, o container precisa de acesso à stack de rede do host.

```bash
docker run -it --name GerenciaConfiguracao --privileged -p 2222:22 ubuntu:24.04 /bin/bash
Passo 2: Atualizar o Repositório de Pacotes
Dentro do container, antes de instalar qualquer coisa:

Bash
apt update
🔑 Configuração de Acesso Remoto (SSH)
O OpenSSH Server permitirá conexões remotas para administração.

1. Instalação
Bash
apt install openssh-server -y

2. Configuração de Segurança e Ambiente

Crie o diretório necessário para o funcionamento do daemon e altere a senha do usuário root (ou crie um novo usuário).


# Criar diretório do processo
mkdir /var/run/sshd

# Definir senha para o Root (Substitua 'sua_senha_segura')
echo 'root:sua_senha_segura' | chpasswd
⚙️ Configuração Avançada (Opcional):
Para permitir acesso direto do root via SSH (em ambientes de teste), edite o arquivo /etc/ssh/sshd_config.

2. Definição de Regras

Importante: Nunca ative o firewall antes de abrir a porta do SSH!

# Definir regras padrão: Bloquear Entrada, Permitir Saída
ufw default deny incoming
ufw default allow outgoing

# Abrir porta para o SSH
ufw allow 22/tcp

# (Opcional) Abrir porta HTTP para um servidor web
ufw allow 80/tcp

3. Ativação e Status
Bash
# Habilitar o Firewall
ufw enable

# Verificar se as regras foram aplicadas
ufw status verbose


Verificação Final

Se tudo funcionou, você deve conseguir se conectar ao container a partir da sua máquina host (fora do container) usando:


# Usando a porta 2222 que mapeamos no 'docker run'
ssh root@localhost -p 2222



