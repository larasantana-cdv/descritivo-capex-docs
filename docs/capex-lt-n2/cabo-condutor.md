# Algoritmo de Dimensionamento de Cabos para Linhas de Transmissão

Pseudocódigo estruturado em 5 etapas sucessivas de filtragem, partindo da corrente de projeto até a escolha econômica final entre os cabos/arranjos aprovados.

---

## Passo 1 — Cálculo da Corrente de Projeto

$$
I_{\text{proj}} = \frac{P}{\sqrt{3} \cdot V \cdot \cos\phi} \cdot F_{\text{seg}}
$$

**Onde:**
- $P$ = potência a transmitir (W ou MW)
- $V$ = tensão nominal de linha (V ou kV)
- $\cos\phi$ = fator de potência
- $F_{\text{seg}}$ = fator de segurança (ex.: $1{,}15$), para acomodar crescimento de carga

---

## Passo 2 — Filtro por Ampacidade (Critério Térmico)

Para **cada cabo do catálogo**, calcular a ampacidade máxima $I_{\max}$ a partir do balanço térmico (IEEE Std 738), no qual o calor gerado deve igualar o calor dissipado:

$$
Q_{\text{Joule}} + Q_s = Q_c + Q_r
$$

$$
(I^2 \cdot R) + Q_s = Q_c + Q_r
$$

**Onde:**
- $Q_{\text{Joule}} = I^2 \cdot R$ = aquecimento por efeito Joule
- $Q_s$ = ganho de calor por radiação solar
- $Q_c$ = perda de calor por convecção
- $Q_r$ = perda de calor por radiação

Isolando $I$ na equação, obtém-se $I_{\max}$ para a temperatura máxima admissível do condutor.

#### 2.1 Ganho de Calor Solar ($Q_s$)

$$
Q_s = \alpha \cdot Q_{se} \cdot \sin(\theta) \cdot A'
$$

**Onde:**
- $\alpha$ = coeficiente de absortividade solar do condutor ($0{,}23$ para cabo novo a $0{,}91$ para cabo envelhecido/enegrecido)
- $Q_{se}$ = fluxo de calor solar e do céu, corrigido pela altitude do local (tabelado na norma em função da altitude solar $H_c$)
- $\theta$ = ângulo efetivo de incidência dos raios solares sobre o condutor
- $A'$ = área projetada do condutor por unidade de comprimento (numericamente igual ao diâmetro externo $D$)

O ângulo de incidência é obtido por:

$$
\cos(\theta) = \cos(H_c) \cdot \cos(Z_c - Z_l)
$$

**Onde:**
- $H_c$ = altitude solar (ângulo do sol acima do horizonte)
- $Z_c$ = azimute solar
- $Z_l$ = azimute da linha de transmissão

#### 2.2 Perda de Calor por Convecção ($Q_c$)

Calculam-se três hipóteses e adota-se o **maior valor** entre elas (convecção natural e as duas formas de convecção forçada):

**Convecção natural** (sem vento):

$$
Q_{cn} = 3{,}645 \cdot \rho_f^{0{,}5} \cdot D^{0{,}75} \cdot (T_c - T_a)^{1{,}25}
$$

**Convecção forçada — baixa velocidade** (Reynolds baixo):

$$
Q_{c1} = K_{\text{angle}} \cdot \left[ 1{,}01 + 1{,}35 \cdot Re^{0{,}52} \right] \cdot k_f \cdot (T_c - T_a)
$$

**Convecção forçada — alta velocidade** (Reynolds alto):

$$
Q_{c2} = K_{\text{angle}} \cdot 0{,}754 \cdot Re^{0{,}6} \cdot k_f \cdot (T_c - T_a)
$$

$$
Q_c = \max(Q_{cn},\ Q_{c1},\ Q_{c2})
$$

**Onde (número de Reynolds e fator de direção do vento):**

$$
Re = \frac{D \cdot \rho_f \cdot V_w}{\mu_f}
$$

$$
K_{\text{angle}} = 1{,}194 - \cos(\phi) + 0{,}194 \cdot \cos(2\phi) + 0{,}368 \cdot \sin(2\phi)
$$

- $D$ = diâmetro externo do condutor
- $\rho_f$ = densidade do ar (função da temperatura de filme e altitude)
- $V_w$ = velocidade do vento
- $\mu_f$ = viscosidade dinâmica do ar
- $k_f$ = condutividade térmica do ar
- $\phi$ = ângulo entre a direção do vento e o eixo do condutor
- $T_c$ = temperatura do condutor (máxima admissível)
- $T_a$ = temperatura ambiente

#### 2.3 Perda de Calor por Radiação ($Q_r$)

$$
Q_r = 17{,}8 \cdot D \cdot \varepsilon \cdot \left[ \left( \frac{T_c + 273}{100} \right)^4 - \left( \frac{T_a + 273}{100} \right)^4 \right]
$$

**Onde:**
- $\varepsilon$ = coeficiente de emissividade do condutor ($0{,}23$ cabo novo a $0{,}91$ cabo envelhecido)
- $D$, $T_c$, $T_a$ = conforme definidos acima

> ⚠️ **Atenção na implementação:** as constantes numéricas de $Q_c$, $Q_r$ e $Q_s$ variam conforme o sistema de unidades adotado (SI ou unidades inglesas) e a edição da norma IEEE Std 738 utilizada. Antes de codificar, confirme as constantes, unidades de entrada ($D$ em m/mm/pol, $V_w$ em m/s/ft/s etc.) e as tabelas de $Q_{se}$ na versão específica da norma que será referenciada no projeto.

**Critério de rejeição:**

```
PARA CADA cabo NO catálogo:
    CALCULAR I_max (via balanço térmico IEEE 738)
    SE I_max < I_proj ENTÃO:
        REJEITAR CABO (superaquece)
    SENÃO:
        APROVAR CABO → prosseguir para o Passo 3
```

---

## Passo 3 — Filtro por Queda de Tensão

Para **cada cabo aprovado no Passo 2**, calcular a queda de tensão absoluta e percentual:

$$
\Delta V = \sqrt{3} \cdot I_{\text{proj}} \cdot L \cdot \left( R_{\text{ac}} \cdot \cos\phi + X \cdot \sin\phi \right)
$$

$$
\Delta V_{\%} = \left( \frac{\Delta V}{V} \right) \cdot 100
$$

**Onde:**
- $L$ = comprimento da linha (km)
- $R_{\text{ac}}$ = resistência CA do condutor ($\Omega$/km)
- $X$ = reatância indutiva do condutor ($\Omega$/km)

**Critério de rejeição:**

```
PARA CADA cabo APROVADO no Passo 2:
    CALCULAR ΔV e ΔV_%
    SE ΔV_% > ΔV_max ENTÃO:
        REJEITAR CABO (queda de tensão excessiva)
    SENÃO:
        APROVAR CABO → prosseguir para o Passo 4
```

---

## Passo 4 — Filtro pelo Gradiente de Campo Elétrico (Efeito Corona)

Aplicável apenas a linhas de alta tensão, onde o efeito corona se torna relevante.

```
SE V >= 138 kV ENTÃO:
    PARA CADA cabo/arranjo APROVADO no Passo 3:
        CALCULAR gradiente de campo elétrico na superfície (E_max)

        SE E_max > E_crit (aprox. 21 a 24 kV/cm) ENTÃO:
            OPÇÃO A: aumentar o número de subcondutores por fase (feixe/bundle)
                     → recalcular E_max com o novo arranjo
            OPÇÃO B: REJEITAR CABO
        SENÃO:
            APROVAR CABO/ARRANJO → prosseguir para o Passo 5
SENÃO:
    APROVAR CABO/ARRANJO DIRETAMENTE → prosseguir para o Passo 5
```

---

## Passo 5 — Análise Econômica (Escolha Final)

Para **cada cabo/arranjo aprovado** nos passos anteriores, calcular o custo do ciclo de vida.

### 5.1 Custo de Aquisição (CAPEX)

$$
\text{Custo}_{\text{aquisição}} = \text{Custo}_{\text{cabo/km}} \cdot 3 \cdot L \cdot N_{\text{subcondutores}}
$$

**Onde:**
- $N_{\text{subcondutores}}$ = número de subcondutores por fase (feixe)
- O fator $3$ representa as três fases do sistema trifásico

### 5.2 Custo das Perdas Anuais (OPEX)

$$
\text{Custo}_{\text{perdas/ano}} = 3 \cdot I_{\text{proj}}^2 \cdot R_{\text{ac}} \cdot L \cdot 8760 \cdot F_{\text{carga}} \cdot \text{Custo}_{\text{MWh}}
$$

**Onde:**
- $8760$ = número de horas em um ano
- $F_{\text{carga}}$ = fator de carga médio anual
- $\text{Custo}_{\text{MWh}}$ = custo unitário da energia

### 5.3 Custo Total em Valor Presente Líquido (VPL)

$$
\text{VPL}_{\text{total}} = \text{Custo}_{\text{aquisição}} + \text{VPL}\left(\text{Custo}_{\text{perdas/ano}},\ N \text{ anos}\right)
$$

**Onde:**
- $N$ = horizonte de análise (vida útil do projeto, em anos)
- $\text{VPL}(\cdot)$ traz o fluxo anual de perdas a valor presente, considerando a taxa de desconto do projeto

**Critério de decisão:**

```
PARA CADA cabo/arranjo APROVADO nos Passos 2, 3 e 4:
    CALCULAR Custo_aquisição
    CALCULAR Custo_perdas/ano
    CALCULAR VPL_total

RETORNAR: cabo/arranjo com MENOR CUSTO TOTAL EM VPL
```

---

## Fluxo Resumido do Algoritmo

```
INÍCIO
  │
  ├─► Passo 1: Calcular I_proj
  │
  ├─► Passo 2: Filtro térmico (I_max vs I_proj)
  │       └─ Rejeita cabos que superaquecem
  │
  ├─► Passo 3: Filtro de queda de tensão (ΔV_% vs ΔV_max)
  │       └─ Rejeita cabos com queda excessiva
  │
  ├─► Passo 4: Filtro de efeito corona (E_max vs E_crit), se V ≥ 138 kV
  │       └─ Rejeita ou exige feixe de subcondutores
  │
  ├─► Passo 5: Análise econômica (VPL de CAPEX + OPEX)
  │       └─ Compara todos os cabos/arranjos aprovados
  │
  └─► FIM: Retorna cabo/arranjo de menor VPL_total
```
