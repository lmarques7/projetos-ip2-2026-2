# [2026.2] Projetos de Programação Orientada a Objetos

Este repositório contém os requisitos para os projetos da disciplina de **Introdução à Programação 2 / POO**, na UFRPE. Cada projeto foi desenhado para exercitar conceitos de Herança, Polimorfismo, Composição/Agregação e regras de validação complexas.

Além disso, haverá a necessidade de construção dos projetos com base na arquitetura em camadas, seguindo princípios de boas práticas de programação (Clean Code & Clean Architecture). O cronograma de entregas do projeto será combinado diretamente com o professor.

**IMPORTANTE** todos os projetos possuem ao menos um requisito de integração com uma **API pública/base de dados aberta real** (Banco Central, IBGE, BrasilAPI, ANVISA, dados nutricionais, previsão do tempo, catálogos abertos, etc.), além da interface gráfica obrigatória. O objetivo é aproximar os sistemas de cenários reais de consumo de dados externos, incluindo o tratamento de falhas/indisponibilidade dessas fontes.

## Índice de Projetos

| Projeto | Resumo do Projeto | Dado/API Externa | Arquivo |
|---------|-------------------|-------------------|---------|
| **AgroTech** | Gestão de propriedade rural, plantio e colheita com alertas climáticos para aplicação de insumos. | Open-Meteo (previsão do tempo) | [agrotech.md](agrotech.md) |
| **ImportFX** | Loja de produtos importados com cálculo de preço final via cotação de câmbio real do dia. | PTAX — Banco Central do Brasil | [import-fx.md](import-fx.md) |
| **GameVault** | Locadora/loja de jogos eletrônicos com catálogo enriquecido por dados reais de jogos. | FreeToGame API | [game-vault.md](game-vault.md) |
| **EcoColeta** | Cooperativa de reciclagem com rotas de coleta, pesagem e remuneração proporcional dos cooperados. | BrasilAPI (CEP) + IBGE (localidades) | [eco-coleta.md](eco-coleta.md) |
| **ParkAdventure** | Gestão de parque temático/aquático com controle de fila virtual e precificação dinâmica em feriados. | BrasilAPI (Feriados Nacionais) | [park-adventure.md](park-adventure.md) |
| **MobiCidade** | Bicicletas e patinetes compartilhados com bloqueio de corridas sob condições climáticas adversas. | Open-Meteo (previsão do tempo) | [mobi-cidade.md](mobi-cidade.md) |
| **FarmaSmart** | Farmácia com controle de receitas controladas e validação de princípio ativo em base sanitária aberta. | openFDA (Drug Label API) | [farma-smart.md](farma-smart.md) |
| **PetAdote** | ONG de adoção de animais com acompanhamento pós-adoção e informações reais de raças. | TheDogAPI / TheCatAPI | [pet-adote.md](pet-adote.md) |
| **AlimentaBem** | Banco de alimentos com distribuição por prioridade de vencimento (FIFO) e cálculo nutricional agregado. | USDA FoodData Central + BrasilAPI (CEP) | [alimenta-bem.md](alimenta-bem.md) |
| **SeguroAuto** | Cotação e gestão de apólices de seguro veicular com prêmio calculado sobre o valor FIPE do veículo. | Tabela FIPE via BrasilAPI | [seguro-auto.md](seguro-auto.md) |

---

## Observações para Implementação

- **Padrão de Requisitos**: Cada sistema conta com ~20-25 requisitos funcionais detalhados, além de um requisito bônus opcional de exportação de relatório em HTML.
- **Validações**: A seção de **Regras e Restrições** deve ser priorizada no desenvolvimento da lógica de negócio.
- **Tecnologias**: Recomenda-se o uso de Java 17+ e JavaFX para as interfaces gráficas. Se possível, usar JUnit para testes unitários.
- **Consumo de APIs públicas**: recomenda-se `java.net.http.HttpClient` (nativo do Java, sem dependências externas) para as chamadas HTTP, e Jackson ou Gson para o parsing das respostas JSON. A maioria das APIs indicadas não exige chave de acesso; algumas (ex.: TheDogAPI/TheCatAPI, USDA FoodData Central) exigem apenas um cadastro gratuito e instantâneo — nesses casos, a chave nunca deve ser versionada no repositório (usar variável de ambiente ou arquivo de configuração ignorado pelo Git).
- **Resiliência a falhas externas**: nenhuma funcionalidade essencial do sistema pode travar por indisponibilidade da API pública consumida — cada projeto deve manter um cache local do último dado obtido e sinalizar claramente ao usuário quando estiver exibindo dados desatualizados.
- **Desafio opcional (bônus)**: cada projeto inclui um requisito bônus, fora da contagem oficial, propondo a geração de um pequeno relatório em página HTML estática, aberta automaticamente no navegador padrão via `Desktop.getDesktop().browse()` — uma primeira aproximação com HTML/CSS sem exigir conhecimento de frameworks web.
