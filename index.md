# FIR 필터를 설계할때 필터탭(Taps)은 몇개로 결정해야 할까?

FIR 필터를 설계할때면 항상 갖게 되는 의문에 대해 한번 검색해 보았다. 그중 마음에 드는 [내용](https://dsp.stackexchange.com/questions/31066/how-many-taps-does-an-fir-filter-need)이 있어 정리해 본다.  

Bellanger, M 이라는 사람이 쓴책을 인용하면 , FIR 필터에서 Taps를 결정하는 대 있어 핵심은 차단 주파수가 아니라 차단하고자 하는 대역의 신호의 감쇠량과 보존하려고 하는 신호에 포함된 리플 정도, 그리고 가장 중요한 것은 통과대역에서 정지대역의 경사도를(대역전환 폭-천이간격)를 얼마로 할것인가가 Taps의 수를 결정하는 주 요인이다. 

그리고, 
위 조건을 고려한 FIR필터의 필요 Taps은 다음 공식으로 결정할수 있다. 
<br/>
<br/>
<p> $$N\approx \frac 23 \log_{10} \left[\frac1{10 \delta_1\delta_2}\right]\,\frac{f_s}{\Delta f}$$
</p>
$$\begin{align} f_s &amp;\text{ : 샘플링 주파수(sampling rate)}\\
\Delta f&amp; \text{ : 주파수 천이 간격(the transition width),}\\
        &amp; \text{ 천이 간격은 통과대역주파수의 마지막과 저지대역 맨처음 주파수간의 간격이다. }\\
\delta_1 &amp;\text{ the ripple in passband,}\\
         &amp;\text{ ie. "how much of the original amplitude can you afford to vary"}\\
\delta_2 &amp;\text{ the suppresion in the stop band}.
\end{align}$$

<br/>


$\frac{f_s}{100}$

$\Delta f=\frac{f_s}{200}$

$\delta_2 = -60\text{ dB} = 10^{-3}$

$\delta_1 = 10^{-4}$


$$\begin{align}
N_\text{Tommy's filter} &amp;\approx \frac 23 \log_{10} \left[\frac1{10 \delta_1\delta_2}\right]\,\frac{f_s}{\Delta f}\\
&amp;= \frac 23 \log_{10} \left[\frac1{10 \cdot 10^{-4}\cdot10^{-3}}\right]\,\frac{f_s}{\frac{f_s}{200}}\\
&amp;= \frac 23 \log_{10} \left[\frac1{10 \cdot 10^{-7}}\right]\,200\\
&amp;= \frac 23 \log_{10} \left[\frac1{10^{-6}}\right]\,200\\
&amp;= \frac 23 \left(\log_{10} 10^6\right) \,200\\
&amp;= \frac 23 \cdot 6 \cdot 200\\
&amp;= 800\text{ .}
\end{align}$$


$$ N_{taps} = \frac{Atten}{22*B_T}$$


$B_T$ is the normalized transition band $B_T=\frac{F_{stop}- F_{pass}}{F_s}$

$F_{stop}$ and $F_{pass}$ are the stop band and pass band frequencies in Hz and 

$F_s$ is the sampling frequency in Hz.

Example: 60 dB attenuation, $F_s$=100KHz, $F_{pass}$ = 1KHz, $F_{stop}$=3KHz

$N_{taps} = \frac{60}{22*2/100}=137$ taps (rounding up)
