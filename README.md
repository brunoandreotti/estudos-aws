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
