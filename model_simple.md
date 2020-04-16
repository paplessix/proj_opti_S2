# Projet optimisation 

## Modélisation

Dans ce sujet, on cherche à chauffer un bâtiment résidentiel de sorte à minimiser la facture électrique du consommateur, tout en garantissant le  confot des occupants.

### 1. Ecrire la fonction objectif

L'objectif global est de minimiser la facture des occupants de la maison, cela revient à minimiser la fonction suivante.
$$\boxed{P(t)= \int_{t_0}^{t}{p}^{p/c}(t)w(t).dt}$$
- $t_0$ le temps initial
- $w(t)$ la charge souscrite à $t$
- $p_i^{p/c}(t)$ le prix de l'énergie à $t$ suivant si $t$ correspond à une heure pleine ou une heure creuse.
- $P(t)$ le montant de la facture entre $t_0$ et $t$ pour une consommation suivant la charge $w$ au prix $p$

### 2. Modélisation dynamique de la température du bâtiment
On va considérer la maison comme un cube d'arrête $a$, et de face de surface $S=a^2$. On considère alors que les murs sont assez fins par rapport à la taille de l'édifice général pour que l'on puisse considérer la surface intérieure comme égale à la surface extérieure du cube. On alors $S_{ext} = S_{int} = 5.S_{mur}$ (on considère qu'il n'y pas d'échange maison-sol). Onsuppose que les variations de températures sont quasistatiques.

> Les principales sources de perturbation extérieures de la température sont, **l'apport d'énergie par le soleil** par rayonnement , et **l'appport par le chauffage**, on compte aussi en négatif **la perte d'energie au nieveau des murs**.


Notons :
- $t$ le temps
- $T_{int}(t)$ la température de la maison supposée homogène
- $T_{ext}(t)$ la température extérieure
- $T_s(t)$ la température à la surface extérieure de la maison
- $\Phi_s(t)$ le flux solaire moyen($W/m^2$)
- $Q_{chauff}(t)$ la chaleur produite par le chauffage
- $\Phi_{pertes}(t)$ le flux émis de l'intérieur vers l'extérieur 

On peut associer aux murs de la maison une résistance thermique telle que $\Delta_{int\to ext} = R_{th}\Phi_{pertes}$. Par exemple la résistance est donnée par $R_{th} = \frac{e}{\lambda S_{ext}}$ dans le cadre unidimensionnel. 


![Title](modele.JPG)

#### _**Modèle Physique**_
Si l'on considère une petite transformation pendant $dt$. 
L'écriture du premier principe à la maison $\Sigma = { \ air \ à \ l'intérieur}$  nous donne :
$$dU = C_{m}dT = Q_{chauffage} + Q_{Rayonnement}+ Q_{Pertes}$$
On néglige $Q_{anthropique}$.
$$C_{m}dT = w(t)dt + \alpha \Phi_sS_{ext}dt+ \frac{T_{ext}-T}{R_{th}}dt$$
d'où:

$$\boxed{\frac{dT}{dt} + \frac{T}{R_{th}C_{m}} =\frac{1}{C_{m}}\Bigg[w(t) +\alpha \Phi_sS_{ext} +\frac{T_{ext}}{R_{th}}  \Bigg]}$$

### 3. Discrétisation du problème 

Dans le cadre général en notant $T_i = T(\tau i)$, si $\tau$ est suffisament petit devant les durées de variation de $w$, $\Phi_s$ et $T$  on peut écrire :
$$\boxed{\Delta T= T_{i+1}-T_{i}=\frac{\tau}{C_{m}}[w(i\tau) +\alpha \Phi_s(\tau i )S_{ext} + \frac{T_{ext}(\tau i )-T_i}{R_{th}}]}$$

Dans le cadre des données fournies sur $OASIS$. on a $\forall t, w(t) = 0$.
D'où :
$$\Delta T= T_{i+1}-T_{i}=\frac{\tau}{C_{m}}[\alpha \Phi_s(\tau i )S_{ext} + \frac{T_{ext}(\tau i )-T_i}{R_{th}}]$$

Or si on fait l'hyphothèse d'une maison cubique de volume $V = S^{\frac{3}{2}}$. Or $C_m = C_{massique}\rho_{air}V =  C_{massique}\rho_{air}S^{\frac{3}{2}}$. 
D'où $S = (\frac{C_m}{C_{massique}\rho_{air}})^{\frac{2}{3}}$

Or on a $C_{massique} = 1004  J.K^{-1}kg^{-1}$ et $\rho_{air} = 1 \ kg.m^{-3}$
- On remarque que on connait l'évolution temporelle de la  température de la maison pour $W(t)=0$, ainsi que de la température extéireure et l'ensoleillemnent associé
- Les paramètres de notre problème sont :
    - La longueur d'un côté $a$
    - La résistance surfacique thermiques des murs
    - La résistance thermique de l'ensemble de la maison.
    - La part $\alpha$ du flux solaire arrivant à chauffer l'air de la maison. 
    - La capacité de la maison $C_m$

- Cependant on décompte uniquement 3 variables signicatives que sont : $R,\alpha, R_{surf}$. En effet :
  - $a = \sqrt{\frac{R}{R_{surf}}}$
  - $C_m = C_{massique}\rho_{air}a^3$

- On cherche donc les paramètres qui permettent d'approcher les différentes valeurs par une fonction respectant l'equation différentielle présentée plus haut.
- > **Hypothèse** : On va supposer que la constante de discrétisation $\tau$ est très petite devant la constante de temps du modèle $\delta$. Par ailleurs $w$ et $\Phi_s et T_{ext}$ étant mesurée toutes les 30 minutes, on va les supposer constant sur cette durée, et donc ne contraignent en rien la valeur de $\tau$
- Pour trouver les paramètres significatifs on effectue une regression lineaire en utilisant la méthode des moindres carrés. 

