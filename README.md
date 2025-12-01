# Trabalho Final: Proposta de Experimento para TCC

## Sumário

> [Título](#a-adoção-real-de-sistemas-de-verificação-de-vunerabilidade-em-projetos-javascript---pj001)

1. [Contexto e problema](#contexto-e-problema)
1. [Objetivos e questões (Goal / Question / Metric)](#objetivos-e-questões-goal--question--metric)
1. [Escopo e contexto do experimento](#escopo-e-contexto-do-experimento)
1. [Stakeholders e impacto esperado](#stakeholders-e-impacto-esperado)
1. [Riscos de alto nível, premissas e critérios de sucesso](#riscos-de-alto-nível-premissas-e-critérios-de-sucesso)
1. [Modelo conceitual e hipóteses](#modelo-conceitual-e-hipóteses)
1. [Variáveis, fatores, tratamentos e objetos de estudo](#variáveis-fatores-tratamentos-e-objetos-de-estudo)
1. [Desenho experimental](#desenho-experimental)
1. [População, sujeitos e amostragem](#população-sujeitos-e-amostragem)
1. [Plano de análise de dados](#plano-de-análise-de-dados)

> [Informações Adicionais](#informações-adicionais)

---

# A adoção real de sistemas de verificação de vunerabilidade em projetos JavaScript - PJ001

## Contexto e problema

### Descrição do problema / oportunidade

Nos últimos anos observou-se um aumento significativo nos ataques cibernéticos direcionados a sistemas de software, tanto de código aberto quanto comerciais, resultando em prejuízos financeiros e violações de privacidade para os usuários. Grande parte desses ataques explora vulnerabilidades conhecidas em softwares desatualizados ou mal mantidos. Em particular, no ecossistema JavaScript, a atualização tardia de dependências e pacotes é um problema recorrente: estudos indicam que aplicações Node.js levam em média 103 dias para corrigir vulnerabilidades já divulgadas conforme Mahmoud Alfadel (2022) cita em seu trabalho.

Esse atraso prolongado abre uma janela de exposição em que invasores podem explorar falhas conhecidas, evidenciando a necessidade de intervenções que acelerem o processo de correção. Assim, delineia-se uma oportunidade clara para a adoção de ferramentas automatizadas de verificação de vulnerabilidades em projetos JavaScript, de modo a identificar e sanar brechas de segurança de forma proativa.

### Contexto organizacional e técnico

Este estudo foi conduzido em um contexto de pesquisa acadêmica, utilizando dados públicos da plataforma GitHub e ferramentas computacionais para análise. No aspecto organizacional, o trabalho insere-se na iniciativa de melhorar a segurança em projetos de software open source. Do ponto de vista técnico, a investigação focou o ecossistema JavaScript/Node.js, coletando dados de repositórios GitHub por meio da API da plataforma.

Os dados brutos (por exemplo, informações sobre dependências e vulnerabilidades reportadas) foram então processados e analisados com scripts em Python, permitindo extrair métricas e insights relevantes. Por fim, os resultados foram consolidados em visualizações gráficas para embasar a discussão. Esse arranjo técnico, envolvendo coleta automatizada, processamento de dados e apresentação visual, permitiu examinar sistematicamente as práticas de gerenciamento de vulnerabilidades em projetos JavaScript de forma reproduzível e escalável.

### Trabalhos e evidências prévias

Estudos anteriores ressaltam a gravidade do problema das vulnerabilidades em open source e motivam a busca por soluções de verificação automatizada. O relatório Open Source Security and Risk Analysis 2024 (OSSRA) do Synopsys Cybersecurity Research Center (CyRC) revelou que 84% dos codebases analisados continham pelo menos uma vulnerabilidade conhecida de código aberto, e 74% dos codebases apresentavam vulnerabilidades classificadas como de alto risco. Esses números evidenciam não apenas a ubiquidade de vulnerabilidades em projetos de software, mas também a severidade considerável de muitas delas. Além da prevalência das falhas, o crescimento acelerado do volume de vulnerabilidades reportadas tem sido documentado na literatura.

Uma pesquisa abrangente conduzida por Akhavani et al. (2025), examinando tendências em múltiplos ecossistemas open source (incluindo JavaScript/Node.js), identificou um crescimento exponencial nas vulnerabilidades divulgadas, na ordem de 98% ao ano. Esse aumento anual dramático supera em muito o ritmo de crescimento do próprio ecossistema de software open source e indica que as brechas de segurança estão se acumulando cada vez mais rápido. Em conjunto, essas evidências prévias, tanto de fontes industriais (Synopsys) quanto acadêmicas (Akhavani et al.), ressaltam a urgência de adotar práticas e ferramentas mais eficazes de detecção e correção de vulnerabilidades em projetos JavaScript, motivando a realização deste estudo.

### Referencial teórico e empírico essencial

Para fundamentar o presente estudo, é necessário compreender os conceitos-chave de vulnerabilidade de software, as ferramentas de verificação de vulnerabilidades e modelos de adoção de práticas de segurança no desenvolvimento. No contexto de segurança da informação, vulnerabilidade é definida como qualquer fraqueza ou falha em componentes de software que possa ser explorada por agentes maliciosos, resultando em impacto negativo na confidencialidade, integridade ou disponibilidade do sistema. Em outras palavras, uma vulnerabilidade pode ser vista como uma porta de entrada indesejada no software – por exemplo, um erro de programação ou configuração – através da qual invasores obtêm acesso não autorizado, instalam código malicioso ou roubam dados sensíveis. Tais falhas de segurança, quando presentes em projetos de código aberto, tendem a ter alto potencial de propagação devido ao reuso extensivo de bibliotecas e componentes por diversas aplicações. Se uma biblioteca amplamente utilizada contém uma falha, todos os sistemas que dependem dela podem ficar expostos. Pesquisas mostram, por exemplo, que a presença de pacotes desatualizados no ecossistema npm está fortemente correlacionada com o aumento do risco de vulnerabilidades nos projetos dependentes. Dessa forma, manter as dependências atualizadas e corrigir prontamente as falhas conhecidas são práticas essenciais para reduzir a “defasagem técnica” e mitigar riscos de segurança.

Como resposta a esse cenário, surgem as ferramentas automatizadas de verificação de vulnerabilidades, cujo objetivo é identificar e ajudar a corrigir falhas de segurança de forma mais ágil do que seria possível manualmente. Essas ferramentas englobam distintas abordagens tecnológicas. Uma categoria importante são as ferramentas de Software Composition Analysis (SCA), voltadas a inspecionar os componentes de código aberto utilizados em um projeto. Soluções de SCA escaneiam a base de código de uma aplicação para identificar bibliotecas e dependências de terceiros, levantando um inventário (por exemplo, uma SBOM – Software Bill of Materials) de todos os componentes open source em uso. De posse dessa lista, a ferramenta verifica as versões dos componentes contra bases de dados de vulnerabilidades conhecidas (como o catálogo CVE/NVD), detectando automaticamente se alguma dependência está afetada por vulnerabilidades reportadas. Esse processo automatizado permite revelar vulnerabilidades “ocultas” nas bibliotecas importadas, que muitas vezes passam despercebidas pelos desenvolvedores. Em paralelo, outras ferramentas realizam análise estática de código (SAST) buscando padrões de implementação inseguros no próprio código-fonte da aplicação. Tanto as ferramentas de SCA quanto de SAST integram o conjunto de práticas de DevSecOps, que visam inserir controles de segurança ao longo do ciclo de desenvolvimento de software. Ao incorporar essas verificações nos pipelines de CI/CD, as equipes conseguem “mover a segurança para a esquerda”, isto é, detectar vulnerabilidades nas fases iniciais do desenvolvimento, antes que o software seja implantado em produção. Essa filosofia de integrar verificações contínuas aproveita o fato de que falhas corrigidas precocemente são mais baratas e simples de resolver, além de reduzirem significativamente a janela em que o sistema fica vulnerável a ataques.

Em resumo, o referencial teórico-empírico deste estudo apoia-se em conceitos de vulnerabilidades de software e sua dinâmica em ecossistemas de código aberto, bem como nas abordagens modernas de identificação e mitigação dessas falhas. Modelos de segurança em DevOps (DevSecOps) e práticas de continuous security fornecem o pano de fundo conceitual para entender como e por que a integração de ferramentas de verificação de vulnerabilidades pode elevar o nível de segurança em projetos JavaScript. Fundamentado por evidências recentes, como a prevalência generalizada de falhas de segurança
investor e o crescimento acelerado das mesmas, este estudo busca analisar criticamente a adoção dessas ferramentas no mundo real e avaliar em que medida elas estão suprindo a lacuna entre a detecção de vulnerabilidades e sua resolução efetiva nos projetos open source.

[Retorne ao Sumário](#sumário)

## Objetivos e questões

### Objetivo geral

Analisar a adoção de ferramentas de verificação de vulnerabilidades em projetos de código aberto JavaScript, com o propósito de avaliar sua influência na identificação e correção de vulnerabilidades, em relação à melhoria da segurança do código, do ponto de vista dos mantenedores de projetos open source, no contexto de projetos JavaScript amplamente utilizados.

### Objetivos específicos

- O1: Quantificar e caracterizar o nível de adoção de ferramentas de verificação de vulnerabilidades em projetos open source JavaScript (extensão de uso, tipos de ferramentas utilizadas e evolução ao longo do tempo).

- O2: Avaliar o impacto da adoção dessas ferramentas nos resultados de segurança dos projetos (por exemplo, na redução do número de vulnerabilidades e agilidade de correção).

- O3: Identificar fatores e características dos projetos que influenciam ou estão associados à adoção de ferramentas de verificação de vulnerabilidades (como tamanho, popularidade e atividade do projeto).

- O4: Avaliar como as ferramentas de verificação de vulnerabilidades se integram ao processo de desenvolvimento e a percepção dos desenvolvedores em relação ao seu uso (usabilidade, aceitação e efeitos no fluxo de trabalho).

### Questões de pesquisa

Para cada objetivo específico foram definidas questões de pesquisa que orientam a medição. As questões são organizadas por objetivo:
O1 – Adoção de ferramentas nos projetos:

- Q1: Qual a porcentagem de projetos open source em JavaScript que adotam pelo menos uma ferramenta de verificação de vulnerabilidades?

- Q2: Quais tipos de ferramentas de verificação de vulnerabilidades são mais frequentemente adotados (por exemplo, ferramentas de análise estática de código vs. análise de dependências)?

- Q3: Como a adoção dessas ferramentas tem evoluído ao longo do tempo nos projetos (ex.: crescimento anual de adoção)?
O2 – Impacto na segurança do projeto:

- Q4: Projetos que adotam ferramentas de verificação de vulnerabilidades apresentam menos vulnerabilidades não corrigidas em comparação a projetos que não as adotam?

- Q5: O uso dessas ferramentas está associado a um tempo mais rápido de correção de vulnerabilidades identificadas (menor Mean Time to Remediate - MTTR)?

- Q6: A adoção das ferramentas correlaciona-se com melhores indicadores globais de segurança no projeto (por exemplo, maior pontuação de segurança ou maior taxa de vulnerabilidades resolvidas)?
O3 – Fatores influentes na adoção:

- Q7: Projetos de maior porte (e.g., mais colaboradores ou código) apresentam taxas de adoção de ferramentas de vulnerabilidades superiores às de projetos menores?

- Q8: Projetos mais populares (por exemplo, com mais estrelas no repositório ou muito utilizados como dependência) tendem a adotar essas ferramentas com maior frequência?

- Q9: Características de desenvolvimento, como alta atividade (frequência de commits) ou maior maturidade (idade do projeto), influenciam a probabilidade de adoção das ferramentas?
O4 – Integração e percepção no processo:

- Q10: De que forma as ferramentas de verificação de vulnerabilidades estão integradas ao processo de desenvolvimento (por exemplo, integradas em pipelines CI/CD automáticos)?

- Q11: Qual é o nível de satisfação ou aceitação dos desenvolvedores com o uso dessas ferramentas nos projetos?

- Q12: O uso das ferramentas introduz overhead ou ruído no processo de desenvolvimento (por exemplo, aumento no tempo de build ou geração de falsos positivos que exigem triagem)?

### Metricas e rastreabilidade GQM

Para responder a cada questão de pesquisa, foram definidas métricas mensuráveis, garantindo a rastreabilidade segundo o modelo GQM (Goal/Question/Metric). A Tabela 1 apresenta o mapeamento dos objetivos específicos, questões e métricas correspondentes, incluindo uma breve descrição de cada métrica e sua unidade de medida. 

**Tabela 1 – Estrutura GQM com objetivos, questões e métricas.**

| **Objetivo específico**                | **Questão de pesquisa**                                   | **Métrica**                                  | **Descrição da métrica**                                                                                                                                                                                                                                       | **Unidade**           |
| -------------------------------------- | --------------------------------------------------------- | -------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| O1. Adoção de ferramentas nos projetos | Q1. Porcentagem de projetos que adotam alguma ferramenta? | **Taxa de adoção geral (%)**                 | Porcentagem de projetos no conjunto analisado que possuem pelo menos uma ferramenta de verificação de vulnerabilidades implementada em seu fluxo de desenvolvimento.                                                                                           | %                     |
| O1                                     | Q1                                                        | **Ferramentas por projeto (n)**              | Número médio de ferramentas de verificação de vulnerabilidades utilizadas por projeto (por exemplo, projetos que empregam tanto ferramenta de análise estática quanto de análise de dependências).                                                             | Contagem (n)          |
| O1                                     | Q2. Tipos de ferramentas mais adotados?                   | **Taxa de adoção de SAST (%)**               | Percentual de projetos que adotam pelo menos uma ferramenta de análise estática de segurança (SAST – *Static Application Security Testing*, e.g., CodeQL, SonarQube) para detecção de vulnerabilidades em código fonte.                                        | %                     |
| O1                                     | Q2                                                        | **Taxa de adoção de SCA (%)**                | Percentual de projetos que adotam pelo menos uma ferramenta de análise de composição de software (SCA – *Software Composition Analysis*, e.g., scanners de dependências como Dependabot ou Snyk) visando identificar vulnerabilidades em bibliotecas externas. | %                     |
| O1                                     | Q3. Evolução da adoção ao longo do tempo?                 | **Crescimento anual de adoção (%)**          | Incremento percentual médio na taxa de adoção de ferramentas de vulnerabilidades a cada ano (tendência anual), calculado comparando-se a porcentagem de projetos adotantes em anos consecutivos.                                                               | % ao ano              |
| O1                                     | Q3                                                        | **Tempo até adoção (meses)**                 | Tempo médio (em meses) desde o início de um projeto até a primeira adoção de uma ferramenta de verificação de vulnerabilidades. Indicador de quão cedo, em média, projetos incorporam ferramentas de segurança.                                                | Meses                 |
| O2. Impacto na segurança do projeto    | Q4. Menos vulnerabilidades nos projetos com ferramentas?  | **Contagem de vulnerabilidades abertas**     | Número de vulnerabilidades de segurança conhecidas presentes no projeto que permanecem sem correção (incluindo vulnerabilidades em dependências externas) em determinado momento.                                                                              | Contagem (n)          |
| O2                                     | Q4                                                        | **Densidade de vulnerabilidades (por KLOC)** | Quantidade de vulnerabilidades presentes por mil linhas de código do projeto (KLOC = *kilo* linhas de código). Métrica normalizada para comparar projetos de tamanhos diferentes.                                                                              | Vulnerabilidades/KLOC |
| O2                                     | Q5. Redução do tempo de correção de vulnerabilidades?     | **MTTR de vulnerabilidades (dias)**          | *Mean Time to Remediate*: tempo médio em dias para corrigir vulnerabilidades após sua identificação. Mede a agilidade da equipe em resolver falhas de segurança reportadas.                                                                                    | Dias                  |
| O2                                     | Q5                                                        | **MTTU de dependências (dias)**              | *Mean Time to Update*: tempo médio em dias para atualizar dependências vulneráveis após o lançamento de uma versão corrigida. Indica rapidez na atualização de componentes externos frente a falhas divulgadas.                                                | Dias                  |
| O2                                     | Q6. Melhores indicadores globais de segurança?            | **Pontuação de segurança (0–10)**            | Indicador agregado da postura de segurança do projeto, em escala de 0 a 10 (por exemplo, pontuação calculada por uma ferramenta como OpenSSF Scorecard que avalia práticas de segurança do repositório).                                                       | Score (0–10)          |
| O2                                     | Q6                                                        | **Taxa de resolução de alertas (%)**         | Porcentagem dos alertas de vulnerabilidades identificadas que foram resolvidos/corrigidos pelo projeto em um determinado período. Reflete a eficácia em endereçar os problemas apontados pelas ferramentas.                                                    | %                     |
| O3. Fatores influentes na adoção       | Q7. Porte do projeto influencia adoção?                   | **Número de contribuidores ativos**          | Quantidade de desenvolvedores contribuindo ativamente no projeto (por exemplo, número de contribuidores nos últimos 12 meses). Usado como indicador de porte da comunidade do projeto.                                                                         | Contagem (n)          |
| O3                                     | Q7                                                        | **Tamanho do código (KLOC)**                 | Tamanho do código do projeto em milhares de linhas de código (KLOC). Representa a escala do projeto, podendo influenciar a necessidade de ferramentas de verificação de vulnerabilidades.                                                                      | KLOC                  |
| O3                                     | Q8. Popularidade influencia adoção?                       | **Popularidade (estrelas no GitHub)**        | Número de estrelas (*stars*) no repositório GitHub do projeto, indicando popularidade e visibilidade na comunidade.                                                                                                                                            | Contagem (n)          |
| O3                                     | Q8                                                        | **Número de dependentes**                    | Quantidade de outros projetos ou pacotes que dependem deste projeto (conforme registros do gerenciador de pacotes, e.g., npm). Indica o nível de utilização do projeto por terceiros.                                                                          | Contagem (n)          |
| O3                                     | Q9. Atividade/maturidade influencia adoção?               | **Frequência de commits (mês)**              | Média de commits realizados por mês no repositório do projeto, refletindo o grau de atividade de desenvolvimento contínua.                                                                                                                                     | Commits/mês           |
| O3                                     | Q9                                                        | **Idade do projeto (anos)**                  | Tempo de existência do projeto em anos (calculado desde o primeiro commit ou primeira versão). Projetos mais antigos podem ter diferentes padrões de adoção de ferramentas em comparação a projetos mais novos.                                                | Anos                  |
| O4. Integração e percepção no processo | Q10. Integração das ferramentas no processo (CI/CD)?      | **Integração em CI (sim/não)**               | Indica se o projeto possui integração da ferramenta de verificação de vulnerabilidades em sua pipeline de Integração Contínua (CI). Valor booleano (sim=1, não=0) considerado para cálculo de percentual de projetos com integração automatizada.              | Binário (sim/não)     |
| O4                                     | Q10                                                       | **Frequência de scans (por mês)**            | Frequência média de execução das varreduras de vulnerabilidades no projeto, por exemplo, número de execuções da ferramenta (builds ou *jobs* de CI) por mês que incluem verificação de segurança.                                                              | Scans/mês             |
| O4                                     | Q11. Satisfação dos desenvolvedores?                      | **Índice de satisfação (1–5)**               | Grau médio de satisfação dos desenvolvedores com as ferramentas, obtido via questionário em escala Likert de 1 (muito insatisfeito) a 5 (muito satisfeito). Mede a percepção de utilidade e usabilidade da ferramenta pelos contribuidores.                    | Escala 1–5            |
| O4                                     | Q11                                                       | **Taxa de continuidade de uso (%)**          | Percentual de projetos que continuam usando a ferramenta após um determinado período (por ex., ≥ 6 meses após adoção inicial). Reflete a retenção da ferramenta no processo do projeto (não abandono após tentativa inicial).                                  | %                     |
| O4                                     | Q12. Overhead ou ruído no processo?                       | **Overhead no tempo de build (%)**           | Aumento percentual no tempo de build/CI do projeto atribuível à execução da ferramenta de verificação de vulnerabilidades, comparando a duração dos builds com e sem a ferramenta habilitada.                                                                  | %                     |
| O4                                     | Q12                                                       | **Taxa de falsos positivos (%)**             | Proporção dos alertas de vulnerabilidade gerados pela ferramenta que são falsos positivos, ou seja, não correspondem a vulnerabilidades reais no código. Essa taxa elevada pode causar ruído e retrabalho para a equipe de desenvolvimento.                    | %                     |

**Tabela 2 – Definição detalhada das métricas propostas.**

| **Métrica**                                  | **Definição**                                                                                                                                                                                                                                                                                                                                                                      | **Unidade**             | **Fonte**                                                                                                |
| -------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------- | -------------------------------------------------------------------------------------------------------- |
| **Taxa de adoção geral** (%)                 | Porcentagem de projetos analisados que utilizam pelo menos uma ferramenta de verificação de vulnerabilidades em seu processo de desenvolvimento (identificada pela presença de configurações ou workflows de segurança no repositório).                                                                                                                                            | Percentual (%)          | Repositórios do projeto (configurações de CI)                                                            |
| **Ferramentas por projeto** (n)              | Número médio de ferramentas de verificação de vulnerabilidades utilizadas por projeto. Por exemplo, se um projeto utiliza tanto uma ferramenta de SAST quanto uma de SCA, conta-se duas ferramentas.                                                                                                                                                                               | Contagem (n)            | Dados dos repositórios (análise de arquivos de configuração de pipeline)                                 |
| **Taxa de adoção de SAST** (%)               | Porcentagem de projetos que adotam pelo menos uma ferramenta de *Static Application Security Testing* (análise estática de código) para identificar vulnerabilidades em código fonte (ex.: CodeQL no GitHub Actions).                                                                                                                                                              | Percentual (%)          | OpenSSF Scorecard (métrica "SAST" identifica uso de análise estática)                                    |
| **Taxa de adoção de SCA** (%)                | Porcentagem de projetos que adotam pelo menos uma ferramenta de *Software Composition Analysis* para detectar vulnerabilidades em dependências externas (ex.: Dependabot, Snyk).                                                                                                                                                                                                   | Percentual (%)          | OpenSSF Scorecard (métrica "Dependency-Update-Tool")                                                     |
| **Crescimento anual de adoção** (%)          | Taxa de crescimento anual na adoção de ferramentas de vulnerabilidades, calculada comparando-se a porcentagem de projetos adotantes em anos consecutivos. Indica a tendência temporal (aceleração ou estagnação) da adoção no ecossistema ao longo dos anos.                                                                                                                       | Variação % ao ano       | Histórico de commits/configurações dos projetos (análise temporal de adoção)                             |
| **Tempo até adoção** (meses)                 | Tempo médio decorrido desde o início de um projeto (por ex., primeiro commit) até a primeira adoção de uma ferramenta de verificação de vulnerabilidades. Mede quão cedo, em média, os projetos incorporam práticas automatizadas de detecção de vulnerabilidades.                                                                                                                 | Meses                   | Histórico dos repositórios (data de criação vs. data de introdução da ferramenta)                        |
| **Contagem de vulnerabilidades abertas** (n) | Número de vulnerabilidades conhecidas que permanecem sem correção no projeto, incluindo vulnerabilidades identificadas no próprio código e em dependências do projeto. Este valor pode ser obtido a partir de bases de dados de vulnerabilidades (por exemplo, advisories no GitHub ou OSV) ou relatórios das ferramentas de scan.                                                 | Contagem (n)            | Banco de dados de vulnerabilidades (OSV, CVE) / Relatórios de scanner                                    |
| **Densidade de vulnerabilidades** (por KLOC) | Número de vulnerabilidades por mil linhas de código do projeto (KLOC). É uma métrica normalizada de vulnerabilidades, permitindo comparar projetos de tamanhos diferentes quanto à concentração de falhas de segurança. Por exemplo, 0,5 vulnerabilidades/KLOC indica meia vulnerabilidade em média a cada mil linhas.                                                             | Vulnerabilidades/KLOC   | Métrica de qualidade de código adaptada para segurança                                                   |
| **MTTR de vulnerabilidades** (dias)          | *Mean Time to Remediate*: tempo médio necessário para corrigir vulnerabilidades após sua identificação. Reflete a eficiência da equipe em endereçar problemas de segurança; valores menores (ex.: <30 dias) indicam resposta rápida na correção.                                                                                                                                   | Dias                    | Literatura de DevSecOps (métrica de responsividade)                                                      |
| **MTTU de dependências** (dias)              | *Mean Time to Update*: tempo médio para atualizar uma dependência vulnerável após a disponibilização de uma versão corrigida. Mede a agilidade em aplicar patches em componentes de terceiros. É calculado a partir de dados de versão de dependências e notificações de vulnerabilidades.                                                                                         | Dias                    | Adaptado de Rahman *et al.* (2024) – métricas de atualização de dependências                             |
| **Pontuação de segurança** (0–10)            | Score numérico que resume a postura de segurança do projeto, tipicamente em escala de 0 a 10. Pode ser obtido por ferramentas automatizadas como o OpenSSF Scorecard, que avalia vários critérios de boas práticas de segurança e atribui uma pontuação agregada. Uma pontuação 10 indica excelência nas práticas de segurança, enquanto 0 indica ausência dessas práticas.        | Score (0–10)            | OpenSSF Scorecard (conjunto de *checks* de segurança)                                                    |
| **Taxa de resolução de alertas** (%)         | Porcentagem dos alertas de vulnerabilidade gerados pelas ferramentas que foram efetivamente resolvidos (corrigidos ou marcados como mitigados) pelo projeto em determinado período. Por exemplo, se de 20 alertas gerados 15 foram resolvidos, a taxa é 75%. Essa métrica indica a eficácia e diligência da equipe em tratar as issues de segurança encontradas.                   | Percentual (%)          | Relatórios das ferramentas / rastreamento de issues no repositório (marcadas como resolvidas)            |
| **Número de contribuidores ativos** (n)      | Quantidade de contribuidores que participaram ativamente do projeto num determinado período (por exemplo, número de pessoas que enviaram commits nos últimos 12 meses). Indica o tamanho da comunidade desenvolvedora ativa do projeto, o que pode influenciar capacidade de adoção de novas práticas.                                                                             | Contagem (n)            | Dados do repositório (histórico de commits e contribuidores)                                             |
| **Tamanho do código** (KLOC)                 | Tamanho do código-fonte do projeto medido em milhares de linhas de código (KLOC). Inclui código fonte principal (excluindo dependências externas). Projetos maiores (mais KLOC) podem ter maior superfície propensa a vulnerabilidades e demandar mais ferramentas de verificação.                                                                                                 | KLOC                    | Repositório do projeto (contagem de linhas de código)                                                    |
| **Popularidade (estrelas no GitHub)** (n)    | Número de "estrelas" (favoritações) que o repositório do projeto possui no GitHub. É um indicador de popularidade e visibilidade: projetos muito estrelados tendem a ter mais atenção da comunidade, podendo influenciar a adoção de medidas de segurança pela exposição que possuem.                                                                                              | Contagem (n)            | GitHub (dados de estrelas do repositório)                                                                |
| **Número de dependentes** (n)                | Quantidade de outros projetos ou pacotes que declararam dependência deste projeto (conforme registros em gerenciadores de pacote, como npm). Indica a importância do projeto no ecossistema: um número alto sugere que falhas de segurança nesse projeto podem ter amplo impacto, possivelmente motivando maior adoção de ferramentas de proteção.                                 | Contagem (n)            | Dados do ecossistema (ex.: API do npm/Libraries.io)                                                      |
| **Frequência de commits** (mês)              | Frequência média de commits no repositório do projeto, medida em número de commits por mês. Calculada dividindo-se o total de commits num período pelo número de meses desse período. Representa o quão ativo é o desenvolvimento; projetos mais ativos podem integrar ferramentas de forma contínua ao fluxo de trabalho.                                                         | Commits/mês             | Repositório do projeto (histórico de commits)                                                            |
| **Idade do projeto** (anos)                  | Tempo de existência do projeto em anos desde seu início (primeiro commit ou primeira versão lançada). Projetos mais antigos podem ter adotado ferramentas gradualmente ou enfrentar desafios legados, enquanto projetos novos podem iniciar já com práticas modernas.                                                                                                              | Anos                    | Repositório do projeto (data de início)                                                                  |
| **Integração em CI** (sim/não)               | Indica se o projeto possui integração da ferramenta de verificação de vulnerabilidades em sua pipeline de Integração Contínua (CI) ou entrega contínua (CD). Valor “sim” significa que há ao menos um job ou etapa automatizada de scan de segurança no processo de CI do projeto (por exemplo, um workflow do GitHub Actions dedicado a análise de vulnerabilidades).             | Booleano (sim=1, não=0) | Arquivos de configuração CI (ex.: presença de GitHub Action de segurança)                                |
| **Frequência de scans** (por mês)            | Freqüência média com que as varreduras de vulnerabilidades são executadas no projeto, medida em execuções por mês. Pode ser inferida do histórico de runs no CI (por exemplo, número de builds mensais contendo etapas de scan) ou agendamentos configurados (ex.: scans diários/semanais).                                                                                        | Scans/mês               | Logs do pipeline CI / Configuração da ferramenta (agendamento de execuções)                              |
| **Índice de satisfação** (1–5)               | Grau médio de satisfação dos desenvolvedores com a ferramenta de verificação de vulnerabilidades, medido por meio de pesquisa utilizando escala Likert de 1 a 5 (1 = muito insatisfeito, 5 = muito satisfeito). Reflete a opinião dos usuários quanto à usabilidade, falsas alarmes e utilidade da ferramenta no desenvolvimento seguro.                                           | Escala Likert (1–5)     | Pesquisa com desenvolvedores do projeto (questionário)                                                   |
| **Taxa de continuidade de uso** (%)          | Porcentagem de projetos que continuam usando a ferramenta de verificação de vulnerabilidades após um certo tempo da adoção inicial (por exemplo, após 6 ou 12 meses). É calculada dividindo o número de projetos que mantiveram a ferramenta ativa pelo número total que a adotou naquele período. Indica retenção da prática de scan ao longo do tempo (não abandono) no projeto. | Percentual (%)          | Histórico do repositório (verificação de permanência ou remoção da ferramenta nas configurações)         |
| **Overhead no tempo de build** (%)           | Aumento relativo na duração do processo de build/CI do projeto devido à execução da ferramenta de scan de vulnerabilidades. Calculado comparando a duração média dos builds com a ferramenta habilitada versus builds sem a ferramenta. Um valor de 20% significa que os builds ficam 20% mais lentos em média por conta do scan.                                                  | Percentual (%)          | Pipeline CI do projeto (dados de tempos de execução dos jobs de build)                                   |
| **Taxa de falsos positivos** (%)             | Porcentagem dos alertas de vulnerabilidade emitidos pela ferramenta que são identificados como falsos positivos (ou seja, não representam de fato uma vulnerabilidade real no código). Uma taxa alta de falsos positivos (e.g., >5%) indica que a ferramenta gera muitos alarmes inválidos, o que pode desperdiçar esforço da equipe e causar *alert fatigue*.                     | Percentual (%)          | Relatórios da ferramenta e verificação manual de alertas (classificação dos alertas quanto à veracidade) |


[Retorne ao Sumário](#sumário)

## Escopo e contexto do experimento

### Escopo funcional e de processo

Este experimento abrange a avaliação da adoção de ferramentas automatizadas de verificação de vulnerabilidades no ciclo de desenvolvimento de projetos open source escritos em JavaScript. Estão incluídas tanto ferramentas de análise estática de segurança de código (SAST) quanto ferramentas de varredura de dependências e componentes (SCA), já que ambas contribuem para identificar vulnerabilidades em diferentes níveis. O foco funcional recai sobre o processo de desenvolvimento seguro: consideramos atividades como integração de scanners em pipelines de CI/CD, monitoramento de alertas de segurança gerados e ações de correção derivadas desses alertas. Como parte do escopo, serão analisados repositórios públicos no GitHub (ou plataforma similar) em que seja possível detectar a presença ou ausência dessas ferramentas e extrair métricas relacionadas. 

Serão incluídos no estudo projetos de código aberto em JavaScript que atendam a critérios como: possuir histórico de desenvolvimento ativo, utilizar um sistema de controle de versão acessível (git), e ter dados disponíveis sobre vulnerabilidades (por exemplo, via advisories ou registros de dependências). Também focaremos projetos que utilizem pelo menos uma ferramenta de verificação de vulnerabilidades ou que explicitamente não utilizem (para efeito de comparação). Estarão excluídos do escopo projetos inativos ou arquivados, projetos muito recentes (sem histórico suficiente para análise temporal) e projetos que não utilizem JavaScript como linguagem principal. Além disso, não entraremos no detalhe de outras práticas de segurança não automatizadas (por exemplo, revisões de código manuais ou auditorias externas), concentrando a análise nas ferramentas automatizadas de detecção de vulnerabilidades. Técnicas de teste dinâmico (DAST) puramente manuais ou testes de invasão também estão fora do escopo, dado o foco em ferramentas automatizadas integradas ao processo de desenvolvimento.

### Contexto do estudo

O experimento será conduzido no contexto de projetos de software open source voltados ao ecossistema JavaScript (principalmente projetos npm hospedados no GitHub). Esses projetos variam desde bibliotecas amplamente reutilizadas até aplicativos completos de código aberto. Em geral, tratam-se de organizações de desenvolvimento voluntárias ou comunitárias, nas quais os mantenedores e contribuidores assumem os papéis de desenvolvedores e gerentes do projeto. Não há uma estrutura hierárquica formal como em empresas, mas muitos projetos relevantes possuem colaboradores experientes e, em alguns casos, patrocinadores corporativos ou apoio de fundações open source. 

O tipo de organização aqui é a comunidade open source distribuída. Isso significa que os processos podem ser menos formalizados e os recursos limitados, diferindo de um ambiente corporativo. O tipo de projeto é predominantemente bibliotecas, frameworks ou ferramentas de desenvolvimento em JavaScript disponibilizadas livremente. Tais projetos frequentemente dependem de alto nível de confiança da comunidade e adoção ampla, o que torna a segurança um aspecto sensível. Experiência dos envolvidos: presume-se uma variedade de níveis de experiência – desde desenvolvedores seniores com forte conhecimento de segurança até contribuidores ocasionais com pouca familiaridade em práticas de segurança. Essa heterogeneidade pode afetar tanto a adoção das ferramentas quanto a interpretação de seus resultados. 

No contexto atual, nota-se um incentivo crescente da comunidade para melhorar a segurança dos projetos open source; iniciativas como OpenSSF Scorecard e programas de recompensas por vulnerabilidades refletem essa preocupação. No entanto, estudos recentes indicam que uma parcela significativa dos projetos ainda não adota ferramentas de segurança consistentes – por exemplo, cerca de 40% dos desenvolvedores/organizações não utilizam tecnologias-chave de segurança como SAST ou SCA. Especificamente no ecossistema npm/JavaScript, a adoção de scanners de vulnerabilidades ainda é relativamente baixa (uma análise mostrou que apenas ~3% dos projetos populares usam ferramentas SAST). Esse cenário contextualiza a importância de investigar em que condições essas ferramentas estão sendo adotadas e quais benefícios estão trazendo.

### Premissas consideradas

Para delinear o experimento, assumimos algumas premissas como verdadeiras:

- Existência de dados observáveis;
- Homogeneidade de interpretação das métricas;
- Uso intencional das ferramentas;
- Efetividade básica das ferramentas;
- Independência parcial de fatores externos;

### Restrições práticas

No planejamento e condução deste experimento, reconhecemos diversas restrições práticas:

- Tempo e cronograma;
- Acesso a dados e ferramentas;
- Recursos computacionais;
- Variabilidade das ferramentas;
- Participação dos desenvolvedores.

### Limitações previstas

Mesmo com o planejamento cuidadoso, este experimento apresenta limitações que podem afetar a generalização e validade dos resultados:

- Validade externa (generalização);
- Viés de amostra;
- Confundimento de variáveis;
- Medição imperfeita das vulnerabilidades;
- Influência de falsos positivos e negativos;
- Feedback qualitativo limitado;
- Evolução tecnológica durante o estudo;

[Retorne ao Sumário](#sumário)

## Stakeholders e impacto esperado

### Principais stakeholders

Os principais stakeholders interessados neste estudo e em seus resultados incluem:

- Mantenedores e desenvolvedores de projetos open source;
- Equipe de segurança da informação / pesquisadores de segurança;
- Usuários e integradores de software open source;
- Organizações de suporte ao open source;
- Desenvolvedores de ferramentas de segurança.

###  Interesses e expectativas dos stakeholders

Cada stakeholder mencionado possui interesses e expectativas específicos em relação ao experimento e seus possíveis resultados:

- Mantenedores/desenvolvedores de projetos;
- Equipe/pesquisadores de segurança;
- Usuários/integradores de open source;
- Organizações de suporte;
- Desenvolvedores de ferramentas de segurança.

### Impactos potenciais no processo e no produto

- Melhoria dos processos de desenvolvimento (DevSecOps);
- Ajustes de workflow pela redução de ruído;
- Maior qualidade e segurança dos produtos de software;
- Transparência e confiança;
- Identificação de pontos de melhoria nas ferramentas de verificação;
- Mudanças culturais e de prioridade;
- Possíveis impactos negativos (a serem gerenciados).

[Retorne ao Sumário](#sumário)

## Riscos de alto nível, premissas e critérios de sucesso

### Riscos de alto nível

No planejamento deste experimento, foram identificados diversos riscos de alto nível que podem ameaçar o sucesso ou a execução tranquila do estudo. Eles se dividem em riscos de natureza técnica, operacional e organizacional:

- Riscos Técnicos:
    - Qualidade dos dados coletados;
    - Falsos positivos em excesso;
    - Inconpatibilidade de ferramentas.
- Riscos Operacionais:
    - Atrasos na coleta e processamento;
    - Baixa adesao dos participantes;
    - Recursos humanos limitados.
- Riscos organacionais
    - Mudança de escopo ou prioridades;
    - Dependência de terceiros;
    - Sensibilidade e percussão dos resultados.

### Critérios globais de sucesso

Definimos a seguir os critérios globais de sucesso do experimento – condições pelas quais consideraríamos o esforço bem-sucedido (Go) ou, em caso de não atingimento, indicariam falha significativa (No-go). Esses critérios servem como checkpoints de avaliação objetiva:

- Cobertura de dados adequada;
- Validação das métricas-chaves;
- Enajamento mínimo dos participantes;
- Alinhamento dps participantes;
- Alinahmento com objetivos GQM;
- Reprodutibilidade e confiabilidade.

### Critérios de parada antecipada

Também estabelecemos critérios de parada antecipada – condições sob as quais o experimento deveria ser interrompido precocemente para evitar desperdício de esforço ou conclusões inválidas. Esses critérios funcionam como gatilhos de emergência:

- Falha cr´tica na coleta de dados;
- Amostra insufuciente ou nao representativa;
- Desvio do escopo inicial;
- Indicativos de nenhum resultado significativo;
- Recursos esgotados ou obstáculo instransponível.

[Retorne ao Sumário](#sumário)

## Modelo conceitual e hipóteses

### Modelo conceitual do experimento
Explique, em texto ou esquema, como você acredita que os fatores influenciam as respostas (por exemplo, “técnica A reduz defeitos em relação a B”).

### Hipóteses formais (H0, H1)
Formule explicitamente as hipóteses nulas e alternativas para cada questão principal, incluindo a direção esperada do efeito quando fizer sentido.

### Nível de significância e considerações de poder
Defina o nível de significância (por exemplo, α = 0,05) e comente o que se espera em termos de poder estatístico, relacionando-o ao tamanho de amostra planejado.

### Hipóteses formais (H₀, H₁)

As hipóteses serão testadas em relação às principais variáveis de resposta. Para cada métrica, temos uma hipótese nula (H₀) e uma alternativa (H₁), com direção do efeito esperada.

**Hipótese 1 – Número de vulnerabilidades conhecidas (M1)**

H₀: A média de vulnerabilidades conhecidas ativas é igual entre projetos que adotam e os que não adotam ferramentas.

H₁: Projetos que adotam ferramentas possuem menor média de vulnerabilidades conhecidas ativas.

**Hipótese 2 – Tempo médio de correção de vulnerabilidades (MTTR) (M2)**

H₀: O tempo médio de correção de vulnerabilidades não difere entre os dois grupos.

H₁: Projetos com ferramentas corrigem vulnerabilidades mais rapidamente (menor MTTR).

**Hipótese 3 – Proporção de dependências atualizadas (M3)**

H₀: Não há diferença na proporção de dependências atualizadas entre os dois grupos.

H₁: Projetos com ferramentas têm maior proporção de dependências atualizadas.

**Hipótese 4 – Taxa de aceitação de correções automatizadas (M4)**

H₀: A taxa média de aceitação de PRs automáticos é igual entre projetos com e sem ferramentas.

H₁: Projetos com ferramentas apresentam maior taxa de aceitação de correções automatizadas.

**Hipótese 5 – Tempo médio de atualização de dependências (MTTU) (M5)**

H₀: O tempo médio de atualização de dependências não varia entre os grupos.

H₁: Projetos com ferramentas atualizam dependências mais rapidamente (menor MTTU).

As hipóteses serão testadas individualmente, com ajustes para múltiplas comparações conforme necessário (por exemplo, Bonferroni ou FDR), para evitar inflar a taxa de falso positivo global.

### Nível de significância e considerações de poder

Nível de significância (α): Será adotado o valor padrão de α = 0,05, ou seja, aceitaremos até 5% de probabilidade de erro do tipo I (rejeitar uma hipótese nula verdadeira).

Poder estatístico esperado (1 - β): Esperamos obter um poder mínimo de 80%, o que implica uma probabilidade de 20% ou menos de cometer erro tipo II (falha em detectar um efeito que realmente existe). Esse nível é considerado adequado para estudos empíricos em Engenharia de Software.

Tamanho da amostra: A meta é coletar dados de pelo menos 100 repositórios em cada grupo (controle e tratamento), o que permitiria detectar efeitos de tamanho médio (Cohen's d ≈ 0,5) com poder ≥ 80%, assumindo distribuição normal aproximada e testes paramétricos. Se a distribuição não for normal, serão utilizados testes não paramétricos (Mann-Whitney, etc.), com potencial perda de poder, mas ganhos em robustez.

Análise prévia de poder: Caso seja necessário refinar o planejamento amostral, poderá ser realizada uma análise de poder a priori com base na variância estimada das métricas nos dados históricos ou em estudos similares.

[Retorne ao Sumário](#sumário)

## Variáveis, fatores, tratamentos e objetos de estudo

### Objetos de estudo

Os objetos de estudo consistem em projetos open source escritos em JavaScript, incluindo todo o ecossistema de artefatos associados. Isso abrange o código-fonte dos repositórios, seus arquivos de configuração de dependências (por exemplo, package.json e package-lock.json), quaisquer arquivos de configuração YAML relacionados a pipelines de integração contínua (especialmente aqueles configurando ferramentas de verificação de vulnerabilidades), bem como os registros de alertas de vulnerabilidades ou issues de segurança nos repositórios. Em resumo, analisamos os repositórios de código e seus metadados para observar evidências da adoção de ferramentas de verificação de vulnerabilidades (como, por exemplo, Dependabot, Snyk ou similares) e os efeitos dessa adoção nos projetos.

### Sujeitos/participantes

Não há participação direta de indivíduos via experimentação controlada; em vez disso, os “sujeitos” são os próprios projetos e, indiretamente, suas equipes de desenvolvimento (mantenedores e contribuidores da comunidade open source). Em termos práticos, cada projeto JavaScript selecionado serve como uma unidade experimental. Consideramos que as decisões tomadas pelos mantenedores (por exemplo, habilitar ou não uma ferramenta de varredura de vulnerabilidades) representam as intervenções naturais que queremos analisar. Assim, os participantes envolvidos de forma indireta são os mantenedores e contribuidores desses projetos, cujas ações e métricas (como commits de correção de falhas, tempo de resposta a alertas, etc.) serão observadas nos dados históricos. A seleção dos projetos para análise seguirá critérios de relevância (por exemplo, projetos ativos e populares o suficiente para fornecer dados significativos) e aleatoriedade na escolha dentro desses critérios, de forma a obter uma amostra representativa dos projetos JavaScript open source. Vale notar que um estudo recente analisando 500 repositórios JavaScript identificou que cerca de 46,4% dos projetos chegaram a usar a ferramenta Dependabot em algum momento. Esse dado ressalta que há uma divisão substancial na comunidade – quase metade dos projetos adota uma ferramenta automatizada de verificação de vulnerabilidades (no caso, focada em dependências), enquanto a outra metade não – justificando a definição de dois grupos distintos para comparação (adotantes vs. não adotantes).

### Variáveis independentes (fatores) e seus níveis 

O principal fator independente considerado é a adoção de ferramentas de verificação de vulnerabilidades pelo projeto. Esse fator é de natureza categórica binária, com dois níveis principais: projetos que adotam pelo menos uma ferramenta de verificação de vulnerabilidades, integrando-a em seu processo de desenvolvimento (por exemplo, habilitando scanners de dependências ou analisadores de código de segurança em sua pipeline CI); e projetos que não adotam tais ferramentas. Em outras palavras, o fator “adoção de ferramenta” possui níveis “Adota” vs. “Não Adota”. A classificação de um projeto em um nível ou outro será determinada pela presença de evidências de uso de ferramentas de varredura de vulnerabilidade – por exemplo, presença de arquivos de configuração do Dependabot ou GitHub Actions de scan, histórico de commits ou pull requests gerados por essas ferramentas, ou métricas derivadas (como o indicador Dependency-Update-Tool do OpenSSF Scorecard). Cabe ressaltar que, por se tratar de um estudo observacional, esse fator não é manipulado ativamente pelos pesquisadores, mas observado conforme ocorre naturalmente em cada projeto (fator de seleção). Caso pertinente, poderemos considerar subníveis adicionais de acordo com o tipo específico de ferramenta adotada (por exemplo: adoção de scanner de dependências vs. scanner de código estático), porém o delineamento principal trata a adoção de qualquer ferramenta de verificação de vulnerabilidades de forma agregada (sim/não) para fins de comparação primária.

### Tratamentos

As condições experimentais correspondem aos grupos de comparação definidos pelo fator acima. Teremos dois grupos principais de análise: Grupo de Controle, composto por projetos que não adotam nenhuma ferramenta de verificação de vulnerabilidades automatizada; e Grupo de Tratamento, composto por projetos que adotam ao menos uma dessas ferramentas em seu processo. Esses grupos representam, respectivamente, a condição “ausência da intervenção” vs. “presença da intervenção” (onde a intervenção de interesse é a introdução de ferramentas de verificação de vulnerabilidades no fluxo de desenvolvimento do projeto). Dessa forma, trata-se de um delineamento comparativo entre projetos sem e com adoção da ferramenta. Cada projeto dentro do grupo de tratamento pode ter adotado possivelmente diferentes ferramentas ou práticas, mas todos compartilham a característica de utilizar algum mecanismo automatizado de detecção de vulnerabilidades. Já os projetos do grupo de controle não possuem nenhuma integração desse tipo. Tratamentos combinados: Como há apenas um fator principal (adoção da ferramenta, binário), não há combinações multifatoriais de tratamentos neste estudo; há simplesmente as duas condições mutuamente exclusivas mencionadas. A tabela a seguir resume o fator e seus níveis (tratamentos):

| **Fator (Variável Independente)**                       | **Níveis (Tratamentos)**                                                                 |
| ------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| Adoção de ferramenta de verificação de vulnerabilidades | - Não adota ferramenta (Grupo de Controle) <br> - Adota ferramenta (Grupo de Tratamento) |

### Variáveis dependentes 

As variáveis dependentes representam as medidas de resultado que buscamos avaliar para quantificar os efeitos (se houver) da adoção das ferramentas de verificação de vulnerabilidades. Cada variável de resposta está ligada a aspectos de segurança do projeto, sob a hipótese de que projetos que utilizam scanners de vulnerabilidades apresentarão melhores indicadores. Abaixo definimos claramente cada métrica de resultado considerada:

- Contage de vulnerabilidades conhecidas não resolvidas
- Tempo médio de correção de vulnerabilidade (MTTR)
- Tempo medio de atualização de dependências (MTTU)
- Taxa de aceitação de correções automáticas
- Proporção de dependências atualizadas

Todas as variáveis dependentes acima são quantitativas. Elas fornecem medidas objetivas de segurança e manutenção do projeto. A escolha dessas medidas se baseia em literatura recente que correlaciona a adoção de práticas/ferramentas de segurança com melhorias mensuráveis nesses indicadores. Assim, se a ferramenta de verificação de vulnerabilidades estiver trazendo benefícios, esperamos observar (a) contagens menores de vulnerabilidades abertas, (b) tempos reduzidos de correção/atualização, e (c) altas taxas de aplicação de correções sugeridas, em comparação ao grupo de controle.

Para garantir que as comparações entre os grupos sejam justas e que os resultados obtidos sejam de fato devido à adoção da ferramenta (e não a outros fatores), identificamos uma série de variáveis de controle. Estas são características dos projetos que serão mantidas constantes, equilibradas entre os grupos ou explicitamente incluídas nos modelos de análise para neutralizar sua influência. Também poderemos usar algumas delas como critérios de bloqueio, estratificando a análise por categorias. As principais variáveis de controle propostas são:

- Tamanho do projeto
- Idade do projeto (maturidade)
- Núemro de contribuidores ativos
- Popularidade do projeto
- Dompínio/tipo do projeto
- Complexidade das dependências

Em adição às variáveis acima, que podemos medir diretamente, adotaremos estratégias de bloqueio quando possível. Por exemplo, poderemos dividir a amostra em blocos de acordo com a categoria de projeto (bibliotecas vs. aplicações) ou por quartis de popularidade, e então comparar a efetividade da adoção dentro de cada bloco. Isso assegura que a comparação “com vs. sem ferramenta” ocorra entre projetos de natureza semelhante, reduzindo viés. Também nos permite observar se o efeito da ferramenta é consistente em diferentes contextos (por exemplo, em projetos grandes vs. pequenos).

### Possíveis variáveis de confusão conhecidas 

Reconhecemos diversos fatores externos que podem interferir nos resultados, atuando como variáveis de confusão, isto é, correlacionando-se tanto com a adoção da ferramenta quanto com os resultados de segurança. Já mencionamos muitos deles como controles; aqui destacamos os principais e por que devem ser monitorados:

- Tamanho e complexidade do projeto
- Atividade e maturidade do projeto
- Conhecimento/experência da equipe em segurança
- Pressão externa e contexto do projeto

Em suma, todas essas variáveis de confusão conhecidas serão monitoradas e, sempre que possível, integradas ao desenho (como controles ou blocos) ou à análise estatística (como covariáveis). Isso segue recomendações de estudos empíricos anteriores que destacam a importância de contabilizar características do repositório ao avaliar práticas de segurança. A Tabela 3 abaixo sintetiza todas as principais variáveis discutidas, indicando seu tipo, definição operacional e unidade de medida:

**Tabela 3: Resumo das variáveis do estudo, com suas classificações, definições e unidades.**

| **Variável**                                            | **Tipo**                                       | **Definição operacional**                                                                                                                                                                                                                                         | **Unidade de medida**                          |
| ------------------------------------------------------- | ---------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------- |
| Adoção de ferramenta de verificação de vulnerabilidades | Independente (Fator) – Categórica binária      | Indica se o projeto utiliza ao menos uma ferramenta automatizada de varredura de vulnerabilidades (e.g., Dependabot, Snyk). Determinado pela presença de configurações ou histórico de uso da ferramenta no repositório.                                          | Binário (Sim/Não)                              |
| Contagem de vulnerabilidades não resolvidas             | Dependente – Quantitativa discreta             | Número de vulnerabilidades de segurança conhecidas ainda presentes no projeto (alertas abertos não corrigidos). Obtido via alertas de segurança do GitHub ou relatórios de vulnerabilidades nas dependências.                                                     | Contagem (número absoluto de vulnerabilidades) |
| Tempo médio de correção de vulnerabilidades (MTTR)      | Dependente – Quantitativa contínua             | Média de tempo decorrido entre a identificação de uma vulnerabilidade e sua correção no projeto. Calculado a partir de timestamps de alertas abertos e fechados (ou commits de fix).                                                                              | Tempo (dias)                                   |
| Tempo médio de atualização de dependências (MTTU)       | Dependente – Quantitativa contínua             | Média de tempo para o projeto atualizar suas dependências após lançamento de novas versões, especialmente aquelas que corrigem falhas. Baseado na diferença de data entre a disponibilização de uma versão corrigida e o update correspondente no `package.json`. | Tempo (dias)                                   |
| Taxa de aceitação de correções automáticas              | Dependente – Quantitativa contínua (proporção) | Porcentagem de sugestões de correção (PRs automáticos de ferramentas) que foram mescladas com sucesso no código do projeto. Calculado como (nº de PRs de correção aceitos / nº total de PRs de correção propostos * 100).                                         | Porcentagem (%)                                |
| Proporção de dependências atualizadas                   | Dependente – Quantitativa contínua (proporção) | Fração das dependências do projeto que estão atualizadas (na última versão ou sem vulnerabilidades conhecidas). Pode ser derivada do número de dependências sem alertas dividido pelo total de dependências.                                                      | Proporção (0 a 1) or %                         |
| Tamanho do projeto (LOC)                                | Controle – Quantitativa contínua               | Tamanho do código fonte do projeto medido em linhas de código (LOC) ou número de arquivos. Obtido via ferramentas de contagem de linhas no repositório.                                                                                                           | Contagem (nº de linhas ou arquivos)            |
| Idade do projeto                                        | Controle – Quantitativa contínua               | Tempo de existência do projeto desde o primeiro commit até o fim do período de estudo.                                                                                                                                                                            | Tempo (anos)                                   |
| Nº de contribuidores ativos                             | Controle – Quantitativa discreta               | Total de desenvolvedores que contribuíram com commits no projeto no último período relevante (por exemplo, último ano).                                                                                                                                           | Contagem (nº de pessoas)                       |
| Popularidade (estrelas no GitHub)                       | Controle – Quantitativa discreta               | Popularidade do projeto medida pelo número de estrelas no GitHub (proxy de uso/comunidade).                                                                                                                                                                       | Contagem (nº de estrelas)                      |
| Tipo de projeto                                         | Controle (ou Bloco) – Categórica nominal       | Categoria do software: por exemplo, **biblioteca** (reutilizável por outros projetos) vs **aplicação** (produto final), ou outras classificações de domínio. Definido via inspeção manual ou metadados do projeto.                                                | Rótulo categórico (ex.: biblioteca/app)        |
| Nº de dependências diretas                              | Controle – Quantitativa discreta               | Quantidade de pacotes/bibliotecas listados como dependências diretas no `package.json` do projeto. (Opcionalmente podemos considerar também dependências transitivas agregadas).                                                                                  | Contagem (nº de dependências)                  |
| Atividade recente (commits/mês)                         | Controle – Quantitativa contínua               | Taxa de atividade de desenvolvimento medida pelo número médio de commits por mês no último ano. Indica quão ativo está o projeto atualmente.                                                                                                                      | Frequência (commits por mês)                   |
| (Outros potenciais confounders qualitativos)            | Confusão (não mensurável diretamente)          | Ex.: Cultura de segurança da equipe, apoio corporativo, criticidade do projeto, etc., que não possuem métrica direta mas podem influenciar. Serão considerados qualitativamente na interpretação dos resultados.                                                  | *N/A*                                          |


[Retorne ao Sumário](#sumário)

## Desenho experimental

### Tipo de desenho 

O estudo adotará um desenho quase-experimental de caráter observacional comparativo, frequentemente chamado de estudo ex post facto. Trata-se de um desenho comparativo porque avalia diferenças entre dois grupos (projetos que adotam vs. não adotam a ferramenta), porém não-randomizado devido à natureza do problema – não é possível atribuir aleatoriamente a adoção de ferramenta a projetos open source independentes. Em vez disso, observamos um fenômeno já ocorrido (adoção voluntária da ferramenta) e comparamos com um controle. Assim, o delineamento se assemelha a um estudo de coorte retrospectivo: identificamos uma coorte de projetos “expostos” à intervenção (ferramenta de verificação) e comparamos com uma coorte não exposta, acompanhando determinados resultados de segurança. Esse desenho é comparativo observacional pois avalia diferenças entre grupos definidos por um fator de interesse, mas sem interferência do pesquisador na atribuição do fator. Também podemos classificá-lo como um desenho entre sujeitos (between-subjects), já que diferentes projetos pertencem a condições distintas. Optamos por esse tipo de delineamento porque ele reflete as condições do mundo real da adoção de ferramentas em projetos open source, ao mesmo tempo em que nos permite inferir associações entre adoção e resultados. Embora seja observacional, tomaremos cuidados (descritos adiante) para aproximar as condições de um experimento controlado no que tange à comparabilidade dos grupos.

Adicionalmente, incorporaremos elementos de desenho em blocos no processo de análise, ao estratificar os projetos por características (como tipo ou tamanho do projeto) antes da comparação. Isso adiciona um aspecto de controle experimental dentro do delineamento observacional, aumentando a precisão ao eliminar variações indevidas dentro de cada bloco. Em suma, o desenho é principalmente comparativo observacional com controle estatístico de variáveis de confusão, aproximando-se de um quasi-experimento. Não se trata de um experimento fatorial completo (pois temos essencialmente um fator principal), mas cobrimos diferentes cenários através de blocos.

### Randomização e alocação 

Devido à natureza retrospectiva do estudo, não há como randomizar quais projetos adotam ou não a ferramenta – essa decisão já foi tomada autonomamente pelas comunidades dos projetos. No entanto, introduziremos randomização em dois aspectos para reduzir vieses:

- Seleção Aleatória da Amostra
- Alocação/Emparelhaamento

Em termos de alocação às condições, poderíamos dizer que os projetos “auto-selecionaram” a condição (adoção vs. não adoção). Nosso papel no desenho é, portanto, realocar analiticamente os projetos em grupos comparáveis. Em resumo, não há distribuição aleatória clássica de sujeitos às condições, mas compensamos isso pela aleatoriedade na seleção da amostra e por ajustes posteriores via emparelhamento/matching.

### Balanceamento e contrabalanço: 

Garantir o balanceamento entre os grupos é crucial para validade interna neste desenho observacional. Empregaremos várias estratégias para assegurar que os grupos Adotantes e Não Adotantes sejam comparáveis:

- Balanceamento de variáveis de controle
- Contrabalanço de efeitos de ordem
- Estratificação

Em síntese, o design assegura balanceamento via seleção cuidadosa e técnicas analíticas. Como é impossível contrabalançar ordem de tratamento (pois não há aplicação sequencial de tratamento), focamos em balancear as características intrínsecas dos sujeitos entre as condições. Tais precauções seguem boas práticas de estudos quase-experimentais em Engenharia de Software, aumentando a confiabilidade de que diferenças observadas nos resultados devem-se à variável independente (adoção da ferramenta) e não a vieses de seleção.

[Retorne ao Sumário](#sumário)

## População, sujeitos e amostragem

### População-alvo

A população-alvo deste estudo são projetos open source JavaScript hospedados no GitHub que possuem atividade recente e base de contribuidores ativos, representando a prática real de manutenção de software nesse ecossistema.

### Critérios de inclusão de sujeitos

- Projetos escritos majoritariamente em JavaScript ou Node.js
- Repositórios públicos, com pelo menos 50 estrelas e 5 contribuidores
- Ter histórico de commits nos últimos 12 meses
- Usar gerenciadores de pacotes como npm ou yarn

### Critérios de exclusão de sujeitos

- Repositórios arquivados, descontinuados ou com menos de 10 commits no total
- Projetos pessoais, forks sem atividade, ou espelhos automatizados
- Repositórios sem package.json (indicador da estrutura de dependências)

### Tamanho da amostra planejado

Pretende-se analisar no mínimo 100 repositórios com ferramentas de verificação e 100 sem ferramentas, totalizando 200 projetos. Esse tamanho é suficiente para detectar efeitos moderados com poder estatístico ≥ 80% e nível de significância α = 0,05.

### Método de seleção / recrutamento

A seleção será feita por amostragem por conveniência com critérios sistemáticos, utilizando consultas à API do GitHub e filtragem automática com base nas condições de inclusão e exclusão. Repositórios serão selecionados aleatoriamente entre os que atendem aos critérios.

### Treinamento e preparação dos sujeitos

Como o experimento é observacional sobre repositórios, não haverá intervenção com participantes humanos diretos. No entanto, os dados e critérios de análise serão padronizados e documentados em um manual para garantir reprodutibilidade e consistência na coleta e interpretação dos dados.

[Retorne ao Sumário](#sumário)

## Plano de análise de dados

### Estratégia geral de análise

Cada questão de pesquisa será respondida comparando métricas-chave (como número de vulnerabilidades, MTTR, proporção de dependências atualizadas) entre os grupos de projetos com e sem ferramentas de verificação, controlando para variáveis como idade e popularidade do repositório.

### Métodos estatísticos planejados

- Testes de média (t-test, Mann-Whitney U) para comparação entre grupos
- Regressão linear múltipla e logística, para controle de variáveis de confusão
- Correlação de Spearman entre nível de adoção e intensidade das práticas de segurança
- Análise de cluster ou segmentação, se surgirem perfis distintos de projetos

### Tratamento de dados faltantes e outliers

- Repositórios com dados incompletos nas métricas principais serão excluídos da análise
- Outliers serão identificados por IQR (interquartil range) ou z-score (>3 desvios padrão), mas não removidos automaticamente — serão analisados separadamente e tratados com cautela
- Se a quantidade de dados faltantes for baixa (<5%), poderá ser feita imputação média ou mediana para manter a amostra

### Plano de análise para dados qualitativos

Caso sejam incluídos dados qualitativos (ex.: descrição de issues ou PRs automatizados), será utilizada análise de conteúdo por codificação aberta, identificando padrões de motivação, aceitação ou rejeição de práticas automatizadas de segurança.

[Retorne ao Sumário](#sumário)

---

## Referências

- [Open Source, Open Threats? Investigating Security Challenges in Open-Source Software](https://arxiv.org/abs/2506.12995)
- [On the Discoverability of npm Vulnerabilities in Node.js Projects](https://das.encs.concordia.ca/publications)
  - Journals - 2022
- [Open Source Security and Risk Analysis Report 2024](https://static.carahsoft.com/concrete/files/1617/1597/8665/2024_Open_Source_Security_and_Risk_Analysis_Report_WRAPPED.pdf)

---

# Informações Adicionais

## Autor

- [Jhonata Silveira Dias](https://github.com/jhonatasdias)
  - Engenheiro de Software/Desenvolvedor

## Responsável principal (PI / dono do experimento)

- [Jhonata Silveira Dias](https://github.com/jhonatasdias)
  - Engenheiro de Software/Desenvolvedor

## Projeto / produto / iniciativa relacionada

Este experimento está vinculado à linha de pesquisa em Segurança em Desenvolvimento de Software e à iniciativa de investigação sobre a adoção de práticas DevSecOps em projetos open source, no contexto da produção acadêmica do autor. O estudo se alinha a objetivos mais amplos de compreender como práticas automatizadas, como análise estática e detecção de vulnerabilidades, estão sendo aplicadas (ou negligenciadas) em ecossistemas amplamente utilizados, como o JavaScript/Node.js. Além disso, o experimento pode subsidiar futuras iniciativas práticas voltadas à melhoria de ferramentas de segurança ou à proposição de diretrizes para manutenção segura de repositórios open source. O projeto também pode ser considerado parte de uma possível monografia de graduação (ou outro tipo de trabalho, se aplicável), com resultados potencialmente relevantes para publicação em conferências ou eventos técnicos na área de engenharia de software e segurança cibernética.

[Retorne ao Sumário](#sumário)
