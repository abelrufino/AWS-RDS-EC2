<img width="128" height="128" alt="aws" src="https://github.com/user-attachments/assets/93da4daf-5642-4bac-8b1c-6c9707016fee" />

# AWS-RDS-EC2
Criar um servidor de banco de dados e interagir com o banco de dados usando um aplicativo
 
Este laboratório **Este laboratório foi criado para reforçar o conceito de utilização de uma instância de banco de dados gerenciada pela AWS para atender às necessidades de banco de dados relacional.**.

---

## 🚀 Objetivo do laboratório 🚀

Depois de concluir este laboratório, você será capaz de:

- Executar uma instância de banco de dados do Amazon RDS com alta disponibilidade.
- Configurar a instância de banco de dados para permitir conexões do seu servidor web.
- Abrir um aplicativo web e interagir com seu banco de dados.

---
Cenário
Você começa com a seguinte infraestrutura:

![architecture-lab1](https://github.com/user-attachments/assets/088ea348-a361-4cc4-9ede-44befd1cb987)

---
No final do laboratório, essa é a infraestrutura:

![architecture-lab2](https://github.com/user-attachments/assets/24db1097-5054-4b07-be77-9d675040c51a)

##  Etapa 1: Tarefa 1: Criar um grupo de segurança para a instância de banco de dados do RDS

Nesta tarefa, você criará um grupo de segurança para permitir que o servidor web acesse a instância de banco de dados do RDS. O grupo de segurança será usado quando você executar a instância de banco de dados.

No Console de Gerenciamento da AWS, selecione o menu  Serviços e escolha VPC em Redes e entrega de conteúdo.

No painel de navegação à esquerda, clique em Grupos de segurança.

Clique em Criar grupo de segurança e configure:

Nome do grupo de segurança: DB Security Group

Descrição: Permit access from Web Security Group

VPC: Lab VPC (VPC do laboratório)

Agora você adicionará uma regra ao grupo de segurança para permitir solicitações de entrada do banco de dados. No momento, o grupo de segurança não tem regras. Você adicionará uma regra para permitir acesso pelo Web Security Group (Grupo de segurança da web).

Na seção Regras de entrada, clique em Adicionar regra e configure:

Tipo: MySQL/Aurora (3306)

Origem: digite sg no campo de pesquisa e selecione Web Security Group (Grupo de segurança da web).

Isso configura o grupo de segurança do banco de dados para permitir tráfego de entrada na porta 3306 de qualquer instância do EC2 associada ao Web Security Group (Grupo de segurança da web).

Role até a parte inferior da tela e clique em ##Criar grupo de segurança.

Você usará esse grupo de segurança ao iniciar o banco de dados do Amazon RDS.
