## Formulação dos Problemas de Otimização

O problema de selecionar os locais ótimos para a instalação de usinas solares é modelado como um problema de **Programação Inteira Binária (PIB)**. A metodologia foi dividida em duas grandes fases:

1.  **Pré-processamento:** Uma fase de filtragem geoespacial para definir um universo de áreas candidatas viáveis.
2.  **Otimização:** A aplicação de modelos de PIB para selecionar o subconjunto ótimo de áreas desse universo.

---

### 1. Definição do Universo de Candidatas (Pré-processamento)

Antes da execução do solver, um universo $I$ de áreas candidatas foi definido. Este conjunto **não** é o Brasil inteiro. Uma área $i$ (representando um pixel de 1km²) só pertence a $I$ se ela atender a **todas** as seguintes condições:

1.  **Exclusão Legal/Social:** A área $i$ **não** pode estar sobre nenhuma camada do `mascara_exclusao_total.tif` (que inclui UCs, TIs, Quilombolas, Assentamentos e Sítios Arqueológicos).
2.  **Exclusão Física (Topografia):** A declividade média da área $i$ deve ser **menor ou igual a 5 graus**. (Esta condição também já está embutida no `mascara_exclusao_total.tif`).
3.  **Exclusão de Viabilidade Econômica:** A irradiação solar média na área $i$ deve ser **maior ou igual a 5,0 kWh/m²/dia**. (Baseado na referência metodológica de Saccardo, 2024).

O solver de otimização irá operar **apenas** sobre o conjunto de áreas $i \in I$ que passaram por todos esses filtros.

---

### 2. Parâmetros e Variáveis de Decisão

Para cada área candidata $i \in I$:

**Parâmetros (Dados de Entrada):**
* **$A_i$**: A área do pixel $i$. Assumido como $A_i = 1$ (unidade de 1km²).
* **$P_i$**: O Potencial de Geração da área $i$ (em GW), calculado a partir do raster de irradiação solar e um fator de eficiência de conversão.
* **$C_i$**: O Custo de Instalação na área $i$ (em R$). Será calculado como:
    * $C_i = C_{fixo} + (K_{rede} \cdot d_i)$
    * Onde $C_{fixo}$ é o custo CAPEX (equipamentos) e $K_{rede}$ é o custo por km de linha de transmissão, multiplicado pela distância $d_i$ (em km) da área $i$ até a rede mais próxima.

**Variável de Decisão (Saída do Modelo):**
* **$x_i$**: Uma variável binária, onde:
    * $x_i = 1$ se uma usina for construída na área $i$.
    * $x_i = 0$ caso contrário.

---

### 3. Formulação: Modelo 1 (Base - "Simples")

Este modelo servirá como linha de base e para a disciplina de Otimização, conforme sugerido pelo orientador ("começar simples").

* **Objetivo:** Maximizar o potencial de geração total.
* **Restrição:** Limite máximo de área total a ser instalada.

$$
\begin{align*}
\text{Maximizar } Z &= \sum_{i \in I} (P_i \cdot x_i) \\
\text{Sujeito a:} \\
\sum_{i \in I} (A_i \cdot x_i) &\leq \text{AreaMaxima} && \text{(Restrição de Orçamento de Área)} \\
x_i &\in \{0, 1\} && \forall i \in I
\end{align*}
$$

---

### 4. Formulação: Modelo 2 (Avançado - "Incremento")

Este modelo representa a proposta final do TCC, alinhada a objetivos de política pública.

* **Objetivo:** Minimizar o custo total de instalação.
* **Restrição:** Atingir a meta de expansão de geração solar definida no **PDE 2034** (25,8 GW).

$$
\begin{align*}
\text{Minimizar } Z &= \sum_{i \in I} (C_i \cdot x_i) \\
\text{Sujeito a:} \\
\sum_{i \in I} (P_i \cdot x_i) &\geq 25,8 && \text{(Restrição de Meta de Geração)} \\
x_i &\in \{0, 1\} && \forall i \in I
\end{align*}
$$