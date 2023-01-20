**[[Hermite矩阵]]：** 设$A\in C^\ce{n*n}$，若$A^H = A$，则称$A$为Hermite矩阵。若$A^H = -A$，则称A为反Hermite矩阵。

类比实数域中的对称矩阵来记忆。

例如：
$$
A = \begin{pmatrix}
3&1+i&3+i\\
1-i&0&5-2i\\
3-i&5+2i&9
\end{pmatrix}
$$
则
$$
\bar{A} = 
\begin{pmatrix}
3&1-i&3-i\\
1+i&0&5+2i\\
3+i&5-2i&9
\end{pmatrix}可知(\bar{A})^T =A^H= A
$$
由此可知A为Hermite矩阵。
$$
B=\begin{pmatrix}
-3i&-1+i&2-3i\\
1+i&2i&-1-i\\
-2-3i&1-i&0
\end{pmatrix}
$$
则
$$
\bar{B} = 
\begin{pmatrix}
3i&-1-i&2+3i\\
1-i&-2i&-1+i\\
-2+3i&1+i&0
\end{pmatrix}可知(\bar{B})^T =B^H= -B
$$
由此可知B为反Hermite矩阵。

Hermite矩阵的性质：
1. $A^H = A\Leftrightarrow a_{ij} = \bar{a}_{ji}\Leftrightarrow Re(a_{ij}) = Re(a_{ji}),Im(a_{ij}) = -Im(a_{ji}).(i,j=1,\cdots ,n)$ 
即Hermite矩阵沿着主对角线对称的元素实部相同，虚部互为相反数。
2. $A^H = -A\Leftrightarrow a_{ij} = -\bar{a}_{ji}\Leftrightarrow Re(a_{ij}) = -Re(a_{ji}),Im(a_{ij}) = Im(a_{ji}).$
$(i,j=1,\cdots ,n)$即Hermite矩阵沿着主对角线对称的元素实部互为相反数，虚部相同。
3. 实对称矩阵是实Hermite矩阵。酉空间的度量矩阵是Hermite矩阵，欧氏空间的度量矩阵是实对称矩阵。