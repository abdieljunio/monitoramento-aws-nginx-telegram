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
