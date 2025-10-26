# Registro de Decisões de Projeto - TCC Usinas Solares

Este documento registra as principais decisões de metodologia, arquitetura e ferramentas tomadas ao longo do desenvolvimento do projeto.

---

### 1. Estratégia de Nuvem e Portfólio

**Decisão (28/09/2025):**
O TCC será desenvolvido com a mentalidade de um projeto de **portfólio de Engenharia de Dados**. A plataforma de nuvem escolhida para hospedar os dados e, potencialmente, o processamento, é o **Google Cloud Platform (GCP)**.

**Justificativa:**
* Alinhar o projeto acadêmico com objetivos de carreira.
* Aplicar boas práticas de mercado (CI/CD, versionamento, documentação) para demonstrar competências profissionais.
* Aproveitar os créditos e o nível gratuito do GCP para viabilizar um projeto de ponta a ponta sem custos.

---

### 2. Versionamento e Fluxo de Trabalho Git

**Decisão (28/09/2025):**
O projeto seguirá um fluxo de trabalho baseado em **feature branches** e **Pull Requests (PRs)** para a branch `main`. As mensagens de commit seguirão o padrão **Conventional Commits**. A branch `main` será protegida contra pushes diretos.

**Justificativa:**
* Garantir a estabilidade da branch `main`.
* Manter um histórico de commits limpo, legível e profissional.
* Permitir a execução de verificações automáticas (CI/CD) em cada PR.

---

### 3. Qualidade de Código e CI/CD

**Decisão (28/09/2025):**
A ferramenta padrão para linting e formatação de código é o **Ruff**. Um pipeline de **Integração Contínua (CI)** foi implementado com **GitHub Actions** para verificar a qualidade do código a cada push e PR.

**Justificativa:**
* Automatizar a garantia de qualidade e consistência do código.
* Utilizar ferramentas modernas e de alta performance (Ruff) que são padrão na indústria.
* Integrar práticas de DevOps ao projeto acadêmico.

---

### 4. Sistema de Coordenadas de Referência (CRS)

**Decisão (25/09/2025):**
O projeto adotará o **`EPSG:4674` (SIRGAS 2000 Geográfico, em graus)** como o CRS padrão para todos os dados armazenados.

**Justificativa:**
* Padrão Nacional oficial do Brasil (IBGE).
* Garantir consistência e evitar erros de alinhamento espacial entre as camadas.

**Processo:**
* Para operações que exigem medições em metros (ex: buffers), os dados serão reprojetados "sob demanda" para um CRS projetado adequado (ex: **`EPSG:31983` - SIRGAS 2000 / UTM Zone 23S**). O resultado será convertido de volta para `EPSG:4674` antes de ser salvo.

---

### 5. Análise de Proximidade da Rede Elétrica

**Decisão (25/09/2025):**
A "proximidade" da rede elétrica será modelada através da criação de anéis de **buffer**. Para a EDA, foi usado um buffer de 10 km. Para o modelo de otimização final, a estratégia será de múltiplos anéis para representar diferentes custos de conexão.

**Justificativa:**
* Modelar o custo de conexão de forma mais realista do que uma restrição binária (perto/longe).
* Aumentar a sofisticação e o alinhamento do modelo com a realidade econômica de projetos de energia.

---

### 6. Tratamento de Sítios Arqueológicos

**Decisão (28/09/2025):**
Os sítios arqueológicos, que são dados pontuais, serão tratados como áreas de restrição através da criação de um **buffer circular de 1 km** ao redor de cada ponto.

**Justificativa:**
* A legislação brasileira exige a proteção da "vizinhança" dos sítios arqueológicos para mitigar os impactos de obras no entorno. Como não há um raio de proteção fixo e universal definido em lei, um buffer de 1 km foi adotado como uma medida metodológica conservadora para este estudo em escala nacional, em linha com o princípio de precaução na proteção do patrimônio cultural.

**Base Legal:**
* A proteção do patrimônio cultural no Brasil é um preceito da **Constituição Federal de 1988 (Art. 216)**, que define os sítios arqueológicos como patrimônio a ser protegido.
* O **Decreto-Lei nº 25/1937** é o instrumento que institui o tombamento e estabelece a proteção da "vizinhança" de bens tombados, conceito que se aplica aos sítios.
* A **Lei nº 3.924/1961** dispõe especificamente sobre os monumentos arqueológicos, reforçando sua proteção como bens da União. Embora seja anterior à Constituição de 1988 e faça referência a artigos de constituições passadas, seu conteúdo foi "recepcionado" pela constituição atual, pois sua essência é compatível e reforçada pelo Art. 216.
* A **Instrução Normativa IPHAN nº 01/2015** regulamenta o processo de licenciamento ambiental e estabelece os procedimentos para a definição de perímetros de proteção caso a caso, com base em estudos de impacto.

**Referências Legais:**
* [Decreto-Lei nº 25 de 1937](http://portal.iphan.gov.br/uploads/legislacao/Decreto_no_25_de_30_de_novembro_de_1937.pdf)
* [Lei nº 3.924 de 1961](https://www.jusbrasil.com.br/legislacao/128682/lei-3924-61)
* [Instrução Normativa IPHAN nº 01 de 2015](http://portal.iphan.gov.br/uploads/legislacao/INSTRUCAO_NORMATIVA_001_DE_25_DE_MARCO_DE_2015.pdf)

---

### 7. Tratamento das Áreas de Restrição (UC, TI, Quilombola, Assentamento)

**Decisão (28/09/2025):**
Todas as áreas de Unidades de Conservação (UC), Terras Indígenas (TI), Territórios Quilombolas e Assentamentos Rurais serão tratadas como zonas de exclusão e farão parte da máscara consolidada.

**Justificativa:**
* A exclusão dessas áreas se baseia em impedimentos legais, constitucionais e de função social que inviabilizam a instalação de projetos energéticos de grande porte.
* **Base Legal Principal:**
    * **UCs:** Lei nº 9.985/2000 (SNUC).
    * **TIs:** Art. 231 da Constituição Federal de 1988.
    * **Quilombolas:** Art. 68 do ADCT da Constituição Federal de 1988.
    * **Assentamentos:** Lei nº 4.504/1964 (Estatuto da Terra) e sua função social.

**Processo:**
* Uma pesquisa detalhada por decretos e portarias específicas para cada camada será realizada durante a fase de redação da monografia para enriquecer a bibliografia. Para a modelagem, a justificativa principal acima é suficiente.

---


### 8. Resolução dos Dados Topográficos

**Decisão (20/10/2025):**
Os dados de topografia (Modelo Digital de Elevação e Declividade) serão processados e exportados do Google Earth Engine com uma resolução espacial de **90 metros** por pixel.

**Justificativa:**
* A resolução nativa de 30m do SRTM gera arquivos excessivamente grandes (~4 GB) para uma análise em escala nacional, impactando negativamente o armazenamento e o tempo de processamento.
* A resolução de 90m oferece um excelente balanço entre o detalhe do terreno e a performance computacional, sendo adequada para o escopo estratégico do projeto e mais compatível com a granularidade das outras camadas de dados.
* Esta mudança resulta em uma redução de aproximadamente 90% no tamanho do arquivo, tornando o fluxo de trabalho mais eficiente.

# Primeira reunião de acompanhamento
Não chamar de restrição as "restrições de instalação", restrição é mais as s.a. da modelagem
Na hora de rodar o solver, avaliar se tem que aumentar a granularidade
Processamento nas máquinas do GTA (maquina sem GPU, só CPU) ou no GCP
Rodar problemas menores depois de selecionar os top 10 (?) numa granularidade menor
## Qual a restrição pra não colocar na área inteira? Tenho um número X de painéis? Tem uma quantidade de irradiação que eu quero alcançar? Q q impede de tudo zer X_i = 1? Tenho recursos infinitos? Procurar na dissertação de mestraddo de Saccardo.
A máscara de restrição n é restrição, é pre processamento dos dados, pois no final somente as áreas possíveis seriam alimentadas no solver.
Continuidade das áreas: Definir uma unidade mínima de painéis solares (quanto mais realista melhor, mas pode ser suposto) (qual o mínimo q uma empresa de paineis aceitaria instalar, 100 painéis? 200?), tipo áreas vizinas não necessariamente são a mesma área (pois seria muito difícil modelar isso no programa)
Próxima etapa da modelagem pra pensar: adicionar a proximidade às linhas de transmissão à função objetivo (mais próximo é melhor) e avaliar os pesos que atribuo à distância e à irradiação.
Formulação de p-dispersão, possivelmente queremos dispersar as UFVs ao invés de concentrar tudo. Motivação: curtailmente do setor elétrico? Colocar pesos na função objetivo, peso no espalhamento e peso na irradiação (b e 1-b)

### 9. Estratégia de Análise Multiescala e Granularidade

**Decisão (26/10/2025):**
A análise será conduzida com uma granularidade baseada nos dados de irradiação solar. A análise topográfica em escala nacional será processada com uma resolução de **1 km (1000 metros)**.

**Justificativa:**
* A análise exploratória revelou que a granularidade efetiva do projeto, definida pelos polígonos do Atlas Brasileiro de Energia Solar, é de aproximadamente **10 km x 10 km**.
* Usar a resolução nativa do SRTM (30m ou 90m) em escala nacional seria computacionalmente inviável e desnecessariamente detalhado para a fase de prospecção macro.
* A resolução de 1 km para a topografia oferece um excelente balanço entre o detalhe do terreno e a performance computacional, sendo consistente com a granularidade dos outros dados e alinhada com a sugestão do orientador para uma análise multiescala.