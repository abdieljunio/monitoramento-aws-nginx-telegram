# Guia de Implantação e Monitoramento de Site no AWS com Nginx e Notificações via Telegram

## Descrição do Repositório

Este repositório contém um Guia de Implantação e Monitoramento de Site na AWS utilizando Nginx para servir páginas web e um bot do Telegram para notificações automáticas sobre a disponibilidade do site.

## Principais funcionalidades:

- Implantação de uma instância EC2 na AWS
- Configuração do Nginx para servir páginas web
- Automação do serviço Nginx com systemd
- Criação de um script Python para monitoramento do site
- Envio de notificações via Telegram em caso de falha
- Agendamento do monitoramento com systemd timers

## Tecnologias utilizadas:

1.  AWS EC2
2.  Nginx
3.  Python (requests, logging)
4.  Telegram Bot API
5.  Systemd (services & timers)
6.  Ubunto

## Pré-requisitos

- Conta na AWS
- Visual Studio Code (ou outro terminal de sua preferência)
- Telegram
- Acesso a internet

# 1.Criar uma **VPC** na AWS com:

✅ Duas **sub-redes públicas** (para acesso externo).

✅ Duas **sub-redes privadas** (para futuras expansões).

✅ Uma **Internet Gateway** conectada às sub-redes públicas.

### 1.1. Na tela inicial da Amazon AWS clique na barra de pesquisa (seta rosa) e digite "vpc", depois clique no ícone VPC (seta verde).
![Criando vpc-pt0](https://github.com/user-attachments/assets/51d05497-005c-45eb-b87e-399e083532e7)
### 1.2. Cliqueem "Create VPC" (seta verde).
![Criando vpc-pt1](https://github.com/user-attachments/assets/85c61403-4d38-4f96-8a8b-1084b3d1bf19)
### 1.3. Selecione a opção "VPC and more" (seta verde), verifique se todas as configurações estão como nas imagens e depois clique no botão "Create VPC".
![criando vpc-pt2](https://github.com/user-attachments/assets/8de527e7-a9c2-45a6-9b41-65d4fc77c06b)
![criando vpc-pt3](https://github.com/user-attachments/assets/8583c01f-209c-47ec-a5ad-b346e4453f63)
### 1.4. Se tudo estiver certo aparecerá uma tela parecida com essa:
![criando vpc-pt4](https://github.com/user-attachments/assets/f3b9a755-8267-4cbf-888f-4a260210e3f9)
Após esses passos volte para a tela inicial clicando no icone AWS co canto superior esquerdo da última imagem

# 2.Criar um Security Group:
### 2.1. Na tela inicial da Amazon AWS clique na barra de pesquisa (seta verde) e digite "ec2", depois clique no ícone "EC2"(seta rosa).
![sg - pt0](https://github.com/user-attachments/assets/6b7b14d2-3115-4240-a463-824a8f733c0e)
### 2.2. clique em "Security Groups"(seta rosa).
![sg - pt1](https://github.com/user-attachments/assets/00ca9531-cd1c-4799-bbfa-5c0a8156d209)
### 2.3. Clique no botão cor-de-laranja no canto superior direito "Create Security Group".
![sg - pt2](https://github.com/user-attachments/assets/46378682-cbc5-4be9-a6ae-82e087f28a00)
### 2.4. Faça as seguintes configurações (Lembre-se de escolher a VPC criada no passo 1)
 ![sg - pt3](https://github.com/user-attachments/assets/aa590052-94ce-4120-bc02-e58807e450f1)
 ![sg - pt4](https://github.com/user-attachments/assets/a1a49064-3e68-4226-ac28-b0d9f3d3a4e5)
#### Para adicionar as regras basta clicar no botão "Add rule".
#### Depois basta clicar no botão cor-de-laranja "Create security group"
![sg - pt5](https://github.com/user-attachments/assets/77a485af-11fa-45f0-a8ef-c6f88f89be84)

# 3.Criar uma EC2
### 3.1. Repita o passo 2.1 [clique aqui](#21-na-tela-inicial-da-amazon-aws-clique-na-barra-de-pesquisa-seta-verde-e-digite-ec2-depois-clique-no-ícone-ec2seta-rosa)
### 3.2. Na barra do lado direiro clique em "Instances".
![ec2 - pt0](https://github.com/user-attachments/assets/c28b4369-6a46-4c38-8bf1-bbc24064d9ef)
### 3.3. Clique no botão cor-de-laranja no cannto superiot direito "Launch instances"
![ec2 - pt1](https://github.com/user-attachments/assets/b656b107-3116-45a4-a054-fb85d616baf2)
### 3.4 Configurando a instância:
No campo "Name and tags" digite um nome para sua instância e no campo "Application and OS Images (Amazon Machine Image)" selecione a opção Ubunto.
![ec2 - pt2](https://github.com/user-attachments/assets/d728c87a-2dcc-43d2-838c-65aad125c02f) 

O campo "Instance tipe" eu deuxei no t2.micro pois ela faz parte do **free tier** mas o aconselhável é usar uma t3.micro 

![ec2 - pt3](https://github.com/user-attachments/assets/0df12938-8c79-437c-81e0-20a9872d88cb)

No campo "Key pair(login)" selecione a sua chave ssh.
No campo "Network Settings" haverá um botão azul escrito "Edit" clique nele e faça as seguintes configurações:
- VPC: escolha vpc criada por você
- Subnet: escolha uma subnet publica(1a ou 1b)
- Auto-assign public IP : Enable
- Firewall (security group): clique em "Select existing security group"
- Common security groups: escolha o security group criado anteriormente
  ![ec2 - pt4](https://github.com/user-attachments/assets/a41bab14-cdab-46fe-8a73-ceee032eba5d)

  No campo "Configure storage" eu deixei nas configuração padrão.
  
  ![ec2 - pt5](https://github.com/user-attachments/assets/4e2c69b0-d97e-42aa-abf7-de70c111e199)
### 3.5. Por último é só clicar no botão cor de laranja no canto inferior direito "Launch instance" (primeira imagem do passo 3.4) e você será redirecionado para esssa tela:

![ec2 - pt6](https://github.com/user-attachments/assets/1905df3f-832a-4137-8ae1-5000ee3bf1e0)

# 4. Conectar à instancia via SSH.
### 4.1. Abra o Visual Studio Code (VS Code) e pressione as teclas Ctrl+Shift+' para abrir um novo terminal e siga a imagem para abrir o Git Bash:
![pt0](https://github.com/user-attachments/assets/b5dd151f-dbb4-4c2e-8ed9-469ae5b6a9d5)

### 4.2
