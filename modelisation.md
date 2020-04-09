# Projet optimisation 

## Modélisation

Dans ce sujet, on cherche à chauffer un bâtiment résidentiel de sorte à minimiser la facture électrique du consommateur, tout en garantissant le  confot des occupants.

### 1. Ecrire la fonction objectif

L'objectif global est de minimiser la facture des occupants de la maison, cela revient à minimiser la fonction suivante.
$$P(x)= \sum_{i=1}^N{p}_i^{p/c}w_i\tau$$
avec :
- $N$ le nombre d'étapes de discrétization de la période d'intérêt
- $w_i$ la charge souscrite entre $i$ et $i+1$
  - On a $x_{i+1}-x_i = \tau = Cte$
- $p_i^{p/c}$ le prix de l'énergie à $i$ suivant si $i$ correspond à une heure pleine ou une heure creuse.
- $P$ le montant de la facture pour une consommation suivant les décisions contenues dans $x = (x_1,...,x_N)$

### 2. Modélisation dynamique de la teméprature du bâtiment
On va considérer la maison comme un cube d'arrête $a$, et de face de surface $S=a^2$. On considère alors que les murs sont assez fins par rapport à la taille de l'édifice général pour que l'on puisse considérer la surface intérieure comme égale à la surface extérieure du cube. On alors $S_{ext} = S_{int} = 5.S$ (on considère qu'il n'y pas d'échange maison-sol)


Notons :
- $t$ le temps
- $T_{int}(t)$ la température de la maison supposée homogène
- $T_{ext}(t)$ la température extérieure
- $T_s(t)$ la température à la surface extérieure de la maison
- $\Phi_s(t)$ le flux solaire moyen($W/m^2$)
- $\Phi_{chauff}(t)$ le flux intérieur  produit par le chauffage
- $\Phi_{pertes}(t)$ le flux émis de l'intérieur vers l'extérieur 
- $\Phi_{conv}(t)$ le flux transmis de la surface extérieure à l'air par convection
- $\alpha_i$ la part du flux solaire qui atteint réellement un mur 
  
Les principales sources de perturbation extérieures de la température sont, l'apport d'énergie par le soleil par rayonnement , et l'appport par le chauffage, on compte aussi la perte d'energie au nieveau des murs.

On peut associer aux murs de la maison une résistance thermique telle que $\Delta T = R_{th}\Phi$. La résistance est donnée par $R_{th_i} = \frac{e}{\lambda S}$ dans le cadre unidimensionnel. Et on définit $R_{th}$ la résistance équivalente aux murs telle que $\frac{1}{R_{th}} = \sum_{i=1}^5{\frac{1}{R_{th_i}}}$.


DETERMINATION DES ALPHA I


Si on se place à la surface extérieure d'un mur (que l'on considère comme une surface grise)  à la température $T_s$ du mur on peut alors écrire le bilan de Flux : 
$$\Phi_{reçu} = \Phi_{émis}$$
$$\Phi_{solaire}+\Phi_{dissip-maison} = \Phi_{conv}+\Phi_{rayonnement}$$
$$S_i\alpha_i\Phi_s + \Phi_{pertes} = S_i\sigma T_s^4 +\Phi_{conv}$$
$$S_i\alpha_i\Phi_s + \frac{T_{int}-T_s}{R_{th}}= S_i\sigma T_s^4 +S_ih(T_s-T_{ext})$$
en effet il n'ya pas d'accumulation d'énergie possible d'où sur le mur  : 
$$ \sigma T_{s_i}^4 + T_{s_i}(h+\frac{1}{R_{th_i}S_i}) = \alpha_i\Phi_s + hT_{ext} +\frac{T_{int}}{R_{th}} $$

Si l'on considère une petite transformation pendant $dt$. 
L'écriture du premier principe à la maison { Murs + air} $ nous donne :
$$dU = C_{m}dT = Q_{chauffage} + Q_{Rayonnement}+ Q_{Pertes} + Q_{anthropique}$$
On néglige $Q_{Rayonnement}$ et $Q_{anthropique}$ en supposant une maison opaque et sans fenêtres. 

$$C_{m}dT = W(t)dt + \sum_{i=1}^5{\frac{T_{s_i}-T_{int}}{R_{th_i}}}dt$$
$$dT =\frac{1}{C_{m}}[W(t)dt + \sum_{i=1}^5{\frac{T_{s_i}-T_{int}}{R_{th_i}}}dt]$$

où $W$ La puissance du radiateur à $t$, $C_{m}$ la capacité thermique de la maison
Or $T=T_{int}$

$$\frac{dT}{dt} + \frac{T}{R_{th}C_{m}} =\frac{1}{C_{m}}\Bigg[W(t) + \sum_{i=1}^5{\frac{T_{s_i}(t)}{R_{th_i}}}\Bigg]$$
$$T_H= Aexp(-\frac{t}{R_{th}C_{m}})$$
et on a donc la solution explicite: 
$$T(t)= {exp(-\frac{t}{R_{th}C_{m}})} \Bigg(K+ \int_{t_0}^t{\frac{1}{C_{m}}\Bigg[W(s) + \sum_{i=1}^5{\frac{T_{s_i}(s)}{R_{th_i}}}\Bigg]exp(\frac{s}{R_{th}C_{m}})ds }\Bigg)$$
Avec les conditions aux limites $K = T_1$

Qui dans notre cas discret  peut s'approximer par : 
$$T_j = {exp(-\frac{j\tau}{R_{th}C_{m}})} \Bigg(T_1+ \sum_{l=1}^{j-1}{\Bigg[\frac{1}{C_{m}}\Big[W(l\tau) + \sum_{i=1}^5{\frac{T_{s_i}(l\tau)}{R_{th_i}}}\Big]exp(\frac{l\tau}{R_{th}C_{m}})\tau \Bigg]}\Bigg)$$


Dans le cas ou les temps de variations des différentes variables est très grand de vant le temps tau on peut apporximer T en utilisant un schéma d'Euler Explicite :

$$\Delta T= T_{i+1}-T_{i}=\frac{\tau}{C_{m}}[W(i\tau) + \sum_{j=1}^5{\frac{T_{s_i}(\tau i)-T_{i}}{R_{th_j}}}]$$

$T{s_i}(\tau j)$ vérifiant : $\sigma T_{s_i}^4 + T_{s_i}(h+\frac{1}{R_{th_i}S_i}) = \alpha_i\Phi_s + hT_{ext} +\frac{T_{int}}{R_{th}}$

On peut alors écrire $T_j = T_1 + \sum_{i=1}^{j-1}{\frac{\tau}{C_{m}}[W(i\tau) + \sum_{j=1}^5{\frac{T_{s_i}(\tau i)-T_{i}}{R_{th_j}}}]}$

### 3. Identification des variables 
- On remarque que on connait l'évolution temporelle de la  température de la maison pour $W(t)=0$, ainsi que de la température extéireure et l'ensoleillemnent associé
- Les paramètres de notre problème sont  : 
    - La résistance thermique d'un mur 
    - La capacité thermique de notre maison
- On cherche donc les paramètres qui permettent d'approcher les différentes valeurs par une fonction de la forme présentée plus haut 
- On effectue pour cela une régression. 

On trouve alors les valeurs  :

### 4. Problème d'optimisation

Le problème d'optimisation est donc :
$$ {min}_{(w_i), i\in {1,..,N}} P(x)= \sum_{i=1}^N{p}_i^{p/c}w_i\tau $$
s.t.
$$ 0 \leq w_i \leq w_{max} -(1) $$
$$  20°C \leq Ti \leq 21°C - (2)