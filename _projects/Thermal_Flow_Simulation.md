---
header:
    teaser: "/assets/thermal_flow/poster_3.png"

title: Thermal Flow Simulation
classes: wide
---

# 2-D Transient-State Heat Sink Thermal Flow Simulation

## 簡介

- 本報告想要替大學專題的前半部分做一個總結，因此我們找了一個實際的熱傳導片，來試圖解決一個實際的暫態熱傳導問題，並做出它的熱傳導電腦模型。用於觀察它受到穩定的熱通量時，隨時間的溫度變化。

### 背景與目的

- 本項專題旨在模擬散熱片截面在暫態系統下，隨時間的溫度變化與傳熱係數 h 的關係。其目的是利用基礎熱傳理論,探索利用數值方法將欲模擬的模型拆解成一龐大矩陣來進行運算,並利用電腦模擬將其完整的溫度分布圖予以顯示。最後利用多個傳熱系數所算出的運算結果,探討傳熱係數、收斂溫度、收斂時間三者的關係。

### 研究方法

- 基於一現實的純鋁散熱片，我們設計了一個鳍形散熱片截面的規格，並決定其熱傳導屬性、給予一個穩定的熱通量、決定散熱氣體為空氣 (空氣的傳熱屬性由學長提供)，。
- 一個鳍形散熱片由眾多狹縫與鰭片組成，其溫度分布在理想中也是對稱分布的，因此我們只要分析半個鰭片與半個狹縫，即下圖的L型截面，我們就可以得知整個散熱片的全貌，如下圖：

![image](/assets/thermal_flow/poster_1.png){: width="75%" }

- 如果打算使用數值方法來建立此散片的電腦模型，因此我們需要將其劃分稱許多網格，並決定網格的密度，而從上圖可以看到我們將圖形分成661個節點。

- 判斷每一個網格的所在位置，如果是在邊界，則根據所在邊界 ( A 至 F ) 給予指定的邊界條件。
    - A ： 穩定的熱量來源。

    $$ -k \frac{\partial T}{\partial x} = 20000 \left( \frac{W}{m^2} \right) $$

    - B、F ： 散熱片的上下兩端連接著其他部分的散熱片，由於是對稱的，我們可以視為它為絕熱。

    $$ \frac{\partial T}{\partial y} = 0 $$

    - C、E ： 散熱面，有穩定的空氣流過。

    $$ - k \frac{\partial T}{\partial x} = h \left( T - T_{\infty} \right) $$

    - D : 同樣為散熱面，但維度不同。

    $$ - k \frac{\partial T}{\partial y} = h \left( T - T_{\infty} \right) $$

- 除此以外的網格則套用Governing Equation ：

    $$ \frac{1}{\alpha} \frac{\partial T}{\partial t} = \nabla^2 T = \frac{\partial^2 T}{\partial x^2} + \frac{\partial^2 T}{\partial y^2} $$

- 其他變數數值 ：

    - 熱通量 ： $$ q'' = 20000 \left( \frac{W}{m^2} \right) $$
    - 網格大小 ：$$ \Delta x = \Delta y = 0.2 \cdot 10^{-3} \; (m) $$
    - 時間間隔 ： $$ \Delta t = 0.04 \cdot 10^{-3} \; (s) $$
    - 環境溫度 ： $$ T_{\infty} = 20^\circ C $$
    - 熱傳導係數 ( 純鋁 ) ： $$ k = 237 \left( \frac{W}{m \cdot K} \right) $$
    - 熱擴散率 ( 純鋁 ) ： $$ \alpha = 0.097 \cdot 10^{-3} \; \left( \tfrac{m^2}{s} \right) $$

    - 勘誤 ： 在海報中寫熱擴散率 $$ \alpha $$ 是空氣的，那是筆誤。 
    - 補充 ： 由於使用的差分法是 Explicit Method ，因此在  $$ \Delta x = \Delta y $$ 下，必須符合以下的 Von-Neumann Stalbility 才會收斂：

    $$ \alpha \frac{dT}{dx^2} \leq \frac{1}{4} $$

- 空氣流速與平均熱傳導係數 ( 空氣 ) 的關係 ：

    | $$ v \; ( \tfrac{m}{s} ) $$ | 0.5     | 1       | 1.5     | 2       | 2.5     |
    |---------|---------|---------|---------|---------|---------|
    | $$ h_{avg} \; (\tfrac{W}{m^{2} \cdot K}) $$ | 30.7378 | 36.5277 | 41.5978 | 46.0269 | 49.976  |

- 接著使用 Taylor Series ，將邊界條件與 Governing Equation 中的偏微分算式轉成使用相鄰點計算出近似答案：

    $$ \frac{\partial T}{\partial t} = \frac{T(t) - T(t - \Delta t)}{\Delta t}, \quad\frac{\partial T}{\partial x} = \frac{T(x) - T(x - \Delta x)}{\Delta x}, \quad\frac{\partial T}{\partial y} = \frac{T(y) - T(y - \Delta y)}{\Delta y} $$

    $$ \frac{\partial^2 T}{\partial x^2} = \frac{T(x + \Delta x) - 2T(x) + T(x - \Delta x)}{\Delta x^2}, \quad\frac{\partial^2 T}{\partial y^2} = \frac{T(y + \Delta y) - 2T(y) + T(y - \Delta y)}{\Delta y^2} $$

- 最後將節點溫度照順序排成一個 661x1 的向量矩陣，數值皆為 $$ 20^\circ C $$ ( 假設鋁片初始為室溫 )，稱其為 $$ T_0 $$ 。而其公式經由泰勒級數轉換後，得到一個661x661的係數方陣 A ， Con 是一個 661x1 的向量矩陣，負責公式中的常數部分 ( 其實只有 A 面有，其餘皆 0 )：

![image](/assets/thermal_flow/main_mat_eq.png){: width="50%" }

- 由圖可知，經過一次計算之後，時間會經過 $$ \Delta t = 0.04 \cdot 10^{-3} \; (s) $$ ，因此我們反覆進行計算直至溫度收斂成穩態後再進行分析。
- 使用 MATLAB 進行模擬， [程式資料](https://github.com/orangeistoxic/Undergrad_Thesis_Heat_Transfer/tree/main/Heat) 。

### 結果與討論

- 我們模擬了在不同風速下的散熱結果，下圖是一個時間對上該時刻最高溫度的數據圖，可以明顯觀察到散熱的風速越快，溫度趨於穩態的速度也越快，穩態的溫度也越低。
- 但是從表格的風速是等距上升，但穩態溫度卻沒有等距下降的特點可以發現，風速對於散熱的效益會逐漸降低。

![image](/assets/thermal_flow/poster_2.png){: width="50%" }
![image](/assets/thermal_flow/poster_3.png){: width="50%" }

- 這是在 $$ h=41.59 \; (v = 1.5) $$ 時在不同時間下的溫度截面

![image](/assets/thermal_flow/poster_4.png){: width="50%" }

- 由於整體的溫度跨度過於巨大，導致單一時間點之間的溫度差距肉眼稍難辨識。但是這四張圖可以顯示不同時間點的溫度差分布都是相當接近的。

### 結論與未來

- 目前進度仍只局限於單一規格的散熱片尺寸，接下來會希望能將此模型能夠適用不同規格的散熱片，做出更廣泛的應用。
- 除此之外未來希望能夠跟先前學長留下的流速與熱傳係數做整合，去做更實際面的評估。例如探討溫度與收斂結果的關係，並研究何種流速才能最大化降溫效益。

### Reference

- INCROPERA, FRANK P. Incropera’s Principle of Heat and Mass Transfer. Global Edition, Wiley, 2017.
- Yovanovich, M. Michael. Asymptotes and Asymptotic Analysis for Development of Compact Models for Microelectronics Cooling.

### 海報

<div style="text-align: center;">
    <iframe src="/assets/thermal_flow/poster_HCrevised.pdf" width="50%" height="600px"></iframe>
</div>