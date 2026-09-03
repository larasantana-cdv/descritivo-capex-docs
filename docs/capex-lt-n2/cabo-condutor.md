# Cabo Condutor

Para a escolha do cabo, vamos observar 4 critérios:

* Capacidade Térmica (Ampacidade)
* Queda de Tensão Permitida
* Limite do Efeito Corona (se tensão $$ $\ge 138\text{ kV}$ $$)
* Análise Econômica de Custo Mínimo (menor custo global: CAPEX + Perdas)

### Passo 1: Calcular a Corrente de Projeto (I\_proj)

$$
I_{proj} = \frac{P}{ \sqrt{3} \cdot  V \cdot  cos_{\phi}\cdot \text{Fator de Segurança}}
$$

### Passo 2: Filtrar Cabos pela Ampacidade (Critério Térmico)

Para cada cabo no catálogo: Calcular a Ampacidade Máxima (I\_max) usando a equação de balanço térmico (IEEE 738):&#x20;

$$
I_{max} = \frac{Q_c + Q_r  - Q_s}{R(T_c)}
$$

1\. **Ganho de Calor Solar** $$Q_s$$

$$
Q_s = \alpha \cdot D \cdot Q_{se}
$$

* $$\alpha:$$ Taxa de absortividade solar do condutor.
* D: Diâmetro externo do condutor (m)&#x20;
* $$Q_{se}$$: Radiação solar total corrigida (W/m²)

2.2. Perda de Calor por Convecção  $$Q_C$$

```
    Se I_max < I_proj:
        REJEITAR CABO (Superaquece)
```

### Passo 3: Filtrar pela Queda de Tensão

Para cada cabo aprovado no Passo 2: Calcular Queda de Tensão: Delta\_V = sqrt(3) \* I\_proj \* L \* (R\_ac \* cos\_phi + X \* sin\_phi) Delta\_V\_perc = (Delta\_V / V) \* 100 $$f(x) = x * e^{2 pi i \xi x}$$![](<../.gitbook/assets/image (2).png>)

```
Se Delta_V_perc > Delta_V_max:
    REJEITAR CABO (Queda de tensão excessiva)
```

### Passo 4: Filtrar pelo Gradiente de Campo Elétrico (Efeito Corona)

Se V >= 138 kV: Para cada cabo/arranjo aprovado: Calcular Gradiente de Campo Elétrico na superfície (E\_max) Se E\_max > Gradiente de Disruptura do Ar (E\_crit, aprox. 21-24 kV/cm): OPÇÃO A: Aumentar o número de subcondutores por fase (Feixe) OPÇÃO B: REJEITAR CABO

### Passo 5: Análise Econômica (Escolha Final)

Para cada cabo/arranjo APROVADO nos passos anteriores: 1. Custo de Aquisição = Custo do Cabo/km \* 3 \* L \* N\_subcondutores 2. Custo das Perdas Anuais = 3 \* (I\_proj^2) \* R\_ac \* L \* 8760 horas \* Fator\_de\_Carga \* Custo\_MWh 3. Custo Total em Valor Presente (VPL) = Custo de Aquisição + VPL(Custo das Perdas em N anos)

RETORNAR: Cabo/Arranjo com MENOR CUSTO TOTAL EM VPL
