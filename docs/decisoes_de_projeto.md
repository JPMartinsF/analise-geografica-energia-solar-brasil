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

---

### 9. Estratégia de Análise Multiescala e Granularidade

**Decisão (26/10/2025):**
A análise será conduzida com uma granularidade baseada nos dados de irradiação solar. A análise topográfica em escala nacional será processada com uma resolução de **1 km (1000 metros)**.

**Justificativa:**
* A análise exploratória revelou que a granularidade efetiva do projeto, definida pelos polígonos do Atlas Brasileiro de Energia Solar, é de aproximadamente **10 km x 10 km**.
* Usar a resolução nativa do SRTM (30m ou 90m) em escala nacional seria computacionalmente inviável e desnecessariamente detalhado para a fase de prospecção macro.
* A resolução de 1 km para a topografia oferece um excelente balanço entre o detalhe do terreno e a performance computacional, sendo consistente com a granularidade dos outros dados e alinhada com a sugestão do orientador para uma análise multiescala.

---

### 10. Distinção Metodológica: Pré-processamento vs. Restrições do Solver

**Decisão (08/11/2025):**
Conforme sugestão do orientador, os critérios de exclusão (UCs, TIs, Sítios Arqueológicos, Declividade > 5°, etc.) **não** serão modelados como "restrições" (`constraints`) dentro do solver de otimização matemática.

**Justificativa:**
* Estas áreas representam "impossibilidades" físicas, legais ou metodológicas, e não escolhas a serem feitas pelo modelo.

**Processo:**
* Todas as camadas de exclusão serão consolidadas em um único arquivo (`mascara_exclusao_total.gpkg` ou `.tif`) durante a **Fase 1 (Pré-processamento)**.
* O modelo de otimização (Fase 2) será alimentado *apenas* com as áreas candidatas que já são válidas (ou seja, que estão *fora* da máscara de exclusão). Isso torna o problema muito mais leve e computacionalmente tratável.

---

### 11. Metodologia de Otimização (Programação Inteira vs. MCDM)

**Decisão (08/11/2025):**
A análise da dissertação de Saccardo (referencial bibliográfico principal) revelou uma diferença metodológica crucial. A dissertação de Saccardo utiliza **MCDM (Multi-Criteria Decision Making)**, que resulta em um *mapa de aptidão* (um "ranking" de áreas de 0 a 100). Nosso projeto utiliza **Programação Inteira Binária**, que *seleciona* áreas candidatas (`xi ∈ {0,1}`).

**Impacto:**
* O modelo MCDM não necessita de uma "restrição de orçamento", pois seu objetivo é classificar.
* Nosso modelo de Programação Inteira **exige** uma restrição global para evitar uma solução trivial (onde `xi = 1` para todas as áreas válidas).

**Status:**
* A definição exata desta restrição (ex: `Σ (Area_i * x_i) <= Limite_Total_Area` ou `Σ (Potencial_Geracao_i * x_i) >= Meta_Nacional_Geracao`) é a principal pendência metodológica a ser definida para a Fase 2 do projeto.

---

### 12. GAPs e Decisões de Escopo (Pós-Análise Bibliográfica)

**Data (08/11/2025):**
A revisão bibliográfica (baseada em Saccardo) identificou novos critérios potenciais para o modelo. O status de cada um foi definido para gerenciar o escopo e o cronograma do projeto.

**12.1. GAP Crítico (Pendente): Recursos Hídricos**
* **Descoberta:** A literatura aponta rios, lagos (para evitar APPs e risco de inundação) e a linha de costa (para evitar corrosão salina) como critérios de exclusão críticos.
* **Status:** **Pendente.** Dada a alta complexidade de processar esses dados (ex: aplicar buffers de APP variáveis para rios conforme a lei) e o prazo apertado (entrega em Dez/2025), a inclusão desta camada será validada com o orientador (Prof. Rodrigo) antes de ser iniciada.

**12.2. Critérios Fora do Escopo (Trabalhos Futuros)**
* **Decisão:** Os critérios de **Tipo de Solo**, **Falhas Geológicas** e **Altitude Elevada**, embora citados na literatura, serão formalmente classificados como **fora do escopo** deste TCC.
* **Justificativa:** A alta complexidade na obtenção e processamento dos dados (Solos, Falhas) e a baixa relevância do critério no contexto geográfico brasileiro (Altitude) não justificam o impacto no cronograma. Serão incluídos na dissertação como **Limitações e Sugestões para Trabalhos Futuros**.

**12.3. Critério de Viabilidade: Irradiação Solar Mínima**
* **Decisão:** Além de usar a irradiação na função objetivo (ou na restrição de meta), será aplicado um **filtro de exclusão** no pré-processamento. Áreas com irradiação abaixo de um limiar mínimo de viabilidade econômica serão removidas.
* **Justificativa:** Esta é uma prática padrão da indústria para evitar a seleção de locais economicamente inviáveis. A literatura de referência (Saccardo, 2024) valida esta abordagem, adotando um limiar de **5,0 kWh/m²/dia** como o nível "Neutro" (o mais baixo) em seu modelo de classificação.
* **Status:** (Proposta para validação com o orientador).

---

### 13. Estratégia de Consolidação da Máscara de Exclusão

**Decisão (09/11/2025):**
Todas as camadas de exclusão (incluindo as consolidadas de vetor, como `mascara_exclusao_final.gpkg`, e as de raster, como `mascara_declividade_maior_5graus.tif`) serão combinadas em um **único arquivo raster final**: `mascara_exclusao_total.tif`.

**Justificativa:**
* **Eficiência de Combinação:** É a abordagem mais eficiente para unificar fontes de dados mistas (vetoriais e raster) em um formato comum. A alternativa (vetorizar o raster de declividade) seria computacionalmente inviável.
* **Eficiência de Aplicação (Fase 2):** Um arquivo raster final é computacionalmente otimizado para a próxima fase. Ele permite "remover" as áreas de exclusão dos rasters de oportunidade (como Irradiação Solar) através de álgebra de mapas (multiplicação de arrays `numpy`), o que é instantâneo. A alternativa (usar a máscara vetorial para "clipar" o raster) exigiria operações espaciais complexas e lentas.
* **Processo:** A consolidação é feita rasterizando qualquer camada de vetor para que ela se alinhe perfeitamente com a grade (shape e transform) do raster de molde (ex: `declividade_brasil_srtm_1km.tif`) e, em seguida, combinando todas as máscaras de raster usando operações lógicas (`numpy.logical_or`).

---

### 14. Formato de Dados para Otimização (Camada Prata/Ouro)

**Decisão (04/02/2026):**
Para a etapa de otimização (Solver), os dados espaciais (shapefiles, tiffs) serão convertidos e armazenados no formato tabular **CSV** (ou Parquet para grandes volumes).

**Justificativa:**
* **Natureza do Solver:** Algoritmos de programação linear (PuLP, Gurobi) exigem matrizes numéricas, não geometrias complexas.
* **Performance:** Carregar CSV/Parquet é significativamente mais rápido e consome menos memória RAM do que carregar geometrias via GeoPandas, o que é crítico para evitar travamentos em análises de larga escala.
* **Limpeza:** Separa a lógica de processamento geoespacial (pesado) da lógica de otimização matemática.

---

### 15. Estrutura de Armazenamento GCP (Inputs do Solver)

**Decisão (04/02/2026):**
Foi criada uma nova estrutura de diretórios no Google Cloud Storage (Bucket) especificamente para armazenar as matrizes de entrada do modelo matemático, separada dos arquivos brutos e processados geoespaciais.
* Caminho: `tcc-usinas-solares-brasil-dados/solver_inputs/`

**Justificativa:**
* Evitar a mistura de arquivos intermediários de geoprocessamento (TIFFs, GPKGs) com os dados finais prontos para o solver.
* Facilitar a leitura direta pelo script de otimização sem necessidade de refazer o ETL geográfico.

---

### 16. Definição das Metas de Expansão (Target MW)

**Decisão (04/02/2026):**
O objetivo do modelo de otimização será alocar capacidade instalada suficiente para atender à "Expansão Indicativa" prevista no planejamento energético nacional.
* **Fonte dos Dados:** Plano Decenal de Expansão de Energia (PDE) 2034, Tabela I-5 (Expansão da Oferta de Energia Elétrica - Referência).
* **Metodologia de Cálculo:** Soma da expansão indicativa fotovoltaica centralizada prevista para o período de 2028 a 2034.

**Valores Definidos:**
* **Cenário Referência (Base):** **8.600 MW** (Valor exato do PDE 2034).
* **Cenário Pessimista:** **6.880 MW** (80% da Referência - Simulação de estagnação baseada no Box 2.2 do PDE).
* **Cenário Otimista:** **10.320 MW** (120% da Referência).

---

### 17. Tratamento da Restrição Orçamentária (Investimento)

**Decisão (04/02/2026):**
O valor de investimento total estimado no PDE (R$ 31,18 bilhões) será utilizado como **parâmetro de validação e análise de eficiência**, e não como uma restrição rígida (*Hard Constraint*) no modelo inicial.

**Justificativa:**
* **Variabilidade Regional:** O custo de implantação varia drasticamente por região (logística, terraplanagem, conexão). Usar uma média nacional como teto rígido poderia tornar o problema inviável artificialmente em regiões mais caras.
* **Estratégia de Defesa:** O objetivo é minimizar o custo para atingir a meta de 8.600 MW. Comparar o custo resultante do modelo *versus* o orçamento do PDE permitirá conclusões analíticas valiosas (ex: "O modelo otimizado economizou X bilhões" ou "O orçamento do PDE está subestimado para a realidade geográfica").

---

### 18. Processamento do ETL Fundiário (Notebook 08)

**Decisão (04/04/2026):**
Reduzir o espaço de busca contínuo para um conjunto discreto de latifúndios viáveis em MG, cruzando dados vetoriais (INCRA) com o raster de exclusão (topografia + restrições ambientais).

**Processo e Resultados:**
* **Unificação de Fontes:** As bases do SIGEF e SNCI foram unificadas para formar a malha fundiária certificada de Minas Gerais, partindo de um universo inicial de **293.780 propriedades**.
* **Corte de Área Bruta:** Aplicou-se o filtro metodológico exigindo área $\geq 100$ hectares. Isso reduziu a base para **63.712 propriedades** (redução primária de 78,3%, eliminando minifúndios).
* **Correção de Projeção (Área do Pixel):** Para evitar a distorção geográfica do EPSG:4326 ao calcular a área útil, projetou-se um pixel simulado no centro de MG (Lon -44, Lat -18) no CRS UTM métrico (EPSG:31983). A área cravou em **94.54 hectares por pixel**.
* **Cruzamento Espacial (Vector vs Raster):** Utilizou-se a biblioteca `rasterstats` (`zonal_stats`) para somar os pixels inviáveis dentro de cada fazenda e subtraí-los do total, encontrando a **Área Útil** real.
* **Corte de Área Útil:** Após aplicar o filtro de 100 hectares sobre a *área útil* (livre de morros e reservas), o número de latifúndios viáveis caiu para apenas **8.150 propriedades** (descarte de 87,2% das áreas brutas).
* **Cálculo de Potencial:** Utilizando a métrica do NREL (3,6 ha/MW), as 8.150 propriedades somam um potencial técnico mapeado de **1.080 GW**.
* **Exportação:** Os dados finais limpos (geometria + área útil + potencial MW) foram salvos como `candidatos_solares_mg_limpo.gpkg` na camada Prata/Ouro para a próxima etapa de cálculo de distância.