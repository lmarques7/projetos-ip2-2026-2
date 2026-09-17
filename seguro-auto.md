# Sistema SeguroAuto

## Descrição

Sistema para gestão de cotação e emissão de apólices de seguro veicular, controlando veículos, segurados, coberturas contratadas e sinistros abertos. O sistema deve calcular o prêmio do seguro e a franquia com base no valor de mercado real do veículo, e ajustar o prêmio de acordo com o histórico de sinistros do segurado (sistema de bônus/malus).

Como diferencial, o valor de mercado de cada veículo é obtido automaticamente através da Tabela FIPE, consultada via API pública, eliminando a necessidade de o operador digitar manualmente o valor do bem segurado.

## Requisitos Funcionais

### 1. Cadastro de Veículos e Segurados
- REQ01: Cadastrar segurados (nome, CPF, histórico de sinistros — inicialmente zero)
- REQ02: Cadastrar veículos (placa, marca, modelo, ano), consultando o valor de mercado atual via **Tabela FIPE (BrasilAPI)**
- REQ03: Atualizar periodicamente o valor FIPE armazenado de um veículo sob demanda do operador

### 2. Coberturas
- REQ04: Implementar herança para tipos de cobertura: Básica (danos a terceiros), Completa (colisão + terceiros + roubo) e Terceiros (somente danos a terceiros)
- REQ05: Definir percentual de franquia e percentual do prêmio sobre o valor FIPE para cada tipo de cobertura

### 3. Apólices
- REQ06: Emitir apólice vinculando segurado, veículo e cobertura escolhida, com vigência de 12 meses
- REQ07: Calcular o prêmio da apólice como percentual do valor FIPE do veículo, ajustado pelo histórico de sinistros do segurado (bônus para quem não tem sinistro, malus para quem tem)
- REQ08: Calcular o valor da franquia da apólice como percentual do valor FIPE

### 4. Sinistros
- REQ09: Abrir sinistro vinculado a uma apólice ativa, com data, descrição e valor estimado do dano
- REQ10: Encerrar sinistro, registrando o valor efetivamente pago pela seguradora
- REQ11: Atualizar automaticamente o histórico de sinistros do segurado ao encerrar um sinistro

### 5. Relatórios
- REQ12: Relatório de apólices ativas por tipo de cobertura, exportável em CSV
- REQ13: Relatório de sinistralidade (total pago em sinistros / total arrecadado em prêmios) por período
- REQ14: Listar apólices que vencem nos próximos 30 dias

### 6. Regras e Restrições
- REQ15: **Bloquear** a abertura de sinistro para uma apólice fora do prazo de vigência (vencida)
- REQ16: **Não permitir** que um mesmo veículo possua duas apólices ativas simultaneamente
- REQ17: **Validar** que o valor estimado do dano em um sinistro não exceda o valor FIPE do veículo na data de abertura
- REQ18: **Bloquear** a emissão de cobertura "Básica" para veículos com mais de 15 anos de fabricação sem vistoria prévia registrada
- REQ19: **Garantir** que o cálculo de prêmio utilize sempre o valor FIPE mais recente consultado para o veículo
- REQ20: **Impedir** o encerramento de um sinistro sem o registro do valor efetivamente pago

## Possíveis APIs/Bibliotecas

JavaFX, Tabela FIPE via BrasilAPI (https://brasilapi.com.br/docs#tag/FIPE — pública e sem chave), `java.net.http.HttpClient`, Jackson/Gson, Java Time API, JUnit.

**Requisito bônus (opcional, fora da contagem oficial):** gerar a apólice como página HTML estática, com o detalhamento de cobertura, franquia e valor FIPE utilizado, aberta automaticamente no navegador via `Desktop.getDesktop().browse()`.
