---
header:
    teaser: "/assets/undergrad_thesis/P4_c_tts2.png"

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

    | ![image](/assets/undergrad_thesis/P2_b_1.jpg){: width="90%" } | ![image](/assets/undergrad_thesis/P2_b_2.jpg){: width="90%" } |

    | ![image](/assets/undergrad_thesis/P2_b_3.jpg){: width="90%" } | ![image](/assets/undergrad_thesis/P2_b_4.jpg){: width="90%" } |

    ![image](/assets/undergrad_thesis/P2_b_5.jpg){: width="50%" }



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

    | ![image](/assets/undergrad_thesis/P3_2.png){: width="90%" } | ![image](/assets/undergrad_thesis/P3_3.png){: width="90%" } |
    
- (f) 小題

    - [Code](https://github.com/orangeistoxic/Undergrad_Thesis_Heat_Transfer/tree/main/P3)
    - 使用前面小題找到的邊界條件與Governing Equation，使用差分法求出答案。

    ![image](/assets/undergrad_thesis/P3_1.png){: width="50%" }

- 一維穩態，又是線性的問題其實相當簡單，但此題主要是為了引入 Problem 4 以及學期海報的題目。

### Problem 4 

- 補充 ： 學期海報為類似題型的延伸版本，詳細計算可以去 [這裡](/projects/Thermal_Flow_Simulation/) 。

- 這是一個基於 Problem 3 的延伸題，從穩態變成一個暫態熱傳導問題。雖然仍舊可以視為一維題目來看，但是為了練習仍舊當作普通的二維題目來進行模擬。

- [Code](https://github.com/orangeistoxic/Undergrad_Thesis_Heat_Transfer/tree/main/problem4) ，從此題開始都用 MATLAB了。

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

###  Problem 4 延伸 ： 不同解線性方程的數值方法與其計算速度

- 如果說有限差分法是把一PDE問題轉為離散的代數矩陣，那以下的不同方法便是如何將這個矩陣 ( 通常很大 ) 快速的算出來。

#### 誤差計算 ：
- 由於以下兩種方法都是以迭代的方式讓結果趨於收斂，而判斷標準便是使誤差小於一個指定大小。

$$ Error = | x^{k} - x^{exact} | $$

- $$ x^{k} $$ 是數值解在迭代 k 次後的向量，而 $$ x^{exact} $$ 則是解析解。兩者每個元素相差之絕對值總和便是第 k 次的誤差。

- 但是這裡實作的方式是兩次相鄰迭代的相差，主要是為了模擬當沒有解析解時所使用的方法。

$$ Error = | x^{k} - x^{k-1} | $$

#### Gauss–Seidel 方法

- 假設有一個 $$ Ax=b $$ 的矩陣，則我們可以將他拆成 $$ (D+L+U)x=b　$$ ，其中 $$ D $$ 是對角矩陣， $$ L $$ 是下三角矩陣， $$ U $$ 是上三角矩陣。
- 而 Gauss–Seidel 方法則是將算式轉為如下：

$$ (D+L)x^{t+1}=b-Ux^{t} $$

- 其中 $$ x^{t} $$ 代表第 t 次的迭代，而 $$ x^{t+1} $$ 則是第 t+1 次。以 Problem 4 來看就是第 t 時刻以及他的下一個時刻。
- 將上述式子做轉換之後我們得到如下 ( A是一個 n by n 方陣 ) ：

$$ x_i^{(t+1)} \;=\; \frac{1}{a_{ii}} \left(b_i - \sum_{j=1}^{i-1} a_{ij} x_j^{(t+1)} - \sum_{j=i+1}^{n} a_{ij} x_j^{(t)}\right),\quad i = 1,2,\dots,n $$

- Gauss–Seidel 方法的概念是當你要求 $$ x_i^{t+1} $$ ，也就是第t+1輪的第 i 個值時，使用 Jacobi 方法去計算。但是對於已經有更新到第 t+1 輪的新數據，也就是第 1 到 i-1 個，就直接使用新數據，而剩下的就沿用第 t 輪的舊數據 ( 第 i+1 到 n 個 ) 。
- 此方法的特性是比 Jacobi 法收斂更快，而且適用於大型稀疏矩陣，就像我們的問題 !
- 但是此方法並非一定會收斂，它必須是 " 嚴格對角優勢矩陣 " 或是 " 正定矩陣 " 才會保證收斂。

- 成果：
    - 下圖為直接用 MATLAB 內建矩陣計算與 Gauss–Seidel 方法 ( 我自己手刻 ) 兩者的執行時間。
    - 皆模擬 Problem 4 經過 70000 秒的最高溫度變化 $$ (dt = 10 \; (s)) $$ 
    - Gauss–Seidel 方法誤差標準 ： $$ Error < 10^{-5}    $$

    | MATLAB 計算， RunTime : 220.49s | Gauss–Seidel 方法， RunTime : 46041.20s |
    |------|------|
    | ![image](/assets/undergrad_thesis/P4_ex_MATLAB.png){: width="90%" } | ![image](/assets/undergrad_thesis/P4_ex_GauSei.jpg){: width="90%" } |

- 可以明顯看出 MATLAB 內建矩陣計算非常的快，在本研究的過程中沒有其他方法會比它快。這會在文章最後會稍微提到為甚麼。
- Note : MATLAB 內建矩陣計算 " 並不是 " 迭代法，它本質上就是高斯消去法而已。

#### Jacobi 方法

- Gauss–Seidel 方法是基於 Jacobi 方法的延伸，因此兩者的概念也差不多
- 假設有一個 $$ Ax=b $$ 的矩陣，則我們可以將他拆成 $$ (D+L+U)x=b　$$ ，其中 $$ D $$ 是對角矩陣， $$ L $$ 是下三角矩陣， $$ U $$ 是上三角矩陣。
- 而 Jacobi 方法則是將算式轉為如下：

$$ Dx^{t+1}=b-(L+U)x^{t} $$

- 其中 $$ x^{t} $$ 代表第 t 次的迭代，而 $$ x^{t+1} $$ 則是第 t+1 次。以 Problem 4 來看就是第 t 時刻以及他的下一個時刻。
- 將上述式子做轉換之後我們得到如下 ( A是一個 n by n 方陣 ) ：

$$ x_i^{(t+1)} \;=\; \frac{1}{a_{ii}} \left(b_i - \sum_{j \neq i}^{n} a_{ij} x_j^{(t)}\right),\quad i = 1,2,\dots,n $$

- Jacobi 法與 Gauss–Seidel 法的最大差別就是， Jacobi 法永遠都是使用前一個時刻的結果來做計算，因此 Jacobi 法可以實現平行計算，而 Gauss–Seidel 法不行。
- 標準收斂情況 ( 必要且充分 ) 如下，其中 $$ R = (L+U) $$ ， $$ ρ $$ 是譜半徑，矩陣所有特徵值的絕對值的最大值。

$$ \rho (D^{-1}R) < 1 $$ 

- 而保證收斂 ( 充分 ) 的條件是 " 嚴格對角優勢矩陣 " ，指對於每一行，對角線上的元素之絕對值大於其餘元素絕對值的和。 ( 若不符合則還是有可能收斂 )

$$ |a_{ii} | = \sum_{j \neq i}^{n} | a_{ij} | $$

- 成果：
    - 此方法也是我手刻。
    - 通常我都用 $$ dt = 10 \; (s) $$ ，來計算，但是 Jacobi 法在如此大的時間跳動下不會收斂，因此我改成 $$ dt = 1 \; (s) $$ ，但是仍然模擬 70000 秒。
    - 誤差標準 ： $$ Error < 10^{-10}    $$
    - 儘管 $$ dt $$ 縮小，收斂標準變嚴格，但是 Jacobi 方法的執行速度仍舊遠快於 Gauss–Seidel 方法。

    | Jacobi 方法， RunTime : 11703.28s |
    |------|
    | ![image](/assets/undergrad_thesis/P4_ex_Jacobi.png){: width="90%"} |


## 研究內容 ： 解線性方程的數值方法與其計算速度

- 自此開始便到了專題的後半部分，現在的研究重心轉向了數種解大型矩陣的計算方法以及他們的速度。就像 Problem 4 的延伸一樣。

- 所有的 Code 都在 [這裡](https://github.com/orangeistoxic/Undergrad_Thesis_Heat_Transfer/tree/main/Problem3_Extend)

- 並且，為了保持重點在不同計算方法的速度，我們回到了 Problem 3 ，二維穩態熱傳導問題。這樣就不需要考慮時間間隔的問題。

### 複習 : 誤差計算 ：
- 以迭代的方式讓結果趨於收斂，而判斷標準便是使誤差小於一個指定大小。

$$ Error = | x^{k} - x^{k-1} | $$

### 四方法計算 Problem 3 

- Gauss Elimination 
    - 也就是直接計算，這裡直接在 MATLAB 呼叫 `x=A\b`。
    - Remind : 他是唯一一個非迭代法的算法。
    - Note : 但是直接呼叫 MATLAB 算式本身卻複雜很多，這關係到 MATLAB 本身對於矩陣計算的優化，最後面會討論。

    $$ Ax=b $$

- Gauss-Seidel
    - 上面 Problem 4 有提到，為 Jacobi法的延伸。
    - $$ D $$ 是對角矩陣， $$ L $$ 是下三角矩陣， $$ U $$ 是上三角矩陣。

    $$ (D+L)x^{t+1}=b-Ux^{t} $$

- Successive Over-Relaxation ( SOR )
    - 逐次超鬆弛迭代法 SOR 是 Gauss-Seidel 法的一種變體，當時提出的目的是為了能在計算機上面自動求解方程組。
    - $$ D $$ 是對角矩陣， $$ L $$ 是下三角矩陣， $$ U $$ 是上三角矩陣。

    $$ (D+\omega L)x^{t+1} = \omega b - [\omega U + (\omega -1)D]x^{t} $$

    - 其中 $$ \omega > 1 $$ 是鬆弛因子（ relaxation factor ），而當 $$ \omega = 1 $$ 時就是Gauss-Seidel 法。
    - 而 $$ \omega $$ 的選擇這裡就不贅述 ( 因為我也不熟 )。

- Biconjugate ( BiCG )
    - 雙共軛梯度法 BiCG 是一種用來解一般非對稱的線性系統，他是由 Conjugate Gradient 共軛梯度法的推廣版本。
    - 此方法的原理有些複雜，但是基本概念是建立兩組互相雙共軛的 Krylov 子空間。透過這樣的構造，可以保證殘差向量沿著「雙共軛方向」逐步收斂。

    - 他適合 非對稱矩陣 的大規模稀疏線性系統，而且可以透過預處理來加速迭代的計算。
    - 後面還有一些變體會再次提到。
    

- 成果：
    - Note : GE 和 BiCG 是直接用內建函式來計算，而 GS 和 SOR 是手刻代碼。
    - 網格數量 ： 我令在 Problem 3 中的 $$ GridSize = \Delta x = \Delta y $$，而 $$ GridSize $$ 大小從 $$ 2^{-3} $$ 到 $$ 2^{-8} $$ 共六種，再配合題目的金屬大小便可以推得網格數量。而且網格數量即為 n ，而 矩陣 A 就是一個 n by n 的矩陣。
    - 收斂標準 ： $$ Error < 5 \times 10^{-5} $$
    - SOR 的 $$ \omega = 0.5 $$
    - BiCG 沒有預處裡

    | RunTime (s) vs 網格數量 | RunTime (s) vs $$ log_{10} 網格數量 $$ |
    |------|------|
    | ![image](/assets/undergrad_thesis/P3EX_4Meth_Time.png){: width="90%"} | ![image](/assets/undergrad_thesis/P3EX_4Meth_Time_log.png){: width="90%"} |

    - IterCount 代表迭代的次數

    | IterCount vs 網格數量 | IterCount vs $$ log_{10} 網格數量 $$ |
    |------|------|
    | ![image](/assets/undergrad_thesis/P3EX_4Meth_Iter.png){: width="90%"} | ![image](/assets/undergrad_thesis/P3EX_4Meth_Iter_log.png){: width="90%"} |

    - Note : 可以看到除了 GE ( MATLAB 本身自己的優化 ) 以外，是 BiCG 最快，這有可能是因為 MATLAB 本身的內建函式中，它自動偵測到結果不太收斂，所以自己將收斂標準提升至 $$ 1 $$ 到 $$ 10^{-1} $$ 之間。

    - BiCG Residual in each Iteration
        - 圖太多便沒有每個 $$ GridSize $$ 結果都擺。
    
    | $$ GridSize = 2^{-3} $$ | $$ GridSize = 2^{-5} $$ | $$ GridSize = 2^{-7} $$ |
    |------|------|------|
    | ![image](/assets/undergrad_thesis/P3EX_4Meth_BiCG_G2-3.png){: width="100%"} | ![image](/assets/undergrad_thesis/P3EX_4Meth_BiCG_G2-5.png){: width="100%"} | ![image](/assets/undergrad_thesis/P3EX_4Meth_BiCG_G2-7.png){: width="100%"} |

### SOR 分析不同 $$ \omega $$

- 由於 Gauss-Seidel 法與  Jacobi 法的譜半徑 ( spectral radius )， $$ \rho (T_g) $$ 跟 $$ \rho (T_j) $$ 結果皆大於 1 ，因此無法用以下公式找出最佳 $$ \omega $$ 。

![image](/assets/undergrad_thesis/P3EX_SOR_optimalw.png){: width="30%"}

- 因此我用 Trial-and-Error Method 嘗試找出最佳 $$ \omega $$ 。 ( 就是暴力解 )
- 成果
    - 收斂標準 ： $$ Error < 5 \times 10^{-6} $$
    - IterCount 代表迭代的次數

    | IterCount vs $$ log_{10} 網格數量 $$ | RunTime (s) vs $$ log_{10} 網格數量 $$ |
    |------|------|
    | ![image](/assets/undergrad_thesis/P3EX_SOR_Iter_log.png){: width="90%"} | ![image](/assets/undergrad_thesis/P3EX_SOR_Time_log.png){: width="100%"} |

    - 可以看到當 $$ \omega = 1.9 $$ 時大約有最佳的速度。
    - Note : 當 $$ \omega > 1.95 $$ 時會發生太大的震盪導致結果不會收斂。


### 分析不同的預處裡 Preconditions

- 像 BiCG 這種收斂速度過快的迭代法高度依賴矩陣條件數，如果條件數很差會導致收斂很慢甚至發散。因此預處裡的目的便是使問題矩陣本身能夠更快收斂。
- 以下我們便用數種常見的預處理矩陣 ( SOR 、 ILU ) 來分析他們的速度。
- Incomplete LU Factorization ( ILU 分解 )
    - 類似 LU 分解，但是在大規模稀疏矩陣下， LU 分解會產生許多 fill-in ，指原本是 0 的地方變成非 0 ，這會使記憶體與計算時間大幅增加。
    - 因此 ILU 的概念就是在分解的過程中捨棄部分非零元素，以保持稀疏結構，其結果近似 LU 分解。

- 此外 MATLAB 中有內建數種不同的 BiCG 函式，還有 GMRES ，便一起模擬了。

- 成果
    - RunTime (s) vs $$ log_{10} 網格數量 $$

    | No Precondition | Precondition: SOR (w=1.9) | Precondition: ILU |
    |------|------|------|
    | ![image](/assets/undergrad_thesis/P3EX_PreCon_NoP_Time_log.png){: width="90%"} | ![image](/assets/undergrad_thesis/P3EX_PreCon_SOR_Time_log.png){: width="90%"} | ![image](/assets/undergrad_thesis/P3EX_PreCon_ILU_Time_log.png){: width="90%"} |

    - IterCount vs $$ log_{10} 網格數量 $$
        - IterCount 代表迭代的次數 

    | No Precondition | Precondition: SOR (w=1.9) | Precondition: ILU |
    |------|------|------|
    | ![image](/assets/undergrad_thesis/P3EX_PreCon_NoP_Iter_log.png){: width="90%"} | ![image](/assets/undergrad_thesis/P3EX_PreCon_SOR_Iter_log.png){: width="90%"} | ![image](/assets/undergrad_thesis/P3EX_PreCon_ILU_Iter_log.png){: width="90%"} |

    - 以下是各種組合的 Residual 。通常每一種組合都會模擬 $$ GridSize = 2^{-3} $$ 到 $$ GridSize = 2^{-8} $$ ，但是如果顯示出每一個的 Residual 會太多，引此每種組合使顯示兩種 $$ GridSize $$ 。
    

    - BiCG Residual ( No Precondition )

    | $$ GridSize = 2^{-4} $$ | $$ GridSize = 2^{-7} $$ |
    |------|------|
    | ![image](/assets/undergrad_thesis/P3EX_PreCon_BiCG_Resi_NoP_2-4.png){: width="80%"} | ![image](/assets/undergrad_thesis/P3EX_PreCon_BiCG_Resi_NoP_2-7.png){: width="80%"} |

    - BiCGSTAB Residual ( No Precondition )

    | $$ GridSize = 2^{-4} $$ | $$ GridSize = 2^{-7} $$ |
    |------|------|
    | ![image](/assets/undergrad_thesis/P3EX_PreCon_BiCGSTAB_Resi_NoP_2-4.png){: width="80%"} | ![image](/assets/undergrad_thesis/P3EX_PreCon_BiCGSTAB_Resi_NoP_2-7.png){: width="80%"} |

    - BiCGSTABl Residual ( No Precondition )

    | $$ GridSize = 2^{-4} $$ | $$ GridSize = 2^{-7} $$ |
    |------|------|
    | ![image](/assets/undergrad_thesis/P3EX_PreCon_BiCGSTABl_Resi_NoP_2-4.png){: width="80%"} | ![image](/assets/undergrad_thesis/P3EX_PreCon_BiCGSTABl_Resi_NoP_2-7.png){: width="80%"} |

    - GMRES Residual ( No Precondition )
        - GMRES 在沒有預處理的情況下無法收斂。沒有結果。

    - BiCG Residual ( SOR $$ (\omega = 1.9) $$ )
        - 此組合的震盪太大，導致 $$ GridSize = 2^{-7} $$ 跟 $$ GridSize = 2^{-8} $$ 無法收斂。

    | $$ GridSize = 2^{-3} $$ | $$ GridSize = 2^{-6} $$ |
    |------|------|
    | ![image](/assets/undergrad_thesis/P3EX_PreCon_BiCG_Resi_SOR_2-3.png){: width="80%"} | ![image](/assets/undergrad_thesis/P3EX_PreCon_BiCG_Resi_SOR_2-6.png){: width="80%"} |

    - BiCGSTAB Residual ( SOR $$ (\omega = 1.9) $$ )

    | $$ GridSize = 2^{-4} $$ | $$ GridSize = 2^{-7} $$ |
    |------|------|
    | ![image](/assets/undergrad_thesis/P3EX_PreCon_BiCGSTAB_Resi_SOR_2-4.png){: width="80%"} | ![image](/assets/undergrad_thesis/P3EX_PreCon_BiCGSTAB_Resi_SOR_2-7.png){: width="80%"} |

    - BiCGSTABl Residual ( SOR $$ (\omega = 1.9) $$ )

    | $$ GridSize = 2^{-4} $$ | $$ GridSize = 2^{-7} $$ |
    |------|------|
    | ![image](/assets/undergrad_thesis/P3EX_PreCon_BiCGSTABl_Resi_SOR_2-4.png){: width="80%"} | ![image](/assets/undergrad_thesis/P3EX_PreCon_BiCGSTABl_Resi_SOR_2-7.png){: width="80%"} |


    - GMRES Residual ( SOR $$ (\omega = 1.9) $$ )

    | $$ GridSize = 2^{-4} $$ | $$ GridSize = 2^{-7} $$ |
    |------|------|
    | ![image](/assets/undergrad_thesis/P3EX_PreCon_GMRES_Resi_SOR_2-4.png){: width="80%"} | ![image](/assets/undergrad_thesis/P3EX_PreCon_GMRES_Resi_SOR_2-7.png){: width="80%"} |

    - BiCG Residual ( ILU )

    | $$ GridSize = 2^{-4} $$ | $$ GridSize = 2^{-7} $$ |
    |------|------|
    | ![image](/assets/undergrad_thesis/P3EX_PreCon_BiCG_Resi_ILU_2-4.png){: width="80%"} | ![image](/assets/undergrad_thesis/P3EX_PreCon_BiCG_Resi_ILU_2-7.png){: width="80%"} |

    - BiCGSTAB Residual ( ILU )

    | $$ GridSize = 2^{-4} $$ | $$ GridSize = 2^{-7} $$ |
    |------|------|
    | ![image](/assets/undergrad_thesis/P3EX_PreCon_BiCGSTAB_Resi_ILU_2-4.png){: width="80%"} | ![image](/assets/undergrad_thesis/P3EX_PreCon_BiCGSTAB_Resi_ILU_2-7.png){: width="80%"} |

    - BiCGSTABl Residual ( ILU )

    | $$ GridSize = 2^{-4} $$ | $$ GridSize = 2^{-7} $$ |
    |------|------|
    | ![image](/assets/undergrad_thesis/P3EX_PreCon_BiCGSTABl_Resi_ILU_2-4.png){: width="80%"} | ![image](/assets/undergrad_thesis/P3EX_PreCon_BiCGSTABl_Resi_ILU_2-7.png){: width="80%"} |

    - GMRES Residual ( ILU )

    | $$ GridSize = 2^{-4} $$ | $$ GridSize = 2^{-7} $$ |
    |------|------|
    | ![image](/assets/undergrad_thesis/P3EX_PreCon_GMRES_Resi_ILU_2-4.png){: width="80%"} | ![image](/assets/undergrad_thesis/P3EX_PreCon_GMRES_Resi_ILU_2-7.png){: width="80%"} |


### MATLAB 矩陣計算討論

- 從前面的數項圖表可以發現，直接使用MATLAB的 `x = A\b` 總是比其他方法還要快上許多，甚至以時間複雜度來講也是比其他方法還要快，如果單純以高斯消去法的話這是不可能的。

- 我的指導教授表示，直接方法在小問題上確實比較快，但是在大問題上會因為無法以平行計算而導致計算變慢，但是這還是不能解釋在時間複雜度上的問題，而且我懷疑 50000 x 50000 以上的矩陣還能叫做小問題嗎。

- 因此我做了一些調查，發現了一些導致MATLAB矩陣計算如此高效的可能因素。

#### MATLAB 自帶優化　

- 使用  Intel MKL BLAS
    - MATLAB 使用了 Intel 的 Math Kernel Library (MKL) BLAS ，其使用了 SIMD (Single Instruction Multiple Data) 來優化計算效率，它使 CPU 可以藉由將矩陣劃分為向量塊 ( Vectorized Blocks ) 來同時計算多個數值。同時 MKL 也支援多線程 ( Multi-Threading )，將多個計算內容分到不同的 CPU 來提升性能表現。

- 高度優化的快取管理
    - 沒什麼可說的， MATLAB 內部將這點的優化做的很好

- 自適應的求解方式選擇
    - MATLAB 美式在計算前會先快速分析矩陣的類型 ( 如稀疏、上三角、對角等等 )，而不是直接使用高斯消去法，以此來減少計算時間。
    - 這是來自 MATLAB 文件，當使用 mldivide 也就是 `\` 運算子時會經過下面流程圖。

    | 密集矩陣 | 稀疏矩陣 |
    |------|------|
    | ![image](/assets/undergrad_thesis/MAT_solver_dense.png){: .img-bg-white width="100%"} | ![image](/assets/undergrad_thesis/MAT_solver_sparse.png){: .img-bg-white width="100%"} |

#### A\b 與 BiCGSTAB 的比較

- 我稍微改善了 BiCGSTAB 配上 ILU 的程式碼，並重跑了一次，可以發現兩者的複雜度相當的接近。
- Note :　BiCGSTAB 中間有個缺失的資料點，那是因為它在網格大小為 $$ 2^{-8} $$ 結果的震盪太大導致函式無法收斂。

![image](/assets/undergrad_thesis/MAT_GE_vs_BiCGSTAB.jpg){: width="50%"}


## 心得

- 這個持續了一年多的熱流專題，我一直覺得內容沒有很酷，中間老實說也有不少偷懶的時刻。但是在我整理完之後，其實還是發掘了不少有趣的部份，尤其是第三四題以及他們的延伸，對於不同算法的討論，並找出他的運算速度並嘗試優化它們。

- 最後的研究結論實在是有點反高潮，弄了這麼久結果還是比不上一個簡單的 MATLAB 運算子，實在是有點沮喪。

- 最後感謝吳教授以及蔡學長，在大三到大四初這段時間持續的每周會議，不斷給予指正與討論，謝謝你們。