---
title: Undergraduate Thesis 
classes: wide


---

# Finite Difference Approaches to PDEs with Matrix Solvers

# 利用矩陣計算工具以有限差分計算偏微分方程

## 專題概覽

- 專題內容分成兩大部分：  

    - 學習數值方法 Finite Difference Method，並應用於C++或是MATLAB以解決數種PDE問題，例如：

        - 1D Poisson equation
        - 2D Poisson equation
        - Heat transfer equation
        - Wave equation

    - 學期海報發表作為一個研究總結，做了獨立一篇文章，請見 [此處](/projects/Thermal_Flow_Simulation/)。

    - 第二部分則是研究數種解決大型矩陣的方法，如：

        - 直接解法 ： Gauss Elimination 高斯消去法
        - 以及迭代方法 ：
            - Jacobi
            - SOR
            - CG、BiCG
        - 以及探討方法之間的速度與MATLAB矩陣計算的優化方法

## 研究內容 ： Learning Finite Difference Method

- 學習內容除了透過

    - Prof. Qiqi Wang 在 Youtube 上的 OCW ( [Link](https://www.youtube.com/playlist?list=PLcTpn5-ROA4xkuVzXOqIccUVyhzGUN9dw) )
    - 課本 Numerical Analysis by Richard L.Burden and J.Douglas Faires

- 最後以以下這份題目作為進度的錨點，方便追蹤進展。

<div style="text-align: center;">
    <iframe src="/assets/undergrad_thesis/FD_problems.pdf" width="50%" height="600px"></iframe>
</div>

- 以下展示內容以該份題目的研究圖表為主

### Problem 1

- Using C++ ( [Code](https://github.com/orangeistoxic/Undergrad_Thesis_Heat_Transfer/tree/main/P1) ) 解本題 (b) 部分

- 兩個常微分方程 ODE ，在不同的網格大小 ∆x 上所呈現不同的誤差大小，且呈冪次收斂關係

- ODE :　\\(i\\)

    - Numerical_Solution
    - 可見三個數值方法解 ( 藍、紅、灰 ) 以及解析解 ( 黃 ) 三者相當接近，這點可以在第二張圖數值解誤差都極小這點同樣獲得驗證。

    ![image](/assets/undergrad_thesis/P1_i_1.png){: width="50%" }

    - \\(log(Solution Error)\\) vs \\(log(deltaX)\\) 
    - 注意上下圖的顏色不代表同樣的解法
    - 由下圖可以看出，網格大小與誤差成一階收斂的關係

    ![image](/assets/undergrad_thesis/P1_i_2.png){: width="50%" }

- ODE :　\\(ii\\)

    - Numerical_Solution
    - 本題理論上使用差分法不會有誤差，但我猜測是在電腦計算時的精度問題，導致存在一些極小的誤差。

    ![image](/assets/undergrad_thesis/P1_ii_1.png){: width="50%" }

    - \\(log(Solution Error)\\) vs \\(log(deltaX)\\)

    ![image](/assets/undergrad_thesis/P1_ii_2.png){: width="50%" }

### Problem 2

- 此為一個典型的一維的 Heat Equation 問題，利用 FTCS ( forward time central space )、BTCS ( backward time central space ) 以及 CN ( Crank-Nicolson ) 三種不同的差分法來解開。    

- (b) 小題

    - 此題討論三種方法之Trucation Error對於時間與空間大小的收斂關係
    - 從下圖可以看出FTCS和BTCS ：

        - 對時間一階收斂
        - 對空間二階收斂

    - 而對於CN ：

        - 對時間二階收斂
        - 對空間二階收斂


(改改改矮改改)
<div style="display: flex; flex-wrap: wrap; gap: 2px;">
    <img src="/assets/undergrad_thesis/P2_b_1.jpg" style="width: 40%;" />
    <img src="/assets/undergrad_thesis/P2_b_2.jpg" style="width: 40%;" />
    <img src="/assets/undergrad_thesis/P2_b_3.jpg" style="width: 40%;" />
    <img src="/assets/undergrad_thesis/P2_b_4.jpg" style="width: 40%;" />
    <img src="/assets/undergrad_thesis/P2_b_5.jpg" style="width: 40%;" />
</div>



- (d)、(e) 小題

    - [Code](https://github.com/orangeistoxic/Undergrad_Thesis_Heat_Transfer/tree/main/P2)
    - 此二題討論三者與 Von-Neumann Stability 之間的關係。
    - Von-Neumann Stability 表示如果在此條件下才會收斂，則他是 Condintionally Stable，若無論如何都會收斂，則稱為 Unconditionally Stable。

    ![image](/assets/undergrad_thesis/P2_d_von.png){: width="25%" }

    - 使用了2 norm跟 max norm 兩種方式計算誤差
    - FTCS ：

    ![image](/assets/undergrad_thesis/P2_d_1.png){: width="100%" }

    ![image](/assets/undergrad_thesis/P2_d_2.png){: width="50%" }
    ![image](/assets/undergrad_thesis/P2_d_3.png){: width="50%" }

    - 根據資料可以得知，FTCS 是Conditionally Stable。而且斜率也符合對空間的二階收斂。
    - BTCS：

    ![image](/assets/undergrad_thesis/P2_e_1.png){: width="100%" }

    ![image](/assets/undergrad_thesis/P2_e_2.png){: width="50%" }
    ![image](/assets/undergrad_thesis/P2_e_3.png){: width="50%" }

    - 根據資料可以得知，FTCS 是Unconditionally Stable。而且斜率也符合對空間的二階收斂。
    - CN Method：

    ![image](/assets/undergrad_thesis/P2_e_4.png){: width="100%" }

    ![image](/assets/undergrad_thesis/P2_e_5.png){: width="50%" }
    ![image](/assets/undergrad_thesis/P2_e_6.png){: width="50%" }

    - 根據資料可以得知，CN Method 是Unconditionally Stable。而且斜率也符合對空間的二階收斂。還有不同 r 誤差值比起 BTCS 更加接近。
    

### Problem 3

- 這是一個二維熱傳導問題，它沒有熱源 ( Heat Source ) 與吸熱源 ( Heat Sink )。而且由於在 y 軸上沒有變化，因此他實際上是一個一維問題。

![image](/assets/undergrad_thesis/P3_problem.png){: width="50%" }

- (a) 、 (b) 、 (c) 小題

    - 使用解析解計算各處溫度，以及解釋為何他可以簡化成一維問題。

    ![image](/assets/undergrad_thesis/P3_2.png){: width="50%" }
    ![image](/assets/undergrad_thesis/P3_3.png){: width="50%" }

- (f) 小題

    - [Code](https://github.com/orangeistoxic/Undergrad_Thesis_Heat_Transfer/tree/main/P3)
    - 使用前面小題找到的邊界條件與Governing Equation，使用差分法求出答案。

    ![image](/assets/undergrad_thesis/P3_1.png){: width="50%" }

- 一維穩態，又是線性的問題其實相當簡單，但此題主要是為了引入 Problem 4 以及學期海報的題目。

### Problem 4 

- 補充 ： 學期海報為類似題型的延伸版本，詳細計算可以去 [這裡](/projects/Thermal_Flow_Simulation/) 。

- 這是一個基於 Problem 3 的延伸題，從穩態變成一個暫態熱傳導問題。雖然仍舊可以視為一維題目來看，但是為了練習仍舊當作普通的二維題目來進行模擬。

- [Code](https://github.com/orangeistoxic/Undergrad_Thesis_Heat_Transfer/tree/main/problem4) ，從此題開始都用 MATLAB了。S

- 同樣與 Problem 3 一樣，都要求出穩態時的溫度分布 ( a 小題 ) ，除此之外找到收斂時的時間 ( b 小題 ) ，最後以收斂時間分成五份，觀察五段時間之間不同的溫度差異 ( c 小題 ) 。

![image](/assets/undergrad_thesis/P3_problem.png){: width="50%" }

- 暫態問題多了一些變數必須考慮：
    - 熱擴散率 ( 銅 ) ： $$ \alpha = 10^{-4} \; (\frac{m^2}{s}) $$
    - 初始溫度 ( 此題與環境溫度相同 ) ： $$ T_0 = 20 \; (^\circ C) $$

- 二維暫態熱傳導的主導公式 Governing Equation 是：

$$ \frac{1}{\alpha} \frac{\partial T}{\partial t} = \nabla^2 T = \frac{\partial^2 T}{\partial x^2} + \frac{\partial^2 T}{\partial y^2} $$

- 邊界條件 ： 

    - 左側 ( 熱源面 ) ： 

    $$  q^{''} = -k \frac{\partial T}{\partial x} $$

    - 上下側 ( 絕熱面 ) ：

    $$ \frac{\partial T}{\partial y} = 0 $$

    - 右側 ( 散熱面 ) : 

    $$ - k \frac{\partial T}{\partial x} = h \left( T - T_{\infty} \right) $$

    - 最後使用泰勒級數將算式中的偏微分部分轉換成以相近點計算出近似值的算式。 ( 在下圖的筆記可以找到 )

- 其餘變數：

    - 熱通量 ： $$  q^{''} = 4000 \; (\frac{W}{m^2}) $$
    - 熱傳導係數 ： $$ k = 400 \left( \frac{W}{m \cdot K} \right) $$
    - 熱擴散率 ： $$ \alpha = 10^{-4} \; (\frac{m^2}{s}) $$
    - 初始溫度 ： $$ T_0 = 20 \; (^\circ C) $$
    - 熱傳導係數 ( 散熱 ) : $$ h = 400 \left( \frac{W}{m \cdot K} \right) $$ 
    - 環境溫度 ： $$ T_{\infty} = 20^\circ C $$
    - 網格大小 ： $$ \Delta x = \Delta y = 1 \cdot 10^{-2} \; (m) $$
    - 時間間隔 ： $$ \Delta t = 10 \; (s) \; or \; 1 \; (s) $$

    - 補充 ： 在第一次的模擬中發現收斂的時間相當久，因此將在後續的模擬中會在1秒與10秒之間來回變動，依據不同方法的速度決定。

- ( a ) 小題

    | All Condition and Equation | Final data put into Code |
    |-------|-------|
    | ![image](/assets/undergrad_thesis/P4_note1.jpg){: width="90%" } | ![image](/assets/undergrad_thesis/P4_note2.jpg){: width="95%" }  |

- ( b ) 小題

    - 下圖為每一個迭代的最高溫度所製的圖表 ( x 軸為時間 $$ (s) $$ ， y 軸為溫度 $$ (^\circ C) $$ )
    - 可以看到溫度收斂的時間大約為 60000 秒。
    - 本次模擬的 $$ dt = 10 \; (s) $$ 。

    ![image](/assets/undergrad_thesis/P4_b_1.png){: width="50%" }

- ( c ) 小題

    - " tss " 指抵達穩態溫度的時間。
    - 與上圖是同一次模擬， $$ dt = 10 \; (s) $$ 。

    | 1/5 tss |
    |------|
    | ![image](/assets/undergrad_thesis/P4_c_tts1.png){: width="75%" } |

    | 2/5 tss | 3/5 tss |
    |------|------|------|
    | ![image](/assets/undergrad_thesis/P4_c_tts2.png) | ![image](/assets/undergrad_thesis/P4_c_tts3.png) |

    | 4/5 tss | 5/5 tss |
    |------|------|
    | ![image](/assets/undergrad_thesis/P4_c_tts4.png) | ![image](/assets/undergrad_thesis/P4_c_tts5.png) |

- 延伸 ： 不同差分法與其計算速度 (還沒做) 

## 期末成果

## 心得