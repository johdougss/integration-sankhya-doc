# Descrição

Esta integração permite sincronizar informações financeiras entre os sistemas Teia Card e Sankhya, garantindo um fluxo
contínuo e consistente de dados. Abaixo estão descritas as principais funcionalidades dessa integração:

![integracao_01](./assets/integracao-01.png)

## Envio de Vendas e Parcelas para o Teia Card

![integracao_01-a](./assets/integracao-01-a.png)

### Objetivo

Capturar e transferir do Sankhya para o Teia Card informações sobre vendas e suas parcelas.

### Processo

O sistema de integração busca no Sankhya dados sobre vendas registradas e suas parcelas associadas.
As informações incluem detalhes como valor da venda, número de parcelas, datas de vencimento e cliente. Esses dados são
enviados para o Teia Card, onde podem ser utilizados para controlar recebimentos e verificar
divergências.

### Frequência

A integração pode ser configurada para rodar periodicamente, garantindo que as informações no Teia
Card estejam sempre atualizadas com as vendas mais recentes do Sankhya. É configurada para rodar uma vez ao dia,
sempre com dados de `D-1`. Isso significa que as vendas e parcelas são buscadas no Sankhya e enviadas ao Teia Card com
informações referentes ao dia anterior.

Com a conclusão dessa etapa, o cliente poderá acompanhar as vendas diretamente no Teia Card e verificar eventuais
divergências com as adquirentes. Isso facilitará o controle dos recebimentos e o monitoramento das vendas registradas no
Sankhya, garantindo uma visão integrada e atualizada dos dados financeiros.

## Recebimento de Baixas das Parcelas do Teia Card e Envio para o Sankhya

![integracao_01-b](./assets/integracao-01-b.png)

### Objetivo

Registrar no Sankhya as baixas (pagamentos realizados) das parcelas conforme forem atualizadas no Teia
Card.

### Processo

O sistema de integração verifica no Teia Card as baixas das parcelas, identificando quais foram efetivamente
pagas. Essas informações incluem identificador da parcela, data de pagamento e valor pago. O sistema então envia esses
dados de baixa para o Sankhya, atualizando o status das parcelas como pagas.

### Frequência

As baixas das parcelas registradas no Teia Card também são enviadas ao Sankhya com dados de `D-1`,
mantendo ambos os sistemas sincronizados com atualizações diárias.

Ao concluir essa etapa, o cliente poderá visualizar,
no Sankhya, todas as vendas que foram efetivamente pagas pela
adquirente ou banco.

## Benefícios da Integração

- **Redução de Erros Manuais**: A automação do processo minimiza a necessidade de entrada manual de dados, reduzindo
  erros e inconsistências.
- **Maior Controle Financeiro**: Ambos os sistemas podem ter uma visão consolidada das vendas e recebimentos,
  facilitando o controle financeiro e tomada de decisões.

---

# Configurações

## Configuração para Envio de Vendas e Parcelas ao Teia Card

Para realizar o envio de dados de vendas e parcelas do Sankhya para o Teia Card, é necessário configurar o sistema de
integração para acessar a API do Sankhya. Abaixo estão os passos e parâmetros de configuração necessários:

![integracao_01-a](./assets/integracao-01-a.png)

### Guia para Geração de Tokens de Produção e Sandbox na Sankhya

#### 1. Geração do Token de Produção

**API de Serviços Gateway**

Para integrar com a Sankhya, é necessário gerar um token de produção. Consulte
a [Documentação Sankhya](https://developer.sankhya.com.br/reference/api-de-integra%C3%A7%C3%B5es-sankhya) para obter
mais detalhes sobre a API de integrações.

**Configuração do Gateway**

Acesse
a [Documentação Sankhya](https://ajuda.sankhya.com.br/hc/pt-br/articles/10007620733463-Configura%C3%A7%C3%B5es-Gateway)
para configurar o Gateway.

**Passo a Passo:**

##### **Acesse o Menu `Configurações Gateway`**

![img.png](./assets/img.png)

##### **Configure um Gateway**

- Configure um nome para a integração, por exemplo: `Netunna`.
- Clique no botão "Adicionar" para vincular uma aplicação.

![img_1.png](./assets/img_1.png)

##### **Vincule uma Aplicação**

- Clique em "Buscar Aplicação".
- Nome da Aplicação: `Netunna`.
- Fornecedor: `NETUNNA SOFTWARE LTDA`.

![img_2.png](./assets/img_2.png)

##### **Crie um Novo Usuário**

1. Acesse o menu "Usuários".
2. Crie um novo usuário:

> **Observação:** Os dados desse usuário não precisam ser compartilhados, pois ele NÃO será utilizado para
> autenticação
> via API. Sua criação serve apenas para vincular a aplicação ao Gateway e gerar o token de acesso

| Campo           | Valor               |
|-----------------|---------------------|
| Nome            | Netunna             |
| Senha           | ****                |
| Grupo           | 0 \<SEM GRUPO>      |
| Empresa         | 1 \<Empresa Matriz> |
| Tipo de Usuário | Integração          |

![img_4.png](./assets/img_4.png)

##### **Vincule o Usuário ao Gateway**

- Retorne à configuração do Gateway e vincule o usuário criado.
- Salve o procedimento e o token será gerado automaticamente.

Ex:

```dotenv
SANKHYA_TOKEN=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee
```

![img_3.png](./assets/img_3.png)

#### 2. Geração do Token de Sandbox

Para criar o token de `Sandbox`, o procedimento é o mesmo descrito para o token de `Produção`. No entanto, é
importante destacar um alerta:

Alguns clientes possuem um servidor com uma instância adicional do Sankhya, que pode ser chamada de `sandbox`, `teste`,
`homologação` ou `cópia de produção`. Nesses casos, a `APPKEY` utilizada será a de `Produção`, e a autenticação deverá
ser feita através da URL `https://api.sankhya.com.br/login`.

Isso significa que, mesmo em um ambiente de testes, a `APPKEY` de produção será usada para autenticação, enquanto a URL
de acesso permanece a mesma. Certifique-se de seguir o mesmo processo de geração do token, adaptando apenas o ambiente
conforme necessário.

**A partir de 1º de outubro de 2024**, a Sankhya disponibilizou um ambiente de `sandbox` dedicado. Consulte
a [Documentação Sankhya](https://developer.sankhya.com.br/reference/appkey-de-sandbox) para mais informações. Esse
ambiente permite o uso de um `APPKEY` de `Sandbox` para autenticação, que deve ser realizada através da URL
`https://api.sandbox.sankhya.com.br/login`.

Portanto, ao fornecer os tokens de **Produção** e **Sandbox**, informe-nos se a autenticação deve ser realizada com a
nossa `APPKEY` de **Produção** ou de **Sandbox**. Isso nos ajudará a garantir que a integração seja configurada
corretamente para o ambiente desejado.

#### Autenticação obsoleta

A autenticação não será realizada por métodos alternativos que envolvem acesso direto à base de dados do cliente.

Por exemplo, usando `MobileLoginSP` e Cookie com `JSESSIONID`.

```http request
POST http://client.xx.ativy.com:50093/mge/service.sbr?serviceName=MobileLoginSP.login&outputType=json
Content-Type: application/json

{
    "serviceName": "MobileLoginSP.login",
    "requestBody": {
        "NOMUSU": {
            "$": "USUARIO 1"
        },
        "INTERNO":{
            "$": "password1"
        },
        "KEEPCONNECTED": {
            "$": "S"
        }
    }
}
```

```http request
GET http://client.xx.ativy.com:50092/mge/service.sbr?serviceName=DbExplorerSP.executeQuery&outputType=json
Cookie: JSESSIONID=avvmuddfkuzNHdxwDhzddp0f8F52e13d4.master; path=/mge
Content-Type: application/json

{
    "serviceName": "DbExplorerSP.executeQuery",
    "requestBody": {
        "sql": "SELECT * FROM TSIEMP EMP WHERE CODEMP=1"
    }
}
```

Portanto, não será necessário informar endereços URL, como, por exemplo:

- `client-test.sankhyacloud.com.br/mge`
- `client.sankhyacloud.com.br/mge`
- `client.xx.ativy.com:30029`

### 3. **Autorização do DbExplorer**

O sistema de integração precisa de autorização no serviço `DbExplorerSP`, disponível via API do Sankhya. Essa
autorização permite executar consultas SQL diretamente no sistema, possibilitando a captura de dados de vendas e suas
parcelas.

Pesquise por `Acessos`:

![dbexplorer-acessos.png](./assets/dbexplorer-acessos.png)

Selecine o modulo: `DbExplorer`:

![dbexplorer-modulo.png](./assets/dbexplorer-modulo.png)

Selecione o usuário criado na geração do token, selecione todas as permissões e clique no botão `Finalizar`;

![dbexplorer-permissao.png](./assets/dbexplorer-permissao.png)

> "serviceName": "DbExplorerSP.executeQuery"

![integracao-07-a.png](./assets/integracao-07-a.png)

### 4. **Buscar somente vendas.**

Alguns clientes Sankhya utilizam a tabela `TGFCAB` para armazenar dados que não estão diretamente relacionados a
vendas. Nesse caso, é importante configurar um filtro para garantir que apenas os dados de vendas sejam considerados.
Para isso, defina o identificador do tipo de operação de venda:
Pode existir mais de um código.

```dotenv
SANKHYA_SALE_OPERATION_TYPE_ID=3200,3201,3202
```

> Conhecido como TOP na Sankhya

### 5. **Bandeiras**<a id="brand-list"></a>

#### Opção 1:

Abaixo está a descrição das bandeiras mapeadas e aceitas no Sankhya. No sistema Sankhya, a tabela que contém as
informações das bandeiras é `TGFTEF.BANDEIRA`.

| Sankhya          | Teia Card |
|------------------|-----------|
| AMERICAN EXPRESS | 3         |
| AMEX             | 3         |
| ELO              | 6         |
| HIPER            | 4         |
| HIPERCARD        | 4         |
| MAESTRO          | 2         |
| MASTER           | 2         |
| MASTERCARD       | 2         |
| VISA             | 1         |
| ELECTRON         | 1         |
| CABAL            | 9         |
| BANESCARD        | 8         |

Para mais detalhes sobre as bandeiras permitidas no Teia Card, consulte
a [documentação da API](https://api.saferedi.nteia.com/api/documentation/#api-Enumerador-Bandeira).


> Variações de maíusculo e minusculo e acento das palavras são aceitas, Ex: `Visa`, `MasterCard`

```env
BRAND_NAME_OPTION=1
```

#### Opção 2:

Uma outra alternativa é recuperar o nome da bandeira da coluna `TGFTEF.NOMEREDE`.

```env
BRAND_NAME_OPTION=2
```

### 6. **Adquirentes**<a id="acquire-list"></a>

### Opção 1:

Abaixo está a descrição das adquirentes mapeadas e aceitas no Sankhya. No sistema Sankhya, a coluna que contém o nome da
adquirente é `TGFTIT.FISCAL`.

| Sankya   | Teia Card |  
|----------|-----------|
| CIELO    | 6         |
| PAGARME  | 39        |
| REDE     | 18        |
| REDECARD | 18        |
| STONE    | 24        |
| SICREDI  | 42        |

Para mais detalhes sobre as adquirentes permitidas no Teia Card, consulte
a [documentação da API](https://api.saferedi.nteia.com/api/documentation/#api-Enumerador-Adquirente).

> Variações de maíusculo e minúsculo e acento das palavras são aceitas, Ex: `RedeCard`, `Cielo`

```env
ACQUIRER_NAME_OPTION=1
```

### Opção 2:

Uma alternativa é recuperar o nome da adquirente da coluna `TGFPAR.NOMEPARC`

Ex:

```sql 
-- noinspection SqlNoDataSourceInspectionForFile

SELECT
    ...
    PAR.NOMEPARC AS ACQUIRER_NAME
    ...
    JOIN TGFPAR AS PAR
ON TGFTIT.CODPARCTEF = PAR.CODPARC
```

```env
ACQUIRER_NAME_OPTION=2
```

### 7. **Meio de Captura**<a id="capture-method-list"></a>

Para identificar o tipo de captura no Sankhya, é analisado o campo `TGFTIT.DESCRTIPTIT`.

1. Por padrão é `PDV`
2. Se no campo conter a palavra `POS` será interpretado como `POS`.

| Sankya `TGFTIT.DESCRTIPTIT`    | Teia Card |
|--------------------------------|-----------|
| POS MASTERCARD/ VISA/ ELO 1X   | POS       |
| CARTÃO DE CRÉDITO              | PDV       |
| CARTÃO DE CRÉDITO              | PDV       |
| CARTÃO CRÉDITO À VISTA TEF     | PDV       |
| MASTERCARD/ VISA/ ELO 7X A 12X | PDV       |

Para mais detalhes sobre os meios de captura no Teia Card, consulte
a [documentação da API](https://api.saferedi.nteia.com/api/documentation/#api-Enumerador-MeioCaptura).

### 8. **Tipo de Serviço**<a id="service-type-list"></a>

No Sankhya, o campo `TGFTIT.SUBTIPOVENDA` é utilizado para identificar o tipo de titulo:

- **`7`**: Representa **Cartão de Crédito**
- **`8`**: Representa **Cartão de Débito**

| Sankhya `TGFTIT.SUBTIPOVENDA` | Nº de Parcelas | Teia Card                    |
|-------------------------------|----------------|------------------------------|
| (7) Cartão de Crédito         | 1              | (1) Vendas crédito à vista   |
| (7) Cartão de Crédito         | 3              | (2) Vendas crédito parcelado |
| (8) Cartão de Débito          | 1              | (3) Débito à vista           |

#### Cenário Alternativo: Campo `SUBTIPOVENDA` Vazio ou Inválido

Caso o campo `TGFTIT.SUBTIPOVENDA` esteja vazio ou possua valores diferentes de `7` e `8`, utilizamos como alternativa o
campo `TGFTIT.DESCRTIPTIT`. A interpretação segue as seguintes regras:

1. Se o campo contiver a palavra `DÉBITO`, será interpretado como `Cartão de Débito`.
2. Se o campo contiver as expressões `CRÉDITO`, `1X`, ..., `12X`, será interpretado como `Cartão de Crédito`.

- Para transações de crédito, o número de parcelas determinará se será enviado como `crédito parcelado` ou `crédito à
  vista` para o Teia Card.

#### Mapeamento `DESCRTIPTIT` para o Teia Card

| Sankhya `TGFTIT.DESCRTIPTIT` | Nº de Parcelas | Interpretado      | Teia Card                    |
|------------------------------|----------------|-------------------|------------------------------|
| CARTÃO DE CRÉDITO            | 8              | Cartão de Crédito | (2) Vendas crédito parcelado |
| AMEX CRÉDITO 1X              | 1              | Cartão de Crédito | (1) Vendas crédito à vista   |
| POS MASTERCARD/VISA/ELO 1X   | 1              | Cartão de Crédito | (1) Vendas crédito à vista   |
| MASTERCARD 2X A 6X           | 6              | Cartão de Crédito | (2) Vendas crédito parcelado |
| MASTERCARD/VISA/ELO 7X A 12X | 7              | Cartão de Crédito | (2) Vendas crédito parcelado |
| CARTÃO DE DÉBITO             | 1              | Cartão de Débito  | (3) Débito à vista           |
| ELO DÉBITO                   | 1              | Cartão de Débito  | (3) Débito à vista           |

Para mais detalhes sobre os tipos de serviço no Teia Card,
consulte a [documentação da API](https://api.saferedi.nteia.com/api/documentation/#api-Enumerador-ServicoTipo)
oficial ou entre em contato com o suporte técnico.

### 9. **Banco de dados**

Existem clientes da Sankhya que utilizam bancos de dados `Oracle` e `SQL Server`. Portanto, é fundamental especificar o
tipo
de banco de
dados para garantir que as consultas SQL sejam elaboradas de acordo com as necessidades específicas de cada cliente

| Nome       | Código |
|------------|--------|
| SQL Server | sqlsrv |
| Oracle     | ocl    |

```dotenv
SANKHYA_DB_CONNECTION=ocl
```

### 10. **Captura das vendas**

A captura das vendas ocorrerá uma vez por dia, com base na data de criação do registro. Esse processo de captura será
realizado no dia seguinte à criação do registro, ou seja, em D-1.

Uma vez que a venda é capturada, quaisquer
atualizações subsequentes feitas na venda não serão refletidas na captura realizada, mantendo o estado do registro
conforme estava no momento da captura. Dessa forma, é importante garantir que os dados estejam completos e corretos
antes da captura, pois alterações posteriores não serão consideradas.

#### Sql para captura das vendas e parcelas

Exemplo de SQL (Oracle):

```sql
-- noinspection SqlNoDataSourceInspectionForFile

-- consulta os pedidos
select t2.*
from (select rownum AS "rn", t1.*
      from (select "TX".*
            from (select "CAB"."NUNOTA"  as "ORDER_ID",
                         "CAB"."NUMNOTA" as "ORDER_NUMBER",
                         "CAB"."DTNEG"   as "DATE",
                         "CAB"."VLRNOTA" as "GROSS_VALUE"
                  from "TGFCAB" CAB
                           inner join "TGFFIN" FIN on "CAB"."NUNOTA" = "FIN"."NUNOTA"
                           inner join "TGFTIT" TIT on "FIN"."CODTIPTIT" = "TIT"."CODTIPTIT"
                           inner join "TGFTEF" TEF
                                      on "TEF"."NUFIN" = "FIN"."NUFIN" and "TEF"."DESDOBRAMENTO" = "FIN"."DESDOBRAMENTO"
                  where "CAB"."CODEMP" = 1
                    and "CAB"."DTNEG" = TO_DATE('20241101', 'YYYYMMDD')
                    and "CAB"."CODTIPOPER" in (1100, 1102)
                  group by "CAB"."NUNOTA", "CAB"."NUMNOTA", "CAB"."DTNEG", "CAB"."VLRNOTA") TX) t1) t2
where t2."rn" between 1 and 2500

-- Recupera os títulos financeiros dos pedidos e agrupa por formas de pagamento, formando as vendas e suas parcelas
select "CAB"."NUNOTA"        as "ORDER_ID",
       "TIT"."FISCAL"        as "ACQUIRER_NAME",
       "TEF"."BANDEIRA"      as "BRAND_NAME",
       "TEF"."NUMNSU"        as "NSU",
       "TEF"."AUTORIZACAO"   as "AUTHORIZATION_CODE",
       "TEF"."NUFIN"         as "INSTALLMENT_ID",
       "TEF"."VLRTRANSACAO"  as "GROSS_VALUE",
       "TIT"."CODTIPTIT"     as "PAYMENT_METHOD_ID",
       "TIT"."DESCRTIPTIT"   as "PAYMENT_METHOD_NAME",
       "TIT"."SUBTIPOVENDA"  as "PAYMENT_METHOD_TYPE",
       "FIN"."CODPARC"       as "PARTNER_ID",
       "FIN"."DESDOBRAMENTO" as "UNFOLDING",
       "FIN"."DTVENC"        as "PAYMENT_FORECAST",
       "FIN"."CARTAODESC"    as "COMMISSION_VALUE"
from "TGFCAB" CAB
         inner join "TGFFIN" FIN on "CAB"."NUNOTA" = "FIN"."NUNOTA"
         inner join "TGFTIT" TIT on "FIN"."CODTIPTIT" = "TIT"."CODTIPTIT"
         inner join "TGFTEF" TEF on "TEF"."NUFIN" = "FIN"."NUFIN" and "TEF"."DESDOBRAMENTO" = "FIN"."DESDOBRAMENTO"
where "CAB"."NUNOTA" in (68684)
```

Exemplo de SQL (Sql Server):

```sql
-- noinspection SqlNoDataSourceInspectionForFile

-- Consulta os pedidos
SELECT TOP 2500 [TX].*
FROM (
    SELECT
    [CAB].[NUNOTA] AS [ORDER_ID], [CAB].[NUMNOTA] AS [ORDER_NUMBER], [CAB].[DTNEG] AS [DATE], [CAB].[VLRNOTA] AS [GROSS_VALUE]
    FROM
    [TGFCAB] AS [CAB]
    INNER JOIN
    [TGFFIN] AS [FIN] ON [CAB].[NUNOTA] = [FIN].[NUNOTA]
    INNER JOIN
    [TGFTIT] AS [TIT] ON [FIN].[CODTIPTIT] = [TIT].[CODTIPTIT]
    INNER JOIN
    [TGFTEF] AS [TEF] ON [TEF].[NUFIN] = [FIN].[NUFIN]
    AND [TEF].[DESDOBRAMENTO] = [FIN].[DESDOBRAMENTO]
    WHERE
    [CAB].[CODEMP] = 1
    AND [CAB].[DTNEG] = '20250201'
    AND [CAB].[CODTIPOPER] IN (
    3200, 3201, 3202, 3203, 3206, 3208, 3209, 3220, 3230
    )
    GROUP BY
    [CAB].[NUNOTA], [CAB].[NUMNOTA], [CAB].[DTNEG], [CAB].[VLRNOTA]
    ) AS [TX];

-- Recupera os títulos financeiros dos pedidos e agrupa por formas de pagamento, formando as vendas e suas parcelas
SELECT
    [TEF].[BANDEIRA] AS [BRAND_NAME], [TIT].[FISCAL] AS [ACQUIRER_NAME], [CAB].[NUNOTA] AS [ORDER_ID], [TEF].[NUMNSU] AS [NSU], [TEF].[AUTORIZACAO] AS [AUTHORIZATION_CODE], [TEF].[NUFIN] AS [INSTALLMENT_ID], [TEF].[VLRTRANSACAO] AS [GROSS_VALUE], [TIT].[CODTIPTIT] AS [PAYMENT_METHOD_ID], [TIT].[DESCRTIPTIT] AS [PAYMENT_METHOD_NAME], [TIT].[SUBTIPOVENDA] AS [PAYMENT_METHOD_TYPE], [FIN].[CODPARC] AS [PARTNER_ID], [FIN].[DESDOBRAMENTO] AS [UNFOLDING], [FIN].[DTVENC] AS [PAYMENT_FORECAST], [FIN].[CARTAODESC] AS [COMMISSION_VALUE]
FROM
    [TGFCAB] AS [CAB]
    INNER JOIN
    [TGFFIN] AS [FIN]
ON [CAB].[NUNOTA] = [FIN].[NUNOTA]
    INNER JOIN
    [TGFTIT] AS [TIT] ON [FIN].[CODTIPTIT] = [TIT].[CODTIPTIT]
    INNER JOIN
    [TGFTEF] AS [TEF] ON [TEF].[NUFIN] = [FIN].[NUFIN]
    AND [TEF].[DESDOBRAMENTO] = [FIN].[DESDOBRAMENTO]
WHERE
    [CAB].[NUNOTA] IN (1
    , 2
    , 3);

```

> Nao é possivel realizar a mesma consulta usando o serviço `LoadRecord`.

### 11. **Captura de Empresas e Lojas**

Para capturar as empresas e suas respectivas lojas no sistema Sankhya.

#### **1. Captura das Empresas**

No Sankhya, para ser considerada uma **empresa**, o valor de `CODEMPMATRIZ` deve ser igual ao de `CODEMP`. Ou seja, a
matriz é identificada quando ela mesma é a sua própria matriz.

##### **Consulta para buscar apenas empresas (matrizes):**

```sql
-- noinspection SqlNoDataSourceInspectionForFile

SELECT "EMP"."CODEMP",
       "EMP"."RAZAOSOCIAL",
       "EMP"."CGC",
       "EMP"."CODEMPMATRIZ"
FROM "TSIEMP" EMP
WHERE "EMP"."CODEMP" = "EMP"."CODEMPMATRIZ";
```

##### **Exemplo de retorno:**

| CODEMP | NAME           | NUMBER         | CODEMPMATRIZ |
|--------|----------------|----------------|--------------|
| 5      | EMPRESA 1 LTDA | 10491094000100 | 5            |

#### **2. Captura das Lojas**

As lojas são identificadas pelo campo `CODEMPMATRIZ`, que aponta para o código da empresa matriz à qual elas pertencem.
Para capturar todas as lojas de uma determinada empresa, basta utilizar o valor de `CODEMP` da matriz como filtro no
campo `CODEMPMATRIZ`.

##### **Consulta para buscar lojas de uma empresa específica:**

```sql
-- noinspection SqlNoDataSourceInspectionForFile

SELECT "EMP"."CODEMP",
       "EMP"."RAZAOSOCIAL",
       "EMP"."CGC",
       "EMP"."CODEMPMATRIZ"
FROM "TSIEMP" EMP
WHERE "EMP"."CODEMPMATRIZ" = 5;
```

##### **Exemplo de retorno para `CODEMPMATRIZ = 5`:**

| CODEMP | NAME        | NUMBER         | CODEMPMATRIZ |
|--------|-------------|----------------|--------------|
| 10     | LOJA 1 LTDA | 10491094000101 | 5            |
| 22     | LOJA 2 LTDA | 10491094000102 | 5            |

## Recebimento de Baixas das Parcelas do Teia Card e Envio para o Sankhya

![integracao_01-b](./assets/integracao-01-b.png)

## Permissões para o usuário

É necessário conceder permissões ao usuário criado na integração

Pesquise por `Acesso`, depois selecione o módulo `Movimentação Financeira`

![financeiro-modulo.png](./assets/financeiro-modulo.png)

Selecione o usuário criado na integração, ex: `Netunna`, selecione as permissões e clique em `Finalizar`

![financeiro-permissoes.png](./assets/financeiro-permissoes.png)

## Serviço Baixa FinanceiroSP (Baixar Titulo)

Para realizar a baixa, é o utilizado o serviço `BaixaFinanceiroSP.baixarTitulo` via API Sankhya.
[documentação Sankhya](https://developer.sankhya.com.br/reference/mapeamento-de-servi%C3%A7o#lista-de-servi%C3%A7os-%C3%BAteis).

```text
Serviço: BaixaFinanceiroSP.baixarTitulo
Tela: Movimentação Financeira
Botão: Baixar
```

Exemplo:

```js
data = {
    "serviceName": "BaixaFinanceiroSP.baixarTitulo",
    "requestBody": {
        "dadosBaixa": {
            "dtbaixa": "02/11/2024",
            "nufin": 274376,
            //...
            "valoresBaixa": {
                "tipoJuros": "I",
                "tipoMulta": "I",
                "taxaAdm": 2.48,
                "vlrDesconto": 0,
                "vlrCalculado": 105.85,
                "vlrDesdob": 108.33,
                "vlrDespesasCartorio": 0,
                "vlrJuros": 0,
                "vlrMulta": 0,
                "vlrTotal": 105.85,
                "vlrMultaNeg": 0,
                "vlrJurosNeg": 0,
                "jurosLib": 0,
                "multaLib": 0,
                "vlrMoeda": 0,
                "vlrVarCambial": 0
            },
            "dadosBancarios": {
                "codConta": 13,
                "codLancamento": 1,
                "numDocumento": 664,
                "codTipTit": 61,
                "vlrMoedaBaixa": 0,
                "contaParaCaixaAberto": 0,
                "historico": "Sk9BTw=="
            },
            "dadosAdicionais": {
                "codEmpresa": 3,
                "codTipoOperacao": 1500
            },
            //...
        }
    }
}

```

Assim que a parcela é liquidada — ou seja, quando o pagamento é realizado pela adquirente ou identificado no banco,
ela é recebida via API do Teia Card e, em seguida, enviada via API para baixa no Sankhya.

![integracao-02.png](./assets/integracao-02.png)

### 1. **Tipo de operação da baixa**

Para realizar a baixa, é necessário configurar o valor do campo `codTipoOperacao` na API.

```js
data = {
    "serviceName": "BaixaFinanceiroSP.baixarTitulo",
    "requestBody": {
        "dadosBaixa": {
            //...
            "dadosAdicionais": {
                "codEmpresa": 5,
                "codTipoOperacao": 14
            },
            // ...
        }
    }
}
```

[documentação Sankhya](https://ajuda.sankhya.com.br/hc/pt-br/articles/360044599934-Baixa-Financeira-pela-Central-de-Notas)

![img.png](./assets/sankhya-baixa-01.png)

Ex: `14`, `1500`, `4300`

```dotenv
SANKHYA_SETTLEMENT_OPERATION_TYPE_ID=4300
```

### 2. **Atualização de Taxa Aministrativa**

O Sankhya não permite realizar a baixa de um título quando há divergência na taxa administrativa cobrada pelo cartão em
relação à taxa inicialmente registrada.
Por exemplo, se no Sankhya foi registrada uma taxa administrativa de `4,70`, mas o Teia Card recebeu da adquirente um
valor de `4,75`, será necessário atualizar a taxa antes de efetuar a baixa do título.
Para realizar essa atualização, utilizamos o serviço `DatasetSP.save`.

```json
{
  "serviceName": "DatasetSP.save",
  "requestBody": {
    "entityName": "Financeiro",
    "standAlone": false,
    "fields": [
      "CARTAODESC"
    ],
    "records": [
      {
        "pk": {
          "NUFIN": 323267
        },
        "values": {
          "0": 4.75
        }
      }
    ]
  }
}
```

### 3. **Liberação de limites**

A partir da versão `4.31` da Sankhya, o desconto de um título passou a exigir a liberação de limite. Para permitir a
baixa sem passar por esse processo, é necessário configurar a liberação de limites como `full` para o usuário no
evento
`29`.

![liberação de limite](./assets/finance-limit.jpg)

### 4. **Valor Bruto da parcela**

O Sankhya não permite realizar a baixa caso seja informado um valor bruto diferente do registrado.

Por exemplo, se no Sankhya o valor bruto do título (parcela) foi registrado como `70,59` (parcela 8), mas o Teia Card
recebeu da adquirente o valor bruto de `70,66` (parcela 8), existe um a diferença entre os valores.
Essa diferença será informada como `vlrJuros` quando o valor recebido for maior ou como `vlrDesconto` quando for menor.

| Parcela | Sankhya | Teia Card | Diferença |
|---------|---------|-----------|-----------|
| 1       | 70,63   | 70,62     | 0,01      |
| 2       | 70,63   | 70,62     | 0,01      |
| 3       | 70,63   | 70,62     | 0,01      |
| 4       | 70,63   | 70,62     | 0,01      |
| 5       | 70,63   | 70,62     | 0,01      |
| 6       | 70,63   | 70,62     | 0,01      |
| 7       | 70,63   | 70,62     | 0,01      |
| 8       | 70,59   | 70,66     | 0,07      |

### Exemplo

```dotenv
# Usuário Netunna
SANKHYA_USERNAME=integracao.nnsankhya@netunna.com.br
# Codigo de Baixa
SANKHYA_SETTLEMENT_OPERATION_TYPE_ID=4300
# Codigo de vendas (TOP)
SANKHYA_SALE_OPERATION_TYPE_ID=3200,3201,3202
# Banco de dados Sankhya
SANKHYA_DB_CONNECTION=sqlsrv

SANKHYA_DAYS_TOLERANCE=3
SANKHYA_SALE_GROSS_VALUE_TOLERANCE=0.05
SANKHYA_INSTALLMENT_GROSS_VALUE_TOLERANCE=0.11
# Token Produção
SANKHYA_TOKEN=db4axxxx-6ff9-41ac-a919-0c033a3aaa12
# Token Homologação
#SANKHYA_TOKEN=c72axxxx-f5dc-4485-a5a1-b7684exxxeae
```

---

# Dados com baixa qualidade

Quando os dados estão corretamente preenchidos, é possível enviá-los via API para o Teia Card. Dessa forma, quando o
Teia Card retorna as parcelas baixadas, conseguimos identificar as parcelas e suas respectivas vendas através de um
identificador previamente enviado.

![integracao-02.png](./assets/integracao-02.png)

Quando os dados da venda são enviados, é possível compará-los com as informações fornecidas pela adquirente. Essa
comparação permite identificar divergências entre os dois conjuntos de dados, garantindo maior precisão no controle e na
validação das transações realizadas.

![integracao-06-a.png](./assets/integracao-06-a.png)

Quando não for possível enviar a venda ao Teia Card, a transação será recebida pela adquirente e disponibilizada via API
apenas após a liquidação da parcela. Nesse cenário, será necessário adotar métodos alternativos para identificar a
venda, já que ela não terá um identificador previamente enviado pelo Integrador.

![integracao-03.png](./assets/integracao-03.png)

## Identificação alternativa como último recurso

Esse processo deve ser utilizado apenas como último recurso. É essencial priorizar a correção dos problemas que
impedem o envio adequado das vendas, garantindo que o fluxo padrão seja mantido e evitando a necessidade de
identificação manual ou baseada em critérios alternativos.

O não envio das vendas ao Teia Card inviabiliza a comparação precisa com os dados fornecidos pela adquirente, o que
reduz a eficácia na identificação de divergências entre os sistemas.

![integracao-06-b.png](./assets/integracao-06-b.png)

Isso aumenta o risco de discrepâncias não
detectadas, que podem ser cobradas a mais pela adquirente. Caso o cliente perceba uma cobrança indevida, com base nas
informações do ERP, ele pode solicitar o reembolso de todo o valor pago a mais. Isso demonstra que a ferramenta cumpriu
seu papel ao permitir a identificação desses problemas, proporcionando ao cliente a oportunidade de recuperar valores
cobrados indevidamente, o que é um benefício significativo.

## Motivos para uma venda não ser enviada ao Teia Card.

Existem alguns motivos pelos quais uma venda pode não ser enviada:

![integracao-04.png](./assets/integracao-04.png)

1. Bandeira não permitida. [consulte a lista](#brand-list)
2. Adquirente não permitida. [consulte a lista](#acquire-list)
3. Tipo de Serviço não permitido. [consulte a lista](#service-type-list)
4. Meio de captura não permitido. [consulte a lista](#capture-method-list)
5. Nsu ausente.

## Critérios para encontrar uma venda não identificada previamente

Será utilizado um conjunto de informações essenciais para o processamento, incluindo: **valor bruto**, **NSU**, **data
da venda** e **código de autorização**. Esses dados são fundamentais para identificar as transações analisadas.

Para avaliar as transações, serão aplicados os seguintes critérios de tolerância, que determinam os limites aceitáveis
para discrepâncias ou variações nos dados. Esses critérios permite que os processos atendam aos padrões definidos,
facilitando a identificação.

![integracao-05.png](./assets/integracao-05.png)

### 1. **Dias de tolerância**

Número de dias para tolerância de data na conciliação:
> Exemplo: Uma venda realizada em 01/10 foi registrada no Sankhya apenas em 02/10. Como a diferença é de apenas um dia e
> está dentro da tolerância permitida, essa venda seria corretamente identificada.

```dotenv
SANKHYA_DAYS_TOLERANCE=3
```

### 2. **Tolerância de valor bruto da venda**

Margem de tolerância no valor bruto da venda.

> Exemplo: Em uma venda de 21,34 registrada na Sankhya, a adquirente informou o valor de 21,36. Como essa diferença
> está dentro da tolerância definida, a venda seria reconhecida e conciliada corretamente.

```dotenv
SANKHYA_SALE_GROSS_VALUE_TOLERANCE=0.05
```

### 3. **Tolerância de valor bruto da parcela**

Margem de tolerância no valor bruto da parcela. Em vendas parceladas em até 12x, os centavos são incluídos na
primeira parcela:

> Exemplo: Para uma venda de 565,00 dividida em 8 parcelas, a Sankhya e a adquirente aplicaram regras diferentes para
> arredondamento dos centavos. Isso gerou uma pequena diferença de 0,07 centavos em uma das parcelas, causada pelas
> variações nas regras de cálculo usadas por cada sistema.

| Parcela | Sankhya | Teia Card | Diferença |
|---------|---------|-----------|-----------|
| 1       | 70,63   | 70,62     | 0,01      |
| 2       | 70,63   | 70,62     | 0,01      |
| 3       | 70,63   | 70,62     | 0,01      |
| 4       | 70,63   | 70,62     | 0,01      |
| 5       | 70,63   | 70,62     | 0,01      |
| 6       | 70,63   | 70,62     | 0,01      |
| 7       | 70,63   | 70,62     | 0,01      |
| 8       | 70,59   | 70,66     | 0,07      |

```dotenv
SANKHYA_INSTALLMENT_GROSS_VALUE_TOLERANCE=0.11
```

### Exemplo

```dotenv
SANKHYA_DAYS_TOLERANCE=3
SANKHYA_SALE_GROSS_VALUE_TOLERANCE=0.05
SANKHYA_INSTALLMENT_GROSS_VALUE_TOLERANCE=0.11
```

