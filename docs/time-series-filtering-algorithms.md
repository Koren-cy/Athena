# 时序信号滤波算法深度调研

> 更新时间：2026-08-03 | 涵盖 10+ 大类、50+ 种算法

---

## 目录

1. [概述与分类框架](#1-概述与分类框架)
2. [经典线性滤波器](#2-经典线性滤波器)
3. [频率域滤波器](#3-频率域滤波器)
4. [自适应滤波器](#4-自适应滤波器)
5. [非线性滤波器](#5-非线性滤波器)
6. [小波变换滤波器](#6-小波变换滤波器)
7. [统计与机器学习滤波](#7-统计与机器学习滤波)
8. [保边滤波器](#8-保边滤波器)
9. [鲁棒滤波器](#9-鲁棒滤波器)
10. [实时与多速率滤波器](#10-实时与多速率滤波器)
11. [状态空间/贝叶斯滤波器](#11-状态空间贝叶斯滤波器)
12. [形态学与集合经验模态分解](#12-形态学与集合经验模态分解)
13. [滤波器选型指南](#13-滤波器选型指南)

---

## 1. 概述与分类框架

### 1.1 按线性度

| 类型 | 特点 | 代表算法 |
|------|------|----------|
| **线性** | 满足叠加原理；输出是输入的线性组合 | 移动平均、FIR、IIR、Butterworth、Savitzky-Golay |
| **非线性** | 不满足叠加原理；可处理非高斯噪声 | 中值滤波、形态学滤波、Rank-Order 滤波 |

### 1.2 按时变特性

| 类型      | 特点            | 代表算法                   |
| ------- | ------------- | ---------------------- |
| **时不变** | 系数固定，不随时间变化   | 经典 FIR/IIR、Butterworth |
| **自适应** | 系数随信号统计特性自动调整 | LMS、RLS、Kalman         |

### 1.3 按时域/变换域

| 域 | 特点 | 代表算法 |
|----|------|----------|
| **时域** | 直接在时间轴上卷积/递推 | MA、EMA、Kalman、中值滤波 |
| **频域** | FFT → 乘传递函数 → IFFT | 频谱滤波、Notch、Bandpass |
| **时频域** | 同时在时间和频率上分析 | 小波变换、STFT 滤波 |

### 1.4 按因果性

- **因果滤波器**：输出仅依赖当前及过去输入 → 可用于实时系统
- **非因果滤波器**：输出依赖未来输入 → 仅适用于离线/批处理（如零相位滤波 `filtfilt`）

---

## 2. 经典线性滤波器

### 2.1 简单移动平均（SMA — Simple Moving Average）

$$y[n] = \frac{1}{M} \sum_{k=0}^{M-1} x[n-k]$$

| 维度       | 评价                           |
| -------- | ---------------------------- |
| **复杂度**  | O(M) 朴素 / O(1) 增量式           |
| **频率响应** | 低通，旁瓣高（-13dB 第一旁瓣）           |
| **延迟**   | (M-1)/2 个样本（群延迟恒定）           |
| **适用**   | 快速平滑、去除白噪声、实时系统              |
| **劣势**   | 矩形窗频谱泄漏严重；对脉冲噪声敏感；需存储 M 个历史值 |

**变体：**
- **WMA（加权移动平均）**：近大远小的线性/指数权重
- **CMA（累积移动平均）**：$y[n] = y[n-1] + \frac{x[n] - y[n-1]}{n}$，适合流式数据的增量均值

### 2.2 指数移动平均（EMA — Exponential Moving Average）

$$y[n] = \alpha \cdot x[n] + (1-\alpha) \cdot y[n-1], \quad \alpha \in (0,1)$$

| 维度       | 评价                                                               |     |
| -------- | ---------------------------------------------------------------- | --- |
| **复杂度**  | O(1)                                                             |     |
| **存储**   | 仅需上一输出值                                                          |     |
| **频率响应** | 一阶低通 IIR，截止频率 $f_c = \frac{\alpha}{2\pi\Delta t (1-\alpha)}$（近似） |     |
| **延迟**   | 频率相关（非线性相位），$\tau \approx (1-\alpha)/\alpha$ 样本                  |     |
| **适用**   | 实时平滑、传感器融合、控制系统、金融时序                                             |     |
| **优势**   | 极简实现，无历史缓冲区，自然衰减旧数据                                              |     |

**双指数平滑（Double EMA / Holt）：** 对趋势也做 EMA，适合有趋势的信号。
**三指数平滑（Triple EMA / Holt-Winters）：** 再加季节性分量。

### 2.3 零相位（双向）滤波 — `filtfilt`

通过前向+反向滤波消除相位失真：

$$y = \text{reverse}(\text{filter}(b, a, \text{reverse}(\text{filter}(b, a, x))))$$

| 维度 | 评价 |
|------|------|
| **相位** | 严格零相位 |
| **幅频** | 相当于两次滤波，衰减翻倍（设计时需调整阶数） |
| **因果性** | 非因果，仅离线使用 |
| **适用** | 离线数据后处理、生物医学信号（ECG/EEG） |

### 2.4 FIR 滤波器（Finite Impulse Response）

$$y[n] = \sum_{k=0}^{N-1} h[k] \cdot x[n-k]$$

| 维度 | 评价 |
|------|------|
| **稳定性** | 无条件稳定（无反馈） |
| **相位** | 可设计为严格线性相位（系数对称） |
| **实现** | 直接型、转置型、快速卷积（FFT） |
| **代价** | 锐截止需要高阶（几十到几百阶） |

**窗函数设计法：**
| 窗类型 | 主瓣宽度 | 旁瓣衰减 | 应用场景 |
|--------|---------|---------|---------|
| Rectangular | 最窄 | -13 dB | 最差，频谱泄漏严重 |
| Hann (Hanning) | 中等 | -31.5 dB | 通用，频率分辨率与泄漏的折中 |
| Hamming | 中等 | -42.7 dB | 类似 Hann，旁瓣更低 |
| Blackman | 最宽 | -58 dB | 需要极低旁瓣的场景 |
| Kaiser | 可调 | 可调（$\beta$ 参数控制） | 灵活，最常用 |

**频率采样法 & Parks-McClellan（Remez）最优设计：** 使用等波纹逼近，在给定阶数下获得最优的 min-max 逼近。

**典型应用：** 音频处理、通信基带滤波、抽取/插值前的抗混叠

### 2.5 IIR 滤波器（Infinite Impulse Response）

$$y[n] = \sum_{k=1}^{N} a[k] \cdot y[n-k] + \sum_{k=0}^{M} b[k] \cdot x[n-k]$$

| 维度 | 评价 |
|------|------|
| **效率** | 低阶即可实现锐截止（比 FIR 少 5-10 倍系数） |
| **稳定性** | 需保证极点全在单位圆内 |
| **相位** | 非线性（需用 `filtfilt` 补偿） |
| **数值** | 高 Q 值、低频时易出现数值精度问题 |

#### 2.5.1 Butterworth（巴特沃斯）

- **特征：** 通带最大平坦（无纹波），阻带单调衰减
- **传递函数：** $|H(j\omega)| = \frac{1}{\sqrt{1 + (\omega/\omega_c)^{2N}}}$
- **-3dB 截止点：** $\omega_c$，与阶数无关
- **适用：** 需要平坦通带的场景（通用首选）

#### 2.5.2 Chebyshev Type I（切比雪夫 I 型）

- **特征：** 通带等纹波，阻带单调衰减
- **传递函数：** $|H(j\omega)| = \frac{1}{\sqrt{1 + \epsilon^2 T_N^2(\omega/\omega_c)}}$
- **效率：** 相同阶数下截止比 Butterworth 更陡
- **适用：** 可容忍通带纹波、需要锐截止

#### 2.5.3 Chebyshev Type II（切比雪夫 II 型 / 逆切比雪夫）

- **特征：** 通带单调，阻带等纹波
- **特点：** 零点在 $j\omega$ 轴上，阻带有陷波效果
- **适用：** 对通带平坦度要求高、阻带纹波可接受的场景

#### 2.5.4 Elliptic（椭圆 / Cauer）

- **特征：** 通带和阻带均有等纹波
- **效率：** 给定阶数下过渡带最窄（最陡峭截止）
- **代价：** 通带纹波 + 阻带纹波，相位非线性最强
- **适用：** 阶数受限、需最锐截止的场景

#### 2.5.5 Bessel（贝塞尔）

- **特征：** 群延迟最平坦 → 相位线性度最好
- **代价：** 截止衰减最缓（相同阶数下）
- **适用：** 需要保持波形形状的场景（示波器、脉冲传输）

### 2.6 Savitzky-Golay 滤波器

在滑动窗口内做多项式最小二乘拟合，取拟合值作为输出。

$$y[n] = \sum_{k=-m}^{m} c_k \cdot x[n+k]$$

| 维度 | 评价 |
|------|------|
| **本质** | 时域 FIR 滤波器，系数由多项式阶数和窗口长度决定 |
| **优势** | 保留高阶矩（峰值、宽度）；不削平信号峰 |
| **劣势** | 对异常值敏感；频域特性不直观 |
| **参数** | 窗口长度 `window_length`（奇数）+ 多项式阶数 `polyorder` |
| **适用** | 光谱平滑、色谱峰检测、生物信号预处理 |

**关键洞见：** 相比同窗口 SMA，S-G 能更好地保留信号峰的高度和宽度信息。

**频域特点：** 低频平坦，高频衰减，但有纹波——不是单调低通。

---

## 3. 频率域滤波器

### 3.1 FFT 谱滤波

```
x → FFT → 频谱 → 频域掩码（乘） → IFFT → y
```

| 维度 | 评价 |
|------|------|
| **灵活性** | 任意频率响应：低通/高通/带通/带阻/多带/自定义 |
| **相位** | 可实现零相位（只修幅值） |
| **块处理** | 重叠-相加/重叠-保留法处理长信号 |
| **代价** | O(N log N) / 块，非流式；块边界需加窗避免伪影 |
| **适用** | 离线批量处理、精确频段提取、工频（50/60Hz）及其谐波去除 |

**关键技巧：**
- **频域掩码必须保证共轭对称**（实数信号 → Hermitian 对称），否则输出为复数
- **频域加窗**（Hann/Gaussian 窗在频域，对应时域卷积）：避免时域 Gibbs 振铃
- **工频陷波**：直接置零 50/60Hz 及奇次谐波 bins → 比时域 notch 更精确

### 3.2 Notch（陷波）滤波器

专门抑制特定频率（如电网 50/60Hz 干扰）。

**IIR 二阶陷波器（双二次型）：**

$$H(z) = \frac{1 - 2\cos\omega_0 \cdot z^{-1} + z^{-2}}{1 - 2r\cos\omega_0 \cdot z^{-1} + r^2 z^{-2}}$$

- $r$ 越接近 1，陷波越窄、越深（但瞬态响应越长）
- 级联多个 notch 可去除工频谐波

### 3.3 带通滤波器组（Filter Bank）

将信号分解为多个子带，分别滤波后再合成。

- **均匀滤波器组**（如 DFT 滤波器组）
- **倍频程滤波器组**（1/1, 1/3 octave）
- **应用：** 音频均衡器、脑电分析、振动模态分解

### 3.4 STFT（短时傅里叶变换）时频滤波

在时频谱上做 2D 掩码，实现时变滤波。

- **优点：** 可处理频率随时间变化的信号
- **缺点：** 分辨率受限于海森堡不确定性（$\Delta t \cdot \Delta f \ge 1/4\pi$）；STFT 重构需满足 COLA（Constant Overlap-Add）约束

---

## 4. 自适应滤波器

### 4.1 LMS（Least Mean Squares）

$$ \mathbf{w}[n+1] = \mathbf{w}[n] + \mu \cdot e[n] \cdot \mathbf{x}[n] $$

其中 $e[n] = d[n] - \mathbf{w}[n]^T \mathbf{x}[n]$。

| 维度 | 评价 |
|------|------|
| **复杂度** | O(N) 每样本（极低） |
| **收敛** | 慢（取决于输入相关性） |
| **稳态误差** | 非零（有失调量 $\mu \cdot \text{tr}(R)/2$） |
| **步长 $\mu$** | 大→收敛快但失调大；小→稳态好但慢 |

**变体：**
| 算法 | 特点 |
|------|------|
| **NLMS**（归一化 LMS） | $\mathbf{w}[n+1] = \mathbf{w}[n] + \frac{\mu}{\|\mathbf{x}[n]\|^2 + \epsilon} e[n] \mathbf{x}[n]$；抗输入幅度波动 |
| **Sign LMS** | 仅用误差符号，硬件实现极简 |
| **Leaky LMS** | 加入权值衰减防止系数溢出 |
| **Block LMS** | 块处理，可用 FFT 加速（频域 LMS） |
| **Affine Projection** | 介于 LMS 和 RLS 之间 |

**应用：** 回声消除、主动噪声控制、信道均衡、系统辨识

### 4.2 RLS（Recursive Least Squares）

$$ \mathbf{w}[n] = \mathbf{w}[n-1] + \mathbf{g}[n] \cdot e[n] $$
$$ \mathbf{g}[n] = \frac{\mathbf{P}[n-1]\mathbf{x}[n]}{\lambda + \mathbf{x}^T[n]\mathbf{P}[n-1]\mathbf{x}[n]} $$
$$ \mathbf{P}[n] = \frac{1}{\lambda}\left(\mathbf{P}[n-1] - \mathbf{g}[n]\mathbf{x}^T[n]\mathbf{P}[n-1]\right) $$

| 维度 | 评价 |
|------|------|
| **复杂度** | O(N²)（比 LMS 高一个数量级） |
| **收敛** | 极快（一个数量级优于 LMS） |
| **稳态误差** | 渐近最优（Wiener 解） |
| **遗忘因子 $\lambda$** | 平衡跟踪能力与稳态精度 |
| **风险** | 数值不稳定（协方差矩阵 P 失去正定性） |

**数值稳定化：** 平方根 RLS（QR-RLS）使用 Cholesky 分解传播 $\mathbf{P}^{1/2}$，鲁棒性更强。

### 4.3 Kalman 滤波器（卡尔曼滤波）

**状态方程：**
$$ \mathbf{x}_k = \mathbf{F}_k \mathbf{x}_{k-1} + \mathbf{B}_k \mathbf{u}_k + \mathbf{w}_k, \quad \mathbf{w}_k \sim \mathcal{N}(0, \mathbf{Q}_k) $$

**观测方程：**
$$ \mathbf{z}_k = \mathbf{H}_k \mathbf{x}_k + \mathbf{v}_k, \quad \mathbf{v}_k \sim \mathcal{N}(0, \mathbf{R}_k) $$

**递推两步：**
1. **预测（先验）：**
   $$ \hat{\mathbf{x}}_{k|k-1} = \mathbf{F}_k \hat{\mathbf{x}}_{k-1|k-1} + \mathbf{B}_k \mathbf{u}_k $$
   $$ \mathbf{P}_{k|k-1} = \mathbf{F}_k \mathbf{P}_{k-1|k-1} \mathbf{F}_k^T + \mathbf{Q}_k $$

2. **更新（后验）：**
   $$ \mathbf{K}_k = \mathbf{P}_{k|k-1} \mathbf{H}_k^T (\mathbf{H}_k \mathbf{P}_{k|k-1} \mathbf{H}_k^T + \mathbf{R}_k)^{-1} $$
   $$ \hat{\mathbf{x}}_{k|k} = \hat{\mathbf{x}}_{k|k-1} + \mathbf{K}_k (\mathbf{z}_k - \mathbf{H}_k \hat{\mathbf{x}}_{k|k-1}) $$
   $$ \mathbf{P}_{k|k} = (\mathbf{I} - \mathbf{K}_k \mathbf{H}_k) \mathbf{P}_{k|k-1} $$

| 维度 | 评价 |
|------|------|
| **最优性** | 线性高斯条件下为 MMSE 最优 |
| **复杂度** | O(d³)（d 为状态维度） |
| **超参数** | Q（过程噪声协方差）和 R（测量噪声协方差）——调参关键 |
| **扩展性** | 见下方变体 |

#### Kalman 变体系列

| 算法 | 处理 | 方法 | 复杂度 |
|------|------|------|--------|
| **Kalman Filter (KF)** | 线性 + 高斯 | 解析递推 | O(d³) |
| **Extended KF (EKF)** | 非线性 | Jacobian 一阶线性化 | O(d³) |
| **Unscented KF (UKF)** | 非线性 | Sigma 点变换（无需求导） | O(d³)（点更多） |
| **Cubature KF (CKF)** | 非线性 | 球面-径向容积积分 | O(d³) |
| **Ensemble KF (EnKF)** | 高维 | Monte Carlo 集合 | O(N·d)（N 为集合大小） |
| **Particle Filter (PF)** | 非线性 + 非高斯 | 序贯重要性采样 | O(N·d) |
| **Information Filter** | 信息形式 | 对偶于 KF（协方差→信息矩阵） | O(d³) |
| **Square-Root KF** | 数值稳定 | Cholesky 传播 | O(d³) |

**关键区别：**
- **EKF vs UKF：** UKF 对强非线性系统精度更高（达到二阶 Taylor），且无需计算 Jacobian
- **KF vs PF：** 粒子滤波可处理任意分布（多模态、厚尾），但计算量大；状态维度高时需大量粒子（维数灾难）

**应用：** 导航/GPS/IMU 融合、目标跟踪、电池 SOC 估计、经济预测

### 4.4 Wiener 滤波器（维纳滤波）

频域最优线性滤波器，最小化 MSE：

$$ H(f) = \frac{S_{xs}(f)}{S_{xx}(f)} = \frac{S_{ss}(f)}{S_{ss}(f) + S_{nn}(f)} \quad \text{(信号与噪声不相关时)} $$

| 维度 | 评价 |
|------|------|
| **性质** | 平稳信号的最优线性滤波器（MMSE 意义） |
| **前提** | 需已知信号和噪声的功率谱密度（或可估计） |
| **因果性** | Wiener-Hopf 方程可求因果解 |
| **对比 Kalman** | Wiener 用于平稳信号（频域）；Kalman 用于非平稳（时域递推） |

**应用：** 图像去模糊、语音增强（谱减法的基础）、地震信号去噪

---

## 5. 非线性滤波器

### 5.1 中值滤波器（Median Filter）

$$ y[n] = \text{median}\{x[n-k], \ldots, x[n], \ldots, x[n+k]\} $$

| 维度 | 评价 |
|------|------|
| **对脉冲噪声** | 出色（完全消除孤立离群点） |
| **保边能力** | 可保持阶跃边缘（不像 MA 会模糊边缘） |
| **对高斯噪声** | 不如均值滤波有效（效率约 0.7 vs MA） |
| **复杂度** | O(M log M) 朴素 / O(log M) 用双堆结构 |
| **统计性质** | 根信号（root signal）概念——重复滤波会在有限步内收敛 |

**变体：**
- **加权中值滤波：** 对窗口内样本赋予整数权重（重复采样）
- **中心加权中值滤波：** 仅增大中心样本权重 → 在平滑和保边间折中
- **递归中值滤波：** 用已滤波样本替换窗口中的原始样本

### 5.2 形态学滤波器（Morphological Filters）

来自数学形态学（集合运算），作用于信号"形状"而非频率：

| 操作 | 效果 | 对偶 |
|------|------|------|
| **腐蚀 (Erosion)** | 收缩信号，去除正峰值 | — |
| **膨胀 (Dilation)** | 扩展信号，填充负谷值 | 腐蚀的对偶 |
| **开运算 (Opening)** | 腐蚀→膨胀：去除正脉冲，保基线 | 闭运算的对偶 |
| **闭运算 (Closing)** | 膨胀→腐蚀：填充负脉冲 | 开运算的对偶 |
| **Top-hat** | 原信号 - 开运算 → 提取正脉冲 | Black-hat（检测负谷值） |

**结构元素（SE）：** 平坦型、三角型、自定义——决定形态尺度。

**应用：** 基线漂移矫正（ECG）、脉冲提取、滚动轴承故障特征增强

### 5.3 Rank-Order 滤波器

窗口内排序后选任意分位数（中值是 50%）：

$$ y[n] = x_{(r)}, \quad r \in [0, M-1] $$

- **最小值滤波（r=0）：** 暗通道先验（图像去雾）
- **最大值滤波（r=M-1）：** 峰值包络
- **百分位数滤波：** 比中值更灵活——较大的 r 偏保守（更平滑），较小的 r 偏敏感

### 5.4 决策导向/阈值类去噪

#### 小波阈值去噪（Donoho-Johnstone）

1. DWT 分解
2. 对各层细节系数做软/硬阈值处理：
   - **硬阈值：** $\eta_H(w) = w \cdot \mathbf{1}_{|w| > \lambda}$
   - **软阈值：** $\eta_S(w) = \text{sgn}(w) \cdot \max(|w| - \lambda, 0)$
3. IDWT 重构

**通用阈值（VisuShrink）：** $\lambda = \sigma \sqrt{2 \log N}$
**自适应阈值（SureShrink, BayesShrink）：** 按子带分别计算最优阈值

**软 vs 硬：**
- 硬阈值：无偏，但可能引入伪 Gibbs 振荡
- 软阈值：有偏（信号幅值被缩小 $\lambda$），但更平滑

---

## 6. 小波变换滤波器

### 6.1 离散小波变换（DWT）

将信号分解为不同尺度的时频分量：

$$ x(t) = \sum_k c_{J,k} \phi_{J,k}(t) + \sum_{j=1}^{J} \sum_k d_{j,k} \psi_{j,k}(t) $$

- $c_{J,k}$：最粗尺度的近似系数（低频）
- $d_{j,k}$：尺度 $j$ 的细节系数（带通）
- $\phi$：尺度函数；$\psi$：小波函数

**Mallat 算法（快速 DWT）：** 级联的二通道滤波器组，O(N) 复杂度。

**常用小波基：**

| 小波族 | 特点 | 典型用途 |
|--------|------|----------|
| **Daubechies (dbN)** | 紧支撑、正交、N 阶消失矩 | 通用，最常用 |
| **Symlet (symN)** | 近对称的 Daubechies 改进版 | 需要近线性相位时 |
| **Coiflet (coifN)** | 尺度函数也有消失矩 | 信号平滑近似 |
| **Biorthogonal** | 线性相位（双正交） | 图像压缩（JPEG2000） |
| **Morlet** | 复值，Gaussian 包络 | 时频分析（CWT） |
| **Mexican Hat** | 高斯二阶导数 | 边沿检测 |

### 6.2 小波包分解（Wavelet Packet Decomposition）

不仅分解近似系数，也分解细节系数——得到完整的二叉树。

- **优势：** 频率分辨率更灵活（可自适应选择最优基）
- **最佳基选择：** 基于熵最小化（Shannon 熵等）
- **应用：** 与 DWT 同，但需要更精细的频段划分时

### 6.3 平稳小波变换（SWT / Undecimated WT）

省略下采样步骤 → 平移不变性。

- **优点：** 无 Gibbs 伪影；阈值去噪效果更好
- **代价：** O(N log N)，冗余表示（每层 N 个系数）
- **对比 DWT：** DWT 去噪后可能有人工伪影，SWT 更平滑

### 6.4 经验小波变换（EWT — Empirical Wavelet Transform）

数据驱动的自适应小波——根据信号频谱自动划分频带并构建小波滤波器组。

| 维度 | 评价 |
|------|------|
| **优势** | 完全自适应，无需预设小波基 |
| **频带划分** | 基于频谱峰值检测 |
| **对比 EMD** | 数学理论基础更坚实（有重构公式保证） |

### 6.5 最大重叠离散小波变换（MODWT）

- 所有尺度的系数个数相同（无下采样）
- 平移不变
- 可处理任意长度信号（DWT 要求 2^J 整数倍）
- 方差估计更准确

### 6.6 可调 Q 小波变换（TQWT — Tunable Q-Factor WT）

允许调整 Q 因子（中心频率/带宽）以匹配信号振荡特性：

- **高 Q 因子：** 分析振荡性强的信号（如 EEG 节律）
- **低 Q 因子：** 分析瞬态/脉冲信号
- **参数：** Q 因子 + 冗余度 + 分解层数

---

## 7. 统计与机器学习滤波

### 7.1 Hodrick-Prescott (HP) 滤波器

分离趋势（Trend）与周期（Cycle）分量：

$$ \min_{\tau} \left\{ \sum_{t=1}^{T} (y_t - \tau_t)^2 + \lambda \sum_{t=3}^{T} (\tau_t - 2\tau_{t-1} + \tau_{t-2})^2 \right\} $$

- **第一项：** 拟合优度（趋势偏离数据的惩罚）
- **第二项：** 平滑度（趋势曲率的惩罚）
- **$\lambda$：** 平滑参数——$\lambda \to \infty$ 趋势趋近线性；$\lambda \to 0$ 趋势→数据
- **标准值：** $\lambda = 1600$（季度数据），$\lambda = 14400$（月度数据），$\lambda = 100000$（年度数据）

**问题：** 端部效应（两端趋势估计不可靠）；伪周期风险。

**改进：** HP 滤波 + Hamilton 回归，或改用 Christiano-Fitzgerald / Baxter-King 带通滤波。

### 7.2 LOESS / LOWESS（局部加权散点平滑）

逐点做加权局部多项式回归：

$$ \min_{\beta} \sum_{i} K\left(\frac{x_i - x_0}{h}\right) \cdot (y_i - \beta_0 - \beta_1(x_i - x_0) - \cdots)^2 $$

| 维度 | 评价 |
|------|------|
| **核函数 K** | 通常 Tricube：$K(u) = (1 - |u|^3)^3 \cdot \mathbf{1}_{|u|<1}$ |
| **带宽 h** | 控制平滑度（大→平滑，小→贴近数据） |
| **鲁棒变体** | Robust LOESS：迭代重加权（Bisquare 权重），降权离群点 |
| **复杂度** | O(N·span) — 大数据慢 |
| **适用** | 探索性数据分析、非参数趋势提取、降噪 |

### 7.3 高斯过程（GP）回归平滑

$$ f(x) \sim \mathcal{GP}(m(x), k(x, x')) $$

| 维度 | 评价 |
|------|------|
| **优势** | 天然提供不确定性（方差）估计 |
| **平滑度** | 由核函数决定（RBF → 无限阶可导；Matérn 3/2 → 一阶可导） |
| **复杂度** | O(N³) — 大数据需要稀疏近似（SVGP、Nyström） |
| **核选择** | RBF（平滑信号）、Matérn（低可导性）、Periodic（周期性信号） |

**常用核组合：** `RBF + WhiteNoise + Periodic`

### 7.4 奇异谱分析（SSA — Singular Spectrum Analysis）

非参数方法，将信号分解为趋势 + 振荡 + 噪声：

1. **嵌入（Embedding）：** 构造轨迹矩阵（Hankel 矩阵），窗口长度 $L$
2. **SVD（分解）：** 提取 $L$ 个主成分（$\sqrt{\lambda_i}$ 为奇异值）
3. **分组（Grouping）：** 将奇异向量按趋势/周期/噪声分组
4. **对角平均（Reconstruction）：** 将分组矩阵转回一维序列

| 维度 | 评价 |
|------|------|
| **优势** | 无模型；可分离非线性趋势；无需预设周期 |
| **劣势** | 窗口长度 L 的选择关键但不直观；分组需要专家判断 |
| **适用** | 气候时间序列、地球物理信号、经济周期分析 |

**扩展：** MSSA（多变量 SSA）、SSA 预测（subspace-based forecasting）

### 7.5 PCA / Kernel PCA 去噪

- **PCA：** 将滑动窗口嵌入的高维数据投影到前 k 个主成分（低维子空间），丢弃噪声子空间
- **Kernel PCA：** 非线性扩展——通过核函数隐式映射到高维，再 PCA

**对比 SSA：** PCA 去噪是 SSA 的核心步骤（SSA = 嵌入 + PCA + 重构）。

### 7.6 总变差去噪（Rudin-Osher-Fatemi / TV Denoising）

$$ \min_{u} \frac{1}{2} \|u - f\|_2^2 + \lambda \|\nabla u\|_1 $$

- **L1 正则化梯度：** 促成分段常数解（保边）
- **$\lambda$：** 正则化强度
- **求解：** Chambolle 投影算法、ADMM、Primal-Dual（Chambolle-Pock）
- **TV-L1 变体：** 数据保真项也使用 L1（对离群点更鲁棒）

### 7.7 字典学习 / 稀疏表示

$$ \min_{D, \alpha} \sum_i \|x_i - D\alpha_i\|_2^2 + \lambda \|\alpha_i\|_1 \quad \text{s.t.} \|d_j\|_2 = 1 $$

- 学习信号的稀疏表示字典，噪声不可稀疏表示 → 被滤除
- **算法：** K-SVD、在线字典学习（ODL）
- **应用：** 图像去噪（效果极好）、地震信号去噪

### 7.8 深度学习去噪

| 架构 | 特点 |
|------|------|
| **Autoencoder (DAE)** | 自监督：加噪→编码→重构；不用干净标签 |
| **U-Net (1D/2D)** | 编码器-解码器 + 跳跃连接；保留细节 |
| **WaveNet / TCN** | 膨胀因果卷积；捕获长程时间依赖 |
| **LSTM/GRU 去噪** | 序列建模；适合低频漂移 |
| **GAN 去噪** | 对抗训练；生成更自然的去噪信号 |
| **Diffusion Models** | 逐步去噪；当前 SOTA |
| **Transformer** | 自注意力捕获全局依赖 |

**优点：** 可学习复杂噪声模式（非高斯、信号相关噪声）  
**缺点：** 需要训练数据；可解释性差；泛化到未见过的噪声类型可能不可靠

---

## 8. 保边滤波器（Edge-Preserving）

虽然多用于 2D 图像，但很多有 1D 信号版本。

### 8.1 双边滤波器（Bilateral Filter）

$$ y[n] = \frac{\sum_{k} x[n-k] \cdot G_s(k) \cdot G_r(x[n-k] - x[n])}{\sum_{k} G_s(k) \cdot G_r(x[n-k] - x[n])} $$

- **$G_s$：** 空间核（距离越远，权重越小）
- **$G_r$：** 值域核（幅度差越大，权重越小）——**保边的关键**
- **两个 $\sigma$：** $\sigma_s$（空间尺度）、$\sigma_r$（幅度尺度）
- **复杂度：** 朴素 O(N·W)，有快速近似 O(N)

### 8.2 引导滤波器（Guided Filter）

边保留的同时避免双边滤波的"梯度反转"伪影：

$$ y_i = a_k x_i + b_k, \quad \forall i \in \omega_k $$

- 在每个窗口 $\omega_k$ 中，输出 $y$ 是引导信号 $x$（通常即输入自身）的线性变换
- **复杂度：** O(N)，与窗口大小无关
- **优势：** 无梯度反转，边缘保持优于双边滤波

### 8.3 各向异性扩散（Perona-Malik）

$$ \frac{\partial u}{\partial t} = \nabla \cdot \left( g(\|\nabla u\|) \nabla u \right) $$

- **$g(s)$：** 扩散系数 → $g \approx 1$（平坦区，快速平滑）；$g \approx 0$（边缘，停止扩散）
- **典型扩散函数：**
  - $g(s) = e^{-(s/K)^2}$
  - $g(s) = \frac{1}{1 + (s/K)^2}$
- **参数 K：** 控制"什么是边缘"的梯度阈值
- **1D 版本：** 可用于时序保边去噪

---

## 9. 鲁棒滤波器

### 9.1 Hampel 滤波器

在滑动窗口中检测并替换离群点：

$$ \text{若 } |x[n] - \text{median}(w)| > \kappa \cdot \text{MAD}(w) \text{ → 替换为 median}(w) $$

- **MAD：** Median Absolute Deviation (对正态，MAD ≈ 0.6745 σ)
- **$\kappa$：** 阈值系数（通常 $\kappa = 3$，对应 3σ 规则）
- **优势：** 同时检测+校正；对脉冲噪声鲁棒
- **对比中值滤波：** Hampel 只在需要时替换（选择性），中值滤波总是替换

### 9.2 MAD 离群点剔除

与 Hampel 的检测部分相同，但只标记/移除，不替换。

### 9.3 重复中值滤波（Repeated Median / Siegel）

对带趋势的信号做鲁棒线性拟合（对多个斜率取中值，再对截距取中值）。

- **崩溃点：** 50%（可容忍一半数据为离群点）
- **应用：** 鲁棒的趋势/断点估计

### 9.4 $\ell_1$ 趋势滤波（L1 Trend Filtering）

$$ \min_{u} \frac{1}{2} \|u - y\|_2^2 + \lambda \|D^{(k)} u\|_1 $$

- $D^{(k)}$ 是 k 阶差分矩阵
- **k=1：** 分段常数（对比 TV 去噪）
- **k=2：** 分段线性（对比 HP 滤波但用 L1 而非 L2）
- **k=3：** 分段二次

**优势：** 与 HP 滤波不同，L1 趋势滤波对趋势跳变更鲁棒，且不产生伪振荡。

---

## 10. 实时与多速率滤波器

### 10.1 CIC 滤波器（Cascaded Integrator-Comb）

极简单的多速率滤波器（无乘法器）：

$$ H(z) = \left(\frac{1 - z^{-RM}}{1 - z^{-1}}\right)^N $$

- **结构：** N 级积分器 + 抽取/插值 + N 级梳状
- **优势：** 无乘法，硬件友好；高抽取率首选
- **劣势：** 通带衰减（droop），需补偿 FIR
- **应用：** Σ-Δ ADC/DAC 中的抽取/插值、软件无线电

### 10.2 半带滤波器（Half-Band Filter）

- 通带和阻带对称于 $f_s/4$
- 近半数系数为 0（偶数位置除中心外） → 乘法减半
- **应用：** 2x 抽取/插值的多级实现

### 10.3 多相滤波器（Polyphase Filter）

将 FIR 按抽取/插值因子 M 分解为 M 个子滤波器：

$$ H(z) = \sum_{k=0}^{M-1} z^{-k} E_k(z^M) $$

- **优势：** 先抽取再滤波（而非先滤波再丢弃样本）→ 计算量降为 1/M
- **实现：** 多相 FIR + Farrow 结构（分数延迟）

### 10.4 滑动/在线式滤波器的实时策略

| 策略 | 适用算法 | 方法 |
|------|----------|------|
| **滑动窗口** | 中值、S-G、MA | 环形缓冲区 + 增量统计更新 |
| **指数遗忘** | EMA、RLS | 递推公式，O(1) 更新 |
| **块处理 + 重叠** | FFT 滤波、STFT | 重叠-相加/保留 |
| **并行 Kalman** | 传感器阵列 | 每个传感器独立 KF 实例 |

---

## 11. 状态空间/贝叶斯滤波器

### 11.1 粒子滤波（Particle Filter / SMC）

用加权粒子集 $\{x_k^{(i)}, w_k^{(i)}\}_{i=1}^N$ 表示状态后验分布：

1. **预测：** 从提议分布采样 $x_k^{(i)} \sim q(x_k | x_{k-1}^{(i)}, z_k)$
2. **更新：** $w_k^{(i)} \propto w_{k-1}^{(i)} \frac{p(z_k|x_k^{(i)}) p(x_k^{(i)}|x_{k-1}^{(i)})}{q(x_k^{(i)}|x_{k-1}^{(i)}, z_k)}$
3. **重采样：** 避免粒子退化（系统重采样、残差重采样）

| 维度 | 评价 |
|------|------|
| **状态分布** | 任意（多模态、厚尾均可） |
| **系统模型** | 任意非线性、非高斯 |
| **复杂度** | O(N·d)；高维需大量粒子（维数灾难） |
| **重采样策略** | 系统重采样（最常用）、分层、残差 |

**改进：**
- **Rao-Blackwellized PF：** 部分状态可解析积分，减小方差
- **Auxiliary PF：** 利用最新观测改善提议分布
- **Regularized PF：** 连续核密度近似

### 11.2 隐马尔可夫模型（HMM）滤波

$$ p(x_k | z_{1:k}) \propto p(z_k | x_k) \sum_{x_{k-1}} p(x_k | x_{k-1}) p(x_{k-1} | z_{1:k-1}) $$

- **特点：** 离散状态空间
- **前向-后向算法：** 前向（滤波）+ 后向（平滑）
- **Viterbi 算法：** 最大后验路径
- **应用：** 语音识别、行为状态推断、金融体制切换模型

### 11.3 贝叶斯在线变点检测（BOCD）

检测并适应信号的统计特性变化：

$$ p(r_k | z_{1:k}) \propto \sum_{r_{k-1}} p(r_k | r_{k-1}) \int p(z_k | \theta) p(\theta | z_{r_{k-1}:k-1}) d\theta $$

- $r_k$：自上次变点的运行长度（run-length）
- **优势：** 在线的、概率的变点检测；可同时估计多个假设模型
- **输出：** 变点概率 + 各段的参数后验

---

## 12. 形态学与集合经验模态分解

### 12.1 EMD（经验模态分解 — Empirical Mode Decomposition）

**无需预设基函数**的数据自适应分解：

1. 找信号的所有局部极值点
2. 上包络（极大值样条插值）+ 下包络（极小值样条插值）
3. 提取均值包络 → 减去 → 得到候选 IMF
4. **筛选（Sifting）：** 重复直至满足 IMF 条件（零均值 + 极值/零交叉数 ≤ 1 差）
5. 残差作为新信号，重复提取下一个 IMF

$$ x(t) = \sum_{i=1}^{N} \text{IMF}_i(t) + r_N(t) $$

| 维度 | 评价 |
|------|------|
| **优势** | 完全自适应；适合非平稳/非线性信号 |
| **劣势** | 模态混叠（mode mixing）；端点效应；缺乏数学理论 |
| **滤波用途** | 选择性地重构：去掉高频 IMF（去噪）或低频 IMF（去趋势） |

### 12.2 EEMD（集合经验模态分解 — Ensemble EMD）

加白噪声 → EMD → 平均 → 消除模态混叠：

$$ \text{EEMD}: x(t) + \epsilon \cdot w_i(t) \rightarrow \text{EMD} \rightarrow \text{平均所有试验} $$

- **原理：** 白噪声在整个时频空间均匀分布，迫使 EMD 在不同尺度上分离
- **参数：** 噪声幅度 $\epsilon$（0.1-0.4σ）+ 集合数 $N_e$（100-500）

### 12.3 CEEMDAN（自适应噪声完备集合 EMD）

相比 EEMD 收敛更快、重构更精确（逐个 IMF 而非整条信号添加噪声）。

### 12.4 VMD（变分模态分解 — Variational Mode Decomposition）

求解约束变分优化问题来提取模态——**有严格数学基础**的非递归分解：

$$ \min_{\{u_k\}, \{\omega_k\}} \left\{ \sum_k \left\| \partial_t \left[ \left(\delta(t) + \frac{j}{\pi t}\right) * u_k(t) \right] e^{-j\omega_k t} \right\|_2^2 \right\} $$
$$ \text{s.t.} \sum_k u_k = x $$

- **$K$：** 模态数量（需预设）
- **$\alpha$：** 带宽约束（数据保真度）
- **ADMM 求解：** 交替更新模态和中心频率
- **vs EMD：** 更抗噪声和采样；无模态混叠；但需预设 K
- **滤波用途：** 选择感兴趣的模态重构

### 12.5 ICEEMDAN（改进的自适应噪声完备 EMD）

进一步减小 EEMD/CEEMDAN 中的残留噪声和伪模态。

### 12.6 经验小波变换（EWT）

前面在小波部分已介绍——构建自适应小波滤波器组，介于 EMD 和 DWT 之间的折中方案。

---

## 13. 滤波器选型指南

### 13.1 按目标选算法

| 目标 | 推荐算法（按优先级） |
|------|---------------------|
| **快速平滑（实时）** | EMA → SMA → SG（离线） |
| **精确低通（离线）** | Butterworth (`filtfilt`) → FIR（Parks-McClellan）→ Elliptic |
| **去脉冲噪声** | 中值滤波 → Hampel → 形态学开运算 |
| **去高斯噪声** | Wiener → SG → 小波阈值 |
| **去除特定频率干扰** | Notch（50/60Hz）→ 频域掩码 → 自适应 LMS |
| **保边去噪** | 中值滤波 → Total Variation → 双边滤波 → 引导滤波 |
| **趋势提取** | HP → LOESS → L1 趋势滤波 → SSA |
| **时频分析 + 去噪** | DWT（小波阈值）→ SWT → VMD → EEMD |
| **非平稳信号去噪** | Kalman（有模型）→ EMD/EEMD → VMD → 自适应 LMS |
| **多传感器融合** | Kalman → UKF → EnKF（高维）→ 粒子滤波 |
| **在线/实时滤波** | EMA → Kalman → LMS → RLS |
| **未知噪声，无模型** | SSA → 小波阈值 → 中值滤波 → GP |

### 13.2 按信号特性选算法

| 信号特性 | 推荐算法 |
|----------|----------|
| 平稳、噪声已知 | Wiener、Butterworth、FIR |
| 平稳、噪声未知 | 小波阈值（通用阈值）、SG |
| 非平稳、线性变化 | Kalman、LMS、RLS |
| 非平稳、非线性 | EKF、UKF、粒子滤波、EMD/EEMD/VMD |
| 稀疏尖峰 + 噪声 | 中值滤波、L1 趋势滤波、Morphology |
| 周期信号（已知周期） | 频域掩码、梳状滤波器、锁定放大器 |
| 带趋势 + 噪声 | HP、LOESS、SSA、SG |
| 多分量叠加 | EMD/EEMD/VMD、SSA、Filter Bank |
| 突发/瞬态为主 | 形态学滤波、STA/LTA 触发检测 |
| 高维/多通道 | 多变量 SSA (MSSA)、多通道 Wiener |

### 13.3 按工程约束选算法

| 约束 | 推荐算法 |
|------|----------|
| **极低延迟（<1ms）** | EMA、一阶 IIR、中值滤波（增量式） |
| **极小存储（<10 样本）** | EMA、IIR (biquad)、Kalman |
| **功耗/硅面积受限** | CIC、Hogenauer、Sign-LMS |
| **无需先验知识** | 中值滤波、EMD、SSA、小波通用阈值 |
| **可解释性要求高** | Butterworth、FIR、Kalman、SSA |
| **需不确定性量化** | GP、Kalman、粒子滤波 |
| **实时变点检测** | BOCD、CUSUM、自适应 Kalman |
| **样本少（<100）** | GP、LOESS（小带宽） |

### 13.4 组合策略（常见混合方案）

| 场景 | 流水线 |
|------|--------|
| **ECG 预处理** | Baseline Wander（中值/形态学）→ 肌电噪声（SG/wavelet）→ 工频（notch） |
| **IMU 姿态估计** | 加速度计低通（Butterworth）→ 陀螺仪高通 → 互补滤波 / Mahony → Kalman 融合 |
| **振动故障诊断** | 趋势去除（HP/SSA）→ 带通（FIR）→ 包络解调 → 形态学 Top-hat |
| **金融去噪** | 中值/EMA 平滑 → EMD 趋势-周期分解 → SSA 去噪 |
| **语音增强** | STFT → 谱减法（Wiener 变体）→ ISTFT → OMLSA（最优修正对数谱幅度估计） |
| **EEG 伪迹去除** | ICA（独立成分分析）分离眼电/肌电伪迹 → 小波阈值去噪 → 自适应 LMS |
| **传感器数据清洗** | Hampel（离群点）→ 中值滤波（脉冲）→ Kalman 平滑（高斯噪声）|

---

## 参考文献（主要）

1. Oppenheim, A. V., & Schafer, R. W. (2010). *Discrete-Time Signal Processing*. 3rd ed. Prentice Hall. — FIR/IIR/滤波器设计圣经
2. Haykin, S. (2013). *Adaptive Filter Theory*. 5th ed. Pearson. — LMS/RLS/Kalman 全面覆盖
3. Daubechies, I. (1992). *Ten Lectures on Wavelets*. SIAM. — 小波理论基础
4. Huang, N. E., et al. (1998). "The empirical mode decomposition and the Hilbert spectrum..." *Proc. R. Soc. Lond. A*. — EMD 开山之作
5. Dragomiretskiy, K., & Zosso, D. (2014). "Variational Mode Decomposition." *IEEE TSP*. — VMD
6. Donoho, D. L., & Johnstone, I. M. (1994). "Ideal spatial adaptation by wavelet shrinkage." *Biometrika*. — 小波阈值去噪
7. Kim, S.-J., et al. (2009). "L1 Trend Filtering." *SIAM Review*.
8. Golyandina, N., & Zhigljavsky, A. (2020). *Singular Spectrum Analysis for Time Series*. 2nd ed. Springer.
9. Tomasi, C., & Manduchi, R. (1998). "Bilateral filtering for gray and color images." *ICCV*. — 双边滤波
10. He, K., Sun, J., & Tang, X. (2013). "Guided Image Filtering." *IEEE TPAMI*. — 引导滤波
11. Thakur, G., et al. (2013). "The Synchrosqueezing algorithm for time-varying spectral analysis..." *Signal Processing*. — SST 与 TQWT
12. Arulampalam, M. S., et al. (2002). "A tutorial on particle filters..." *IEEE TSP*. — 粒子滤波教程

---

> **维护说明：** 本文档随算法研究进展持续更新。新增算法时请同步更新 §13 选型指南。
