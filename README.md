# Trabalho Final: Proposta de Experimento para TCC

## Sumário

1. [Título](#a-adoção-real-de-sistemas-de-verificação-de-vunerabilidade-em-projetos-javascript---pj001)
1. [Contexto e problema](#contexto-e-problema)
1. [Informações Adicionais](#informações-adicionais)

---

# A adoção real de sistemas de verificação de vunerabilidade em projetos JavaScript - PJ001

## Contexto e problema

### Descrição do problema / oportunidade

Nos últimos anos observou-se um aumento significativo nos ataques cibernéticos direcionados a sistemas de software, tanto de código aberto quanto comerciais, resultando em prejuízos financeiros e violações de privacidade para os usuários. Grande parte desses ataques explora vulnerabilidades conhecidas em softwares desatualizados ou mal mantidos. Em particular, no ecossistema JavaScript, a atualização tardia de dependências e pacotes é um problema recorrente: estudos indicam que aplicações Node.js levam em média 103 dias para corrigir vulnerabilidades já divulgadas conforme Mahmoud Alfadel (2022) cita em seu trabalho.

Esse atraso prolongado abre uma janela de exposição em que invasores podem explorar falhas conhecidas, evidenciando a necessidade de intervenções que acelerem o processo de correção. Assim, delineia-se uma oportunidade clara para a adoção de ferramentas automatizadas de verificação de vulnerabilidades em projetos JavaScript, de modo a identificar e sanar brechas de segurança de forma proativa.

### Contexto organizacional e técnico

Este estudo foi conduzido em um contexto de pesquisa acadêmica, utilizando dados públicos da plataforma GitHub e ferramentas computacionais para análise. No aspecto organizacional, o trabalho insere-se na iniciativa de melhorar a segurança em projetos de software open source. Do ponto de vista técnico, a investigação focou o ecossistema JavaScript/Node.js, coletando dados de repositórios GitHub por meio da API da plataforma.

Os dados brutos (por exemplo, informações sobre dependências e vulnerabilidades reportadas) foram então processados e analisados com scripts em Python, permitindo extrair métricas e insights relevantes. Por fim, os resultados foram consolidados em visualizações gráficas para embasar a discussão. Esse arranjo técnico, envolvendo coleta automatizada, processamento de dados e apresentação visual, permitiu examinar sistematicamente as práticas de gerenciamento de vulnerabilidades em projetos JavaScript de forma reproduzível e escalável.

### Trabalhos e evidências prévias (internos e externos)

Estudos anteriores ressaltam a gravidade do problema das vulnerabilidades em open source e motivam a busca por soluções de verificação automatizada. O relatório Open Source Security and Risk Analysis 2024 (OSSRA) do Synopsys Cybersecurity Research Center (CyRC) revelou que 84% dos codebases analisados continham pelo menos uma vulnerabilidade conhecida de código aberto, e 74% dos codebases apresentavam vulnerabilidades classificadas como de alto risco. Esses números evidenciam não apenas a ubiquidade de vulnerabilidades em projetos de software, mas também a severidade considerável de muitas delas. Além da prevalência das falhas, o crescimento acelerado do volume de vulnerabilidades reportadas tem sido documentado na literatura.

Uma pesquisa abrangente conduzida por Akhavani et al. (2025), examinando tendências em múltiplos ecossistemas open source (incluindo JavaScript/Node.js), identificou um crescimento exponencial nas vulnerabilidades divulgadas, na ordem de 98% ao ano. Esse aumento anual dramático supera em muito o ritmo de crescimento do próprio ecossistema de software open source e indica que as brechas de segurança estão se acumulando cada vez mais rápido. Em conjunto, essas evidências prévias, tanto de fontes industriais (Synopsys) quanto acadêmicas (Akhavani et al.), ressaltam a urgência de adotar práticas e ferramentas mais eficazes de detecção e correção de vulnerabilidades em projetos JavaScript, motivando a realização deste estudo.

### Referencial teórico e empírico essencial

Para fundamentar o presente estudo, é necessário compreender os conceitos-chave de vulnerabilidade de software, as ferramentas de verificação de vulnerabilidades e modelos de adoção de práticas de segurança no desenvolvimento. No contexto de segurança da informação, vulnerabilidade é definida como qualquer fraqueza ou falha em componentes de software que possa ser explorada por agentes maliciosos, resultando em impacto negativo na confidencialidade, integridade ou disponibilidade do sistema. Em outras palavras, uma vulnerabilidade pode ser vista como uma porta de entrada indesejada no software – por exemplo, um erro de programação ou configuração – através da qual invasores obtêm acesso não autorizado, instalam código malicioso ou roubam dados sensíveis. Tais falhas de segurança, quando presentes em projetos de código aberto, tendem a ter alto potencial de propagação devido ao reuso extensivo de bibliotecas e componentes por diversas aplicações. Se uma biblioteca amplamente utilizada contém uma falha, todos os sistemas que dependem dela podem ficar expostos. Pesquisas mostram, por exemplo, que a presença de pacotes desatualizados no ecossistema npm está fortemente correlacionada com o aumento do risco de vulnerabilidades nos projetos dependentes. Dessa forma, manter as dependências atualizadas e corrigir prontamente as falhas conhecidas são práticas essenciais para reduzir a “defasagem técnica” e mitigar riscos de segurança.

Como resposta a esse cenário, surgem as ferramentas automatizadas de verificação de vulnerabilidades, cujo objetivo é identificar e ajudar a corrigir falhas de segurança de forma mais ágil do que seria possível manualmente. Essas ferramentas englobam distintas abordagens tecnológicas. Uma categoria importante são as ferramentas de Software Composition Analysis (SCA), voltadas a inspecionar os componentes de código aberto utilizados em um projeto. Soluções de SCA escaneiam a base de código de uma aplicação para identificar bibliotecas e dependências de terceiros, levantando um inventário (por exemplo, uma SBOM – Software Bill of Materials) de todos os componentes open source em uso. De posse dessa lista, a ferramenta verifica as versões dos componentes contra bases de dados de vulnerabilidades conhecidas (como o catálogo CVE/NVD), detectando automaticamente se alguma dependência está afetada por vulnerabilidades reportadas. Esse processo automatizado permite revelar vulnerabilidades “ocultas” nas bibliotecas importadas, que muitas vezes passam despercebidas pelos desenvolvedores. Em paralelo, outras ferramentas realizam análise estática de código (SAST) buscando padrões de implementação inseguros no próprio código-fonte da aplicação. Tanto as ferramentas de SCA quanto de SAST integram o conjunto de práticas de DevSecOps, que visam inserir controles de segurança ao longo do ciclo de desenvolvimento de software. Ao incorporar essas verificações nos pipelines de CI/CD, as equipes conseguem “mover a segurança para a esquerda”, isto é, detectar vulnerabilidades nas fases iniciais do desenvolvimento, antes que o software seja implantado em produção. Essa filosofia de integrar verificações contínuas aproveita o fato de que falhas corrigidas precocemente são mais baratas e simples de resolver, além de reduzirem significativamente a janela em que o sistema fica vulnerável a ataques.

Em resumo, o referencial teórico-empírico deste estudo apoia-se em conceitos de vulnerabilidades de software e sua dinâmica em ecossistemas de código aberto, bem como nas abordagens modernas de identificação e mitigação dessas falhas. Modelos de segurança em DevOps (DevSecOps) e práticas de continuous security fornecem o pano de fundo conceitual para entender como e por que a integração de ferramentas de verificação de vulnerabilidades pode elevar o nível de segurança em projetos JavaScript. Fundamentado por evidências recentes, como a prevalência generalizada de falhas de segurança
investor e o crescimento acelerado das mesmas, este estudo busca analisar criticamente a adoção dessas ferramentas no mundo real e avaliar em que medida elas estão suprindo a lacuna entre a detecção de vulnerabilidades e sua resolução efetiva nos projetos open source.

[Retorne ao Sumário](#sumário)

## Objetivos e questões (Goal / Question / Metric)

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
