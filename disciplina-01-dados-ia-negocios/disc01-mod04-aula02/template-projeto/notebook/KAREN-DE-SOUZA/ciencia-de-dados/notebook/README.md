Análise de CHURN Empresa de Streaming
![image](https://github.com/danielesantiago/TemplatePED/assets/64613885/cc24a702-46b8-4263-a764-8c65c56f02ac)

## 📌 Visão Geral
Este projeto analisa a base de clientes de um serviço de streaming para entender quem cancela, por quê, em que momento da vida esse cancelamento ocorre. Utilizamos análise exploratória de dados (limpeza, análise univariada, bivariada) e frameworks de negócios (cohort, RFM, Pareto) para transformar um extrato bruto de clientes em hipótses acionáveis sobre retenção. 

📄 [Veja a análise no Jupyter Notebook]([https://github.com/unipds-ciencia-de-dados/ciencia-de-dados/tree/08996dd64c4610826e091d6fd536c4a4cd97ad7b/disciplina-01-dados-ia-negocios/disc01-mod04-aula02/template-projeto/reports/figures)](https://github.com/KAREN-DE-SOUZA/ciencia-de-dados/blob/main/notebook/Churn%20Streaming%20_1.ipynb)

## 💼 Entendimento do Negócio

Em um serviço de assinatura, reter um cliente custa muito menos do que adquirir um novo — por isso, entender o churn é uma das análises de maior impacto financeiro. A empresa deste caso desconfia que vem perdendo assinantes, mas não sabe quem cancela, por que cancela, nem em que momento da relação isso acontece. Este projeto explora os dados para responder a essas perguntas antes de qualquer modelo preditivo.

**Tipos de Análise Realizados:**
- Análise univariada com limpeza e tratamento de dados  
- Análise bivariada (relação de cada variável com o churn) 
- Análise de retenção por Cohort   
- Segmentação de clientes com RFM adaptado para assinatura   
- Análise de Pareto dos motivos de cancelamento

**Principais Indicadores Chave de Desempenho:**
 Retenção por safra de assinatura (cohort)   
 NPS por grupo de churn   
 Tempo de vida do cliente (meses de assinatura)   
 Análise do Modelo Atual   

 ## 📊 Análise do Modelo Atual ##
 base bruta chegou com diversos problemas de qualidade típicos de um trato exportado às pressas de múltiplos sistemas: valores em escalas isturadas, categorias escritas de várias formas, ausências disfarçadas e datas mazenadas como texto. A primeira etapa do projeto foi diagnosticar e tratar ses problemas — porque nenhuma conclusão é confiável sobre um dado jo.   m cuidado central da análise foi o viés de maturação: clientes muito recentes parecem como "não churn" apenas porque ainda não tiveram tempo de ncelar. Para comparar os grupos de forma justa, a análise de churn nsiderou apenas clientes com pelo menos 220 dias de assinatura — uma emissa assumida e registrada explicitamente.  

## 🛠 Pré-processamento 
O pré-processamento foi conduzido com python, tratando cada problema de qualidade de forma justificada (corrigir, remover, manter ou imputar), e criando as variáveis necessárias para os frameworks.   

Considerações Importantes:   
Variáveis de engajamento (dias ativos, recência, horas assistidas) são contaminadas pelo próprio churn (vazamento de target) — quem cancelou parou de usar porque saiu, não o contrário — e por isso não foram usadas para explicar o churn.  
 
_Etapas do Pré-processamento no Pipeline:_
Remoção de clientes duplicados (cliente_id repetido)   
Conversão das colunas de data para o tipo datetime   
Padronização de categorias (plano, canal_aquisicao) e tratamento de ausências disfarçadas ("?" na região)   
Correção de escala em valor_mensal (valores em centavos divididos por 100)   
Tratamento de valores impossíveis em idade   
Feature engineering: recencia, meses_assinatura e receita_acumulada   

## 🤖 Frameworks e Avaliação

Este projeto aplica frameworks de análise de negócio para extrair valor dos dados de forma estatística:  

Cohort: matriz de retenção por safra de assinatura, lida por linha, coluna e diagonal.   
RFM adaptado: como não há compra repetida, adaptamos as três dimensões — Recência (dias desde o último acesso), Frequência (uso no último mês) e Monetário (valor do plano) — validando a segmentação pela taxa de churn de cada grupo.   
Pareto: concentração dos motivos de cancelamento, para identificar os poucos motivos que respondem pela maioria das saídas. 

## 📈 Insights e Conclusões

As principais descobertas da análise:   
O NPS é o sinal mais forte de churn: clientes que cancelaram tinham NPS médio de 4,1, contra 7,4 dos que permaneceram.   

A retenção piorou nas safras mais recentes: comparando o mesmo mês de vida entre coortes, as safras de 2023 retêm menos que as de 2022 — um alerta sobre a qualidade da aquisição ou da experiência ao longo do tempo.   

O plano Básico tem a maior taxa de cancelamento, enquanto idade, região e ciclo (mensal/anual) praticamente não separam os grupos.   

Poucos motivos concentram a maioria dos cancelamentos (Pareto), indicando que os esforços devem se concentrar em Preço ou Conteúdos Novos (a verificar com análises de custo, benchmarking com concorrentes, etc)   

Nota importante: Todas as conclusões são hipóteses embasadas, não relações causais provadas. Confirmar causa me exigiria experimentação (ex.: testes A/B).

## 📜 Estrutura do Projeto

A estrutura de diretórios do projeto foi organizada da seguinte forma:
```
├── README.md 
├── data
│ ├── processed
│ └── raw
├── models
├── notebooks 
├── reports
│ └── figures 
├── requirements.txt
├── src
│ ├── __init__.py 
│ ├── data
│ │ └── [NomeDoScriptDeDados].py 
│ ├── features
│ │ └── [NomeDoScriptDeFeatures].py 
│ ├── models
│ │ ├── predict_model.py 
│ │ └── train_model.py 

```

## 🚧 Próximos Passos

Investigar a causa da piora de retenção nas safras recentes (o que mudou na aquisição ou no produto ao longo de 2023).   

Validar as hipóteses de churn com um teste A/B — por exemplo, uma ação de retenção direcionada aos clientes nos primeiros meses de vida.   Construir um modelo preditivo de churn usando apenas as variáveis limpas (não contaminadas), com o NPS como principal preditor.   

Aprofundar a análise de funil de onboarding para reduzir o gargalo de conversão inicial.   
