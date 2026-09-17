# Sistema EcoColeta

## Descrição

Sistema para gestão de uma cooperativa de reciclagem, organizando cooperados (catadores), pontos de coleta seletiva, rotas de coleta e a pesagem/classificação dos materiais recolhidos. O sistema deve calcular a remuneração de cada cooperado proporcionalmente ao peso e tipo de material que efetivamente coletou.

Como diferencial, o cadastro de pontos de coleta é validado e enriquecido automaticamente a partir do CEP informado, usando uma API pública brasileira, completando bairro, cidade e UF sem digitação manual, e os relatórios agrupam os pontos de coleta por município usando dados oficiais do IBGE.

## Requisitos Funcionais

### 1. Cadastro de Cooperados e Pontos de Coleta
- REQ01: Cadastrar cooperados (nome, CPF, data de ingresso na cooperativa)
- REQ02: Cadastrar pontos de coleta informando apenas o CEP, completando automaticamente bairro/cidade/UF via **BrasilAPI (CEP)**
- REQ03: Implementar herança para tipos de material reciclável: Papel, Plástico, Vidro e Metal, cada um com preço de referência por quilo

### 2. Rotas de Coleta
- REQ04: Criar uma coleta (rota) vinculando um ponto de coleta, uma data e uma equipe de cooperados
- REQ05: Registrar múltiplos cooperados (agregação) participando de uma mesma coleta

### 3. Pesagem e Classificação
- REQ06: Registrar pesagens (composição) por coleta, informando material e peso em kg
- REQ07: Calcular o peso total coletado por tipo de material em uma coleta

### 4. Remuneração
- REQ08: Calcular a remuneração de cada cooperado participante de uma coleta, dividindo o valor total apurado (peso × preço/kg) proporcionalmente entre os cooperados da equipe
- REQ09: Consultar o histórico de remuneração acumulada de um cooperado por período

### 5. Relatórios e Integração Municipal
- REQ10: Relatório de volume coletado por município (usando dados de localidades do **IBGE** para agrupar os pontos de coleta), exportável em CSV
- REQ11: Relatório de eficiência das rotas (peso total / número de cooperados envolvidos)
- REQ12: Ranking dos cooperados com maior volume coletado no mês

### 6. Regras e Restrições
- REQ13: **Bloquear** o cadastro de ponto de coleta caso o CEP informado seja inválido ou não retorne endereço na consulta à API
- REQ14: **Impedir** a criação de uma coleta sem nenhum cooperado vinculado à equipe
- REQ15: **Não permitir** o registro de pesagem em uma coleta que já tenha sido encerrada
- REQ16: **Validar** que toda coleta atinja um peso mínimo total (configurável) para ser considerada "eficiente" nos relatórios
- REQ17: **Bloquear exclusão** de cooperados que possuam remunerações já calculadas e registradas
- REQ18: **Garantir** que o peso de cada pesagem seja maior que zero
- REQ19: **Validar** que um cooperado não participe de duas coletas com datas/horários sobrepostos
- REQ20: **Bloquear** o encerramento de uma coleta que não possua nenhuma pesagem registrada

## Possíveis APIs/Bibliotecas

JavaFX, BrasilAPI (https://brasilapi.com.br — CEP, pública e sem chave), API de Localidades do IBGE (https://servicodados.ibge.gov.br/api/docs/localidades), `java.net.http.HttpClient`, Jackson/Gson, Apache POI (CSV/planilhas), JUnit.

**Requisito bônus (opcional, fora da contagem oficial):** gerar um mapa/relatório HTML estático listando os pontos de coleta agrupados por município, aberto automaticamente no navegador via `Desktop.getDesktop().browse()`.
