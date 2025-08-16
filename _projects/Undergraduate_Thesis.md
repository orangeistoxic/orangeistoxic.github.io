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

<div style="display: flex; flex-wrap: wrap; gap: 2px;">
    <img src="/assets/undergrad_thesis/P2_b_1.jpg" style="width: 40%;" />
    <img src="/assets/undergrad_thesis/P2_b_2.jpg" style="width: 40%;" />
    <img src="/assets/undergrad_thesis/P2_b_3.jpg" style="width: 40%;" />
    <img src="/assets/undergrad_thesis/P2_b_4.jpg" style="width: 40%;" />
    <img src="/assets/undergrad_thesis/P2_b_5.jpg" style="width: 40%;" />
</div>



- (d)、(e) 小題

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
    

### Problem 3 (還沒弄)

- 此題雖然是一個二維穩態熱傳導問題，但實際上是個一維的問題，在y軸上面沒有任何差別
- 從表格更容易看出來
![image](/assets/undergrad_thesis/P3_1.png){: width="50%" }
- 還有其他的計算部分
![image](/assets/undergrad_thesis/P3_2.png){: width="50%" }
![image](/assets/undergrad_thesis/P3_3.png){: width="50%" }

### Problem 4 

- 這是一個基於 Problem 3 的延伸題，從穩態變成一個暫態熱傳導問題，但除以之外變化不多，所以仍然可以視為一個一維問題



## 期末成果

## 心得