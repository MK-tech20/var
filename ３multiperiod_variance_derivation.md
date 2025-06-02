# k期間累積収益率の分散共分散行列：詳細解説

Campbell-Viceira論文の核心である多期間分散共分散行列の導出を段階的に解説します。

## 1. VARモデルの基本設定

### 1.1 VAR(1)モデル

$$\mathbf{z}_{t+1} = \mathbf{A} \mathbf{z}_t + \boldsymbol{\varepsilon}_{t+1}$$

**前提条件:**
- $\mathbf{z}_t$: n×1状態ベクトル（例：6変数）
- $\mathbf{A}$: n×n係数行列（定数）
- $\boldsymbol{\varepsilon}_{t+1} \sim \text{i.i.d. } N(\mathbf{0}, \boldsymbol{\Sigma})$: 残差ベクトル
- $E[\boldsymbol{\varepsilon}_{t+1} \boldsymbol{\varepsilon}_{s+1}'] = \mathbf{0}$ for $t \neq s$（無相関）

### 1.2 VARの解（フォワード・ソリューション）

j期間先の状態ベクトルは：

$$\mathbf{z}_{t+j} = \mathbf{A}^j \mathbf{z}_t + \sum_{i=0}^{j-1} \mathbf{A}^i \boldsymbol{\varepsilon}_{t+j-i}$$

**直感的説明:**
- 第1項：初期状態$\mathbf{z}_t$がj期間後に与える効果
- 第2項：各期のショック$\boldsymbol{\varepsilon}$の累積効果

## 2. k期間累積収益率の定義

### 2.1 累積収益率

k期間の累積収益率は：

$$\mathbf{R}_{t,k} = \mathbf{z}_{t+k} - \mathbf{z}_t$$

これは「時刻tから時刻t+kまでの総収益率」を表します。

### 2.2 累積収益率の展開

VARの解を代入すると：

$$\mathbf{R}_{t,k} = \mathbf{z}_{t+k} - \mathbf{z}_t = \mathbf{A}^k \mathbf{z}_t + \sum_{i=0}^{k-1} \mathbf{A}^i \boldsymbol{\varepsilon}_{t+k-i} - \mathbf{z}_t$$

$$= (\mathbf{A}^k - \mathbf{I}) \mathbf{z}_t + \sum_{i=0}^{k-1} \mathbf{A}^i \boldsymbol{\varepsilon}_{t+k-i}$$

## 3. 分散共分散行列の導出

### 3.1 期待値の計算

累積収益率の期待値は：

$$E[\mathbf{R}_{t,k}] = (\mathbf{A}^k - \mathbf{I}) E[\mathbf{z}_t] + \sum_{i=0}^{k-1} \mathbf{A}^i E[\boldsymbol{\varepsilon}_{t+k-i}]$$

$E[\boldsymbol{\varepsilon}_{t+j}] = \mathbf{0}$なので：

$$E[\mathbf{R}_{t,k}] = (\mathbf{A}^k - \mathbf{I}) E[\mathbf{z}_t]$$

### 3.2 分散共分散行列の核心導出

累積収益率の分散共分散行列は：

$$\text{Var}(\mathbf{R}_{t,k}) = E[(\mathbf{R}_{t,k} - E[\mathbf{R}_{t,k}])(\mathbf{R}_{t,k} - E[\mathbf{R}_{t,k}])']$$

$\mathbf{R}_{t,k} - E[\mathbf{R}_{t,k}]$を計算すると：

$$\mathbf{R}_{t,k} - E[\mathbf{R}_{t,k}] = (\mathbf{A}^k - \mathbf{I})(\mathbf{z}_t - E[\mathbf{z}_t]) + \sum_{i=0}^{k-1} \mathbf{A}^i \boldsymbol{\varepsilon}_{t+k-i}$$

### 3.3 無条件分散の場合

定常状態（$E[\mathbf{z}_t] = \boldsymbol{\mu}$が定数）を仮定すると：

$$\text{Var}(\mathbf{R}_{t,k}) = \text{Var}\left(\sum_{i=0}^{k-1} \mathbf{A}^i \boldsymbol{\varepsilon}_{t+k-i}\right)$$

**重要なポイント:** 残差項のみが確率的なので、分散はこの項だけから生じます。

## 4. 最終的な分散共分散行列の公式

### 4.1 主要結果

$$\mathbf{V}_k = \text{Var}(\mathbf{R}_{t,k}) = \sum_{j=0}^{k-1} \mathbf{A}^j \boldsymbol{\Sigma} (\mathbf{A}^j)'$$

### 4.2 導出の詳細

各項が独立なので：

$$\text{Var}\left(\sum_{i=0}^{k-1} \mathbf{A}^i \boldsymbol{\varepsilon}_{t+k-i}\right) = \sum_{i=0}^{k-1} \text{Var}(\mathbf{A}^i \boldsymbol{\varepsilon}_{t+k-i})$$

各項の分散は：

$$\text{Var}(\mathbf{A}^i \boldsymbol{\varepsilon}_{t+k-i}) = \mathbf{A}^i \text{Var}(\boldsymbol{\varepsilon}_{t+k-i}) (\mathbf{A}^i)' = \mathbf{A}^i \boldsymbol{\Sigma} (\mathbf{A}^i)'$$

したがって：

$$\mathbf{V}_k = \sum_{i=0}^{k-1} \mathbf{A}^i \boldsymbol{\Sigma} (\mathbf{A}^i)'$$

## 5. 直感的理解

### 5.1 各項の意味

- **$\mathbf{A}^0 \boldsymbol{\Sigma} (\mathbf{A}^0)' = \boldsymbol{\Sigma}$**: 直近のショックの影響
- **$\mathbf{A}^1 \boldsymbol{\Sigma} (\mathbf{A}^1)'$**: 1期前のショックが今期に与える影響
- **$\mathbf{A}^{k-1} \boldsymbol{\Sigma} (\mathbf{A}^{k-1})'$**: k-1期前のショックの累積影響

### 5.2 累積効果のメカニズム

各期のショック$\boldsymbol{\varepsilon}_{t+j}$は：
1. その期の収益率に直接影響（係数1）
2. 次期以降の収益率に間接影響（係数$\mathbf{A}, \mathbf{A}^2, ...$）

**例：3期間の場合（k=3）**

$$\mathbf{V}_3 = \boldsymbol{\Sigma} + \mathbf{A}\boldsymbol{\Sigma}\mathbf{A}' + \mathbf{A}^2\boldsymbol{\Sigma}(\mathbf{A}^2)'$$

- 第1項：各期の直接的ショック
- 第2項：1期遅れの間接効果
- 第3項：2期遅れの間接効果

## 6. 重要な性質

### 6.1 単調性

一般に$\mathbf{V}_{k+1} - \mathbf{V}_k = \mathbf{A}^k \boldsymbol{\Sigma} (\mathbf{A}^k)' \geq 0$（半正定値）

つまり、期間が長くなるほど分散は増加（または一定）。

### 6.2 収束性

$\mathbf{A}$の固有値が全て1未満の場合：

$$\lim_{k \to \infty} \frac{\mathbf{V}_k}{k} = \text{有限値}$$

### 6.3 短期極限

$k=1$の場合：$\mathbf{V}_1 = \boldsymbol{\Sigma}$（1期間の残差分散）

## 7. 年率換算の標準偏差

### 7.1 年率化の公式

k期間（四半期）の年率標準偏差：

$$\sigma_{i,annual}(k) = \sqrt{\frac{4 \cdot [\mathbf{V}_k]_{ii}}{k}} \times 100$$

### 7.2 意味

- **分子**: k期間の累積分散を年率化（×4）
- **分母**: 期間数kで割って平均化
- **全体**: 1年あたりの平均的リスク

## 8. 具体例：2変数VARの場合

### 8.1 設定

$$\mathbf{z}_t = \begin{bmatrix} r_{s,t} \\ r_{b,t} \end{bmatrix}, \quad \mathbf{A} = \begin{bmatrix} 0.1 & 0.2 \\ 0.3 & 0.4 \end{bmatrix}, \quad \boldsymbol{\Sigma} = \begin{bmatrix} 0.01 & 0.005 \\ 0.005 & 0.008 \end{bmatrix}$$

### 8.2 k=2の計算

$$\mathbf{A}^0 = \mathbf{I} = \begin{bmatrix} 1 & 0 \\ 0 & 1 \end{bmatrix}$$

$$\mathbf{A}^1 = \begin{bmatrix} 0.1 & 0.2 \\ 0.3 & 0.4 \end{bmatrix}$$

$$\mathbf{V}_2 = \boldsymbol{\Sigma} + \mathbf{A}\boldsymbol{\Sigma}\mathbf{A}'$$

計算すると：

$$\mathbf{A}\boldsymbol{\Sigma}\mathbf{A}' = \begin{bmatrix} 0.1 & 0.2 \\ 0.3 & 0.4 \end{bmatrix} \begin{bmatrix} 0.01 & 0.005 \\ 0.005 & 0.008 \end{bmatrix} \begin{bmatrix} 0.1 & 0.3 \\ 0.2 & 0.4 \end{bmatrix}$$

$$= \begin{bmatrix} 0.002 & 0.0013 \\ 0.005 & 0.0047 \end{bmatrix} \begin{bmatrix} 0.1 & 0.3 \\ 0.2 & 0.4 \end{bmatrix} = \begin{bmatrix} 0.00046 & 0.00112 \\ 0.00144 & 0.00338 \end{bmatrix}$$

したがって：

$$\mathbf{V}_2 = \begin{bmatrix} 0.01 & 0.005 \\ 0.005 & 0.008 \end{bmatrix} + \begin{bmatrix} 0.00046 & 0.00112 \\ 0.00144 & 0.00338 \end{bmatrix} = \begin{bmatrix} 0.01046 & 0.00612 \\ 0.00644 & 0.01138 \end{bmatrix}$$

## 9. 実用上の重要性

### 9.1 リスク管理

この公式により：
- 投資期間に応じたリスク測定が可能
- 長期投資の本当のリスクを定量化
- 期間構造を考慮したポートフォリオ最適化

### 9.2 予測可能性の効果

- **平均回帰**: $\mathbf{A}$の要素が負 → 長期分散減少
- **平均回避**: $\mathbf{A}$の要素が正で大 → 長期分散増加
- **相関変化**: 非対角要素が期間とともに影響

## 10. まとめ

$$\boxed{\mathbf{V}_k = \sum_{j=0}^{k-1} \mathbf{A}^j \boldsymbol{\Sigma} (\mathbf{A}^j)'}$$

この公式は：
1. **各期のショックの累積効果**を表現
2. **VAR係数の持続性**を通じて期間構造を生成
3. **投資期間別リスク分析**の基礎を提供
4. **Campbell-Viceira論文の核心**となる数学的結果

VARモデルの動学的性質が、この単純に見える公式を通じて、投資期間に依存する複雑なリスク構造を生み出しているのです。