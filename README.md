# 🛡️ Guia de Configuração: SSH & UFW no Ubuntu 24.04 (Docker)

![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![SSH](https://img.shields.io/badge/SSH-000000?style=for-the-badge&logo=ssh&logoColor=white)
![Firewall](https://img.shields.io/badge/UFW-Firewall-green?style=for-the-badge)

## 📖 Propósito
Este documento fornece um guia passo a passo para instalar e configurar acesso remoto seguro (SSH) e um firewall básico (UFW) em uma imagem minimalista do Ubuntu 24.04 rodando em um container Docker.

Este guia é voltado para fins acadêmicos na disciplina de **Gerência e Configuração de Sistemas Para Internet  (GCSI)**.

---

## 🛠️ Requisitos e Preparação

Devido à natureza minimalista dos containers Docker, a imagem padrão não possui os serviços de rede pré-instalados. É necessário prepará-lo.

### Passo 1: Iniciar o Container com Permissões
Para testar o firewall corretamente, o container precisa de acesso à stack de rede do host.

```
docker run -it --name GerenciaConfiguracao --privileged -p 2222:22 ubuntu:24.04 /bin/bash
```
### Passo 2: Atualizar o Repositório de Pacotes

De um update dentro do container, antes de instalar qualquer coisa.

```
apt update
```
Fica do seu critério acho também mais prático para mim usar a atualização e instalação ao mesmo tempo.
```
apt update && apt install openssh-server -y
```

### 🔑 Configuração de Acesso Remoto (SSH)

O OpenSSH Server permitirá conexões remotas para administração.

caso não tenha instalado logo com o "&&"

1. Instalação

```
apt install openssh-server -y
```

2. Configuração de Segurança e Ambiente

Crie o diretório necessário para o funcionamento do daemon e altere a senha do usuário root (ou crie um novo usuário).

# Criar diretório do processo

mkdir /var/run/sshd

# Definir senha para o Root (lembrando que deve substituir a senha pela sua 'sua senha')

```
echo 'root:defina sua senha' | chpasswd
```

### ⚙️ Configuração Avançada (Opcional):

Para permitir acesso direto do root via SSH (em ambientes de teste), edite o arquivo /etc/ssh/sshd_config.

# Permitir Login do Root

```
sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
```
3. Iniciar o Serviço SSH

```
service ssh start
```

se quiser verificar o status dele basta rodar

```
service ssh status
```
assim ele vai mostrar o status como active ou inative.


### Configuração do Firewall (UFW)

O UFW (Uncomplicated Firewall) gerencia o tráfego de rede de forma simplificada.

1. Instalação

```
apt install ufw -y
```

2. Definição de Regras

# Importante: Nunca ative o firewall antes de abrir a porta do SSH! isso pode perder o acesso remoto.

# Definir regras padrão: Bloquear Entrada, Permitir Saída

```
ufw default deny incoming
ufw default allow outgoing
```

# Abrir porta para o SSH

```
ufw allow 22/tcp
```


# Você também pode abrir porta HTTP para um servidor web


```
ufw allow 80/tcp
```

3. Ativação e Status


# Habilitar o Firewall

```
ufw enable
```

# Agora verifica se ta tudo em ordem, e se foi aplicado as regra

```
ufw status verbose
```

# Finalização


Se tudo funcionou, você deve conseguir se conectar ao container a partir da sua máquina host (fora do container)
 usando:


# porta 2222 que mapea no 'docker run'


```
ssh root@localhost -p 2222
```


