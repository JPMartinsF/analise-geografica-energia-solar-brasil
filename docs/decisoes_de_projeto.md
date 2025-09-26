# Registro de Decisões de Projeto - TCC Usinas Solares

Este documento registra as principais decisões de metodologia, arquitetura e ferramentas tomadas ao longo do desenvolvimento do projeto.

---

### 1. Sistema de Coordenadas de Referência (CRS)

**Decisão (25/09/2025):**
O projeto adotará o **`EPSG:4674` (SIRGAS 2000 Geográfico, em graus)** como o CRS padrão para todos os dados armazenados, tanto brutos (`data/raw/`) quanto processados (`data/processed/`).

**Justificativa:**
* **Padrão Nacional:** É o sistema de referência geodésico oficial do Brasil, garantindo compatibilidade com os dados do IBGE, ANEEL e outras fontes nacionais.
* **Consistência:** Manter um único CRS para todos os dados finais evita erros de alinhamento espacial.

**Processo:**
* Para operações que exigem medições em metros (ex: criação de buffers, cálculo de área), os dados serão reprojetados "sob demanda" para um CRS projetado adequado (ex: **`EPSG:31983` - SIRGAS 2000 / UTM Zone 23S**).
* O resultado de qualquer operação em um CRS projetado será **convertido de volta** para `EPSG:4674` antes de ser salvo.

---

### 2. Análise de Proximidade da Rede Elétrica

**Decisão (25/09/2025):**
A "proximidade" da rede elétrica será modelada através da criação de múltiplos anéis de **buffer** ao redor das linhas de transmissão e subestações de alta tensão (>= 230 kV).

**Justificativa:**
* O custo de conexão é um fator econômico crucial e não-linear. Em vez de uma restrição binária (perto/longe), múltiplos anéis permitem uma análise mais sofisticada, que pode ser usada para pontuar ou custear os locais.
* Para a análise exploratória inicial, foi utilizado um buffer único de **10 km**, um valor representativo que será refinado no modelo de otimização final com base em dados de custo de implantação de LTs.

---