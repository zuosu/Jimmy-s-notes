非负定矩阵A最大特征值得极值刻画
$$
\begin{aligned}
\lambda_{max}(A) &= max \frac{x^TAx}{x^Tx},x\neq 0\\
\lambda_{max}(A) &= max \frac{x^TAx}{||x||^2}\\
\lambda_{max}(A) &= max \frac{x^T}{||x||}A\frac{x}{||x||}\\
\lambda_{max}(A) &= max \ \  x^TAx,|x|=1
\end{aligned}
$$
即计算函数$f(x)=x^TAx$的在定义域|x|=1下的最大值，又$U^{-1}AU=\bar{U}^TAU=\wedge$则$A=U\wedge\bar{U}^T$于是
$$
\begin{aligned}
x^TAx &= x^T(U\wedge\bar{U}^T)x\\
&=(x^TU)\wedge(\bar{U}^Tx)\\
&=\widehat{(\bar{U}^Tx)}^T\wedge(\bar{U}^Tx)\\
令\bar{U}^Tx&=y\\
原式&=\bar{y}\wedge y\\
&=\lambda_1|y_1|^2+\lambda_2|y_2|^2+\cdots+\lambda_n|y_n|^2
\end{aligned}
$$
根据$\bar{U}^T$酉变化的保长度性质$|y_1|^2+|y_2|^2+\cdots+|y_n|^2=1$该式的几何表示即为一个球面。以此作为约束条件便可求特征值的最大值，最终可求得当$y_1=1,y_2,\cdots y_n=0$时取得最大值$\lambda_1$。
