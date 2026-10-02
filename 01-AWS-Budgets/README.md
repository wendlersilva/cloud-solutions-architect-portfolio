# 💰 AWS Budgets — Controle de Custos

## 📋 Visão Geral

Este laboratório demonstra a configuração de um orçamento utilizando o **AWS Budgets**, com o objetivo de monitorar os gastos de uma conta AWS e receber notificações quando um percentual definido do orçamento for atingido.

O orçamento configurado neste laboratório possui um limite de **US$ 10**, com um alerta configurado para ser acionado quando **10% do orçamento** for utilizado.

---

## 🎯 Objetivos

- Acessar o serviço AWS Budgets pelo AWS Management Console
- Criar um orçamento com limite personalizado
- Configurar um alerta baseado em percentual do orçamento
- Configurar notificações por e-mail
- Validar a configuração do orçamento
- Compreender a importância do monitoramento de custos na AWS

---

## ☁️ Serviço AWS Utilizado

### AWS Budgets

O **AWS Budgets** permite definir limites de custo ou uso e configurar alertas para acompanhar o consumo dos recursos AWS.

Neste laboratório, foi criado um orçamento de **US$ 10**, com uma notificação configurada para **10% do valor orçado**.

### 1. Criar orçamento

Primeiro, acesse o serviço **AWS Budgets** e clique em **Criar orçamento**.

![Criar orçamento](01-AWS-Budgets/01-criar-orcamento.png)

---

### 2. Personalizar o orçamento

Na tela de criação, selecione **Personalizar (Avançado)**.

![Personalizar orçamento](01-AWS-Budgets/02-personalizar-orcamento.png)

---

### 3. Definir o valor do orçamento

Configure o orçamento com o valor de **US$ 10**.

![Definindo o orçamento](01-AWS-Budgets/03-definindo-orcamento.png)

---

### 4. Configurar o alerta

Adicione um limite de alerta de **10% do valor orçado** e mantenha o acionador como **Real**.

![Configurando alerta](01-AWS-Budgets/04-alerta.png)

---

### 5. Revisar as configurações

Revise as configurações antes de criar o orçamento.

![Revisão](01-AWS-Budgets/05-revisao.png)

---

### 6. Criar o orçamento

Após revisar as configurações, clique em **Criar orçamento**.

![Criar orçamento](01-AWS-Budgets/06-criar-orcamento.png)

---

### 7. Tela final

Por fim, valide as informações do orçamento criado, incluindo o valor configurado e o valor utilizado.

![Tela final](01-AWS-Budgets/07-tela-final.png)