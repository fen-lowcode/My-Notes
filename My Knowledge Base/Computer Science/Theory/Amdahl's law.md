#theory 

==Gene Amdahl, one of the early pioneers in computing, made a simple but insightful observation== about the ==effectiveness of improving the performance of one part of a system.== 

==This observation has come to be known as Amdahl’s law.== 

The main ==idea is that when we speed up one part of a system, the effect on the overall system performance depends on both how significant this part was and how much it sped up.== 

Consider a system in which executing some application requires time $(T)old$. Suppose some part of the system requires a fraction **==$α$==** of this time, and that we improve its performance by a factor of ==**$k$**==. That is, the component originally required time αTold , and it now requires time ==**$( α (T)old)/k$**==. The overall execution time would thus be

$$

(T) new = (1 − α)(T)old + (α (T) old)/k
$$
$$= (T) old  [(1 − α) + α/k] $$


==From this, we can compute the speedup== **$S = (T)old/(T)new as$**

$$
S = \frac{1}{(1 - \alpha) + \frac{\alpha}{k}} \tag{1.1}
$$


As an example, consider the case where a part of the system that initially consumed 60% of the time 
$(α = 0.6)$ is sped up by a factor of $3 (k = 3)$. 

Then we get a speedup of $1/[0.4 + 0.6/3] = 1.67×.$ Even though we made a substantial improvement to a major part of the system, 

our net speedup was significantly less than the speedup for the one part. 

==This is the major insight of Amdahl’s law to significantly speed up the entire system, we must improve the speed of a very large fraction of the overall system==