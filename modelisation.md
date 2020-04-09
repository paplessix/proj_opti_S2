# Projet optimisation 

## Modélisation

Dans ce sujet, on cherche à chauffer un bâtiment résidentiel de sorte à minimiser la facture électrique du consommateur, tout en garantissant le  confot des occupants.

### 1. Ecrire le fonction objectif.

L'objectif global est de minimiser la facture des occupants de la maison, cela revient à minimiser la fonction suivante.
$$
P(x)= \sum_{i=1}^N{p}_i^{p/c}x_i
$$
avec :
- $N$ le nombre d'étapes de discrétization de la période d'intérêt
- $x_i$ l'énergie consommée entre $i$ et $i+1$
  - On a $x_{i+1}-x_i = \tau = Cte$
- $p_i^{p/c}$ le prix de l'énergie à $i$ suivant si $i$ correspond à une heure pleine ou une heure creuse.
- $P$ le montant de la facture pour une consommation suivant les décisions contenues dans $x = (x_1,...,x_N)$
