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

##  Etapa 1: Criar a VPC  

1. Na página do Console da AWS, ir na busca e digitar VPC
2. Na lateral da página, clicar em Suas VPCs e em seguida Criar VPC
3. Na página Criar VPC, selecionar Somente VPC e em seguida digitar o nome da VPC: Lab VPC
4. Em CIDR IPv4 digitar o endereçamento IP conforme o diagrama: 10.0.0.0/16
5. Clicar em Criar VPC
6. Pronto, a VPC foi criada e configurada
---
