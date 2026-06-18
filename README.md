# 反激电源设计环路计算

## 1. 电源基本输出参数
以下为电源的基本输出参数：

| 参数名称 | 英文名称/符号 | 数值/范围 |
| :--- | :--- | :--- |
| AC输入电压范围 | AC input voltage range | 176V~264V |
| 标准值 | Standard Value | 220V/50Hz |
| 输出电压 | $V_{out}$ | 30V |
| 输出电流 | $I_{out}$ | 2A |
| 开关频率 | $f_{sw}$ | 110kHz |
| 效率 | Efficiency | 89% |
| 匝比 | Ratio of turns | 4 |
| 最大占空比 | $D_{max}$ | 0.33 |

---

## 2. 反馈环路元器件参数
反馈环路用到的光耦和稳压器为 **CNY17-1** 和 **TL431**。

| 参数名称 | 符号 | 数值 |
| :--- | :--- | :--- |
| TL431最小输出电压 | $V_{TL431min}$ | 2.5V |
| 偏置电流 | $I_{bias}$ | 1mA |
| 光耦导通压降 | $V_f$ | 1.39V |
| 集电极-发射极饱和电压 | $V_{CE,sat}$ | 0.25V |
| 光耦电源电压 | $V_{cc}$ | 5V |
| 光耦上拉电阻 | $R_{pullup}$ | 2k$\Omega$ |
| 光电传输比最小值 | $CTR_{min}$ | 0.4 |
| 寄生电容的频率 | $f_{opto}$ | 3183kHz |
| 寄生电容 | $c_{opto}$ | 5.2pF |
| 稳压二极管电压 | $V_z$ | 15V |
| 稳压点电流 | $I_z$ | 1mA |

---

## 3. 计算过程

采用先测后补的方法，在穿越频率即 $22\text{kHz}$ 处的增益为 $+2.7\text{dB}$。设置相位角为 $50^\circ$，增益为 $-2.7\text{dB}$，使其在穿越频率以 $-1$ 的斜率穿越 $0\text{dB}$。

### 3.1 确定光耦二极管侧的限流电阻
$$R_{LED} \le \left( \frac{V_Z - V_f - V_{TL431min}}{V_{cc} - V_{CEsat} + \frac{I_{bias} \cdot R_{pullup}}{CTR_{min}}} \right) \cdot \frac{R_{pullup}}{CTR_{min}} = 1.6\text{k}\Omega$$

留有余量，取 $R_{LED}$ 为 $1\text{k}\Omega$。

### 3.2 光耦增益与相关阻容计算
* **光耦增益 $G_1$：**
$$G_1 = \frac{R_{pullup}}{R_{LED}} \cdot CTR_{min} = 0.8 \approx -1.94\text{dB}$$

* **目标增益 $G_2$：**
$$G_2 = 10^{-\frac{2.7}{20}} = 0.732$$

* **电阻 $R_2$：**
$$R_2 = G_2 \cdot R_1 = 20\text{K}$$

* **极点频率 $f_p$：**
$$f_p = \tan 50^\circ + \tan 50^\circ + 1 = 58.7\text{kHz}$$

* **电容 $C_1$：**
$$C_1 = \frac{1}{2\pi \cdot f_z \cdot R_2} = 9.7 \times 10^{-10}\text{F}$$

* **电容 $C_2$：**
$$C_2 = \frac{1}{2\pi \cdot f_p \cdot R_{pullup}} = 1360\text{pF}$$
> $C_2$ 减去光耦寄生电容，大约取 $1.3\text{nF}$。

* **电阻 $R_z$：**
$$R_z = \frac{(V_{out} - V_z) \cdot R_{pullup}}{\frac{CTR_{min} \cdot (V_{cc} - V_{CEsat})}{R_{pullup}} + (I_{LEDmax} + I_{bias}) \cdot CTR_{min}} = 1.6\text{k}\Omega$$
