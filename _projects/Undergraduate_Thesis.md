---
title: Undergraduate Thesis 

classes: wide
---

# Finite Difference Approaches to PDEs with Matrix Solvers
# 利用矩陣計算工具以有限差分計算偏微分方程

## 專題概覽

- 此大學專題分為兩大部分：  
    - 前半學習數值方法的Finite Difference，並使用它解決數種偏微分問題，例如：
        - 1D Poisson equation
        - 2D Poisson equation
        - Heat transfer equation
        - Wave equation
    - 而學期發表也是關於解決標準的工程偏微分問題
    - 後半部分是探討在計算大型矩陣時，分析使用不同的方法的速度，如：
        - 高斯消去
        - Jacobi
        - SOR
        - CG、BiCG
    - 以及稍微研究一下MATLAB矩陣計算的優化方法

## 研究內容：前半

- 學習內容除了透過
    - Qiqi Wang教授在Youtube上的OCW
    - 課本Numerical Analysis by Richard L.Burden and J.Douglas Faires
- 剩下就是依靠這份學長提供的題目作為練習與會議時的進度指標。

<div style="text-align: center;">
    <iframe src="/assets/undergrad_thesis/FD_problems.pdf" width="50%" height="600px"></iframe>
</div>

- 因此此部分研究內容就是展示各題的延伸圖表(不然看學習筆記也太無聊)
### Problem 1

- 兩個簡單的PDE問題，在不同的∆x上有不同的誤差，且呈冪次收斂關係

- \\(i\\) 
    - Numerical_Solution
    ![image](/assets/undergrad_thesis/P1_i_1.png){: width="50%" }
    - \\(log(Solution Error)\\) vs \\(log(deltaX)\\) ： 一階收斂
    ![image](/assets/undergrad_thesis/P1_i_2.png){: width="50%" }
- \\(ii\\)
    - Numerical_Solution
    ![image](/assets/undergrad_thesis/P1_ii_1.png){: width="50%" }
    - \\(log(Solution Error)\\) vs \\(log(deltaX)\\) ： 二階收斂
    ![image](/assets/undergrad_thesis/P1_ii_2.png){: width="50%" }



## 期末成果

## 心得