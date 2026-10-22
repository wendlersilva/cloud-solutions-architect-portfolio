# ☁️ Amazon EC2, CloudWatch e SNS

## Objetivo

Neste laboratório, foi implementado um monitoramento básico de uma instância Amazon EC2 utilizando o Amazon CloudWatch e o Amazon SNS.

O objetivo foi acompanhar a utilização de CPU da instância, configurar um alarme para identificar uso elevado do processador e receber notificações por e-mail quando o limite estabelecido fosse ultrapassado.

Também foi gerada uma carga controlada de CPU para validar o funcionamento do alarme e das notificações.

## Arquitetura implementada

- **Amazon EC2:** instância utilizada para executar o servidor e gerar carga de CPU.
- **Amazon CloudWatch:** monitoramento das métricas e gerenciamento do alarme.
- **Amazon SNS:** envio de notificações por e-mail.
- **SSH:** acesso remoto à instância para instalação e execução da ferramenta de teste.
- **stress-ng:** ferramenta utilizada para gerar carga de CPU durante a validação.

## 1. Criação da instância EC2

Inicialmente, foi acessado o serviço Amazon EC2 para criar uma instância que seria utilizada nos testes de monitoramento.

![Acessar a criação de instâncias](./01-clicar-em-executar-instancias.png)

A instância foi configurada com um nome de identificação e a imagem Amazon Linux 2023, conforme os requisitos do laboratório.

![Definir nome da instância e AMI](./02-nome-instancia-e-ami.png)

Foi selecionado o tipo de instância `t2.micro`, utilizado neste exercício para executar os testes de carga e monitoramento.

![Selecionar tipo de instância](./03-tipo-de-instancia.png)

Também foi configurado um par de chaves (*Key Pair*) para permitir o acesso remoto à instância por SSH.

![Configurar nome do Key Pair](./04-nome-key-pair.png)

Nas configurações de rede, foi selecionada a VPC padrão e uma sub-rede pública, com atribuição de endereço IP público habilitada. Também foi criado um Security Group para controlar o acesso à instância.

![Configurações de rede e Security Group](./05-configurações-de-rede-e-nome-grupo-de-seguranca.png)

A regra de entrada do Security Group foi configurada para permitir SSH, restrito ao endereço IP de origem do usuário (*My IP*).

![Configurar regra de entrada SSH](./06-regras-de-entrada-ssh.png)

Após revisar as configurações, a instância foi iniciada.

![Executar a instância EC2](./07-clicar-em-executar-instancia.png)

Com a instância em execução, foi anotado seu ID para utilizá-lo na seleção das métricas do CloudWatch. Foi aguardado o tempo necessário para que as métricas estivessem disponíveis.

![Instância EC2 em execução](./08-instancia-em-execucao.png)

## 2. Configuração do alarme no CloudWatch

Com a instância criada, foi acessado o Amazon CloudWatch para configurar o monitoramento da utilização de CPU.

![Acessar o CloudWatch](./09-ir-em-cloud-watch.png)

Na seção de alarmes, foi iniciada a criação de um novo alarme.

![Acessar a seção de alarmes](./10-ir-em-alarme.png)

![Iniciar a criação do alarme](./11-clicar-criar-alarme.png)

Na etapa de seleção de métricas, foi escolhida a categoria de métricas do Amazon EC2 por instância (*Per-Instance Metrics*).

![Selecionar métricas para o alarme](./12-selecione-metricas-e-selecionar-metrica.png)

O ID da instância EC2 foi utilizado para localizar as métricas correspondentes ao recurso criado.

![Localizar métricas da instância EC2](./13-cole-id-da-instancia-e-selecione-ec2-per-metrics.png)

Em seguida, foi selecionada a métrica `CPUUtilization`, responsável por representar a porcentagem de utilização da CPU da instância.

![Selecionar a métrica CPUUtilization](./14-marque-cpuutilization-e-selecionar-metrica.png)

O alarme foi configurado utilizando a estatística **Average**, com período de avaliação de cinco minutos.

![Configurar estatística e período](./15-marque-estatitica-media-periodo-5-minutos.png)

Como condição, foi definido o disparo do alarme quando a utilização média da CPU fosse superior a 70%.

![Definir limite de CPU acima de 70%](./16-marque-cpuuitilization-maior-que-70.png)

## 3. Configuração das notificações com Amazon SNS

Para receber notificações quando o alarme entrasse no estado de alerta, foi criado um tópico do Amazon SNS.

O tópico foi associado a um endereço de e-mail, cuja assinatura precisou ser confirmada para que as notificações pudessem ser recebidas.

![Criar tópico SNS e informar e-mail](./17-em-alarme-criar-novo-topico-inserir-email.png)

Após configurar as ações de notificação, foi definido um nome para identificar o alarme no CloudWatch.

![Definir nome do alarme](./18-nome-alarme.png)

O alarme foi configurado para enviar notificações quando a condição de CPU definida fosse atingida.

## 4. Acesso remoto à instância EC2

Para gerar carga de processamento e testar o alarme, foi realizado o acesso remoto à instância EC2 por SSH, utilizando o ambiente Windows.

![Acessar a instância EC2 via SSH](./19-comando-ssh-via-windows-acessar-ec2.png)

Após estabelecer a conexão, o sistema foi atualizado e a ferramenta `stress-ng` foi instalada. Essa ferramenta permite gerar carga de CPU de forma controlada para validar o comportamento do monitoramento.

![Instalar stress-ng](./20-instalando-stress-ng.png)

Em seguida, foi executado o teste de carga de CPU. A ferramenta foi configurada para gerar processamento durante um período limitado, permitindo observar o aumento da métrica `CPUUtilization`.

![Executar o teste de carga de CPU](./21-disparando-stress.png)

## 5. Validação do alarme e das notificações

Com a carga de CPU em execução, foi acessado o CloudWatch para verificar o estado do alarme.

Após o período de avaliação, a utilização da CPU ultrapassou o limite configurado e o alarme entrou no estado **In alarm**.

![Alarme acionado com uso de CPU em 100%](./22-instancia-em-alarme-100%25-de-uso.png)

Também foi verificado o recebimento da notificação enviada pelo Amazon SNS por e-mail, confirmando a integração entre o CloudWatch e o serviço de notificações.

![Notificação de uso elevado recebida por e-mail](./23-aviso-por-email-uso.png)

Após a validação, o teste de carga foi interrompido para encerrar a geração artificial de processamento.

## 6. Resultado

O laboratório permitiu praticar os fundamentos de monitoramento e observabilidade de recursos na AWS.

Principais resultados:

- Criação e configuração de uma instância Amazon EC2.
- Configuração de acesso SSH com regra de segurança restrita ao IP de origem.
- Seleção da métrica `CPUUtilization` no Amazon CloudWatch.
- Criação de um alarme com limite de utilização de CPU superior a 70%.
- Integração do CloudWatch com um tópico do Amazon SNS.
- Configuração de notificações por e-mail.
- Geração de carga controlada com `stress-ng`.
- Validação do alarme e do recebimento das notificações.

## Serviços e ferramentas utilizados

- Amazon EC2
- Amazon CloudWatch
- Amazon SNS
- Security Groups
- SSH
- Amazon Linux 2023
- stress-ng

## Conhecimentos praticados

- Monitoramento de métricas de infraestrutura.
- Configuração de alarmes no CloudWatch.
- Notificações automatizadas com Amazon SNS.
- Acesso remoto seguro a instâncias EC2.
- Testes controlados de carga de CPU.
- Observabilidade básica de recursos em nuvem.
