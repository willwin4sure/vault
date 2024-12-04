> [!question]
> Solve the recurrence $T(n)=aT(n/b) + f(n)$.

> [!theorem] (Master theorem for recurrences)
> 1. If $f(n)=\mathcal{O}(n^{\log_{b}a-\varepsilon})$ for $\varepsilon>0$, then $T(n)=\Theta(n^{\log_{b}a})$.
> 2. If $f(n)=\Theta(n^{\log_{b}a}\log^{k}n)$ for $k\geq 0$, then $T(n)=\Theta(n^{\log_{b}a}\log^{k+1}n)$.
> 3. If $f(n)=\Omega(n^{\log_{b}a+\varepsilon})$ for $\varepsilon>0$, then $T(n)=\Theta(f(n))$.