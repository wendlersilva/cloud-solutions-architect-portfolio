# 🚀 Laboratório 07 — AWS Serverless Memory Game

## 📌 Sobre o Laboratório

Neste laboratório, foi desenvolvido e integrado um jogo da memória a uma arquitetura Serverless na AWS. A aplicação utiliza serviços gerenciados para hospedar o site, processar os resultados dos jogadores e armazenar as informações das partidas.

A solução utiliza Amazon S3 para hospedagem do site estático, Amazon DynamoDB para armazenamento dos resultados, AWS Lambda para processamento da lógica de negócio e Amazon API Gateway para disponibilizar os endpoints de comunicação entre o frontend e o backend.

## 🎯 Objetivos

- Hospedar uma aplicação web estática utilizando Amazon S3.
- Configurar hospedagem de site estático e acesso público ao conteúdo.
- Criar uma tabela no Amazon DynamoDB para armazenar os resultados.
- Criar uma IAM Role com permissões específicas para a função Lambda.
- Desenvolver uma função AWS Lambda utilizando Python 3.12.
- Criar uma API REST no Amazon API Gateway.
- Integrar o frontend com os serviços backend.
- Testar o funcionamento da aplicação e validar o armazenamento dos resultados.

## 🛠️ Serviços AWS utilizados

- **Amazon S3:** hospedagem do site estático.
- **Amazon DynamoDB:** armazenamento dos resultados das partidas.
- **AWS IAM:** gerenciamento de funções e permissões.
- **AWS Lambda:** execução da lógica de processamento.
- **Amazon API Gateway:** criação dos endpoints REST.
- **Amazon CloudWatch:** registro de logs da função Lambda.

---

## 1. Criação do bucket S3

O primeiro passo foi criar um bucket no Amazon S3 para armazenar os arquivos da aplicação, incluindo a página HTML, arquivos CSS, JavaScript e os recursos visuais do jogo.

Durante a criação, foi necessário desabilitar o bloqueio de acesso público para permitir que o site pudesse ser acessado pelo endpoint de hospedagem estática.

![Acessando o Amazon S3](./imagens/01-ir-em-s3.png)

Após acessar o serviço, foi iniciado o processo de criação de um novo bucket.

![Criar bucket](./imagens/02-criar-bucket.png)

Foi definido um nome exclusivo para o bucket, conforme as regras de nomenclatura do Amazon S3.

![Nome do bucket](./imagens/03-nome-bucket.png)

Para permitir a hospedagem pública do site, foi desmarcada a opção de bloqueio de todo o acesso público.

![Desabilitar bloqueio de acesso público](./imagens/04-desmarque-bloquear-todo-acesso-publico.png)

Com as configurações definidas, o bucket foi criado.

![Bucket criado](./imagens/05-clique-criar-bucket.png)

## 2. Upload dos arquivos da aplicação

Com o bucket disponível, foram enviados os arquivos necessários para executar o jogo da memória. A estrutura inclui a página principal, arquivos de estilo, scripts JavaScript e recursos utilizados pela aplicação.

![Upload dos arquivos e pastas](./imagens/06-faca-upload-arquivos-pastas-clique-carregar.png)

## 3. Configuração da hospedagem de site estático

Na aba de propriedades do bucket, foi localizada a configuração de hospedagem de site estático.

![Propriedades do bucket](./imagens/07-va-propriedades.png)

A hospedagem foi ativada e o documento de índice foi definido como `index.html`, indicando a página que será carregada quando o usuário acessar o site.

![Ativar hospedagem e definir index](./imagens/08-clique-ativar-e-especifique-index.png)

Em seguida, foram revisadas as configurações da hospedagem estática.

![Configuração da hospedagem estática](./imagens/09-hospedagem-site-estatico-editar.png)

## 4. Configuração da política de acesso do bucket

Após ativar a hospedagem, foi necessário configurar as permissões para permitir a leitura pública dos arquivos da aplicação.

A configuração foi realizada na aba de permissões do bucket.

![Aba de permissões](./imagens/10-va-aba-permissoes.png)

Na seção de política do bucket, foi aberta a opção de edição para adicionar a política de acesso.

![Editar política do bucket](./imagens/11-va-politica-bucket-e-editar.png)

Para gerar a política, foi utilizado um gerador de políticas AWS.

![Acessar gerador de políticas](./imagens/12-va-no-site-gerador-de-politicas.png)

No gerador, foram definidos os parâmetros necessários para permitir a leitura dos objetos armazenados no bucket.

![Gerar política e adicionar declaração](./imagens/13-gere-as-politicas-e-clique-add-statement.png)

Após preencher as informações, a política foi gerada.

![Gerar política](./imagens/14-clique-generate-policy.png)

O conteúdo gerado foi copiado para ser utilizado nas permissões do bucket.

![Copiar política gerada](./imagens/15-copie-a-politica-gerada.png)

A política foi colada no campo correspondente dentro do console do Amazon S3 e salva.

![Política adicionada ao bucket](./imagens/16-politica-de-bucket-colada.png)

Com as permissões configuradas, foi realizado um teste de acesso ao site. A página do jogo passou a carregar corretamente pelo endpoint de hospedagem estática.

![Teste da página do jogo](./imagens/17-testando-pagina-jogo-agora-funciona.png)

## 5. Criação da tabela no Amazon DynamoDB

Com o frontend hospedado, o próximo passo foi configurar o armazenamento dos resultados das partidas.

Foi acessado o Amazon DynamoDB para criar uma tabela que receberá os dados dos jogadores.

![Acessar DynamoDB](./imagens/18-ir-dynamo-db.png)

Na página do serviço, foi iniciado o processo de criação de uma nova tabela.

![Criar tabela DynamoDB](./imagens/19-clique-criar-tabela.png)

A tabela foi configurada com uma chave de partição `playerId` e uma chave de classificação `gameId`, permitindo organizar os registros dos jogadores e suas partidas.

![Nome da tabela e chaves](./imagens/20-nome-tabela-dynamo-e-partition-key-e-sort-key.png)

Após definir as configurações, a tabela foi criada.

![Tabela criada](./imagens/21-clique-criar-tabela.png)

## 6. Criação da IAM Role para o AWS Lambda

Para que a função Lambda pudesse acessar o DynamoDB e registrar logs no CloudWatch, foi criada uma função do IAM com as permissões necessárias.

Primeiramente, foi acessado o serviço AWS IAM.

![Acessar IAM](./imagens/22-ir-iam.png)

Na área de gerenciamento de funções, foi iniciada a criação de uma nova Role.

![Acessar funções do IAM](./imagens/23-ir-funcoes.png)

Foi selecionada a opção para criar uma nova função.

![Criar função IAM](./imagens/24-criar-perfil.png)

Como entidade confiável, foi selecionado um serviço da AWS, utilizando o Lambda como caso de uso.

![Selecionar serviço AWS e Lambda](./imagens/25-selecione-servico-da-aws-e-caso-de-uso-lambda.png)

Foi adicionada a política `AWSLambdaBasicExecutionRole`, necessária para permitir que a função registre logs no CloudWatch.

![Selecionar AWSLambdaBasicExecutionRole](./imagens/26-selecione-awslambdabasicexecutionrole.png)

Em seguida, foi definido o nome da Role.

![Definir nome da função](./imagens/27-de-o-nome-funcao.png)

Com as configurações preenchidas, a Role foi criada.

![Criar Role](./imagens/28-clique-criar-perfil.png)

Após a criação, a função foi localizada e aberta para configurar permissões adicionais.

![Abrir Role criada](./imagens/29-clique-na-role-criada.png)

Foi adicionada uma política em linha para conceder acesso às operações necessárias do DynamoDB.

![Criar política em linha](./imagens/30-adicionar-permissoes-criar-politica-em-linha.png)

O documento da política foi inserido com as permissões necessárias para a tabela utilizada pela aplicação.

![Inserir permissões](./imagens/31-cole-permissao.png)

Por fim, a política recebeu um nome e foi criada.

![Nomear e criar política](./imagens/32-defina-nome-politica-e-criar-politica.png)

## 7. Criação da função AWS Lambda

Com a tabela e as permissões configuradas, foi criada a função Lambda responsável por processar as operações da aplicação.

O serviço AWS Lambda foi acessado pelo console.

![Acessar Lambda](./imagens/33-va-em-lambda.png)

Foi iniciado o processo de criação de uma nova função.

![Criar função Lambda](./imagens/34-criar-funcao.png)

A opção de criação da função foi selecionada.

![Iniciar criação da função](./imagens/35-clique-criar-funcao.png)

A função foi configurada do zero, utilizando Python 3.12 como ambiente de execução.

![Configurar função Python 3.12](./imagens/36-criar-do-zero-nome-funcao-e-pyhton312.png)

Nas configurações adicionais, foi selecionada a opção para personalizar a função de execução.

![Personalizar perfil de execução](./imagens/37-clique-confi-adicional-personalizar-perfil-execucao.png)

Foi escolhida a Role criada anteriormente, contendo as permissões necessárias para acessar o DynamoDB e registrar logs.

![Selecionar função de execução](./imagens/38-selecione-funcao.png)

Após revisar as configurações, a função Lambda foi criada.

![Criar função Lambda](./imagens/39-clique-criar-funcao.png)

## 8. Configuração e teste da função Lambda

Com a função criada, foi inserido o código responsável por processar os resultados das partidas, consultar o ranking e recuperar o histórico de um jogador.

![Inserir código na função Lambda](./imagens/40-cole-o-codigo-na-lambda-function.png)

Após inserir o código, foi realizado o deploy para aplicar as alterações.

![Deploy da função](./imagens/41-clique-em-deploy.png)

Na aba de testes, foi configurado um evento para validar o funcionamento da função.

![Acessar aba de testes](./imagens/42-clique-aba-testar.png)

Foi criado um novo evento de teste com o JSON correspondente à operação de envio de resultado.

![Configurar evento de teste](./imagens/43-nome-novo-evento-e-insira-json-evento.png)

A execução foi realizada e o retorno confirmou o funcionamento esperado da função Lambda.

![Teste da função funcionando](./imagens/44-agora-teste-do-evento-funcionando.png)

## 9. Criação da API REST no Amazon API Gateway

Para permitir a comunicação entre o jogo hospedado no S3 e a função Lambda, foi criada uma API REST utilizando o Amazon API Gateway.

O serviço foi acessado pelo console da AWS.

![Acessar API Gateway](./imagens/45-ir-api-gateway.png)

Foi iniciado o processo de criação de uma nova API.

![Criar API](./imagens/46-criar-api.png)

Foi selecionado o tipo REST API, com endpoint Regional, e definido um nome para a API.

![Configurar API REST Regional](./imagens/47-tipo-api-rest-nome-api-e-tipo-regional.png)

## 10. Criação dos recursos e métodos da API

Dentro da API, foram criados os recursos que representam os caminhos utilizados pela aplicação.

O primeiro recurso foi criado para a rota `/resultado`.

![Criar recurso](./imagens/48-dentro-da-api-criar-recurso.png)

Foi definido o nome do recurso e confirmada sua criação.

![Nomear recurso resultado](./imagens/49-nome-resultado-clique-criar-recurso.png)

Com o recurso selecionado, foi iniciado o processo de criação de um método.

![Criar método](./imagens/50-selecionado-recurso-clique-criar-metodo.png)

O método foi configurado para utilizar a função Lambda como integração, habilitando a integração de proxy.

![Configurar método e integração proxy](./imagens/51-metodo-tipo-integracao-integracao-proxy.png)

Em seguida, foi criada a estrutura para a rota `/ranking`.

![Criar recurso ranking](./imagens/52-selecione-barra-e-criar-recurso.png)

O recurso foi selecionado e o método correspondente foi configurado.

![Selecionar ranking e criar método](./imagens/53-selecione-ranking-e-criar-metodo.png)

A integração com a função Lambda foi configurada utilizando o recurso de integração proxy.

![Configurar integração Lambda para ranking](./imagens/54-selecione-metodo--tipo-proxy-integracao-arn-e-criar.png)

Também foi criado o recurso para consultar o histórico de um jogador.

![Criar recurso jogador](./imagens/55-selecione-barra-criar-recurso-nome-jogador.png)

Foi configurada a estrutura do recurso para receber o nome do jogador como parâmetro.

![Configurar parâmetro do jogador](./imagens/56-selecione-jogador-e-coloque-nome-jogador.png)

O método GET foi associado à função Lambda por meio da integração de proxy.

![Configurar método GET do jogador](./imagens/57-selecione{nome}-criar-metodo-get-lambda-function-proxy-arn.png)

## 11. Configuração do CORS e implantação da API

Para permitir que o frontend hospedado no Amazon S3 pudesse realizar chamadas à API, foi habilitado o CORS nos recursos criados.

![Habilitar CORS nos recursos](./imagens/58-cors-habilitado-em-resultado-ranking-nome.png)

Após configurar os recursos e métodos, a API foi implantada.

![Implantar API](./imagens/59-clique-implantar-api.png)

Foi criado o estágio `prod`, disponibilizando a URL de invocação da API.

![Criar estágio prod](./imagens/60-novo-estagio-prod-implantar.png)

## 12. Integração do frontend com a API

Com a API implantada, a URL de invocação foi configurada no arquivo `config.js` da aplicação, desabilitando o modo local para utilizar os serviços AWS.

O arquivo atualizado foi enviado novamente ao bucket S3 para que o site passasse a utilizar o backend serverless.

![Enviar arquivo config atualizado](./imagens/61-carregue-novo-arquivo-config-com-api.png)

## 13. Teste final da aplicação

Após concluir as configurações, foi realizado um teste completo do jogo da memória, incluindo a finalização de uma partida e o envio do resultado para a API.

![Testar jogo e finalizar partida](./imagens/62-testando-jogo-e-terminando.png)

Por fim, foi verificado no Amazon DynamoDB que os resultados da partida foram armazenados corretamente.

![Resultado armazenado no DynamoDB](./imagens/63-resultado-tabela-dynamo-funcionando.png)

---

## 📚 Aprendizados

Neste laboratório, foi possível praticar a integração de diferentes serviços AWS em uma arquitetura Serverless, compreendendo como uma aplicação web pode utilizar serviços gerenciados sem precisar manter servidores tradicionais.

Principais conhecimentos praticados:

- Hospedagem de aplicações estáticas com Amazon S3.
- Configuração de permissões e políticas de acesso.
- Modelagem e armazenamento de dados no Amazon DynamoDB.
- Criação de IAM Roles e políticas com permissões específicas.
- Desenvolvimento e testes de funções AWS Lambda com Python.
- Criação de APIs REST com Amazon API Gateway.
- Integração entre frontend, API e serviços backend.
- Validação do armazenamento de dados no DynamoDB.

## 🔐 Segurança

A hospedagem estática deste laboratório utiliza acesso público ao conteúdo do bucket S3. Essa configuração foi realizada para permitir o acesso ao jogo pelo navegador.

Em ambientes reais, é importante avaliar cuidadosamente as permissões públicas, conceder somente os acessos necessários e nunca armazenar credenciais, senhas ou informações sensíveis nos arquivos públicos da aplicação.