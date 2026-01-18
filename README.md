# prob_robotics_demo
確率ロボティクス課題提出用リポジトリ
# 🤖 Probabilistic Robotics & Machine Learning Visualizer

このリポジトリは、確率ロボティクスおよび機械学習における主要な推論アルゴリズムを可視化したものです。各アルゴリズムの数学的背景と、そのダイナミクスをインタラクティブなアニメーションで確認できます。

## 📝 数学的背景

### 1. 自己位置推定 (Localization)

#### **カルマンフィルタ (Kalman Filter)**
線形ガウスシステムにおいて、状態 $x_t$ の事後分布 $p(x_t | y_{1:t})$ を解析的に求めます。
- **予測ステップ**: 
  $$\hat{x}_t^- = F \hat{x}_{t-1} + B u_t$$
  $$P_t^- = F P_{t-1} F^T + Q$$
- **更新ステップ**: 
  $$K_t = P_t^- H^T (H P_t^- H^T + R)^{-1}$$
  $$\hat{x}_t = \hat{x}_t^- + K_t (y_t - H \hat{x}_t^-)$$
  $$P_t = (I - K_t H) P_t^-$$

#### **MCL (Monte Carlo Localization)**
パーティクルフィルタを用いた非線形・非ガウスな推定手法です。
- **重み更新**: 各パーティクル $x_t^{(i)}$ の重み $w_t^{(i)}$ は、観測モデル $p(y_t | x_t)$ に基づいて計算されます。
  $$w_t^{(i)} \propto w_{t-1}^{(i)} \cdot p(y_t | x_t^{(i)})$$

---

### 2. クラスタリングと混合モデル

#### **EMアルゴリズム (Expectation-Maximization)**
混合ガウス分布 (GMM) の対数尤度を最大化します。
- **Eステップ (負担率)**: 
  $$\gamma_{nk} = \frac{\pi_k \mathcal{N}(x_n | \mu_k, \Sigma_k)}{\sum_{j=1}^K \pi_j \mathcal{N}(x_n | \mu_j, \Sigma_j)}$$
- **Mステップ (パラメータ更新)**: 
  $$\mu_k^{new} = \frac{1}{N_k} \sum_{n=1}^N \gamma_{nk} x_n, \quad \Sigma_k^{new} = \frac{1}{N_k} \sum_{n=1}^N \gamma_{nk} (x_n - \mu_k)(x_n - \mu_k)^T$$

#### **変分推論 (Variational Inference)**
真の事後分布 $p(Z, \pi, \mu | X)$ を近似分布 $q(Z)q(\pi)q(\mu)$ で近似します。本デモでは、混合比 $\pi$ にディリクレ分布 $Dir(\pi | \alpha)$ を仮定しています。
- **自動モデル選択**: データが割り当てられないクラスタ $k$ は、事後パラメータ $\alpha_k$ が事前分布の値 $\alpha_0$ に収束し、混合比の期待値 $E[\pi_k] \to 0$ となり、事実上「消滅」します。

---

### 3. ベイズ回帰 (Bayesian Regression)

重みパラメータ $w$ の事後分布 $p(w | X, y)$ を求めます。
- **事後分布の共分散と平均**:
  $$S_N = (\alpha I + \beta \Phi^T \Phi)^{-1}$$
  $$m_N = \beta S_N \Phi^T y$$
- **予測分布**: 未知の入力 $x^*$ に対して、予測値は以下のガウス分布に従います。
  $$p(t | x^*, X, y) = \mathcal{N}(t | m_N^T \phi(x^*), \sigma_N^2(x^*))$$
  ここで、$\sigma_N^2(x^*) = \frac{1}{\beta} + \phi(x^*)^T S_N \phi(x^*)$ であり、データが増えるほど第2項（モデルの不確実性）が減少します。

---

## 🚀 実行方法

1. 必要なライブラリをインストールします。
   ```bash
   pip install -r requirements.txt
