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

