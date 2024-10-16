# Communication-Convex
整理通信中的最优化问题，以及部分代码。



## Matlab CVX框架

1. cvx选择求解器，开始优化

```matlab
cvx_solver sedumi  
cvx_begin quiet
```

- `cvx_solver sedumi`：指定使用 SeDuMi 求解器。SeDuMi 是一个高效的半定规划求解器，默认为SDPT3。
- `cvx_begin quiet`：开始一个 CVX 优化问题，`quiet` 选项表示在求解过程中不显示输出信息。

2. 变量定义

```matlab
variable bt  
variable R(N,N) hermitian semidefinite
```

- `variable bt`：定义一个优化变量 `bt`，用于后续计算。
- `variable R(N,N) hermitian semidefinite`：定义一个 N×N*N*×*N* 的半正定矩阵 `R`，并且是厄米矩阵（即 `R` 的共轭转置等于 `R` 本身）

3.  表达式定义

```matlab
expression u(length(theta))  
for i=1:length(theta)  
    u(i)=(bt*Pd_theta(i)-a(:,i)'*R*a(:,i));  
end
```

- `expression u(length(theta))`：定义一个表达式 `u`，其长度与 `theta` 相同。
- 循环计算 `u(i)`，表示每个角度下的功率差。

4. 目标函数和约束：

```matlab
minimize norm(u,2)  
subject to  
trace(R)==power;   % 对总功率进行约束
```

- `minimize norm(u,2)`：目标是最小化 `u` 的二范数。
- `trace(R)==power`：约束条件，要求矩阵 `R` 的迹等于总功率。

5. 结束优化

```matlab
cvx_end
```

