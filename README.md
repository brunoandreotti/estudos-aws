# AWS Solution Architect Estudos

## Sumário

- [Introdução](#casos-de-uso-dos-serviços-da-aws)
- [Regiões](#regiões)
- [IAM e AWS CLI](#iam-identity-and-access-management-e-aws-cli)
- [Fundamentos EC2](#fundamentos-ec2)

## Casos de uso dos serviços da AWS

- Permite criar aplicações escaláveis e sofisticadas
- Aplicável para uma alta gama de indústrias
- TI Empresarial, Backup e armazenamento de grande volume de dados, Análise de Big Data
- Hospedar sites, aplicativos, jogos

## Regiões

São as regiões que possuem servidores da AWS (us-est-1, eu-west-3), são um aglomerado (cluster) de data centers.
Os serviços são isolados por região.

Se tentar utilizar um serviço que possui em uma região, em outra, será a mesma coisa de precisar configurar ele do zero.

### Como escolher uma região?

A resposta é: Depende. Mas podemos levar alguns pontos em consideração:

Compliance com a governance e requisitos legais da região: Um dado nunca sai de uma região sem a sua permissão.

Proximidade com os usuários para reduzir a latência

Serviços disponíveis. Nem todos os serviços e serviços novos são disponíveis em todas as regiões ao mesmo tempo.

Preço, uma vez que pode alterar dependendo da região.

### Zonas de disponibilidade (Availability Zones)

Cada região possui várias zonas (min 3 e max 6).

Exemplo:

- Região -> ap-southeast-2
  - Zonas
    - ap-southeast-2a
    - ap-southeast-2b
    - ap-southeast-2c
  
  São zonas fisicamente separadas com servidores conectados para redundância em casa de necessidade (desastres por exemplo)

### Pontos de presença (Points os presence (edge locations))

Os pontos que são entregues os dados para os usuários, gerando uma latência baixa.

### AWS Console

Serviços Globais
  IAM (Identity and Access Management)
  Route 53 (Serviço DNS)
  CloudFront (Content Delivery Network)
  WAF(Web Application Firewall)

Serviços com escopo regional
  Amazon EC2(Infrastructure as a Service)
  Elastic Beanstalk(Platform as a Service)
  Lambda(Function as a Service)
  Rekognition (Software as a Service)

## IAM (Identity and Access Management) e AWS CLI

- É um serviço Global
- A conta root não é para ser usada ou compartilhada, ela é apenas utilizada para configurações
- Para utilizar as funcionalidades é recomendado criar Users que podem ser agrupados
- Grupos só podem ter usuários e não outros grupos

- Ex:

  - Grupo 1 - Alice, Bob e Charles
  - Grupo 2 - David e Edward
  - Grupo 3 - Charles e David
  - Sem grupo - Fred

Como pode-se observar, um usuário pode fazer parte de um grupo, ou de vários grupos (por exemplo Charles e David) ou não fazer parte de grupo nenhum (por exemplo Fred)

### Por que queremos ou precisamos de criar grupos?

Por que queremos permitir eles a usarem as funcionalidades da conta AWS e para isso é necessário conceder permissões aos usuários.

Usuários e Grupos podem ser vinculados utilizando arquivos JSON chamados policies (políticas) que se parece com isso:

```json
{
  "Version": "2024-05-03"
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "ec2:Describe*",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "elasticloadbalancing:Describe*",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "cloudwatch:ListMetrics",
        "cloudwatch:GetMetricsStatistics",
        "cloudwatch:Describe*"
      ],
      "Resource": "*"
    }
  ]
}
```

Essas políticas definem as permissões de cada usuário

Na AWS aplicamos um princípio chamado 'principio do menor privilégio' onde damos apenas os privilégios necessários para o usuário.

### Grupos e Usuários na Prática

- Procurar IAM na busca
- Ir em Users
- Nessa tela é onde será criado os usuários do IAM
- Depois será necessário criar um nome, o tipo de permissão e uma senha.
- Após isso será necessário informar qual grupo o usuário fará parte.
- Nesse primeiro caso iremos criar um grupo com a permissão AdministratorAccess que garante todos os privilégios ao usuário
- Por último é possível criar tags (chave e valor) para o usuário sendo possível criar metadados para ele. Por exemplo: Departamento - Engenharia de Software.

Ao clicar em Users é possível ver detalhes dos usuários criados, como por exemplo quais as permissões dele e como elas foram dadas. Nesse caso é informado que a permissão AdministratorAccess foi dado pelo grupo 'admin' que criamos.

Na aba de Groups é possível visualizar os usuários daquele grupo e quais permissões aquele grupo possui.

Para logar com um usuário criado basta retornar para página de login do console AWS e logar com um IAM User

### IAM Policies

- Todos os usuários de um grupo terão a mesma permissão.
- Caso um usuário faça parte de mais um grupo, ele terá as permissões de todos os grupos.
- Caso um usuário não irá ou não possa fazer parte de um grupo, é possível criar uma política específica para ele - (inline policy).

A estrutura de uma política é em formato de JSON:

```json
{
  "Version": "2012-10-17"
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "ec2:Describe*",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "elasticloadbalancing:Describe*",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "cloudwatch:ListMetrics",
        "cloudwatch:GetMetricsStatistics",
        "cloudwatch:Describe*"
      ],
      "Resource": "*"
    }
  ]
}
```

E consiste em:

- Version: Versão da linguagem da política
- Id: um identificador opcional para o política
- Statements:
  - Sid: um identificador opcional para o Statement
  - Effect: Informa se o Statement garante permissão ou não (Allow, Deny)
  - Principal: indica qual conta, usuário ou role essa política será aplicada.
  - Action: Lista de ações que a política irá permitir ou negar.
  - Resources: Lista de recursos que essas ações serão aplicadas.
  - Condition: condições para quando a política terá efeito (opcional).

### IAM Policies na Prática

Caso o usuário não possua permissão para cessar algum recurso, irá aparecer uma mensagem de acesso negado para ele ao tentar acessar tal recurso.

Para retirar politicas em um usuário basta irmos em:

- Users e retirar as permissões do usuário.

Para adicionar políticas específicas para o usuário basta ir no usuário e clicar em 'criar nova permissão'.
Escolher o tipo de permissão (grupo, permissão específica) e adicionar os grupos ou permissões

### IAM MFA (Password Policy)

É possível criar políticas para a criação de senha dos usuários.
Na AWS podemos configurar as seguintes políticas:

- Mínimo de caracteres necessários
- Requerer tipos específicos de caracteres
- Permitir usuários alterarem a própria senha
- Requerer que usuárias alterer a senha de tempos em tempos (expiração de senha)
- Prevenir uso senha já utilizadas

Também é possível utilizar MFA para aumentar a segurança das contas e dos usuários, principalmente a conta root e usuários admin

MFA (Multi Factor Authentication) = senha que você sabe + dispositivo seguro que você possui.

A vantagem é mesmo que um usuário seja hackeado com a senha, ainda é necessário a permissão do dispositivo seguro para acessar a conta.

Opções de MFA na AWS:

- Aplicativo de autenticação
- Chave Física de Segurança

### Password Policy na Prática

Para criar uma política de senha, no painel IAM:

- Vá em 'Account Settings'
- Após isso irá aparecer a opção de editar a política de senha.

### Maneiras de acessar a AWS

- AWS Management Console (site) acessado por senha + MFA
- AWS CLI (Command Line Interface) protegido por chave de acesso (access key)
- AWS SDK (Software Developer Kit) utilizado para código também protegido por chave de acesso (access key)

As chaves de acesso são criadas através do AWS Console
Os usuários gerenciam as próprias chaves de acesso

Para criar uma chave de acesso basta ir no AWS Console:

- Vá em users e escolha o usuário desejado
- após isso você verá a opção de gerar uma chave de acesso
- Escolha para qual intuito será usada a chave
- Após isso será gerado um Access Key ID e um Secret Access Key

### AWS CLI

- AWS CLI é a interface de linha de comando da AWS, ela permite interagir com os serviços da AWS utilizando um terminal
- Dá acesso direto às APIs públicas dos serviços da AWS
- É possível criar scripts para automatizar e gerencias seus serviços
- Uma alternativa ao AWS MAnagement Console

### AWS SDK

- AWS Software Development Kit AWS
- Utilizado para acessar e gerenciar os serviços das AWS através de código
- Existem SDKs específicos para cada linguagem

### AWS CLI Na Prática

Para configurar o AWS CLI siga os passos:

- No terminal digite 'aws configure'
- Insira o seu AWS Access Key ID
- Digite o nome da sua região no AWS
- Aperte enter na ultima opção

Também é possível acessar o AWS CLI através do AWS CLoudShell que é o terminal 'online' da AWS.

### IAM Roles (Funções) for Services

Roles são permissões dadas para os serviços que utilizaremos na AWS

Por exemplo uma instância do EC2 precisando usar alguma funcionalidade na AWS, para isso é necessário dar a permissão para o serviço executar o que ele precisa.

Dessa maneira podemos dar/criar uma IAM Role para o serviço para que ele possa fazer o que ele precisa.

### IAM Roles Na Prática

No AWS Console, clique em Roles:

- Após isso irá aparecer a opção de criar uma role
- Selecione o serviço e quais permissões você quer dar para o serviço

### IAM Security Tools

IAM Credential Report (account-level)

- Um relatório que lista todos os usuários da sua conta e o status de suas várias credencias

IAM Access Advisor (user-level)

- Mostra as permissões garantidas para um usuário e quando os serviços foram acessados
- Útil para visualizar as permissões do usuário

### IAM Security Tools Na Prática

No AWS Console, dentro do IAM, clique em 'Credential Report'e depois em 'Download credential Report'

No AWS Console, dentro do IAM, clique en 'Users', selecione o usuário e depois clique em 'Access Advisor'

## Fundamentos EC2

### EC2 Básico

EC2 significa Elastic Compute Cloud e é uma Infrastructure as a Service

Consiste em vários serviços:

- Aluguel de máquina virtual (EC2 Instances)
- Guardar dados em drivers virtuais (EBS)
- Distribuir load entre máquina (load balance) (ELB)
- Escalar os serviços utilizando auto-scaling groups (ASG)

Tamanhos e configurações do EC2:

- Sistema operacional: Linux, Windows, MacOS
- Quantidade de poder computacional e núcleos (CPU)
- Quantidade de RAM
- Quantidade de armazenamento
  - Network-attached(EBS e EFS)
  - Hardware-attached(EC2 Instance Store)
- Tipo de rede: Velocidade, IPs públicos
- Regras de Firewall
- Configurações de inicialização: EC2 User Data

EC2 User Data

É possível configurar a inicialização das instâncias utilizando EC2 User data script

Dessa maneira podemos dar comandos para serem feitas algumas configurações enquanto a máquina está sendo iniciada

Esse script roda uma vez quando a instância é inciada pela primeira vez

EC2 user data é utilizada para automatizar algumas tarefas na hora do boot:

- Instalar atualizações
- Instalar softwares
- Baixar arquivos da internet
- Entre outras coisas

o EC2 USer Data Script roda com o usuário root da instância

Existem vários tipos/pacotes de instâncias de EC2, cada um com suas configurações, indo de configurações bem simples até configurações extremamente robustas.

### Subindo uma instância EC2 na Prática

- Primeiro procure por EC2 no AWS Console e entre no painel do EC2
- Clique em 'Instances' e depois em 'Launch Instances'
- Em seguida escolha as configurações necessárias e clique em 'Launch Instance'

### Tipos de Instâncias EC2

É possível utilizar diferentes tipos de instâncias EC2 que são otimizadas para diferentes casos de uso

Podemos acessar a descrição delas aqui: [https://aws.amazon.com/pt/ec2/instance-types/]

Os nomes seguem a seguinte estrutura:

m5.2xLarge

m: instance class

5: generation

2xLarge: tamanho da instância (quanto maior, mais cpu, ram, armazenamento etc)

Tipo para Propósitos Gerais (General Purpose):
Sáo boas para uma diversidade de casos como web servers e repositórios de código
Possuem um bom balanço entre CPU, memória e rede

Tipo para Computação Otimizada (Compute Optimized):
Sào boas para tarefas que precisam de um computação intensiva que precisa de processadores de alta performance
Processamento de grande volume de dados (Batch processing)
Web server de alta performance
High performance computing (HPC)
Machine Learning
Servidores dedicados para jogos

Tipo com Memória Otimizada (Memory Optimized)
Possuem uma grande performance para processar grandes volumes de dados na RAM
Bancos de dados de alta performance
Cache distribuído para web
Banco em memória para BI

Tipo com Armazenamento Otimizado (Storage Optimized)
Utilizado para tarefas que precisam de muitas escrita e leitura ou acessar um grande volume de dados no armazenamento

### Introdução a Grupo de Segurança/Firewall nas instâncias EC2

Grupos de Segurança são o fundamento de segurança de rede na AWS
Eles controlam como o trafego é permitido para dentro ou para fora das instâncias EC2
Grupos de segurança contém somente regras de permissão
As regras de grupos de segurança podem fazer referência por IP ou por grupo de segurança

Os grupos de segurança atuam como um 'firewall'nas instâncias EC2

Eles regulam:

- Acesso à portas
- IPs autorizados
- O tráfego de rede que chega de fora para dentro da instância
- O tráfego de rede que sai da instância

Um grupo de segurança pode ser usado por várias instâncias e uma instância pode ter vários grupos de segurança
Grupos de segurança são vinculados à região
Os grupos funcionam fora da instância, se alguma requisição for bloqueado ela nem chegará até a instância do EC2
É interessante manter um grupo de segurança separado para acesso via SSH
Se a aplicação não estiver podendo ser acessada (time out), provavelmente é um problema de grupo de segurança
Caso a aplicação retorne um erro de 'connection refused', então provavelmente é um erro na aplicação
Por padrão todo o tráfego para dentro da instância é bloqueado e todo tráfego para fora é permitido.

É possível referenciar grupos de segurança em outros grupos de segurança ou seja é possível realizar permissões de acesso baseada em outros grupos de segurança
Por exemplo:

Instância A
Grupo de segurança 1:

- Permite tráfego para dentro da instância
    Permite tráfego de instâncias que possuam o Grupo de Segurança 1
    Permite tráfego de instâncias que possuam o Grupo de Segurança 2

Então qualquer outra instância que possuir o grupo de segurança 1 ou 2 terá permissão de acessar a Instância A

Exemplo:

- Instância B
  - Possui Grupo de Segurança 2

- Instância C
  - Possui Grupo de Segurança 1

- Instância D
  - Possui Grupo de Segurança 3

B -> A = permitido (faz parte do grupo 2)

C -> A = permitido (faz parte do grupo 1)

D -> A = não permitido (não faz parte nem do grupo 1 nem do grupo 2)

Portas Padrões:

- 22 -> SSH - logar em uma instância Linux
- 21 -> FTP (File Transfer Protocol) - upload files into a file share
- 22 -> SFTP (Secure File Transfer Protocol) - upload file using SSH
- 80 -> HTTP - access unsecured websites
- 443 -> HTTPS - access secured websites
- 3389 -> RDP (Remote Desktop Protocol) - logar em uma instância Windows

### Grupos de Segurança na Prática

- No painel do EC2, vá em 'Network e Security' e clique em 'Security Group', esta tela irá mostrar os security groups existentes

Nessas configurações podemos alterar:

- Inbound Rules
  - São as regras de tráfego de fora para dentro do instância
  - Podemos alterar dentro da regra
    - O tipo
    - O protocolo
    - A porta
    - A origem (0.0.0.0/0 significa que aceita tráfego de qualquer ip de origem)
    - Descrição (opcional)

Exemplo:

Caso tenhamos configurado uma regra de inbound para o protocolo http (porta 80) permitindo qualquer origem (0.0.0.0/0), conseguimos acessar o endereço da nossa instância através do navegador (protocolo http) de qualquer computador (qualquer origem)

Lembrando que uma instância pode ter vários security groups e um security group pode estar ligado à várias instâncias
