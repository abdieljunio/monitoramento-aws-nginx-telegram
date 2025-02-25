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

### 4.2 Volte a AWS vá para ec2 clique em "instances", selecione a instância criada e depois clique no botâo azul "Conect" no canto superior direito.
Tela de exemplo:
![pt1](https://github.com/user-attachments/assets/f4cf2371-eab8-4eee-852f-92b91389540b)

### 4.3. Com a seguinte tela aberta, vá ao teminal no VS Code utilize o comando `cd <caminho até a pasta>` para acessar a pasta em que a chave ssh está armazenada, depois copie e cole o codigo do passo 3 da imagem e presssione Enter e em seguida copie e cole o comando de "Exemple" na imagem e pressione Enter depois de alguns segundos digite "yes" para acessar a EC2.
![pt2](https://github.com/user-attachments/assets/9a2017ac-2566-4af7-ae11-3997863d8bf2)

# 5. Instalar o Nginx.
- 5.1. Prieiramente digite os comandos `sudo apt update` para checar atualizações e depois `sudo apt upgrade -y` para realizar as atualizações.
- 5.2. Digite o comando para instalar o nginx: `sudo apt install nginx -y`.
- 5.3. Digite o comando para iniciar o nginx: `sudo systemctl start nginx`. 
- 5.4. Digite o comando para habilitar o nginx: `sudo systemctl enable nginx` .
- 5.5. Digite o comando para checar o status do nginx, que deverá retornar uma mensagem positiva: `sudo systemctl status nginx` .

# 6. Criando uma página HTML.
 - 6.1. Digite o comando para criar um diretótio para o site: `sudo mkdir -p /var/www/meusite.com/html` .
 - 6.2. Digite os comandos para definir as permissões: `sudo chown -R $USER:$USER /var/www/meusite.com/html` `sudo chown -R www-data:www-data /var/www/meusite.com/html` `sudo chmod -R 755 /var/www/meusite.com/html`.
 - 6.3.  Digite o comando para criar o arquivo html: `sudo nano /var/www/meusite.com/html/index.html` .
 - 6.4 cole o codigo:
```
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Carrossel de Informações</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            text-align: center;
            padding: 50px;
        }
        .carousel {
            position: relative;
            max-width: 600px;
            margin: auto;
            overflow: hidden;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            background: white;
        }
        .slides {
            display: flex;
            transition: transform 0.5s ease-in-out;
        }
        .slide {
            min-width: 100%;
            box-sizing: border-box;
            padding: 20px;
        }
        .slide h2 {
            color: #333;
        }
        .slide p {
            color: #666;
            font-size: 18px;
        }
        .navigation {
            position: absolute;
            width: 100%;
            top: 50%;
            display: flex;
            justify-content: space-between;
            transform: translateY(-50%);
            pointer-events: none;
        }
        .navigation button {
            background-color: rgba(0, 0, 0, 0.5);
            border: none;
            color: white;
            padding: 10px;
            cursor: pointer;
            pointer-events: all;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .navigation button:hover {
            background-color: rgba(0, 0, 0, 0.8);
        }
        .navigation button svg {
            width: 20px;
            height: 20px;
        }
        .navigation button.prev {
            margin-left: 10px;
        }
        .navigation button.next {
            margin-right: 10px;
        }
    </style>
</head>
<body>
    <div class="carousel">
        <div class="slides">
            <div class="slide">
                <h2 style="color: #003366;">O que é Nginx?</h2>
                <p>O Nginx é um servidor web de código aberto conhecido por sua alta performance e estabilidade. Ele também pode ser usado como proxy reverso, balanceador de carga e cache HTTP.</p>
            </div>
            <div class="slide">
                <h2 style="color: #FF6600;">Amazon EC2</h2>
                <p>O Amazon Elastic Compute Cloud (EC2) é um serviço da AWS que fornece capacidade de computação escalável na nuvem, permitindo que os usuários executem aplicações em um ambiente virtual.</p>
            </div>
            <div class="slide">
                <h2 style="color: #003366;">Scripts de Monitoramento em Python</h2>
                <p>Scripts de monitoramento em Python são utilizados para verificar a disponibilidade e performance de páginas web, enviando alertas em caso de falhas ou degradação do serviço.</p>
            </div>
        </div>
        <div class="navigation">
            <button class="prev" id="prev" aria-label="Slide anterior">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="white"><path d="M15.41 7.41L14 6l-6 6 6 6 1.41-1.41L10.83 12z"/></svg>
            </button>
            <button class="next" id="next" aria-label="Próximo slide">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="white"><path d="M8.59 16.59L10 18l6-6-6-6-1.41 1.41L13.17 12z"/></svg>
            </button>
        </div>
    </div>

    <script>
        const slides = document.querySelector('.slides');
        const slide = document.querySelectorAll('.slide');
        let currentIndex = 0;

        document.getElementById('next').addEventListener('click', () => {
            currentIndex = (currentIndex + 1) % slide.length;
            updateCarousel();
        });

        document.getElementById('prev').addEventListener('click', () => {
            currentIndex = (currentIndex - 1 + slide.length) % slide.length;
            updateCarousel();
        });

        function updateCarousel() {
            slides.style.transform = `translateX(-${currentIndex * 100}%)`;
        }
    </script>
</body>
</html>


```
pressione Ctrl+X  depis Y depos Enter para salvar e fechar o arquivo.

# 7. Configurar o Nginx para servir a página.
- 7.1. Dogote o comando para criar um arquivo de configuração para o site: `sudo nano /etc/nginx/sites-available/meusite.com`.
E adicione oseguinte comteúdo:
  ```
  server {
    listen 80;
    server_name meusite.com;

    root /var/www/meusite.com/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
   }

  ```
  pressione Ctrl+X  depis Y depos Enter para salvar e fechar o arquivo.
- 7.2. Digite o comando  para criar um link simbólico: `sudo ln -s /etc/nginx/sites-available/meusite.com /etc/nginx/sites-enabled/`.
- 7.3. Digite o comando para desabilitar o site padrão: `sudo rm /etc/nginx/sites-enabled/default` .
- 7.4. Digite o comando para testar a configuração do Nginx: `sudo nginx -t` .
 você receberá uma mensagem dizendo "Syntax is ok" e "teste is successful"
- 7.5 Digite o comando para recarregar o nginx:  `sudo systemctl reload nginx` .

# 8. Acessar a página html.
  Volte para a tela do passo 4.3. e copie o ip disponobilizado no tópico 4, e copie na barra de link do seu navegador para exibir a seguinte página:
![Captura de tela 2025-02-25 173728](https://github.com/user-attachments/assets/a59fd721-4b5a-468e-80c0-1a7b1578fc59)


# 9. Criar um serviço systemd para reiniciar o Nginx automaticamente.
- 9.1. Digite o comanddo `sudo nano /etc/systemd/system/nginx.service` para criar o arquivo de unidade do systemd e adicione o seguinte conteúdo:
```
[Unit]
Description=The NGINX HTTP and reverse proxy server
After=network.target

[Service]
ExecStart=/usr/sbin/nginx
ExecReload=/usr/sbin/nginx -s reload
ExecStop=/usr/sbin/nginx -s quit
Restart=always
RestartSec=5s
Type=forking
PIDFile=/run/nginx.pid

[Install]
WantedBy=multi-user.target

```
 pressione Ctrl+X  depis Y depos Enter para salvar e fechar o arquivo.
- 9.2. Digite o comando para recarregar o systemd: `sudo systemctl daemon-reload` .
- 9.3. Digite o comando para iniciar o serviço: `sudo systemctl start nginx` .
- 9.3. Digite o comando para habilitar o serviço: `sudo systemctl enable nginx` .
- 9.4. Digite o comando para verificar o status do nginx:  `sudo systemctl status nginx`.
  Se o status estiver "active(running)", faça os seguintes testes:
  Use o comando `sudo pkill -f nginx` para desativar o nginx.
  Use o comado `sudo systemctl status nginx` e você verá que ele foi desativado e reiniciou automaticamente.

# 10. Criar o bot no Telegram.
  - 10.1. Na barra de pesqiosa do telegram digite: `BotFather`, e selecione o bot com selo verificado e com o nome EXATAMENTE igual ao digitado.
  - 10.2. No chat do BotFather digite: `/start`, e siga as instruções para criar o bot.
  - 10.3. Após criar o bot você receberá um token, envie uma mensagem qualquer para o seu bot e depois acesse o seguite link: `https://api.telegram.org/bot<SEU_TOKEN>/getUpdates` (Substitua <SEU_TOKEN> pelo token do seu bot).
  - Esse link ira abrir um json, procure o campo `id` e salve este número.
 
# 11. Criar um script em Python para monitoramento do site.
   - 11.1. Digite o comando para instalar o Python3: `sudo apt install python3-pip`
   - 11.2. Digite o comando para criar um diretótio para o script: `sudo mkdir /opt/monitoramento`
   - 11.3. Digite o comando para criar o script: `sudo nano /opt/monitoramento/monitoramento.py`
      coloque o seguinte script e realize as configurações pedidas:
      ```
      #!/usr/bin/env python3

      import requests
      import logging
      from datetime import datetime

      # Configurações
      URL = 'http://seusite.com'  # Substitua pelo URL do seu site
      TELEGRAM_TOKEN = 'SEU_TOKEN_AQUI'  # Substitua pelo token do seu bot
      TELEGRAM_CHAT_ID = 'SEU_CHAT_ID_AQUI'  # Substitua pelo seu chat_id

      # Configuração de logging
      logging.basicConfig(filename='/var/log/monitoramento.log',
                    level=logging.INFO,
                    format='%(asctime)s - %(levelname)s - %(message)s')

      def enviar_notificacao(mensagem):
      """Envia uma mensagem de notificação via Telegram."""
      url = f'https://api.telegram.org/bot{TELEGRAM_TOKEN}/sendMessage'
      payload = {
        'chat_id': TELEGRAM_CHAT_ID,
        'text': mensagem
      }
      try:
        response = requests.post(url, data=payload)
        response.raise_for_status()
      except requests.RequestException as e:
        logging.error(f'Erro ao enviar notificação: {e}')

      def verificar_site():
      """Verifica a disponibilidade do site."""
      try:
        response = requests.get(URL, timeout=10)
        if response.status_code == 200:
            logging.info('Site está disponível.')
        else:
            mensagem = f'Site retornou status code {response.status_code}.'
            logging.warning(mensagem)
            enviar_notificacao(mensagem)
      except requests.RequestException as e:
        mensagem = f'Site está indisponível: {e}'
        logging.error(mensagem)
        enviar_notificacao(mensagem)

      if __name__ == '__main__':
      verificar_site()

      ```

      # 12. Configurar a execução automática do script a cada 1 minuto.
      - 12.1. Digite o comando para criar o arquivo de serviço so systemd: `sudo nano /etc/systemd/system/monitoramento.service`.
        Adicione o seguinte conteúdo:
        ```
        [Unit]
        Description=Monitoramento da disponibilidade do site

        [Service]
        ExecStart=/usr/bin/python3 /opt/monitoramento/monitoramento.py

        ```
        - 12.2. Digite o comando para criar o arquivo de Timer: `sudo nano /etc/systemd/system/monitoramento.timer`.
          Adicione o seguint conteúdo:
          ```

          [Unit]
          Description=Timer para monitoramento do site a cada minuto

          [Timer]
          OnBootSec=1min
          OnUnitActiveSec=1min

          [Install]
          WantedBy=timers.target

          ```
          - 12.3. Digite os seguintes comados separadamente para ativar e iniciar o timer:
            `sudo systemctl daemon-reload`
            `sudo systemctl enable monitoramento.timer`
            `sudo systemctl start monitoramento.timer`
           - 12.4. Para testar as notificações você pode testar os seguintes comandos:
               - Renomear o script que monitora a página html: `sudo mv /var/www/meusite.com/html /var/www/meusite.com/html_bkp` , isso fará você ser notificado do erro 404. Para restaurar `sudo mv /var/www/meusite.com/html_bkp /var/www/meusite.com/html`
               - Parar o nginx desativando o reinício automático: `sudo systemctl disable --now nginx`
               - Comando para testar (parar o nginx por 2 min): `sleep 120 && sudo systemctl start nginx`
               - Para reabilitar o reinício automático : `sudo systemctl enable nginx`
# Conclusão
Com estas etapas, você terá configurado uma instância EC2 na AWS, instalado o Nginx, configurado uma página HTML simples e implementado um sistema de monitoramento para garantir que o seu site esteja sempre disponível, com notificações via Telegram em caso de falha.
