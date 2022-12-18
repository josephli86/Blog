---
author: Joseph Li
categories:
- Bond
date: "2022-12-17"
slug: another-note
tags:
- Bond
title: 固定收益投资组合绩效归因
---

## Campisi

Stephen Campisi在2000年发表的《Primer on Fixed Income Performance
Attribution》一文中提出了专门用于纯债型基金业绩归因分析的Campisi模型。

Campisi模型将纯债基金的收益分解为：

- 票息收入(income effect)
- 债券价格变化(price effect)
  - 国债效应(treasury effect)
  - 利差效应(spread effect)
  - 择券效应(selection effect)

### 单只债券的收益率分解

#### 单期收益率的分解

考虑买入单只债券并持有一段时间后卖出，得到的总收益为期间的票息收益率和价格收益率。

$$
R_{t_1, t_0} = \frac{Int_{t_1, \ t_0}}{DP_{t_0} + NewBuy_{t_1}} + \frac{\Delta CP_{t_1,\ t_0}}{DP_{t_0} + NewBuy_{t_1}}
$$

<div>

> **关于收益率分母选择**
>
> 对于公募基金，从投资人角度，其在任一时间点申购债券基金，其购买成本是申购确认日资产净值，即申购日基金组合的债券净价市值加上累计应计利息。应此，分母的选择应当使用盯市价值，而不选择债券净价，也不选择债券购入成本。
>
> 但从投资经理角度，或从债券买卖角度来看，计算收益率实际上是应该用债券净价做为分母计算收益率。

</div>

根据债券定价公司，价格变化可以约等于债券到期收益率的变化乘以久期。

<span id="eq-bond-valuation">$$
\frac{\partial P}{\partial y} \approx - D \times \Delta y
= - D \times (\Delta y_{treasury} + \Delta y_{credit spread}) 
 \qquad(1)$$</span>

所以，单之债券的收益分解即为：

- Income Effect：票息收益率
- Treasury Effect:：国债收益率变化\*久期
- Spread Effect：利差收益率变化\*久期
- Selection Effect：需要有基准

#### 多期收益率的拆解

假设单只债券每个交易日收益率为$r_{t}$,则在每一个计算日，债券的收益率均可以拆解

<span id="eq-r-breakdown">$$
r_{t} = r_{int, \ t} + r_{treasury, \ t} + r_{spread, \ t}
 \qquad(2)$$</span>

对于一个观察区间，即$t \in \{1,2,3,...,N\}$，其区间收益率

<span id="eq-period-return">$$
1 + r_{1,\ N} = \prod_{t = 1}^{N} (1 + r_{t})
 \qquad(3)$$</span>

为了进一步将公式右侧的收益率分解、归因到每一个因子上，引入**Carino**算法来连接多期归因算法，使得分解收益具有可加性。

<span id="eq-period-return-trans">$$
\ln_{}{(1 + r_{1,\ N})} = \sum_{t = 1}^{N}\ln_{}{(1 + r_{t})} 
 \qquad(4)$$</span>

<span id="eq-re">$$
\frac{\ln_{}{(1 + r_{1,\ N})}}{r_{1,\ N}}r_{1,\ N} = \sum_{t = 1}^{N}\frac{\ln_{}{(1 + r_{t})}}{r_{t}}r_{t}
 \qquad(5)$$</span>

令$k = \frac{\ln_{}{(1 + r_{1,\ N})}}{r_{1,\ N}}$，$k_{t} = \frac{\ln_{}{(1 + r_{t})}}{r_{t}}$，则
([Equation 5](#eq-re))

<span id="eq-proof">$$
\begin{aligned}
r_{1,\ N}  & = \sum_{t=1}^{N}\frac{k_{t}}{k} r_{t} \\
           & = \sum_{t=1}^{N}\frac{k_{t}}{k}(r_{int, \ t} + r_{treasury, \ t} + r_{spread, \ t}) \\
           & =\sum_{t=1}^{N}\frac{k_{t}}{k}r_{int, \ t} + \sum_{t=1}^{N}\frac{k_{t}}{k}r_{treasury, \ t} + \sum_{t=1}^{N}\frac{k_{t}}{k}r_{spread, \ t}
\end{aligned}
 \qquad(6)$$</span>

<div>

> **Note**
>
> 实际上，国债效应、利差效应还可以进一步分解，细化，或按照特定维度进行分解。

</div>

### 基准组合的收益率分解

债券投资基准组合的收益率分散，可以参照各成分债券相应的收益
率分解方法获得。但一般来说债券指数的成分券数量较多，利差效应一般采用总收益率减去收入效应和国债效应的方法倒算得到。

### 债券组合的绩效分析

## 实证案例

### 单券层面收益率计算

在公司实务中，目前单只债券的收益主要拆分及计算方式为：

- 票息收入：以票面利率根据计息规则计算的应计利息
- 估值变动：以中债/中证估值计算的日终估值净价计算估值变动
- 交易损益：卖出价与当日中债估值之间的差

对应的，其收益率的计算公式为：

$$
r_{t_1} = \frac{票息收入_{t_1}}{资产净值_{t_0} + 买入_{t_1}} + \frac{估值变动_{t_1}}{资产净值_{t_0} + 买入_{t_1}} + \frac{交易变动_{t_1}}{资产净值_{t_0} + 买入_{t_1}}
$$

<div>

> **关于交易损益**
>
> 目前，公司系统仅针对卖出债券交易，按照交易净价减去当日中债估值来计算交易损益。而对于买入交易，却未按照同样的逻辑将买入拆分成估值变动、交易损益。这种处理方式存在较大问题，没有真实反映所谓的”交易损益”。应当就买入交易也同样采取类似计算方法。即：
>
> $$
> 中债估值_{t_1} - 买入成本_{t_1} = (中债估值_{t_1} - 中债估值_{t_0}) + (中债估值_{t_0} - 买入成本_{t_1}) 
> $$
>
> 这种处理方法，实际上是将交易损益锚定在中债估值上。衡量的是相对于中债估值曲线上的曲线交易能力。

</div>

### 组合层面收益率计算

## References

1.  Multiperiod Arithmetic Attribution, Jose Menchero, 2004
2.  Primer on Fixed Income Performance Attribution
3.  Practical Portfolio Performance Measurement and Attribution, 2rd
    Edition
