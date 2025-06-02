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

### 3.1 条件付き vs 無条件分散の重要な区別

**重要:** Campbell-Viceira論文では**条件付き分散**を使用しています。

#### 条件付き分散（論文で使用）
投資家が時刻tで$\mathbf{z}_t$を観察した条件での分散：

$\mathbf{V}_k = \text{Var}(\mathbf{R}_{t,k} | \mathbf{z}_t)$

#### 無条件分散（一般的なケース）
$\mathbf{z}_t$も確率変数として扱う分散：

$\text{Var}(\mathbf{R}_{t,k}) = E[\text{Var}(\mathbf{R}_{t,k} | \mathbf{z}_t)] + \text{Var}(E[\mathbf{R}_{t,k} | \mathbf{z}_t])$

### 3.2 条件付き分散の導出

時刻tで$\mathbf{z}_t$が既知の場合：

$\mathbf{R}_{t,k} | \mathbf{z}_t = (\mathbf{A}^k - \mathbf{I}) \mathbf{z}_t + \sum_{i=0}^{k-1} \mathbf{A}^i \boldsymbol{\varepsilon}_{t+k-i}$

**条件付き期待値:**
$E[\mathbf{R}_{t,k} | \mathbf{z}_t] = (\mathbf{A}^k - \mathbf{I}) \mathbf{z}_t$

**なぜなら:** $E[\boldsymbol{\varepsilon}_{t+j} | \mathbf{z}_t] = \mathbf{0}$ for all $j > 0$

**条件付き偏差:**
$\mathbf{R}_{t,k} - E[\mathbf{R}_{t,k} | \mathbf{z}_t] = \sum_{i=0}^{k-1} \mathbf{A}^i \boldsymbol{\varepsilon}_{t+k-i}$

**重要なポイント:** $(\mathbf{A}^k - \mathbf{I}) \mathbf{z}_t$項は条件づけにより**確定的**になるため、分散に寄与しません。

### 3.3 条件付き分散共分散行列

$\text{Var}(\mathbf{R}_{t,k} | \mathbf{z}_t) = \text{Var}\left(\sum_{i=0}^{k-1} \mathbf{A}^i \boldsymbol{\varepsilon}_{t+k-i} \bigg| \mathbf{z}_t\right)$

残差項は$\mathbf{z}_t$と独立なので：

$= \text{Var}\left(\sum_{i=0}^{k-1} \mathbf{A}^i \boldsymbol{\varepsilon}_{t+k-i}\right) = \sum_{i=0}^{k-1} \mathbf{A}^i \boldsymbol{\Sigma} (\mathbf{A}^i)'$

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

## 5. 条件付け vs 無条件の詳細比較

### 5.1 なぜ条件付き分散が重要か

**投資の文脈:**
- 投資家は時刻tで**現在の市場状況$\mathbf{z}_t$を観察**して投資決定を行う
- 関心があるのは「現在の状況を前提とした将来のリスク」
- つまり$\text{Var}(\mathbf{R}_{t,k} | \mathbf{z}_t)$が適切

**具体例:**
- 現在の配当利回りが3%、短期金利が2%の状況で
- 「今後5年間の株式リターンのリスクは？」
- この質問に答えるには条件付き分散が必要

### 5.2 無条件分散の場合（参考）

$\mathbf{z}_t$も確率変数として扱う場合：

$\text{Var}(\mathbf{R}_{t,k}) = \text{Var}((\mathbf{A}^k - \mathbf{I}) \mathbf{z}_t) + \text{Var}\left(\sum_{i=0}^{k-1} \mathbf{A}^i \boldsymbol{\varepsilon}_{t+k-i}\right)$

第1項が追加され、より複雑になります。

### 5.3 実用上の含意

| 分散の種類 | 用途 | 解釈 |
|-----------|------|------|
| **条件付き分散** | 投資戦略、リスク管理 | 現在の市況下での将来リスク |
| **無条件分散** | 長期統計分析 | 歴史的平均的なリスク |

Campbell-Viceira論文では投資家の意思決定が焦点なので、条件付き分散を使用します。

## 6. 直感的理解

### 6.1 条件付き分散の直感

**現在時刻tで$\mathbf{z}_t$を観察済み** → この情報を前提とした将来の不確実性のみが問題

各項の意味：
- **$\mathbf{A}^0 \boldsymbol{\Sigma} (\mathbf{A}^0)' = \boldsymbol{\Sigma}$**: 次期(t+1)のショックの直接影響
- **$\mathbf{A}^1 \boldsymbol{\Sigma} (\mathbf{A}^1)'$**: 次期のショックがt+2期に与える間接影響
- **$\mathbf{A}^{k-1} \boldsymbol{\Sigma} (\mathbf{A}^{k-1})'$**: 次期のショックがt+k期に与える長期影響

### 6.2 累積効果のメカニズム

各期の将来ショック$\boldsymbol{\varepsilon}_{t+j}$ (j=1,2,...,k)は：
1. その期の収益率に直接影響（係数1）
2. 以降の期の収益率に間接影響（係数$\mathbf{A}, \mathbf{A}^2, ...$）

**重要:** 現在の状態$\mathbf{z}_t$は既知なので、不確実性の源泉は**将来のショックのみ**

### 6.3 例：投資家の視点

時刻t=0で投資決定：
- 現在の配当利回り$d_0 = -3.5$（既知）
- 現在の短期金利$r_0 = 0.02$（既知）
- **問題**: 今後3年間の株式リターンの分散は？

**答え**: $\text{Var}(\text{3年株式リターン} | d_0, r_0, ...)$

これが条件付き分散であり、Campbell-Viceira論文の焦点です。

## 7. 重要な性質

### 7.1 単調性

一般に$\mathbf{V}_{k+1} - \mathbf{V}_k = \mathbf{A}^k \boldsymbol{\Sigma} (\mathbf{A}^k)' \geq 0$（半正定値）

つまり、期間が長くなるほど分散は増加（または一定）。

### 7.2 収束性

$\mathbf{A}$の固有値が全て1未満の場合：

$\lim_{k \to \infty} \frac{\mathbf{V}_k}{k} = \text{有限値}$

### 7.3 短期極限

$k=1$の場合：$\mathbf{V}_1 = \boldsymbol{\Sigma}$（1期間の残差分散）

## 8. 年率換算の標準偏差

### 8.1 年率化の公式

k期間（四半期）の年率標準偏差：

$\sigma_{i,annual}(k) = \sqrt{\frac{4 \cdot [\mathbf{V}_k]_{ii}}{k}} \times 100$

### 8.2 意味

- **分子**: k期間の累積分散を年率化（×4）
- **分母**: 期間数kで割って平均化
- **全体**: 1年あたりの平均的リスク

## 9. 具体例：2変数VARの場合

### 9.1 設定

$\mathbf{z}_t = \begin{bmatrix} r_{s,t} \\ r_{b,t} \end{bmatrix}, \quad \mathbf{A} = \begin{bmatrix} 0.1 & 0.2 \\ 0.3 & 0.4 \end{bmatrix}, \quad \boldsymbol{\Sigma} = \begin{bmatrix} 0.01 & 0.005 \\ 0.005 & 0.008 \end{bmatrix}$

### 9.2 k=2の計算

$\mathbf{A}^0 = \mathbf{I} = \begin{bmatrix} 1 & 0 \\ 0 & 1 \end{bmatrix}$

$\mathbf{A}^1 = \begin{bmatrix} 0.1 & 0.2 \\ 0.3 & 0.4 \end{bmatrix}$

$\mathbf{V}_2 = \boldsymbol{\Sigma} + \mathbf{A}\boldsymbol{\Sigma}\mathbf{A}'$

計算すると：

$\mathbf{A}\boldsymbol{\Sigma}\mathbf{A}' = \begin{bmatrix} 0.1 & 0.2 \\ 0.3 & 0.4 \end{bmatrix} \begin{bmatrix} 0.01 & 0.005 \\ 0.005 & 0.008 \end{bmatrix} \begin{bmatrix} 0.1 & 0.3 \\ 0.2 & 0.4 \end{bmatrix}$

$= \begin{bmatrix} 0.002 & 0.0013 \\ 0.005 & 0.0047 \end{bmatrix} \begin{bmatrix} 0.1 & 0.3 \\ 0.2 & 0.4 \end{bmatrix} = \begin{bmatrix} 0.00046 & 0.00112 \\ 0.00144 & 0.00338 \end{bmatrix}$

したがって：

$\mathbf{V}_2 = \begin{bmatrix} 0.01 & 0.005 \\ 0.005 & 0.008 \end{bmatrix} + \begin{bmatrix} 0.00046 & 0.00112 \\ 0.00144 & 0.00338 \end{bmatrix} = \begin{bmatrix} 0.01046 & 0.00612 \\ 0.00644 & 0.01138 \end{bmatrix}$

## 10. 実用上の重要性

### 10.1 リスク管理

この公式により：
- 投資期間に応じたリスク測定が可能
- 長期投資の本当のリスクを定量化
- 期間構造を考慮したポートフォリオ最適化

### 10.2 予測可能性の効果

- **平均回帰**: $\mathbf{A}$の要素が負 → 長期分散減少
- **平均回避**: $\mathbf{A}$の要素が正で大 → 長期分散増加
- **相関変化**: 非対角要素が期間とともに影響

## 11. まとめ

$\boxed{\mathbf{V}_k = \sum_{j=0}^{k-1} \mathbf{A}^j \boldsymbol{\Sigma} (\mathbf{A}^j)'}$

この公式は：
1. **条件付き分散**（現在の市況を前提とした将来リスク）を表現
2. **各期のショックの累積効果**を定量化
3. **VAR係数の持続性**を通じて期間構造を生成
4. **投資期間別リスク分析**の基礎を提供
5. **Campbell-Viceira論文の核心**となる数学的結果

**重要な洞察:** 投資家は現在の市場状況$\mathbf{z}_t$を観察して投資決定を行うため、関心があるのは条件付き分散です。VARモデルの動学的性質が、この単純に見える公式を通じて、投資期間に依存する複雑なリスク構造を生み出しているのです。