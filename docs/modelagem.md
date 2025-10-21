## Formulação do Problema de Otimização

O problema de selecionar os locais ótimos para a instalação de usinas solares é modelado como um problema de **Programação Inteira Binária**. O objetivo é selecionar um conjunto de áreas candidatas que maximize o potencial de geração de energia (representado pela irradiação solar anual), sujeito a um conjunto de restrições geográficas, ambientais e de infraestrutura.

### Variáveis de Decisão

Para cada área candidata `i` em um conjunto de `N` áreas possíveis, definimos uma variável de decisão binária $x_i$:

$$
x_i = \begin{cases} 
1 & \text{se a área } i \text{ for selecionada para instalação} \\
0 & \text{caso contrário} 
\end{cases}
$$

### Função Objetivo

A função objetivo visa **maximizar** a soma da irradiação solar anual de todas as áreas selecionadas.

$$
\text{Maximizar } Z = \sum_{i=1}^{N} (I_i \cdot x_i)
$$

Onde:
* $Z$: Valor total da irradiação solar das áreas selecionadas.
* $I_i$: Irradiação solar anual média da área candidata `i` (em kWh/m²/ano).

### Restrições

O modelo está sujeito às seguintes restrições:

1.  **Exclusão por Uso do Solo (Restrições Ambientais e Sociais):**
    Nenhuma usina pode ser instalada em áreas de restrição previamente mapeadas (Unidades de Conservação, Terras Indígenas, etc.).

    $$
    x_i = 0, \quad \forall i \in \text{Máscara de Exclusão}
    $$

2.  **Proximidade da Rede Elétrica (Restrição de Infraestrutura):**
    Uma área `i` só pode ser selecionada se estiver dentro de uma distância máxima `d_max` da infraestrutura elétrica existente.

    $$
    x_i = 0, \quad \forall i \notin \text{Buffer da Rede Elétrica}
    $$

3.  **Topografia (Restrição Física):**
    Uma área `i` só pode ser selecionada se a sua declividade média `s_i` for menor ou igual a uma declividade máxima aceitável `s_max`.

    $$
    x_i = 0, \quad \forall i \text{ tal que } s_i > s_{max}
    $$