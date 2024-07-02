# Controle de Cargas - Dashboard Power BI

# Visão Geral do Dashboard

![Captura de tela 2024-07-02 083931](https://github.com/Luiz-campos3/GitHub-projetosBI/assets/174439712/d5a54198-b3e3-44d9-a0d4-e7abfdb5bf96)



## Este repositório contém um dashboard desenvolvido no Power BI para o controle de cargas. O dashboard fornece uma visão abrangente e detalhada dos processos de gerenciamento de cargas, incluindo o status das notas fiscais, prazos de entrega e sazonalidade.

## Funcionalidades


## Filtros Interativos: 
### Permitem que o usuário selecione o ano, mês, datas específicas, código da filial, número da carga, número da nota fiscal (NF) e rota da carga para uma análise detalhada.
##


## Quantidade de Notas Fiscais (NF):
![Captura de tela 2024-07-02 094308](https://github.com/Luiz-campos3/GitHub-projetosBI/assets/174439712/6facfc4d-ab6d-4e7e-883e-999bee3e8380)
### Exibe a quantidade total de notas fiscais processadas. 
### Formula DAX(MEDIDA)
``` 
QTD NF = DISTINCTCOUNT(Nome_Tabela[Nº NF])
```

## Quantidade de NF Devolvidas: 
![Captura de tela 2024-07-02 094319](https://github.com/Luiz-campos3/GitHub-projetosBI/assets/174439712/1eda6565-4106-40bd-918f-86c0c68358db)
### Mostra o número total de notas fiscais devolvidas.
### Formula DAX(MEDIDA)
``` 
Qtd NF Dev = 
VAR NotasFiscaisFiltradas =
    FILTER (
        DISTINCT ( Nome_Tabela[Nº NF Dev] ),  -- Filtra apenas valores distintos
        NOT ISBLANK (Nome_Tabela[Nº NF Dev])  -- Exclui valores em branco
    )
RETURN
    COUNTROWS ( NotasFiscaisFiltradas )
```
##


## Quantidade de Cargas:
![Captura de tela 2024-07-02 094331](https://github.com/Luiz-campos3/GitHub-projetosBI/assets/174439712/dab8ce69-27f3-42f3-8c0e-766318cdde74)
### Apresenta o número total de cargas registradas.
### Formula DAX(MEDIDA)
```
QTDCargas = DISTINCTCOUNT(Nome_Tabela[Nº Carga])
```
##


# Status das Notas Fiscais: 
![Captura de tela 2024-07-02 095959](https://github.com/Luiz-campos3/GitHub-projetosBI/assets/174439712/358e62cd-ee4b-4db8-8d81-b00bc0cdce92)
### Gráficos de rosca que mostram a distribuição das notas fiscais com carga, sem carga e sem informações de Retorno.
#### * Valores: QTD NF
#### * Legenda: Coluna_Status_NF
##


## Status de Fechamento das Cargas: 
![Captura de tela 2024-07-02 100611](https://github.com/Luiz-campos3/GitHub-projetosBI/assets/174439712/af9eaaaf-a46c-48d7-b90d-ff273a945d06)
#### * Valores: QTD Carga
#### * Legenda: Coluna_Status_Fechamento
### Visualização do status de fechamento das cargas, indicando se estão dentro ou fora do prazo.
##


## Sazonalidade: 
![Captura de tela 2024-07-02 100812](https://github.com/Luiz-campos3/GitHub-projetosBI/assets/174439712/5910a212-8411-41bb-b71a-bffa993dc8ae)
### Gráfico de linha que exibe a quantidade de cargas fechadas e abertas ao longo do tempo através da Data do Documento.
#### * Eixo X: Data_Documento(Data Faturamento)
#### * Eixo Y: QTD Aberto
#### * Eixo Y: QTD Fechado
### Formula DAX(MEDIDA)
```
QtdAberto = 
CALCULATE(
    DISTINCTCOUNT(Nome_Tabela[Nº Carga]),
    FILTER(
        Nome_Tabela,
        Nome_Tabela[Status Carga] = "Aberto"
    )
)
```
```
QtdFechado = 
CALCULATE(
    DISTINCTCOUNT(Nome_Tabela[Nº Carga]),
    FILTER(
        Nome_Tabela,
        Nome_Tabela[Status Carga] = "Fechado"
    )
)
```
##


## Status das Cargas: 
![Captura de tela 2024-07-02 101401](https://github.com/Luiz-campos3/GitHub-projetosBI/assets/174439712/24348651-d680-444d-bc9f-e2331aa75b59)
### Gráfico de rosca mostrando a quantidade de cargas fechadas, abertas, canceladas e pendentes.
#### * Valores: Prametro
#### * Legenda: Coluna_Status_Carga
##


## Prazo das Cargas: 
![Captura de tela 2024-07-02 101420](https://github.com/Luiz-campos3/GitHub-projetosBI/assets/174439712/c21b70d9-5d71-43e0-8258-3d26357c390b)
### Este gráfico exibe a proporção de cargas entregues dentro e fora do prazo, considerando apenas aquelas com status "aberto" ou "fechado". Além disso, o gráfico mostra a média de dias de entrega. A seleção dos dados pode ser ajustada através de um parâmetro abaixo do gráfico, permitindo uma análise detalhada e personalizada dos prazos de entrega.
#### * Valores: Parametro
#### * Legenda: Coluna_Status_Fechamento
### Formula DAX(Coluna)
```
Dias_Carga = 
VAR DataDocumento = Controle_Carga[Data Documento]
VAR DataFechamento = 
    IF(
        ISBLANK(Controle_Carga[Data Fechamento_T5]),
        TODAY(),  -- Se DataFechamento for em branco, usa a data de hoje
        Controle_Carga[Data Fechamento_T5]
    )
RETURN
    DATEDIFF(DataDocumento, DataFechamento, DAY)

```
```
StatusFechamento = 
IF(
    Controle_Carga[Dias_Carga] <= 3,
    "Dentro do Prazo",
    "Fora do Prazo"
)
```
### Parametro
![Captura de tela 2024-07-02 101434](https://github.com/Luiz-campos3/GitHub-projetosBI/assets/174439712/95bf647e-9207-4018-8edf-7ed9aa5037c9)

### Nele voce pode selecionar 4 medidas 
* QTD Aberto
* Média de dias QTD Aberto
* QTD Fechado
* Média de dias QTD Fechado
##


## Tipo de Prestação de Contas: 
![Captura de tela 2024-07-02 101447](https://github.com/Luiz-campos3/GitHub-projetosBI/assets/174439712/5a30a4fa-2ffc-4fec-b2d5-ccb11c77d19c)
### Gráfico de barras que apresenta a quantidade de entregas concluídas, prestações de contas pendentes e conferências financeiras.
### Formula DAX(Medida)
```
EntregaConcluida = 
COUNTROWS(
    FILTER(
        Nome_Tabela,
       Nome_Tabela[Check Entrega] = "Sim"
    )
)
```
```
PrestracaoEntrega = 
COUNTROWS(
    FILTER(
        Nome_Tabela,
       Nome_Tabela[Check Canhoto] = "Sim"
    )
)
```
```
ConferidoFinanceiro = 
COUNTROWS(
    FILTER(
        Nome_Tabela,
       Nome_Tabela[Check Conferido] = "Sim"
    )
)
```
##
## Requisitos
### Para visualizar e interagir com este dashboard, você precisará do Power BI Desktop. Você pode baixá-lo aqui.
https://www.microsoft.com/pt-br/download/details.aspx?id=58494
## Uso
* Faça o download do arquivo Controle_Carga.pbix
* Abra o arquivo Controle_Carga.pbix no Power BI Desktop.
* Utilize os filtros interativos para explorar os dados conforme necessário.
## Contribuição 
### Se você quiser contribuir para este projeto, sinta-se à vontade para fazer um fork do repositório e enviar um pull request com suas melhorias ou sugestões.

