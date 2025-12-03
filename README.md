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

##  Etapa 1: Criar um grupo de segurança para a instância de banco de dados do RDS

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

Role até a parte inferior da tela e clique em Criar grupo de segurança.

Você usará esse grupo de segurança ao iniciar o banco de dados do Amazon RDS.

---

##  Etapa 2: Criar um grupo de sub-redes de banco de dados

Nesta tarefa, você criará um grupo de sub-redes de banco de dados que é usado para informar ao RDS quais sub-redes podem ser usadas com o banco de dados. Cada grupo de sub-redes de banco de dados requer sub-redes em pelo menos duas Zonas de Disponibilidade.

No Console de Gerenciamento da AWS, selecione o menu  Serviços e escolha RDS em Banco de dados.

No painel de navegação esquerdo, clique em Grupos de sub-redes.

 Se o painel de navegação não estiver visível, clique no ícone de menu  no canto superior esquerdo.

Clique em Criar grupo de sub-redes de banco de dados e configure:

Nome: DB Subnet Group

Descrição: DB Subnet Group

ID da VPC: Lab VPC (VPC do laboratório)

Na seção Adicionar sub-redes para Zonas de disponibilidade, clique em , depois:

Selecione  a primeira Zona de Disponibilidade

Selecione  a segunda Zona de Disponibilidade

Para Sub-redes, clique em , depois:

Para a primeira Zona de Disponibilidade, selecione  10.0.1.0/24

Para a segunda Zona de Disponibilidade, selecione  10.0.3.0/24

Clique em Criar

Isso adiciona a sub-rede privada 1 (10.0.1.0/24) e a sub-rede privada 2 (10.0.3.0/24). Você usará esse grupo de sub-redes de banco de dados ao criar o banco de dados na próxima tarefa.

---

##  Etapa 3: Criar uma instância de banco de dados do Amazon RDS

Nesta tarefa, você configurará e iniciará uma instância de banco de dados multi-AZ do Amazon RDS para MySQL.

As implantações multi-AZ do Amazon RDS proporcionam disponibilidade e durabilidade melhores para instâncias de banco de dados (DB), o que as torna a solução ideal para cargas de trabalho de banco de dados de produção. Quando você provisiona uma instância de banco de dados multi-AZ, o Amazon RDS cria automaticamente uma instância de banco de dados primário e replica sincronicamente os dados para uma instância de espera em uma Zona de Disponibilidade (AZ) diferente.

No painel de navegação à esquerda, clique em Bancos de dados.

Clique em Criar banco de dados

Se aparecer a opção Switch to the new database creation flow (Alternar para o novo fluxo de criação de banco de dados) na parte superior da tela, clique nela.

Escolha Criar banco de dados e selecione Criação padrão.

Na seção Opções do mecanismo, em Tipo de mecanismo, selecione MySQL.

Em Versão do mecanismo, escolha a versão mais recente.

Em Modelos, selecione Dev/teste.

Em Disponibilidade e durabilidade, selecione Instância de banco de dados Multi-AZ.

Em Configurações, defina o seguinte:

Identificador de instância do banco de dados: lab-db

Nome do usuário principal: main

Senha principal: lab-password

Confirmar senha: lab-password

Em Configuração da instância, defina o seguinte para Classe da instância de banco de dados:

Selecione  Classes com capacidade de intermitência (inclui classes t).

Selecione db.t3.medium. 

Em Armazenamento, configure:

Selecione Finalidade geral (SSD) em Tipo de armazenamento.

Em Conectividade, configure:

Nuvem privada virtual (VPC): Lab VPC (VPC de laboratório)

Em Grupo de segurança da VPC, selecione Escolher existente

Em Grupos de segurança da VPC existentes

Use o X para remover padrão.

Selecione Grupo de segurança do banco de dados para realçá-lo em azul.

Em Monitoramento, expanda Configuração adicional e defina o seguinte:

Em Monitoramento aprimorado, escolha  Habilitar monitoramento avançado.

Role para baixo até a seção  Configuração adicional e expanda essa opção. Depois, configure:

Nome do banco de dados inicial: lab

Em Backup, desmarque a opção  Habilitar backups automatizados.

Isso desativará os backups, o que normalmente não é recomendado, mas agilizará a implantação do banco de dados para este laboratório.

Role até a parte inferior da tela e selecione Criar banco de dados

O seu banco de dados agora será executado.

Clique em lab-db (clique no próprio link).

Agora você precisará aguardar aproximadamente 4 minutos para que o banco de dados esteja disponível. O processo de implantação está implantando um banco de dados em duas Zonas de Disponibilidade diferentes.

Observação: se você receber a janela de prompt Suggested add-ons for lab-db (Complementos sugeridos para lab-db), escolha Fechar

Enquanto aguarda, você pode revisar as Perguntas frequentes sobre o Amazon RDS ou tomar um café.

Aguarde até o Status mudar para Modificando ou Disponível.

Role para baixo até a seção Conectividade e segurança e copie o campo Endpoint.

Será semelhante a: lab-db.cggq8lhnxvnv.us-west-2.rds.amazonaws.com

Cole o valor de Endpoint em um editor de texto. Você o usará mais tarde no laboratório.

--- 

##  Etapa 4: Interagir com o seu banco de dados

Nesta tarefa, você abrirá um aplicativo web em execução no servidor da web e o configurará para usar o banco de dados.

Copie o endereço IP do WebServer selecionando a opção i Detalhes da AWS acima dessas instruções que você está lendo no momento.

Abra uma nova guia do navegador da web, cole o endereço IP de WebServer e pressione Enter.

O aplicativo web será exibido com informações sobre a instância do EC2.

Na parte superior da página do aplicativo web, clique no link RDS.

---
<img width="887" height="434" alt="webapp" src="https://github.com/user-attachments/assets/a8569940-21e9-4933-9e04-fc01246e2f59" />


Figura: uma imagem mostrando a interface do aplicativo web.

 

Agora, você vai configurar o aplicativo para se conectar ao banco de dados.

Defina as seguintes configurações:

Endpoint: cole o endpoint que você copiou em um editor de texto anteriormente

Banco de dados: lab

Nome do usuário: main

Senha: lab-password

Clique em Enviar

Uma mensagem será exibida explicando que o aplicativo está executando um comando para copiar informações para o banco de dados. Após alguns segundos, a aplicação exibirá um Address Book (Catálogo de endereços).

O aplicativo Address Book está usando o banco de dados do RDS para armazenar informações.

Teste o aplicativo web adicionando, editando e removendo contatos.

Os dados estão sendo mantidos no banco de dados e são replicados automaticamente para a segunda Zona de Disponibilidade.

 


