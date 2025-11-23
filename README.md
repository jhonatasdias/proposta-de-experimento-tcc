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

Um aspecto fundamental no gerenciamento de vulnerabilidades é a rapidez com que as correções são aplicadas após a descoberta de uma falha. Modelos de ciclo de vida de vulnerabilidades sugerem que, após a divulgação pública de uma brecha, o risco de exploração atinge seu pico, pressionando os mantenedores a reagirem rapidamente. No entanto, conforme discutido, a realidade em muitos projetos é de atrasos consideráveis na aplicação de patches. Nesse contexto, a adoção de ferramentas automatizadas, como bots de atualização de dependências e scanners de segurança integrados, desponta como uma estratégia promissora para diminuir o tempo de exposição. Por exemplo, estudos sobre o bot Dependabot, que automatiza a criação de patches para dependências vulneráveis, demonstraram que desenvolvedores tendem a aceitar e mesclar a maioria das atualizações de segurança propostas em questão de dias, ao passo que, sem tal auxílio, a correção manual de vulnerabilidades pode levar vários meses. Esse achado empírico ilustra o impacto positivo que ferramentas de verificação/atualização automática podem ter: ao agilizar o processo de correção, elas reduzem o intervalo em que projetos ficam suscetíveis a explorações conhecidas. Além disso, tais ferramentas aliviam a carga dos desenvolvedores em rastrear continuamente novos avisos de segurança, funcionando como um mecanismo de alerta e mitigação proativo dentro do fluxo de trabalho de desenvolvimento.

Em resumo, o referencial teórico-empírico deste estudo apoia-se em conceitos de vulnerabilidades de software e sua dinâmica em ecossistemas de código aberto, bem como nas abordagens modernas de identificação e mitigação dessas falhas. Modelos de segurança em DevOps (DevSecOps) e práticas de continuous security fornecem o pano de fundo conceitual para entender como e por que a integração de ferramentas de verificação de vulnerabilidades pode elevar o nível de segurança em projetos JavaScript. Fundamentado por evidências recentes, como a prevalência generalizada de falhas de segurança
investor e o crescimento acelerado das mesmas, este estudo busca analisar criticamente a adoção dessas ferramentas no mundo real e avaliar em que medida elas estão suprindo a lacuna entre a detecção de vulnerabilidades e sua resolução efetiva nos projetos open source.

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