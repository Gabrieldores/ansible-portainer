# ansible-portainer
install ansible portainer


# Ansible Playbook para Implantação do Portainer CE

![Ansible](https://img.shields.io/badge/Ansible-2.10%2B-blue.svg) ![Docker](https://img.shields.io/badge/Docker-blue.svg) ![Portainer](https://img.shields.io/badge/Portainer-CE-brightgreen.svg)

Este projeto utiliza Ansible para automatizar a implantação de uma instância do **Portainer Community Edition** em um servidor Docker remoto. O playbook é idempotente, o que significa que pode ser executado várias vezes sem causar efeitos colaterais indesejados.

## O que este Playbook faz?

-   **Cria um diretório** dedicado para a aplicação no servidor de destino.
-   **Gera um arquivo `docker-compose.yml`** a partir de um template Jinja2.
-   **Inicia os contêineres** do Portainer usando o módulo `docker-compose` do Ansible.
-   Garante que o serviço esteja sempre no estado desejado (rodando).

---

## 📂 Estrutura do Projeto

├── deploy-portainer.yml    # O playbook principal com as tarefas
├── hosts                   # Arquivo de inventário com os servidores
└── templates/
└── docker-compose.yml.j2 # Template do Docker Compose para o Portainer

## 📋 Pré-requisitos

Antes de executar o playbook, certifique-se de que você tem:

1.  **Ansible instalado** na sua máquina local (nó de controle).
    ```bash
    pip install ansible
    ```
2.  **Acesso SSH com chave pública** configurado entre a sua máquina local e o servidor de destino. O Ansible precisa se conectar sem pedir senha.
3.  Um **servidor de destino** (baseado em Debian/Ubuntu) com acesso à internet. O playbook assume que o Docker não está instalado, mas o instalará se necessário.

---

## ⚙️ Configuração

A configuração é feita principalmente no arquivo de inventário.

1.  **Edite o arquivo `hosts`** e preencha com as informações do seu servidor:

    ```ini
    # Arquivo de inventário do Ansible
    [docker_server]
    meuservidor ansible_host=IP_DO_SEU_SERVIDOR ansible_user=SEU_USUARIO_NO_SERVIDOR
    ```
    -   `IP_DO_SEU_SERVIDOR`: O endereço IP do seu host Docker.
    -   `SEU_USUARIO_NO_SERVIDOR`: O usuário com privilégios `sudo` no host Docker.

2.  **(Opcional)** Você pode customizar as variáveis (como portas e nome do diretório) diretamente na seção `vars` do arquivo `deploy-portainer.yml`.

---

## ▶️ Como Executar

1.  Navegue até o diretório raiz do projeto no seu terminal.
2.  Execute o seguinte comando:

    ```bash
    ansible-playbook -i hosts deploy-portainer.yml
    ```
    -   O `-i hosts` especifica que estamos usando o arquivo `hosts` como nosso inventário.

O Ansible se conectará ao servidor e executará todas as tarefas.

---

## ✅ Verificação

Após a execução bem-sucedida do playbook, a instância do Portainer estará disponível no seu navegador.

-   **URL:** `https://IP_DO_SEU_SERVIDOR:9443`
-   **Aviso:** Na primeira vez que você acessar, o navegador exibirá um aviso de segurança devido ao certificado SSL autoassinado do Portainer. Isso é normal. Você pode clicar em "Avançado" e "Prosseguir" com segurança.

Na primeira tela, você precisará criar seu usuário administrador para o Portainer.

---

## 📄 Licença

Este projeto é de código aberto e está sob a [Licença MIT](https://opensource.org/licenses/MIT).