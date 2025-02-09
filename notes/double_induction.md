# Double induction

Por la definición de $\mathcal{X}_k$, $T \in \mathcal{X}_k$ implica que $\alpha(T) \geq \alpha(V_k)$ y $\tau(T) \leq \tau(V_k)$.

Primero, sea $X \in \mathcal{X}_k$ con $\tau(X)$ mínimo, es decir, $\tau(X) = \tau(V_{k+1}) + 1 = \tau(V_k) - n_k + 1$. Dado que

\[
\alpha(V_{k+1}) > \alpha(X) \geq \alpha(V_k) > \alpha(W_{k+1})
\]

entonces $X$ fue llamado por $P_2$ para ser ejecutado en el tiempo $\tau(V_{k+1})$, pero no fue ejecutado. Por lo tanto, en ese momento algún predecesor $X'$ de $X$ no debió haber sido ejecutado. Así, $\tau(X') \geq \tau(V_{k+1})$. Pero esto implica que $\alpha(X') > \alpha(X)$ por la definición del Algoritmo $A$. Dado que $X$ fue ejecutado en el tiempo $\tau(X)$, $X'$ debe ser ejecutado antes que $X$ y

\[
\tau(X') \leq \tau(X) - 1 = \tau(V_{k+1}).
\]

Por lo tanto, $\tau(X') = \tau(V_{k+1})$ y $\alpha(X') > \alpha(W_{k+1})$. Solo hay una posibilidad para $X'$, a saber, $X' = V_{k+1}'$. Así, tenemos que $V_{k+1} ≺ X$.

A continuación, supongamos que para un $j$ fijo, $1 \leq j < n_k$, hemos demostrado que $X \in \mathcal{X}_k$ y  
\[
\tau(X) \leq \tau(V_k) - n_k + j \text{ implica } V_{k+1} ≺ X.
\]
Sea $X' \in \mathcal{X}_k$ con $\tau(X') = \tau(V_k) - n_k + j + 1$. Dado que  
\[
\alpha(V_{k+1}) > \alpha(X') \leq \alpha(V_k) > \alpha(W_{k+1})
\]
entonces, como antes, $X'$ fue llamado por $P_2$ en el tiempo $\tau(V_{k+1})$ para ser ejecutado, pero no estaba listo para ser ejecutado. Por lo tanto, algún predecesor $X''$ de $X'$ no había sido ejecutado para ese momento y debemos tener  
\[
\tau(X'') \geq \tau(V_{k+1}) = \tau(V_k) - n_k.
\]
Además, dado que $X'' ≺ X'$, entonces  
\[
\tau(X'') \leq \tau(X') - 1 = \tau(V_k) - n_k + j.
\]
Si $\tau(X'') = \tau(V_k) - n_k$, entonces dado que $\alpha(X'') > \alpha(X') \geq \alpha(V_k) > \alpha(W_{k+1})$, debemos tener $X'' = V_{k+1}$ y obtenemos $V_{k+1} ≺ X'$ como se requiere. Por lo tanto, podemos asumir que $\tau(X'') \geq \tau(V_k) - n_k + 1$. Por la hipótesis de inducción, dado que  
\[
\tau(V_k) - n_k + 1 \leq \tau(X'') \leq \tau(V_k) - n_k + j,
\]
vemos que $X'' \in \mathcal{X}_k$ y $V_{k+1} ≺ X''$. Por lo tanto, por la transitividad del orden parcial $≺$, obtenemos $V_{k+1} < X'$ y se completa el primer paso de inducción. Esto muestra que  
\[
V_{k+1} ≺ X \quad \text{para todo } X \in \mathcal{X}_k.
\]

Sea $I_k$ el conjunto de tareas en $\mathcal{X}_k$ que no tienen predecesores en $\mathcal{X}_k$. Dado que $V_{k+1} ≺ X$ para todo $X \in \mathcal{X}_k$, no es difícil ver que  
\[
S(V_{k+1}) \cap \mathcal{X}_k = I_k.
\]

Supongamos ahora que para algún $j$, $0 \leq j \leq n_{k+1} - 2$, hemos demostrado que si $T \in \mathcal{X}_{k+1}$ con $\tau(V_{k+1}) - j \leq \tau(T) \leq \tau(V_{k+1})$ entonces $T \prec X$ para todo $X \in \mathcal{X}_k$. Sea $X' \in \mathcal{X}_{k+1}$ con $\tau(X') = \tau(V_{k+1}) - j - 1$. Dado que $X' \in \mathcal{X}_{k+1}$ entonces $\alpha(X') > \alpha(V_{k+1})$. Así, debemos tener $N(X') \geq N(V_{k+1})$ donde recordamos que $N(X')$ se forma tomando la secuencia decreciente de valores $\alpha$ de los sucesores inmediatos de $X'$. Si existe $X'' \in S(X') \cap \mathcal{X}_{k+1}$ entonces por la hipótesis de inducción, dado que $\tau(X'') > \tau(X') = \tau(V_{k+1}) - j - 1$, es decir, $\tau(X'') \geq $\tau(V_{k+1}) - j, entonces $X'' \prec X$ para todo $X \in \mathcal{X}_k$, y por transitividad, $X' \prec X$ para todo $X \in \mathcal{X}_k$. Así, podemos asumir que $S(X') \cap \mathcal{X}_{k+1}$ está vacío. Por el Lema 1, $\alpha(T) < \alpha(V_k)$ si $\tau(T) > \tau(V_k)$. Además, vemos $\alpha(W_k) < \alpha(V_k)$. Una reflexión muestra ahora que, por la definición del Algoritmo A, si $N(X') \geq N(V_{k+1})$ y $X'$ no tiene sucesor en $\mathcal{X}_{k+1}$ entonces debemos tener $S(X') \cap \mathcal{X}_k = I_k$. Esto a su vez implica $X' \prec X$ para todo $X \in \mathcal{X}_k$. Esto completa el paso de inducción y la prueba de que si $T \in \mathcal{X}_k$ y $T' \in \mathcal{X}_{k+1}$, $0 \leq k \leq m$, entonces $T' \prec T$.