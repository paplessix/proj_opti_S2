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
- $\Phi_s(t)$ le flux solaire arrivant sur la maison ($W/m^2$)
- $\Phi_{int}(t)$ le flux intérieur  produit par le chauffage

Les principales sources de perturbation extérieures de la température sont, l'apport d'énergie par le soleil par rayonnement , et l'appport par le chauffage, on compte aussi la perte d'energie au nieveau des murs.

On peut associer aux murs de la maison une résistance thermique telle que $\Delta T = R_{th}\Phi$. La résistance est donnée par $R_{th} = \frac{eL}{S}$ dans le cadre unidimensionnel.

Si l'on considère une petite transformation pendant $dt$. 
L'écriture du premier principe à la maison nous donne :
$$dU = C_{m}dT = Q_{chauffage} + Q_{Rayonnement}+ Q_{Pertes}$$
$$C_{m}dT = W(t)dt + S_{ext}\Phi_s dt+ \frac{T_{ext}-T_{int}}{R_{th}}dt$$
$$dT =\frac{1}{C_{m}}[W(t)dt + S_{ext}\Phi_s dt+ \frac{T_{ext}-T_{int}}{R_{th}}dt]$$

où $W$ La puissance du radiateur à $t$, $C_{m}$ la capacité thermique de la maison
$$\frac{dT}{dt} + \frac{T}{R_{th}C_{m}} =\frac{1}{C_{m}}[W(t) + S_{ext}\Phi_s+ \frac{T_{ext}}{R_{th}}]$$
d'où :
$$\Delta T= T_{i+1}-T_{i}=\frac{\tau}{C_{m}}[W(t) + S_{ext}\Phi_s+ \frac{T_{ext}-T_{i}}{R_{th}}]$$

En supposant que W est constant dans l'intervalle $[i,i+1]$, la durée caractéristique de variation de $\Phi_s$

On considère dans un premier temps la capacité thermique des murs nulles. On supppose que le rayonnement est entièrement absorbé par la maison.

