# Sistema AgroTech

## Descrição

Sistema para gestão de uma propriedade rural, organizando talhões (áreas de plantio), culturas cultivadas, aplicação de insumos agrícolas e colheitas. O sistema deve controlar o ciclo de vida de cada plantio, desde o preparo do talhão até a colheita, garantindo que prazos de carência de defensivos e condições climáticas sejam respeitados antes de liberar operações críticas.

Como diferencial, o sistema integra dados climáticos reais via API pública para apoiar decisões de manejo: alertar sobre risco de geada, bloquear aplicações de defensivo quando há previsão de chuva forte (que lavaria o produto aplicado) e estimar necessidade de irrigação com base na previsão dos próximos dias. Relatórios de produtividade por talhão e por cultura auxiliam o produtor a planejar safras futuras.

## Requisitos Funcionais

### 1. Cadastro de Propriedade e Talhões
- REQ01: Cadastrar talhões (identificação, área em hectares, coordenadas geográficas — latitude/longitude, tipo de solo)
- REQ02: Cadastrar culturas (nome, ciclo médio em dias, época de plantio recomendada)
- REQ03: Implementar herança para tipos de insumo: Fertilizante, Defensivo e Semente, cada um com atributos próprios (ex.: Defensivo possui período de carência em dias)

### 2. Plantio e Manejo
- REQ04: Registrar plantio vinculando talhão, cultura e data de início
- REQ05: Registrar aplicações de insumo (composição) por plantio, incluindo data e quantidade aplicada
- REQ06: Consultar histórico completo de aplicações de um plantio

### 3. Integração Climática (API Pública)
- REQ07: Consultar a previsão do tempo dos próximos 3 dias para as coordenadas do talhão usando a API pública **Open-Meteo** (sem necessidade de chave de acesso)
- REQ08: Exibir na interface um indicador visual de risco climático (chuva forte, geada, tempo seco) para cada talhão com plantio ativo
- REQ09: Manter em cache local a última previsão obtida por talhão, sinalizando ao usuário quando os dados exibidos não são atuais por indisponibilidade da API

### 4. Colheita e Produtividade
- REQ10: Registrar colheita de um plantio (data, quantidade colhida em kg/toneladas)
- REQ11: Calcular produtividade por hectare (quantidade colhida / área do talhão)
- REQ12: Encerrar automaticamente o plantio após o registro de colheita, liberando o talhão para novo ciclo

### 5. Relatórios
- REQ13: Relatório de produtividade por cultura, comparando safras diferentes, exportável em CSV
- REQ14: Relatório de insumos aplicados por talhão em um período, com custo total estimado
- REQ15: Painel (dashboard) com os talhões que tiveram alerta climático nos últimos 7 dias

### 6. Regras e Restrições
- REQ16: **Bloquear** o registro de aplicação de defensivo caso a previsão climática indique chuva acima de um limiar configurável (ex.: 10mm) nas 24 horas seguintes
- REQ17: **Impedir** o registro de colheita antes de decorrido o período de carência do último defensivo aplicado no plantio
- REQ18: **Não permitir** o cadastro de um novo plantio em talhão que já possua plantio ativo (sem colheita registrada)
- REQ19: **Validar** que a data de colheita seja posterior à data de plantio
- REQ20: **Garantir** que toda aplicação de insumo esteja vinculada a um plantio ativo (não encerrado)
- REQ21: **Bloquear exclusão** de um plantio que já possua aplicações de insumo registradas
- REQ22: **Alertar** (sem bloquear) o usuário quando a previsão indicar risco de geada para talhões com cultura sensível ao frio cadastrada como tal

## Possíveis APIs/Bibliotecas

JavaFX, Open-Meteo API (previsão do tempo — https://open-meteo.com, gratuita e sem autenticação), `java.net.http.HttpClient`, Jackson/Gson (parsing JSON), Java Time API, JFreeChart/JavaFX Charts (gráficos de produtividade), JUnit.

**Requisito bônus (opcional, fora da contagem oficial):** gerar um relatório de produtividade em uma página HTML estática (com tabela e gráfico simples em CSS/JS) e abri-la automaticamente no navegador padrão do sistema operacional via `Desktop.getDesktop().browse()`.
