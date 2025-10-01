# Configuração

## Geral

As automações da REST API podem ser configuradas via deep links personalizados. Isso é ótimo para casos de uso onde você gostaria de integrar o Health Auto Export com sua própria plataforma ou de terceiros

## FAQ
**Como obter os dados mais detalhados?**

Para simplificar, se você quiser os dados mais detalhados disponíveis, deve usar as configurações padrão para `Period` e `Aggregation`. Isso fornecerá detalhes segundo a segundo para Heart Rate.

## Campos

### URL

A URL de destino da requisição POST. Tanto `http` quanto `https` são permitidos.

### Name

O nome de exibição da automação.

### Headers

Os cabeçalhos `Content-Type` são definidos automaticamente para o formato de exportação selecionado no aplicativo ou especificado pelo esquema de URL:

- `Content-Type:application/json` é definido automaticamente para formato JSON

- `Content-Type:multipart/form-data` é definido automaticamente para formato CSV

### Format

O formato JSON exporta um único objeto com campos `metrics` e `workouts` contendo os dados solicitados.

O formato CSV exporta dois arquivos, um para métricas de saúde e outro para exercícios.

### Period

- `Default`: Sincroniza dados do dia completo anterior mais dados até a data e hora atuais. Esta sincronização será executada várias vezes ao dia.

- `Since Last Sync`: A cada sincronização, exporta todos os dados desde a última vez que a exportação foi executada até a data e hora atuais. Esta sincronização será executada várias vezes ao dia.

- `Today`: Sincroniza todos os dados da data atual até a hora atual. Esta sincronização será executada várias vezes ao dia.

- `Yesterday`: Sincroniza todos os dados do dia completo anterior. Esta sincronização será executada apenas uma vez ao dia.

- `Previous 7 Days`: Sincroniza dados dos sete dias completos anteriores. Esta sincronização será executada apenas uma vez ao dia.

### Interval

Os seguintes valores são aceitos para definir o intervalo de agregação dos dados a serem sincronizados. No momento, é possível usar apenas o intervalo `days` com exportações CSV.

- `Default`: Quando `Default` é definido e exportando em formato JSON, o Health Auto Export usa o intervalo de segundos para a métrica Heart Rate e minutos para outras métricas. Isso fornece a saída mais detalhada para dados de Heart Rate. Ao exportar para CSV, o intervalo `days` é usado para todas as métricas.

- `minutes`: Os dados são agregados minuto a minuto

- `days`: Os dados são agregados dia a dia

- `weeks`: Os dados são agregados semana a semana

- `months`: Os dados são agregados mês a mês

- `years`: Os dados são agregados ano a ano

## Deep Linking

Uma Exportação de API pode ser configurada via esquema de URL. Isso significa que se você quiser criar um serviço que se integre ao Health Auto Export, seus usuários podem clicar em um link para preencher os campos da Exportação de API, desde que tenham o Health Auto Export instalado.

### Esquema de URL

A configuração dos campos da Exportação de API é feita através do seguinte esquema de URL:

`com.HealthExport://automation`

### Parâmetros

`url (obrigatório)`:

Insira uma URL válida para enviar os dados exportados.

`name (opcional)`

Insira um nome de exibição para a automação. O nome padrão é "New Automation."

`headers (opcional)`:

Insira cabeçalhos como uma lista separada por vírgulas de pares chave-valor. Os cabeçalhos `Content-Type` são aplicados automaticamente de acordo com o tipo de formato selecionado (ex. `application/json`)

`format (opcional)`:

As opções de formato de exportação são CSV e JSON. O padrão é JSON.

- `json (Padrão)`
- `csv`

`datatype (opcional)`:

Define o tipo de dados que será exportado pela automação:

- `healthmetrics (Padrão)`
- `workouts`
- `symptoms`
- `workouts`

`aggregatedata (opcional)`

Exportar dados agregados ou desagregados (quando disponível):

- `true (Padrão)`
- `false`

`period (opcional)`:

Os seguintes valores são aceitos para definir o período de tempo dos dados a serem sincronizados. O padrão é `none`.

- `none (Padrão)`
- `lastsync (Since Last Sync)`
- `today (Today)`
- `yesterday (Yesterday)`
- `previous7Days (Previous 7 Days)`

`interval (opcional)`:

Os seguintes valores são aceitos para definir o intervalo de agregação dos dados a serem sincronizados. No momento, é possível usar apenas o intervalo `days` com exportações CSV.

- `none (Padrão)`
- `minutes`
- `hours`
- `days`
- `weeks`
- `months`
- `years`

`syncinterval (opcional)`:

Os seguintes valores são aceitos para o intervalo de cadência de sincronização:

- `minutes (Padrão)`
- `hours`
- `days`
- `weeks`

`syncquantity (opcional)`:

Os seguintes intervalos de valores são aceitáveis correspondendo ao intervalo associado. O valor `min` é usado como valor padrão para cada caso:

- `minutes`: `min: 5`, `max: 60`
- `hours`: `min: 1`, `max: 24`
- `days`: `min: 1`, `max: 7`
- `weeks`: `min: 1`, `max: 1`

`enabled (opcional)`:

Define uma automação como habilitada/desabilitada quando importada. Uma automação será desabilitada por padrão, dando ao usuário controle total sobre seus dados.

- `false (Padrão)`
- `true`

### Exemplo Completo

`com.HealthExport://automation?url=https://example.com/user?123&headers=Authorization,123456&name=Test%20Link&format=json&period=lastsync&interval=minutes&enabled=true&datatype=workouts&aggregatedata=false&syncinterval=hours&syncquantity=5`


