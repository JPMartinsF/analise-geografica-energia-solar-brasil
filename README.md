# Processamento de Dados Geográficos para Avaliar Locais Propícios para Usinas Solares no Brasil

[![Code Quality Check](https://github.com/JPMartinsF/analise-geografica-energia-solar-brasil/actions/workflows/code-quality.yml/badge.svg)](https://github.com/JPMartinsF/analise-geografica-energia-solar-brasil/actions/workflows/code-quality.yml)

Este repositório contém o desenvolvimento do meu Projeto de Graduação (TCC) para o curso de Engenharia de Computação e Informação da Escola Politécnica (Poli-UFRJ).

## 📝 Descrição do Projeto

O objetivo deste projeto é desenvolver e aplicar um modelo de otimização matemática para identificar os locais mais propícios para a instalação de usinas solares fotovoltaicas no Brasil. A análise utiliza múltiplas camadas de dados geográficos para considerar fatores técnicos, ambientais e de infraestrutura, como irradiação solar, declividade do terreno, proximidade com a rede elétrica e áreas de restrição legal ou ambiental.

Este projeto visa não apenas aplicar técnicas de geoprocessamento e otimização, mas também criar um estudo de caso robusto sobre a prospecção de áreas para energias renováveis, servindo como um portfólio de habilidades em engenharia e ciência de dados.

## 🎯 Objetivos Específicos

- **Coletar e Processar Dados Geoespaciais:** Unificar e tratar dados de diversas fontes públicas (INPE, IBGE, ANEEL, etc.) em formatos vetoriais e raster.
- **Desenvolver um Modelo de Otimização:** Formular um modelo matemático para selecionar as áreas ótimas com base em uma função objetivo (maximizar potencial de geração) e um conjunto de restrições.
- **Análise de Viabilidade Espacial:** Gerar mapas que indiquem as áreas mais adequadas para a instalação de novas usinas.
- **Validação do Modelo:** Comparar os resultados encontrados com a localização de usinas fotovoltaicas já existentes para validar a eficácia do modelo.

## 🛠️ Tecnologias e Ferramentas

* **Linguagem:** Python 3.10+
* **Análise Geoespacial:** GeoPandas, Rasterio, Shapely
* **Análise de Dados:** Pandas, NumPy
* **Visualização:** Matplotlib
* **Otimização:** (a definir, ex: PuLP, Google OR-Tools)
* **Qualidade de Código:** Ruff
* **CI/CD:** GitHub Actions

## 📂 Estrutura do Projeto

O repositório está organizado da seguinte forma:

-   **/data**: Armazena os dados do projeto.
    -   **/raw**: Dados brutos, como baixados das fontes originais. (Não versionado, armazenado no GCP).
    -   **/processed**: Dados intermediários e limpos, prontos para análise.
-   **/notebooks**: Contém os Jupyter Notebooks utilizados para a análise exploratória de dados (EDA) e prototipagem.
-   **/cloud_functions**: Código específico para deploy em ambientes serverless (ex: Google Cloud Functions).
-   **/docs**: Documentação do projeto, incluindo o registro de decisões de arquitetura.

## 🚀 Como Executar o Projeto

1.  **Clone o repositório:**
    ```bash
    git clone [https://github.com/JPMartinsF/analise-geografica-energia-solar-brasil.git](https://github.com/JPMartinsF/analise-geografica-energia-solar-brasil.git)
    cd analise-geografica-energia-solar-brasil
    ```

2.  **Crie e ative um ambiente virtual:**
    ```bash
    python -m venv .venv
    # Windows
    .\.venv\Scripts\activate
    # macOS/Linux
    source .venv/bin/activate
    ```

3.  **Instale as dependências:**
    ```bash
    pip install -r requirements.txt
    ```

4.  **Execute os notebooks** localizados na pasta `/notebooks`.

## 📊 Fontes de Dados

-   **Irradiação Solar:** Atlas Brasileiro de Energia Solar (INPE)
-   **Rede Elétrica:** Empresa de Pesquisa Energética (EPE) / ANEEL
-   **Limites Territoriais:** Instituto Brasileiro de Geografia e Estatística (IBGE)
-   **Áreas de Restrição:** Ministério do Meio Ambiente (MMA), FUNAI, INCRA
-   **Topografia (MDE):** Projeto TOPODATA (INPE)

## 👤 Autor

* **João Pedro Pereira**

## 🙏 Agradecimentos

* **Orientador:** Prof. Rodrigo de Souza Couto (GTA/COPPE-UFRJ)