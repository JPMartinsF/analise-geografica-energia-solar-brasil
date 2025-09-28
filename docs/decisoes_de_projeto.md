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