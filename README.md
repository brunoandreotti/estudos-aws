# AWS Estudos

## Sumário

- [Introdução](#casos-de-uso-dos-serviços-da-aws)
- [Regiões](#regiões)
  - [Como escolher uma região?](#como-escolher-uma-região)
  - [Zonas de disponibilidade (Availability Zones)](#zonas-de-disponibilidade-availability-zones)
  - [Pontos de presença (Points os presence (edge locations))](#pontos-de-presença-points-os-presence-edge-locations)
- [IAM e AWS CLI](#iam-identity-and-access-management-e-aws-cli)
  - [Por que queremos ou precisamos de criar grupos?](#por-que-queremos-ou-precisamos-de-criar-grupos)
  - [Grupos e Usuários na Prática](#grupos-e-usuários-na-prática)
  - [IAM Policies](#iam-policies)
  - [IAM Policies na Prática](#iam-policies-na-prática)
  - [IAM MFA (Password Policy)](#iam-mfa-password-policy)
  - [Password Policy na Prática](#password-policy-na-prática)
  - [Maneiras de acessar a AWS](#maneiras-de-acessar-a-aws)
  - [AWS CLI](#aws-cli)
  - [AWS SDK](#aws-sdk)
  - [AWS CLI Na Prática](#aws-cli-na-prática)
  - [IAM Roles (Funções) for Services](#iam-roles-funções-for-services)
  - [IAM Roles Na Prática](#iam-roles-na-prática)
  - [IAM Security Tools](#iam-security-tools)
  - [IAM Security Tools Na Prática](#iam-security-tools-na-prática)
- [Fundamentos EC2](#fundamentos-ec2)
  - [EC2 Básico](#ec2-básico)
  - [Subindo uma instância EC2 na Prática](#subindo-uma-instância-ec2-na-prática)
  - [Tipos de Instâncias EC2](#tipos-de-instâncias-ec2)
  - [Introdução a Grupo de Segurança/Firewall nas instâncias EC2](#introdução-a-grupo-de-segurançafirewall-nas-instâncias-ec2)
  - [Grupos de Segurança na Prática](#grupos-de-segurança-na-prática)
  - [Vinculando Roles em uma Instância EC2](#vinculando-roles-em-uma-instância-ec2)
  - [Tipos de Pacotes EC2](#tipos-de-pacotes-ec2)
- [EC2 - Solution Architect](#ec2---solution-architect)
  - [Privado x Público x Elastic IP](#privado-x-público-x-elastic-ip)
  - [IP Privado x IP Público x Elastic IP na Prática](#ip-privado-x-ip-público-x-elastic-ip-na-prática)
  - [Placement Groups](#placement-groups)
  - [Placement Groups na Prática](#placement-groups-na-prática)
  - [Elastic Network Interfaces (ENI)](#elastic-network-interfaces-eni)
  - [Elastic Network Interfaces (ENI) na Prática](#elastic-network-interfaces-eni-na-prática)
  - [EC2 Hibernate](#ec2-hibernate)
- [EC2 Instance Storage](#ec2-instance-storage)
  - [Visão geral EBS (Elastic Block Storage)](#visão-geral-ebs-elastic-block-storage)
  - [Volumes EBS na Prática](#volumes-ebs-na-prática)
  - [EBS Snapshots](#ebs-snapshots)
  - [EBS Snapshots na Prática](#ebs-snapshot-na-prática)
  - [AMI (Amazon Machine Image)](#ami-amazon-machine-image)
  - [AMI (Amazon Machine Image) na Prática](#ami-amazon-machine-image-na-prática)
  - [EC2 Instance Store](#ec2-instance-store)
  - [Tipos de Volumes EBS](#tipos-de-volumes-ebs)
  - [EBS Multi-Attach](#ebs-multi-attach)
  - [EBS Encryption](#ebs-encryption)
  - [Amazon Elastic File System (EFS)](#amazon-elastic-file-system-efs)
  - [EBS x EFS](#ebs-x-efs)
- [Escalabilidade e Alta Disponibilidade - ELB e ASG](#escalabilidade-e-alta-disponibilidade---elb-e-asg)
  - [Elastic Load Balancer (ELB)](#elastic-load-balancer-elb)
  - [Application Load Balancer - ALB](#application-load-balancer---alb)
  - [Application Load Balancer na Prática](#application-load-balancer-na-prática)
  - [Network Load Balancer (NLB)](#network-load-balancer-nlb)
  - [Gateway Load Balancer (GWLB)](#gateway-load-balancer-gwlb)
  - [Elastic Load Balancer - ELB - Sticky Sessions (Session Affinity)](#elastic-load-balancer---elb---sticky-sessions-session-affinity)
  - [Balanceamento entre Zonas](#balanceamento-entre-zonas)
  - [Certificados SSL](#certificados-ssl)
  - [Connection Draining/Deregistration Delay](#connection-drainingderegistration-delay)
  - [Auto Scaling Group (ASG)](#auto-scaling-group-asg)
  - [Scaling Policies](#scaling-policies)
- [Amazon RDS (Relational Database Service)](#amazon-rds-relational-database-service)
  - [RDS Replicas](#rds-replicas)
  - [RDS Multi AZ](#rds-multi-az)
  - [Amazon RDS na Prática](#amazon-rds-na-prática)
  - [RDS Custom](#rds-custom)

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

Primeiramente instale a AWS CLI de acordo com seu sistema operacional

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

No AWS Console no IAM Dashboard, clique em Roles:

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

- Tipo para Propósitos Gerais (General Purpose):
  - São boas para uma diversidade de casos como web servers e repositórios de código
  - Possuem um bom balanço entre CPU, memória e rede

- Tipo para Computação Otimizada (Compute Optimized):
  - São boas para tarefas que precisam de um computação intensiva que precisa de processadores de alta performance
  - Processamento de grande volume de dados (Batch processing)
  - Web server de alta performance
  - High performance computing (HPC)
  - Machine Learning
  - Servidores dedicados para jogos

- Tipo com Memória Otimizada (Memory Optimized)
  - Possuem uma grande performance para processar grandes volumes de dados na RAM
  - Bancos de dados de alta performance
  - Cache distribuído para web
  - Banco em memória para BI

- Tipo com Armazenamento Otimizado (Storage Optimized)
  - Utilizado para tarefas que precisam de muitas escrita e leitura ou acessar um grande volume de dados no armazenamento

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

### Vinculando Roles em uma Instância EC2

No painel do EC2, clique na instância desejada, vá em 'Actions', depois em 'Security' e então em 'Modify IAM Role', escolha a Role desejada e depois em 'Update IAM Role'.

Quando conectado na instância do EC2, via ssh ou EC2 connect, não é interessante utilizar o comando 'aws configure'e colocar nossas credenciais para acessar comandos da AWS CLI, pois qualquer pessoa da instituição pode conectar na instância e pegar essas informações que sào confidenciais.

Para permitir o uso do AWS CLI na instância é precisa adicionar as permissões via Roles.

Por exemplo, para executar o comando 'aws iam list-users' no instância é necessário criar uma role com a permissão 'IAMReadOnlyAccess' dessa maneira nossa instância terá a permissão para executar os comandos e envolvam leitura de dados do IAM.

### Tipos de Pacotes EC2

- On-Demand Instances
  - Pague pelo tanto que está sendo usado
  - Recommended for short-term and un-interrupted workloads, where you can't predict how the application will behave

- EC2 Reserved Instances
  - É possível reservar atributos específicos da instância (tipo, região, OS, por exemplo)
  - Recommended for steady-state usage applications (databases por exemplo)
  - É possível comprar e vender as instâncias no marketplace

- Convertible Reserved Instance
  - É uma opção de Reserved Instance onde é possível alterar o tipo da instância, família, OS e escopo

- EC2 Spot Instance
  - São instâncias mais baratas onde você define qual o preço máximo que quer ser pago por pela instância, porém se os preços subirem e ficarem maior do configurado, você perde acesso aquela instância.
  - Por isso é recomendado para aplicações que são resilientes à falhas

- EC2 Dedicated Hosts
  - Um servidor físico dedicado com instância EC2 totalmente dedicado para o seu uso.
  - Útil para casos de uso que envolvam requisitos de compliance

- EC2 Dedicated Instance
  - Possui uma instância dedicada em um hardware dedicado, mas pode ser que compartilhe o hardware com outros usuários
  - A diferente comparando com o Dedicated Hosts, é que no Dedicated Hosts você tem acesso total ao servidor físico em sí.

## EC2 - Solution Architect

### Privado x Público x Elastic IP

- IP Público
  - Significa que a máquina pode ser identificada na internet
  - Precisa ser única dentro de toda a internet (dois máquinas não podem ter o mesmo ip público)

- IP Privada
  - Significa que a máquina pode ser identificada somente dentro da rede privada
  - Os IP precisam ser únicos somente dentro da rede privada
  - Mas duas redes privadas diferentes podem conter os mesmo IPs dentro delas pois elas estão separadas
  - As máquinas dentro dessa rede privada se conectarão na internet através de um NAT + internet gateway (proxy)
  - Somente um range específico de IPs podem ser utilizados como IPs privados

- IP Elastic
  - Quando encerra e inicia uma instância do EC2, o IP público da instância é alterado
  - Caso necessite de um IP público fixo, será necessário um Elastic IP
  - Elastic IP é um IP IPv4 que você possui desde que você não delete ele
  - Você pode vincular a uma instância de cada vez
  - Com isso é possível mascarar uma falha em uma instância redirecionando esse IP fixo para outra instância
  - Porém não é uma alternativa muito boa, o melhor é utilizar um Load Balancer.

### IP Privado x IP Público x Elastic IP na Prática

Por padrão uma instância do EC2 vem com uma IP privado para o rede interna AWS e uma IP público para acessar a web

Ao entrar nos detalhes da instância no EC2 é possível visualizar o IP público no campo 'Public IPv4 address'na aba 'Details'

Somente é possível conectar na instância EC2 através do ssh utilizando o IP público da instância, uma vez que o IP privado somente é reconhecido na rede interna da AWS e nós não estamos conectados a essa rede interna.

Toda a vez que a instância do EC2 é parada e depois inciada novamente, o IP público é alterado.

Para ter um IP público fixo precisamos de um Elastic IP

Para isso, na dashboard do EC2 vá em 'Network e Security' e depois em 'Elastic IP' e clique em 'Allocate Elastic Ip Address'

Para associar o Elastic IP a uma instância, selecione o IP desejado, clique em 'Actions' e depois em 'Associate Elastic IP Address', após isso selecione a instância desejada e salve.

### Placement Groups

Ao criar as instâncias da AWS, é possível informar o lugar que você quer que ela fique podendo ser dentro da mesma Availability Zone (AZ), ou até mesmo no mesmo Data Center dentro da AZ.

Quando um Placement Group é criado, é possível informar as seguintes estratégias para o grupo:

- Clusters: Junte as instâncias em um grupo de baixa latência em uma única AZ
- Spread: Espalhe instâncias entre hardware subjacentes (máximo de 7 instâncias por grupo por AZ) - utilizado para aplicações críticas
- Partition: Espalhe instâncias entre várias partições diferentes (em diferentes sets de racks de hardware) em um AZ. Podendo escalar em centenas de instâncias por grupo

Clusters:

Menor latência, maior largura de banda, porém mais suscetível a falhas.

- As várias instâncias do EC2 ficarão na mesma AZ, dessa forma elas terão a menor latência de comunicação entre elas e uma maior banda.
- Porém, por estarem todas as instâncias na mesma AZ, caso ela falhe, todas as instâncias falharão ao mesmo tempo
- Utilizada para aplicações que precisam de uma latência extremamente baixa e uma grande largura de banda

Spread:

Maior latência, menor largura de banda, porém menos suscetível a falhas.

- Ao contrário do Cluster, no Spread queremos minimizar ao máximo a chance de falhas.
- Nesse caso todas as instâncias estarão em diferentes hardwares.
- As instâncias podem estar na mesma AZ ou em AZ diferentes mas sempre estarão em hardwares diferentes mesmo que estejam na mesma AZ.
- Isso diminui o risco de falha pois, caso um hardware falhe dentro de um AZ, terá outro hardware dentro da AZ com outra instância. E caso uma AZ inteira falhe, terá outra AZ com outros hardwares com instâncias da aplicação.
- É limitado a 7 instâncias por AZ por placement group
- Utilizada em aplicações de precisam de maxima disponibilidade e aplicações críticas onde cada instância precisa estar isolada da falha de outra instância.

Partition

- São divididos em partições dentro de uma AZ e em cada partição pode haver várias instâncias
- Cada partição é um rack de hardware dentro da AZ, dessa forma caso um rack falhe dentro da AZ, outro com outras instâncias ainda estará funcionando.
- Limitado a 7 partições por AZ porém cada partição pode conter centenas de instâncias, diferentemente do Spread que é limitado a 7 instâncias por placement group.

### Placement Groups na Prática

Dentro do dashboard do EC, vá em 'Network and Security' e clique em 'Placement Group' e depois em 'Create placement group', depois escolha as opções do placement group.

Para criar uma instância em um grupo, quando estiver criando a instância, em 'Advanced Details' terá uma opção para escolher o Placement Group daquela instância.

### Elastic Network Interfaces (ENI)

É um componente na VPC (Virtual Private Cloud) que representa um placa de rede virtual (virtual network card).

São esses componentes que dão acesso a rede para as nossas instâncias, porém são utilizadas fora da instância em si. É possível adicionar mais do que 1 ENI em uma instância

Cada ENI pode ter os seguintes atributos:

- IPv4 privado primário e um ou mais IPv4 secundários
- Um Elastic IP (IPv4) por cada IPv4 privado (caso uma instância tenha 2 ENI então ela poderá ter 2 IP privado, podendo assim ter 2 Elastic IP)
- Um IPv4 público
- Um ou mais security group
- Um MAC address

Os ENI podem ser criado independentes de instâncias e serem adicionados ou movidos a qualquer hora de uma instância para outra, dessa maneira movendo um IP privado de uma instância para outra.

### Elastic Network Interfaces (ENI) na Prática

Nos detalhes da instância, na aba 'Networking' temos a opção 'Network Interfaces'.

Para configurar uma ENI, na dashboard do EC2, vá em 'Network and Security' e clique em 'Network Interfaces'.

Depois de configurar uma ENI é possível vincular ela a uma instância e caso necessário, desvincular e vincular em outra instância.

### EC2 Hibernate

Sabemos que podemos parar e terminar instâncias do EC2

- Stop: Os dados no disco (EBS) fica intacto no próximo start da instância
- Terminate: Qualquer volume EBS configurado para ser destruído é perdido, porém volumes configurados para não serem destruídos no Terminate são mantidos.

No start da instância:

- Na primeira vez é feito o boot do OS e é executado o EC2 User Data Script
- Nas outras vezes é somente feito o boot do OS (desde que a instância não seja terminada)

No Hibernate:

- O estado da RAM é preservado
- Significa que o boot do OS será muito mais rápido pois o OS não é parado e nem reiniciado
- O estado da ram é escrito em um arquivo no volume EBS.
- É importante que o volume de armazenamento tenha espaço o suficiente para armazenar todo o tamanho da ram
- Para permitir ECS Hibernate o EC2 Instance Root Volume deve ser do tipo EBS e deve ser encriptado.

## EC2 Instance Storage

### Visão geral EBS (Elastic Block Storage)

EBS é um serviço de armazenamento em blocos na nuvem, escalável e de alta performance projetado para o Elastic Compute Cloud (EC2).

EBS (Elastic Block Storage) Volume é um 'dispositivo' de armazenamento que é possível de vincular a alguma instância enquanto elas rodam.

Permite as instâncias persistirem dados mesmo depois que elas forem terminadas, desse forma nos podemos destruir/terminar instâncias e criar outras novas utilizando o mesmo volume EBS das anteriores.

Alguns EBS podem ser vinculados a varias instâncias, mas normalmente são vinculadas apenas a uma. E uma instância pode ter mais de um EBS.

É vinculado ao AZ.

São dispositivos de rede (não são dispositivos físicos), eles usam a rede para comunicar se comunicar com a instância, podem assim ter alguma latência.

Por serem um dispositivo de rede, podem ser desvinculados de uma instância e vinculados em outra de forma rápida.

Por se tratar de um volume é necessário provisionar a capacidade dele.

Ao criar um volume EBS é possível configurar para que o EBS seja deletado quando a instância é terminada. O volume padrão (root) da instância é configurado para ser deletado ao terminar a instância por padrão, porém é possível configurar para ele não ser deletado. Os outros volumes EBS adicionado não são deletados por padrão ao terminar a instância.

### Volumes EBS na Prática

Ao entrar nos detalhes do sua instância EC2, na aba 'Storage'é possível visualizar os detalhes do armazenamento de sua instância.

Para acessar os detalhes do EBS, no dashboard do EC2, vá em 'Elastic Block Store'.

Para criar um novo volume, na aba 'Elastic Block Store', clique em 'Volumes' e depois em 'Create Volume'. É importante se atentar em criar o volume na mesma AZ da instância que você deseja vincular esse volume.

Para vincular a uma instância, clique no volume, depois em 'Actions' e depois em 'Attach Volume'.

Um volume EBS só estará disponível para vincular a uma instância se a instância e o volume estiverem na mesma AZ.

### EBS Snapshots

Com o EBS Snapshot é possível fazer um backup de um volume EBS a qualquer momento.

Não é necessário desvincular o volume da instância para fazer o snapshot mas é recomendado.

É possível copiar os snapshots entre AZs ou regiões.

Características do EBS Snapshot

- EBS Snapshot Archive
  - É possível mover o snapshot para um 'nível' diferente do 'nível' padrão que ele é criado, esse nível é chamado de 'archive tier'
  - Ele é mais barato que o nível padrão.
  - É como se fosse uma alteração de formato do snapshot para que seja mais barato de armazenar ele, porém ele demora de 24 a 72 horas para ser restaurado, diferente no nível padrão que é instantâneo.

- EBS Recycle Bin
  - É possível configurar regras para que ao deletar um EBS Snapshot ele vá para uma lixeira ao invés de ser totalmente deletado, sendo possível ser recuperado, o tempo de retenção da lixeira pode ser definido em 1 dia e 1 ano.

- Fast Snapshot Restore (FSR)
  - Uma funcionalidade de forçar a inicialização total de um volume para que não haja latência para seu uso já de imediado, porém é um função que é bem cara.

### EBS Snapshot na Prática

Para criar um  snapshot, vá nos detalhes do EBS Volume, selecione o volume desejado, clique em 'Actions' e depois em 'Create Snapshot'

Para visualizar os detalhes do snapshot clique em 'Snapshots' na aba 'Elastic Block Storage'.

É possível copiar o snapshot para outra região selecionando a snapshot, clicando em 'Actions' e depois em 'Copy Snapshot'.

Para criar um novo volume a partir do snapshot, clique em 'Actions' e depois em 'Create volume from snapshot'.

### AMI (Amazon Machine Image)

- AMI são imagens para a customização da um instância EC2.
  - É possível adicionar os próprios softwares, configurações, OS, monitoramento e etc.
  - Com isso é possível ter um tempo de boot e configuração mais rápido pois todos os softwares que iríamos instalar na instância já estão pré-empacotados na imagem.

As imagens são criadas para uma região específica e podem ser copiadas para outras regiões.

Então para iniciar uma instância ECS temos 3 opções, utilizar uma imagem publica disponibilizada pela Amazon ou utilizar uma imagem montada, mas dessa forma nós mesmos somos responsáveis por manter a imagem segura e atualizada ou ainda uma imagem montada por outra pessoa que está disponível no marketplace.

### AMI (Amazon Machine Image) na Prática

Para criar uma imagem, selecione a instância desejada, clique em 'actions' e depois em 'image and templates' e em seguida em 'create image'.

Para ver a lista de imagens disponíveis, no dashboard de EC2, clique em 'images' e depois em 'IMAs'

Após a imagem ser gerada será possível criar instâncias utilizando a imagem.

### EC2 Instance Store

Volumes EBS são drivers de rede com boa performance porém limitada. Por isso caso necessite de uma disco de armazenamento de alta performance, é recomendado utilizar EC2 Instance Store.

Em alguns tipos de instâncias é possível utilizar um armazenamento por hardware, utilizando o hd/ssd que está fisicamente no servidor.

Casos de uso:

- Melhor performance de leitura e escrita (I/O)
- EC2 Instance Store perde os dados armazenados se ele é parado/terminado, então é recomendado para uso de dados que não serão necessários a longo prazo (buffer, cache, conteúdo temporário)
- Risco de perder dados caso a hardware falhe
- Replicações e backups ficam na sua responsabilidade.

### Tipos de Volumes EBS

Volumes EBS podem ser de 6 tipos:

- gp2/gp3 (SSD): SSD com propósitos gerais, bom balanço entre preço e performance para uma grande variedade de aplicações.
- io1/io2 Block Express (SSD): SSD é altíssima performance para aplicações criticas e que necessitam de baixa latência ou alta taxas de transferência.
- st 1 (HDD): HD de baixo custo para aplicações frequentemente acessadas e com taxa de transferência intensas.
- sc 1 (HDD): HD com o custo mais baixo possível para aplicações pouco acessadas.

Como definir qual EBS utilizar:

- Vai depender do tipo de aplicação que será utilizada, os volumes EBS podem ser definidos por tamanho, taxa de transferência, escrita/leitura por segundo (IOPS I/O Ops Per Sec).

Somente gp2/gp3 e io1/io2 Block Express podem ser utilizadas como volume de boot.

Casos de uso para os tipos de volume

SSD de uso geral (gp2/gp3)

- Armazenamento custo-beneficio, baixa latência.
- Pode ser utilizado para volume de boot, virtual desktops, ambiente de desenvolvimento e teste.
- Tamanho pode variar de 1 gigabyte até 16 terabyte
- Características gp3.
  - Base de 3000 IOPS e taxa de transferência de 100 megabytes/seg
  - Pode aumentar o IOPS até 16000 e a taxa de transferência até 1000 megabytes/seg independentemente.
- Características gp2
  - Volumes pequenos gp2 podem chegar até 3000 IOPS.
  - Tamanho do volume e IOPS são conectados, podendo chegar até 16000 IOPS, ou seja, quanto maior o volume, maior o IOPS, sendo a taxa de 3 IOPS por giga de tamanho.
  
SSD Provisionado para IOPS

- Utilizado para aplicações criticas que necessitam de ótima performance de IOPS ou aplicações que necessitam de mais do que 16000 IOPS.
- Bom para aplicações de banco de dados
- io 1 (4gb - 16tb)
  - Máximo de 64000 IOPS para Nitro EC2 Instances e 32000 para outras.
  - Pode aumentar o IOPS independentemente do tamanho do armazenamento.
- io2 (4gb - 64tb)
  - Latência extremamente rápida.
  - IOPS máximo de 256000.
  - Suporta EBS Multi-attach.

HDD

- Não podem ser utilizados como volume de boot.
- 125gb até 16tb.
- st1 (HD otimizado para taxa de transferência)
  - Bom para big data, armazém de dados, processamento de logs.
  - Taxa de transferência máxima de 500 mb/s e máximo de 500 IOPS
- st2.
  - Utilizado para dados que não são frequentemente acessados.
  - Bom para quando o menor custo é importante.
  - Taxa de transferência máxima de 250 mb/s e máximo de 250 IOPS.

### EBS Multi-Attach

Somente está disponível nas famílias io1 e io2 de volume EBS.

Permite vincular o mesmo volume EBS em várias instâncias EC2 na mesma AZ.

Cada instancia terá permissão completa para leitura e escrita.

Casos de uso

- Alcançar uma disponibilidade muito alta em aplicações com cluster em Linux.

Máximo de 16 instâncias EC2 de uma vez.

### EBS Encryption

Ao criar um volume EBS encriptado temos o seguinte:

- Dados são encriptados dentro do volume
- Todos os dados transitados entre a instância e o volume são encriptados
- Todos os snapshots são encriptados
- Todos os volumes criados a partir do snapshot também são encriptados

Todo o processo de encriptar e tirar a criptografia é feito de baixo doa panos pelo EBS

Tem um impacto mínimo na latência

Utiliza chaves do KSM (AES-256)

Copiar um snapshot não encriptado permite poder encriptar ele

Para encriptar um EBS não encriptado que já esta vinculado a uma instância EC2:

- Crie um snapshot do volume EBS
- Copie o snapshot e marque a opção de encriptar o snapshot copiado
- Use o snapshot encriptado para criar um novo volume EBS encriptado
- Vincule o novo volume EBS encriptado na instância desejada

### Amazon Elastic File System (EFS)

É um NFS (network file system) gerenciado que pode ser colocado em vários tipos de EC2

EFS trabalha com instâncias EC2 em múltiplas AZs.

Altamente disponível e escalável, muito caro (3x o custo do gp2), pago por uso.

Com ele é possível conectar instâncias em diferentes AZ pelo mesmo NFS utilizando EFS.

Casos de uso: gerenciamento de conteúdo, web serving, compartilhamento de dados.
Usa o protocolo NFSv4.1.

Utiliza grupos de segurança para controlar acesso ao EFS.

Somente compatível com AMI baseada em Linux.

Possível encriptar utilizando KMS.

A file system escala automaticamente e é pago por uso.

Classes de EFS:

- EFS Scale
  - Aceita milhares de acessos ao mesmo tempo, mais de 10gb/s de taxa de transferência.
  - Cresça para um NFS de escalas de petabytes  automaticamente
- Performance Mode (configurado no momento de criação do EFS)
  - Propósitos gerais (padrão): Para casos de uso que a latência importa
  - Máximo I/O: maior latência porém maior taxa de transferência
- Throughput Mode
  - Bursting
  - Provisionado: Configura a taxa de transferência sem se importar com o tamanho do armazenamento.
  - Elastic: Escala a taxa de transferência baseado no volume de dados automaticamente.

Classes de armazenamento EFS

Storage Tiers (Gerenciamento de ciclo de vida - mova arquivos depois de X dias)

- Standard: para arquivos frequentemente acessados
- Infrequent access: Custa para recuperar arquivos, porém menor preço para armazenar
- Archive: dados raramente acessados, custa 50% menos.

Para configurar o tempo que os arquivos demorar para ir de uma estado de armazenamento para o outro é possível utilizar políticas de ciclo de vida.

Disponibilidade e durabilidade:

- Standard: Multi-AZ, bom para prod
- One Zone: Uma AZ, bom para desenvolvimento, backup e compatível com IA (EFS One Zone IA).

### EBS x EFS

EBS volumes:

- Uma instância (exceto multi-attach io1/io2)
- São específicos de AZ
- gp2: IO aumenta de acordo com o tamanho do disco
- gp3 e io1: IO pode aumentar independentemente do tamanho do disco
- Para migrar um EBS entre AZ
  - Faz um Snapshot do volume EBS
  - Copia para a outra AZ
  - Cria um volume a partir do Snapshot
  - Vincula o volume a uma instância
- O volume root do EBS de uma instância é deletado por padrão caso a instância EC2 seja terminada (é possível alterar esse comportamento)

EFS:

- É um network file system
- Pode ser vinculado a centenas de instâncias em diferentes AZs
- Somente para instâncias com Linux pois utiliza o sistema POSIX
- Possui um preço mais elevado do que o EBS
- Possui Storage Tier para os arquivos, dependendo da frequência que são acessados para diminuir custos

EFS: File System

EBS: Dispositivo de armazenamento na rede

Instance Store: Dispositivo físico de armazenamento no servidor

## Escalabilidade e Alta Disponibilidade - ELB e ASG

Escalabilidade significa que a sua aplicação consegue lidar com alto volume de carga se adaptando dependendo do cenário

Existem dois tipos de escalabilidade:

Vertical

- Significa a necessidade de aumentar o tamanho da instância
- Por exemplo uma aplicação que roda em um t2.micro e vemos a necessidade de escalar ela para rodar em um t2.large
- Comum ser usado em sistemas não distribuídos, por exemplo banco de dados
- RDS e ElastiCache são serviços que podem ser escalados verticalmente
- Existe uma limitação do quanto é possível escalar verticalmente e está ligado a uma limitação de hardware

Horizontal (elasticidade)

- Significa aumentar o número de instâncias da sua aplicação
- Escalabilidade horizontal normalmente significa o uso de sistemas distribuídos
- É fácil de ser realizado com instâncias EC2

Alta disponibilidade normalmente anda junto com escalabilidade horizontal, significa rodar sua aplicação em pelo menos 2 data centers (AZs) diferentes

O principal motivo é para sobreviver a uma perda de data center

Pode ser alta disponibilidade passiva (RDS Multi AZ, por exemplo)

Pode ser alta disponibilidade ativa (escalabilidade horizontal)

### Elastic Load Balancer (ELB)

Load balancers são servidores que vão encaminhar o tráfego para vários servidores (EC2, por exemplo).

Vantagens:

- Distribuir a carga entre várias instâncias
- Expor apenas um ponto de acesso (DNS) na sua aplicação
- Lidar com falhas nas instâncias
- Disponibilizar HTTPS
- Separar trafego público do privado

Elastic Load balance é um load balancer gerenciado pela AWS

- Garantia de funcionamento
- AWS cuida de upgrades, atualizações, manutenção e etc
- AWS disponibilizado apenas algumas opções de alteração

Irá custar menos que configurar o próprio load balancer se será menos trabalhoso de manter e escalar

É integrado com vários sistemas e serviços da AWS (EC2, ECS, CloudWatch e etc)

Health Checks é uma maneira do load balancer verificar a saúde de uma instância, caso aquela instância esteja com algum problema o load ba1lancer não irá jogar requisições para aquela instâncias em específico.

Normalmente é feito utilizando uma rota e uma porta específicas

Tipos de Load Balancer na AWS

- Classic Load Balancer (v1 - geração velha) - 2009 - CLB
  - suporta HTTP, HTTPS, TCP, SSL
  - Por ser antigo, a AWS não recomenda utiliza-lo, será mostrado como depreciado.

- Application Load Balancer (v2 - geração nova) - 2016 - ALB
  - suporta HTTP, HTTPS, WebSocket

- Network Load Balancer (v2 - geração nova) - 2017 - NLB
  - suporta T P, TLS (secure TCP), UDC

- Gateway Load Balancer - 2020 - GWLB
  - Opera na camada 3 (camada de rede) - IP Protocolo

É recomendado o uso das versões mais novas do LB pois ele possui mais funcionalidades

Alguns LBs podem ser configurados como internos ou externos

Ao usar um LB, vamos permitir que todas as requisições http e https sejam feitas para o load balancer, porém para acessar nossa aplicação/instância iremos permitir apenas as requisições do LB, basicamente vamos fazer o link do security group da instância com o security group do LB, dessa forma informando que a instância só aceitara tráfegos vindos do LB

### Application Load Balancer - ALB

Application load balancer faz o load balancer de aplicações http entra máquinas que são agrupadas em target groups

Permite fazer o LB de várias aplicações na mesma máquina (mesma instância EC2)

Suporte para HTTP/2 e WebSockets

Suporta redirecionamento (de HTTP para HTTPS por exemplo)

É possível redirecionar diferentes URLS para diferentes target groups (exemplo.com/user e exemplo.com/posts sendo redirecionados para target groups diferentes)

É possível redirecionar diferentes host names para diferentes target groups (um.exemplo.com e dois.exemplo.com sendo redirecionados para target groups diferentes)

Também é possível fazer redirecionamento para diferentes target groups baseado na Query String e nos Headers da requisição

ALB é útil quando usamos microsserviços e aplicações container-based (Docker e Amazon ECS, por exemplo) pois possui uma funcionalidade de mapeamento de portas para redirecionar para uma porta dinâmica no ECS

É útil pois consegue fazer o load balance para diferentes aplicações, por exemplo:

- Rota /users
  - É redirecionada para o target group que roda o microsserviço de usuários

- Rota /search
  - É redirecionada para o target group que roda o microsserviço de busca

Target groups podem ser:

- Instâncias EC2 (que podem ser gerenciadas por um Auto Scaling Group) - HTTP
- ECS tasks (gerenciados pelo próprio ECS) - HTTP
- Lambda Functions - A request HTTP é traduzida em um evento JSON
- Endereços de IP - precisam ser IPs privados (por exemplo de servidores on-premises)

ALB pode fazer o roteamento para vários target groups e o Health check é feito a nível de target group

Outro exemplo:

- Request com query string ?Platform=Mobile
  - É redirecionado para um target group baseado em instâncias EC2

- Request com query string ?Platform=Desktop
  - É redirecionado para um target group baseado em servidores on-premises utilizando IP privado

Dessa maneira podemos fazer o redirecionamento de diferentes requisições para diferentes target groups baseados em diferentes tecnologias

Bom saber:

- Possui host name fixo (XXX.region.elb.amazonaws.com)
- Os servidores da aplicação não olham o IP doi cliente diretamente, o IP verdadeiro do cliente é inserido no header X-Forwarded-For
- Dessa maneira também podemos acessar a port (X-Forwarded-Port) e o protocolo (X-Forwarded-Proto)

Client IP (12.34.56.78) -> LB <-> LB IP (Private IP) <-> Instância EC2

Dessa maneira a aplicação não recebe a requisição direto do IP do cliente mas sim do IP privado do LB, quem recebe a requisição com o IP do cliente é o LB

### Application Load Balancer na Prática

- Crie 2 instâncias EC2
- No dashboard do EC2, vá até a aba 'Load Balancing' e entre em 'Load Balancers' e depois em 'Create Load Balancer'
- Faça as configurações do load balancer (Security Group, AZs, Target Groups)
- Após isso o LB será criado e será disponibilizado um endereço DNS para acessar o load balancer
- Com isso o LB já estará fazendo o balanceamento de acordo com o target group

Por questões de segurança é interessante termos acesso às nossas instâncias somente através no LB e não através do próprio endereço da instância

Para configurar isso podemos:

- Ir em Security Groups
- Selecionar o Security Groups da instância desejada
- Configurar as Inbound Rules para que acessos HTTP/HTTPS sejam feito somente pelo IP do LB e para isso basta adicionar que aceitará requisições somente do Security Group vinculado ao LB

Dessa maneira não conseguiremos acessar mais as instâncias diretamente pelo endereço delas e sim somente pelo endereço do LB

Também é possível configurar Listener Rules para o LB, para definir para onde o LB irá redirecionar as requisições.

Para isso:

- Nos detalhes do LB desejado, vá na aba Listeners and rules e clique na regra desejada
- Aqui podemos criar novas regras para o LB por exemplo redirecionar baseado no host, path, query strings, headers, método http e etc

### Network Load Balancer (NLB)

- Permite o load balance de requisições TCP e UDP para as instâncias
- Consegue lidar com milhões de requisições por segundo
- Menor latência (100 ms vs 400 ms do ALB)

NLB possui um IP estático por AZ e suporta Elastic IP

Ao falar que a aplicação só pode ser acessada por um, dois ou três IPs específicos, podemos já pensar em NLB

Utilizado para performance extrema

O funcionamento é parecido com ALB, nos criamos Target groups que receberão o redirecionamento do LB e configuramos TCP + Regras para que o LB possa ser acessado

No caso de NLB os Target Groups serão instâncias EC2 e IPs privados (para servidores on-premises por exemplo)

Os target groups podem ser formados pelos 2 tipos de tecnologias ao mesmo tempo

Também é possível colocar para o NLB redirecionar a requisição para um NLB

Os Heath Checks suportam os protocolos TCP, HTTP e HTTPS

### Gateway Load Balancer (GWLB)

É utilizado para deploy, escalar e gerenciar processos de aplicações de rede virtual na AWS de terceiros (3rd party), por exemplo, Firewalls, sistemas de prevenção e detecção de intrusos e etc.

É feito em nível de tráfego de rede.

Users -> Traffic -> GWLB -> Target Group (aplicação/servidores de terceiros) -> GWLB -> Application.

Dessa maneira podemos ver que o tráfego de rede do usuário é enviado primeiro para o GWLB depois o GWLB envia o tráfego para o Targe Group de terceiros para ser feita as validações/processos necessários, depois o resposta é retornada para o GWLB que é encarregado de enviar a resposta para a aplicação final.

Utiliza o protocolo GENEVE na porta 6081

Target Groups:

- Instâncias EC2
- Endereços de IP (precisam ser IPs privados)

### Elastic Load Balancer - ELB - Sticky Sessions (Session Affinity)

É possível implementar uma afinidade para que o mesmo cliente sempre seja redirecionado para a mesma instância por trás do LB

É possível implementar isso no Classic Load Balancer, Application Load Balancer e Network Load Balancer

Funciona utilizando um 'cookie'para informar para qual instância a requisição será redirecionada e possui uma data de expiração que pode ser configurada. Quando expirada as requisições voltam a ser enviadas para as diversas instâncias atrás do LB.

Caso de uso:

- Ter certeza que o usuário não irá perder os dados da sessão, uma vez que estará sempre conectado ao mesmo servidor/instância.

Porém ao ativar essa funcionalidade é possível que ocorra um desbalanceamento de carga.

Cookies:

- Cookie baseado na aplicação
  - Cookie Customizado
    - Gerado pelo target
    - Pode conter qualquer atributo customizado requerido pela aplicação
    - O nome do cookie precisa ser especificado individualmente para cada target group.
    - Não são permitidos os seguintes nomes : AWSALB, AWSALBAPP ou AWSALBTG pois são nomes reservados para o ELB.
  - Cookie de aplicação
    - Gerado pelo LB.
    - Possui o nome AWSALBAPP.
- Cookie baseado na duração
  - Gerado pelo LB.
  - Nome do cookie é AWSALB para o ALB e AWSELB para o CLB.

Para configurar o Sticky Session, vá na aba Target Groups e selecione o Target Group desejado, logo em seguida clique em 'Actions' e depois em 'Edit attributes'.

No final dos atributos marque a opção 'Stickiness' e escolha o tipo do cookie.

### Balanceamento entre Zonas

Com o Balanceamento entre Zonas, o balanceamento entre diferentes AZs com diferentes instâncias da mesma aplicação é feito para que cada instância fique com a mesma quantidade de carga de trabalhado, independentemente da quantidade instância que a AZ possui.

Exemplo:

- AZ1 -> Possui 2 instâncias

- AZ2 -> Possui 8 instâncias

O LB irá dividir de maneira que cada instância fique com 10% de carga por instância (100% de carga / 10 instâncias).

O mesmo exemplo mas sem utilizar o Balanceamento entre Zonas, o load balancer divide igualmente a carga de trabalhando entre as AZ a depender do quantidade de Azs.

Nesse caso por termos 2 AZs cada AZ ficaria com 50% da carga de trabalho, dessa maneira:

- AZ1 -> 50% da carga -> Possui 2 instâncias -> 25% pra cada instância

- AZ2 -> 50% da carga ->  Possui 8 instâncias -> 6.25% pra cada instância

ALB:

- Ativado o balanceamento por zonas por padrão sendo possível desativar a nível de Target Group.
- Por ser ativado por padrão não há cobrança a mais se os dados forem transitados de umas AZ para outra para fazer o balanceamento entre zonas.

NLB/GWLB:

- Desativado por padrão.
- Caso ativado será cobrado a mais caso os dados transitem de uma AZ para outra para fazer o balanceamento entre zonas.

CLB:

- Desativado por padrão.
- Sem custos a mais caso seja ativado.

Para ativar vá até os detalhes do LB desejado, depois em editar atributos e ative a opção 'Cross-zone load balancing'.

Para desativar no caso do ALB é necessário ir até o Target Group e desativar.

### Certificados SSL

Certificado SSL permite que os dados enviados entre o cliente e o LB sejam encriptados.

SSL significa Secure Sockets Layer e é utilizado para encriptar conexões.

TLS é a versão nova do SSL que significa Transport Layer Security.

Atualmente o certificado mais utilizado é o TLS mas é comumente referido ainda como SSL.

Os certificados SSL públicos são expedidos por Certificate Authorities (CA).

Utilizando esse SSL público no nosso LB será possível encriptar a conexão entre o cliente e o LB.

Certificados SSL possuem datas de expiração e precisam ser renovados para ter certeza que o certificado é autêntico.

Como funciona o certificado SSL no LB:

Users -> HTTPS -> LB -> HTTP -> EC2

A requisição entre o cliente e o LB é encriptada utilizando o protocolo HTTPS, porém na hora que chega no LB é desfeita a encriptação para que o LB se comunique com a instância através do protocolo HTTP, uma vez que essa comunicação é feita em uma rede privada.

- o LB usa um certificado X.509 (SSL/TLS server certificate)
- É possível gerenciar as certificados utilizando ACM (AWS Certificate Manager)
- É possível criar e fazer o upload dos próprios certificados
- HTTPS Listeners:
  - É obrigatório especificar um certificado padrão
  - É possível adicionar uma lista de certificados para suportar múltiplos domínios
  - Cliente posem utilizar SNI (Server Name Indicator) para especificar qual hostname eles acessam
  - É possível configurar uma política de segurança para suportar versões antigas/legado do SSL/TLS

SNI (Server Name Indicator):

- SNI resolve o problema de carregar múltiplos certificados SSL em um web server (para servir vários web sites)
- É um protocolo novo e requer que o cliente indique qual o hostname do servidor que ele quer se comunicar no inicio da comunicação do SSL.
- Dessa forma o servidor irá encontrar o certificado correto ou retornar o certificado padrão
- Somente funciona com ALB e NLB e CloudFront

Ao pensar em múltiplos SSL em um LB, pensa em ALB ou NLB pois são eles que suportam o SNI.

### Connection Draining/Deregistration Delay

Para o CLB o nome é Connection Draining e para ALB e NLB é Deregistration Delay

Consiste em um tempo 'dado' para as requisições que ainda estão acontecendo 'esperarem' enquanto uma instância não está funcionando corretamente, enquanto isto está ocorrendo a instância é sinalizada com o estado de 'draining'.

Enquanto uma instância está nesse estado de draining, o LB não fará novas requisições para essa instância.

O tempo de draining pode ser configurado entre 1 e 3600 segundos (padrão é 300 segundos) ou 0 para desativado.

### Auto Scaling Group (ASG)

O objetivo do ASG é:

- Scale out (adicionar instâncias EC@) para corresponder o aumento de carga
- Scale in (remover instâncias EC2) para corresponder a diminuição de carga
- É possível garantir que teremos um número mínimo e máximo de instâncias rodando no nosso ASG
- Também é possível registrar novas instâncias EC2 automaticamente no LB com o ASG
- Recriar uma instância EC2 caso uma anterior seja terminada

O ASG em sí não tem custos, mas será cobrado todo o serviço e recursos utilizado pelo ASG

Podemos configurar:

- Capacidade mínima: O mínimo de instâncias que queremos no ASG
- Capacidade desejada: A quantidade de instâncias que queremos rodando
- Capacidade máxima: o máximo de instâncias que queremos no ASG

O LB pode fazer o Health Check das instâncias que estão no ASG e caso uma não esteja saudável ele informa para o ASG para que o ASG termine/recrie a instância.

Atributos para criar um ASG:

- Launch Template: Contem as informações de como o ASG deve iniciar novas instâncias dentro do grupo
  - AMI + Tipo da instância
  - EC2 User Data
  - Volumes EBS
  - Security Groups
  - SSH Key Pair
  - IAM Roles para as instâncias EC2
  - Network + Subnet info
  - LB info
- Tamanho máximo/mínimo e capacidade inicial
- Políticas de escalonamento (Scaling Policies)

É possível escalar um ASG baseado em alarmes do CloudWatch

### Scaling Policies

- Dynamic Scaling
  - Target Tracking Scale
    - Simple to set-up
    - Exemplo: Preciso que o uso médio de CPU do grupo (ASG) fique em 40%, dessa maneira o ASG vai adicionar ou retirar instâncias para manter a utilização de CPU perto dos 40%
  - Simple/Step Scaling
    - Adicionar ou remover instâncias baseados em alertar do CloudWatch

- Scheduled Scaling
  - Antecipar o escalonamento baseado em métricas já conhecidas
  - Exemplo: Todo dia na hora de pico (11:00 até 13:00) de usuários aumentar a quantidade de instâncias

- Predictive Scaling
  - Fica prevendo continuamente o uso e agenda um escalonamento para o futuro

Métricas mais utilizadas:

- CPUUtilization: Utilização de CPU média entre as instâncias do grupo (ASG)
- RequestCountPerTarget: Garantir que a número de requisições para as instâncias EC2 fiquem estáveis
- Average Network In / Out

Também existe o 'cooldown period' e ocorre logo após um evento de escalonamento acontecer. O ASG não irá criar e nem terminar instâncias durante um certo intervalo de tempo (padrão 300 segundos) com o intuito de esperar as métricas se estabilizarem e ver se o escalonamento teve efeito.

## Amazon RDS (Relational Database Service)

RDS significa Relational Database Service

É um serviço de gerenciamento de banco de dados para bancos de dados que usam SQL como linguagem de query.

Permite criar banco de dados na nuvem que são gerenciados pela AWS, podendo ser:

- Postgres
- MySQL
- MariaDB
- Oracle
- Microsoft SQL Server
- IBM DB2
- Aurora (banco de dados proprietário da AWS)

Algumas vantagens que podemos ter ao usar o RDS ao invés de criar um DB direto na nossa instância EC2:

- Por se tratar de um serviço de gerenciamento ele dispõe de outros serviços além do DB:
  - Provisionamento automático, patch de OS
  - Backup contínuo o possibilidade de restaurar baseado em um data específica
  - Dashboards de monitoramento
  - Possibilidade de configurar em várias AZ para DR (Disaster Recovery)
  - Janelas de manutenção para upgrades
  - Possibilidade de escalonamento (horizontal e vertical)
  - Armazenamento apoiado pelo EBS (gp2 ou io1)
  - Porém não é possível acessar as instâncias RDS por SSH por se tratar de um serviço gerenciado pela AWS
  
RDS Storage Auto Scaling

Ajuda a aumentar o armazenamento na instância do DB RDS dinamicamente.

O RDS percebe que o DB está ficando sendo armazenamento e automaticamente escalona ele com o objetivo de evitar a necessidade de escalonamento manual.

Para isso é necessário definir um tamanho máximo para o escalonamento.

O armazenamento será modificado se:

- O armazenamento livre for menor do que 10% do alocado.
- Se o estado de baixo armazenamento durar pelo menos 5 minutos.
- Se a última alteração de armazenamento tiver sido há mais de 6 horas.

É especialmente útil para aplicações com volumes de dados imprevisíveis.

### RDS Replicas

RDS Replicas são utilizadas para escalonar a taxa de leitura do DB.

Para isso é possível criar até 15 réplicas na mesma AZ, em AZ diferentes e em regiões diferentes.

As replicações são assíncronas, então as leituras são consistentes.

As réplicas também podem ser promovidas para se tornar um DB próprio e sair do estado de réplica.

É necessário atualizar a string de conexão na aplicação para de ela aproveite as réplicas de leitura.

Casos de uso de réplicas de leitura:

No caso de uma aplicação em produção rodando normalmente com seu banco de dados e sua leitura padrão, porém em algum momento será necessário conectar uma aplicação de relatórios nesse banco de dados que fará muito mais leituras dos dados do DB.

Isso poderá fazer com que o DB fique sobrecarregado e gerar lentidão na aplicação em produção.

Para solucionar o problema é possível criar uma réplica de leitura para que a aplicação de relatórios leia somente da réplica e não impacte o DB em produção.

Lembrando que as réplicas de leitura são apenas para leitura ou seja, para operações de SELECT.

Custos envolvidos nas réplicas de leitura:

Normalmente quando dados transitam de uma AZ para outra tem um custo envolvido.

Porém para caso de replicas de leitura RDS, caso estejam em AZs diferentes mas na mesma região, não é cobrado nenhuma taxa para esse transito de dados entre AZs.

Porém caso as réplicas de leitura e o DB principal estejam em regiões diferentes, será cobrado uma taxa para o transito de dados entre elas.

### RDS Multi AZ

Multi AZ é principalmente utilizado para Disaster Recovery

Nesse caso quando algo é alterado no DB principal, o DB 'clonado' que fica em standby na outra AZ também replica a alteração de forma síncrona. A aplicação utiliza apenas um DNS e caso o DB principal dê problemas ou falhe o DNS redirecionará a aplicação para o DB reserva automaticamente.

Multi AZ não é utilizado para escalonamento uma vez que o DB reserva fica em standby na outra AZ, ou seja, nenhuma aplicação pode alterar dados diretamente dele.

Porém também há o caso de que é possível configurar as réplicas de leitura para atuarem como Multi AZ para Disaster Recovery.

Importante saber:

- Ao alterar de Single AZ para Multi AZ:
  - Não há necessidade para parar o DB
  - Só é necessário ir nas configurações do DB e habilitar o Multi AZ.
  - O que acontece por baixo dos panos é que será criado um snapshot do DB e depois esse snapshot será restaurado em outra AZ, por fim será criado uma sincronização entro o DB principal e o DB clonado em standby.

### Amazon RDS na Prática

- No dashboard do RDS vá em 'dashboards' e clique em 'Create Database'
- Escolha qual banco de dados você quer (aurora, Postgres, mysql e etc)
- Escolha o tipo (prod, dev ou free)
- Escolha o tipo de disponibilidade e durabilidade
- Preenche as credenciais
- Escolha o tipo da instância
- Configure o tipo de armazenamento
- Configura as opções de rede.

### RDS Custom

Apesar do RDS ser um serviço gerenciado que não conseguimos configurar muitas coisas, temos 2 exceções que podemos configurar.

São bancos Oracle e Microsoft SQL com OS e bancos customizáveis.

Ainda teremos as vantagens do RDS automatizando o setup, operação e escalonamento dos DBs na AWS

Porem com o RDS custom temos acesso ao DB e ao OS para configurar algumas coisas:

- Definir configurações
- Instalar patches
- Habilitar funcionalidades nativas
- Temos acesso à instância EC2 que roda por baixa utilizando SSH e SSM Session Manager.
- Também é possível desativas as automações do RDS para poder configurar e personalizar de uma maneira mais completa.

### Amazon Aurora

É uma tecnologia proprietária da AWS, ou seja, não é open source.

Postgres e MySQL são suportados como Aurora DB, significa que os drivers Postgres ou MySQL irão funcionar ao se conectar com um AuroraDB.

A Amazon diz que o Aurora é otimizado para Cloud, sendo mais performático que o Postgres e que o MySQL.

O armazenamento do Aurora aumenta automaticamente, indo de 10GB até 128TB.

Aurora pode ter até 15 réplicas de leitura e o processo de criar replicas também é mais rápido no Aurora.

A recuperação do Aurora caso passe por algum erro é instantânea, dessa forma possuindo uma alta disponibilidade nativa.

Aurora custa um pouco a mais que o RDS porém é mais eficiente.

Alta escalabilidade e Escalonamento de leitura no Aurora pois você possui:

- 6 copias de seus dados entre 3 AZs diferentes
  - Aurora precisa apenas de 4 funcionando das 6 instâncias para escrever
  - Aurora precisa apenas de 3 funcionando das 6 instâncias para ler
  - Consegue corrigir automaticamente caso algo dado em alguma instância esteja com algum problema
  - Armazenado é espalhado entre centenas de volumes
  