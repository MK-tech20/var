# 投資期間別相関の数学的説明

Campbell-Viceira論文「The Term Structure of the Risk-Return Tradeoff」における相関の期間依存性の数学的メカニズム

## 1. VARモデルの基本構造

論文では6変数のVAR(1)モデルを使用します：

$$\mathbf{z}_{t+1} = \mathbf{A} \mathbf{z}_t + \boldsymbol{\varepsilon}_{t+1}$$

**変数定義：**
- $\mathbf{z}_t = [r_{tb,t}, r_{s,t}^e, r_{b,t}^e, r_{tb,t}^n, d_t, ys_t]'$ （6×1ベクトル）
- $r_{tb,t}$: 実質T-bill金利
- $r_{s,t}^e$: 株式超過収益率  
- $r_{b,t}^e$: 債券超過収益率
- $r_{tb,t}^n$: 名目T-bill金利
- $d_t$: 対数配当利回り
- $ys_t$: イールドスプレッド
- $\mathbf{A}$: 6×6係数行列
- $\boldsymbol{\varepsilon}_{t+1} \sim N(\mathbf{0}, \boldsymbol{\Sigma})$: 残差ベクトル

## 2. 多期間収益率の導出

### 2.1 基本的な導出

k期間の累積収益率は：

$$\mathbf{z}_{t+k} - \mathbf{z}_t = \sum_{j=1}^{k} (\mathbf{z}_{t+j} - \mathbf{z}_{t+j-1}) = \sum_{j=1}^{k} \boldsymbol{\varepsilon}_{t+j} + \sum_{j=1}^{k} (\mathbf{A} - \mathbf{I}) \mathbf{z}_{t+j-1}$$

### 2.2 VARの解

VARの解を用いると：

$$\mathbf{z}_{t+j} = \mathbf{A}^j \mathbf{z}_t + \sum_{i=0}^{j-1} \mathbf{A}^i \boldsymbol{\varepsilon}_{t+j-i}$$

### 2.3 k期間分散共分散行列

したがって、k期間累積収益率の分散共分散行列は：

$$\mathbf{V}_k = \text{Var}(\mathbf{z}_{t+k} - \mathbf{z}_t) = \sum_{j=0}^{k-1} \mathbf{A}^j \boldsymbol{\Sigma} (\mathbf{A}^j)'$$

この式が**期間構造分析の核心**です。

## 3. 投資期間別相関の計算

### 3.1 相関係数の定義

k期間での資産i と j の相関係数は：

$$\rho_{ij}(k) = \frac{\text{Cov}(R_{i,t+k}, R_{j,t+k})}{\sqrt{\text{Var}(R_{i,t+k}) \cdot \text{Var}(R_{j,t+k})}}$$

ここで$R_{i,t+k}$は資産iのk期間収益率です。

### 3.2 具体的な計算式

分散共分散行列$\mathbf{V}_k$を用いると：

$$\rho_{ij}(k) = \frac{[\mathbf{V}_k]_{ij}}{\sqrt{[\mathbf{V}_k]_{ii} \cdot [\mathbf{V}_k]_{jj}}}$$

### 3.3 年率換算標準偏差

k期間（四半期）の投資での年率標準偏差は：

$$\sigma_i^{annual}(k) = \sqrt{\frac{4 \cdot [\mathbf{V}_k]_{ii}}{k}} \times 100$$

## 4. 相関の期間依存性のメカニズム

### 4.1 短期金利効果（中期での相関上昇）

**株式収益率方程式：**
$$r_{s,t+1}^e = \alpha_s + \beta_{sr} r_{tb,t}^n + \beta_{sd} d_t + \varepsilon_{s,t+1}$$

**債券収益率方程式：**  
$$r_{b,t+1}^e = \alpha_b + \beta_{br} r_{tb,t}^n + \beta_{bys} ys_t + \varepsilon_{b,t+1}$$

**論文のTable 2の重要な係数：**
- $\beta_{sr} = -2.1044$ （株式は短期金利上昇で下落）
- $\beta_{br}$ は間接的に負（債券価格は金利上昇で下落）
- 名目短期金利の自己回帰係数：$\rho_{rr} = 0.9539$ （高い持続性）

**メカニズム：**
名目短期金利の持続性が高いため、短期金利ショックの効果が中期まで持続し、株式と債券の相関を高めます。

### 4.2 配当利回り効果（長期での相関低下）

**配当利回りの持続性：**
$$\rho_{dd} = 0.9613$$

**配当利回りの効果：**
- 株式：$\beta_{sd} = 0.0549 > 0$ （高配当利回り→高株式リターン）
- 債券：間接的に負の効果

**メカニズム：**
長期では配当利回りの影響が支配的となり、相関を低下させます。

## 5. 数値的な相関パターンの展開

### 5.1 一般式

論文の係数を使用すると、k期間相関は次のように展開されます：

$$\rho_{sb}(k) = \frac{\sum_{j=0}^{k-1} [\mathbf{A}^j \boldsymbol{\Sigma} (\mathbf{A}^j)']_{12}}{\sqrt{\sum_{j=0}^{k-1} [\mathbf{A}^j \boldsymbol{\Sigma} (\mathbf{A}^j)']_{11}} \cdot \sqrt{\sum_{j=0}^{k-1} [\mathbf{A}^j \boldsymbol{\Sigma} (\mathbf{A}^j)']_{22}}}$$

### 5.2 期間別の近似

**短期（k=1）での相関：**
残差相関が支配的：$\rho_{sb}(1) \approx 0.119$ （Table 2の残差相関）

**中期（k≈24, 6年）での相関ピーク：**
短期金利効果が累積：
$$\sum_{j=0}^{23} (0.9539)^j \beta_{sr} \beta_{br} \sigma_{r}^2 \approx \text{最大値}$$

**長期（k→∞）での相関：**
配当利回り効果が支配的：
$$\lim_{k \to \infty} \rho_{sb}(k) = \frac{\beta_{sd} \sigma_d^2}{(\beta_{sd}^2 \sigma_d^2)^{1/2} \cdot (\text{bond variance})^{1/2}}$$

## 6. 実証的パターンの数学的説明

### 6.1 観察される山型パターン

論文のFigure 2で観察される特徴的な山型パターン：

| 期間 | 相関 | 支配的要因 |
|------|------|------------|
| **短期（1-4四半期）** | $\rho \approx 20\%$ | 残差相関 |
| **中期（6年頃）** | $\rho \approx 60\%$ | 短期金利効果のピーク |
| **長期（25年）** | $\rho \approx 15\%$ | 配当利回り効果で低下 |

### 6.2 数学的解釈

この山型パターンは以下の数学的構造から生じます：

1. **短期**: $\mathbf{V}_1 \approx \boldsymbol{\Sigma}$ （残差分散共分散行列が支配的）

2. **中期**: $\mathbf{V}_k$ において短期金利の持続的効果が最大化

3. **長期**: $\mathbf{V}_k$ において配当利回りの分散効果が相関を相殺

## 7. 投資への含意

### 7.1 ポートフォリオ分散効果

相関の期間依存性により：

- **短期投資家**: 中程度の分散効果（相関20%）
- **中期投資家**: 限定的な分散効果（相関60%）
- **長期投資家**: 効果的な分散（相関15%）

### 7.2 最適資産配分への影響

$$w_{optimal}(k) = f(\rho_{sb}(k), \sigma_s(k), \sigma_b(k))$$

中期で相関が高くなるため、中期投資家は株式の比重を高める必要があります。

## 8. 結論

投資期間別相関の数学的メカニズムは：

1. **VAR構造**により状態変数の持続性効果が期間により変化
2. **短期金利**の高い持続性が中期で相関を押し上げ
3. **配当利回り**の超長期持続性が長期で相関を押し下げ

この数学的構造により、従来の単一期間ポートフォリオ理論では捉えられない**期間構造**が生まれ、投資期間に応じた戦略の重要性が示されます。

---

*この分析はCampbell and Viceira (2002) "The Term Structure of the Risk-Return Tradeoff"に基づいています。*