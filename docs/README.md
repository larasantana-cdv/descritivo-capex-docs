---
layout:
  width: wide
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
  tags:
    visible: true
  actions:
    visible: true
---

# CAPEX LT - N2

<details>

<summary>DEFINIÇÕES</summary>

Vamos admitir o CAPEX como uma função de três variáveis:&#x20;

* Nível de Tensão.
* Potência.
* Comprimento da Linha.

<p align="center"><span class="math">f: \R^2 \times \N \rightarrow \R  \qquad  f(x,y,z), \in \R\times \N</span>  </p>

$$Potência :: x \in R$$        $$\text{Comprimento da Linha} :: y \in R$$    $$\text{Nível de Tensão} :: z = n  \in \{69,138,230,440,500\}$$

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

</details>

<details>

<summary>MATRIZ DE SENSIBILIDADE</summary>

O objetivo dessa análise é entender o comportamento de cada uma das variáveis em cada um dos itens da composição orçamentária afim de se definir as funções de cálculo. Para cada item, se considera se a variável é afetada (1) ou não (0) mantendo-se as duas outras constantes.&#x20;

<table><thead><tr><th width="163">Item</th><th width="149" data-type="number">x:: Potência</th><th data-type="number">y::Comprimento da Linha</th><th data-type="number">z::Nível de Tensão</th></tr></thead><tbody><tr><td>1.Terrenos</td><td>0</td><td>1</td><td>1</td></tr><tr><td>2.1 Estaiamento</td><td>0</td><td>1</td><td>1</td></tr><tr><td>2.2 Estrutura</td><td>1</td><td>1</td><td>1</td></tr><tr><td>2.3 Isoladores</td><td>0</td><td>1</td><td>1</td></tr><tr><td>2.4 Cabo Condutor</td><td>1</td><td>1</td><td>1</td></tr><tr><td>2.5 Aterramento</td><td>0</td><td>1</td><td>1</td></tr><tr><td>2.6 Cabo Para Raios</td><td>1</td><td>1</td><td>1</td></tr><tr><td>2.7 Ferragens Acessórias</td><td>1</td><td>1</td><td>1</td></tr><tr><td>3.1 Limpeza de Faixa</td><td>0</td><td>1</td><td>1</td></tr><tr><td>3.2 Execução de Fundações</td><td>0</td><td>1</td><td>1</td></tr><tr><td>3.3 Montagem das Estruturas</td><td>0</td><td>1</td><td>1</td></tr><tr><td>3.4 Instalação do Cabo condutor</td><td>1</td><td>1</td><td>1</td></tr><tr><td>3.5 Construção de Acessos</td><td>0</td><td>1</td><td>0</td></tr><tr><td>3.6 Instalação de Aterramento</td><td>1</td><td>1</td><td>0</td></tr><tr><td>4.1 Topografia </td><td>0</td><td>1</td><td>0</td></tr><tr><td>4.2 Geologia e Sondagem</td><td>0</td><td>1</td><td>0</td></tr><tr><td>4.4 Estudos de Engenharia</td><td>0</td><td>1</td><td>0</td></tr><tr><td>4.5 Custos Ambientais </td><td>0</td><td>1</td><td>0</td></tr></tbody></table>

</details>

{% tabs %}
{% tab title="Dimensionamento" %}
[cabo-condutor.md](capex-lt-n2/cabo-condutor.md "mention")
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

