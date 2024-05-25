Bien sûr! Voici une correction détaillée pour l'exercice 3 en utilisant la méthode des éléments finis (FEM).

### Formulation Variationnelle (FV)

#### 1. Formulation variationnelle du problème (1)

Considérons le problème suivant :
\[ \begin{cases}
-\text{div}(k \nabla u) + \alpha u = f & \text{dans } \Omega, \\
u = 0 & \text{sur } \partial \Omega,
\end{cases} \]
avec \( u \in H^1(\Omega) \), \( \alpha \in \mathbb{R}^+ \), \( f \in L^2(\Omega) \), et \( k \in L^\infty(\Omega) \) tel que \( k(x) \geq k_{\min} > 0 \) presque partout dans \( \Omega \).

Pour obtenir la formulation variationnelle, nous multiplions l'équation par une fonction test \( v \in H^1_0(\Omega) \) et intégrons sur \(\Omega\):
\[ \int_\Omega (-\text{div}(k \nabla u) + \alpha u) v \, dx = \int_\Omega f v \, dx. \]

En utilisant la formule de Green (ou l'intégration par parties), et sachant que \( u = 0 \) sur \(\partial \Omega\), nous obtenons :
\[ \int_\Omega k \nabla u \cdot \nabla v \, dx + \int_\Omega \alpha u v \, dx = \int_\Omega f v \, dx. \]

Ainsi, la formulation variationnelle est :
\[ \text{Trouver } u \in H^1_0(\Omega) \text{ tel que } \forall v \in H^1_0(\Omega), \quad \int_\Omega k \nabla u \cdot \nabla v \, dx + \int_\Omega \alpha u v \, dx = \int_\Omega f v \, dx. \]

#### 2. Existence et unicité de la solution

Pour étudier l'existence et l'unicité de la solution du problème (1), nous pouvons utiliser le théorème de Lax-Milgram.

Définissons les formes bilinéaires et linéaires suivantes :
\[ a(u, v) = \int_\Omega k \nabla u \cdot \nabla v \, dx + \int_\Omega \alpha u v \, dx, \]
\[ l(v) = \int_\Omega f v \, dx. \]

La forme bilinéaire \( a(u, v) \) est continue et coercitive. Continuons:
1. **Continuité :** Il existe une constante \( C \) telle que \( \forall u, v \in H^1_0(\Omega) \),
\[ |a(u, v)| \leq C \|u\|_{H^1(\Omega)} \|v\|_{H^1(\Omega)}. \]

2. **Coercivité :** Il existe une constante \( \alpha_0 > 0 \) telle que \( \forall u \in H^1_0(\Omega) \),
\[ a(u, u) \geq \alpha_0 \|u\|_{H^1(\Omega)}^2. \]

Puisque \( k(x) \geq k_{\min} > 0 \) et \( \alpha > 0 \), la coercivité est assurée.

Le théorème de Lax-Milgram garantit alors l'existence et l'unicité de la solution \( u \in H^1_0(\Omega) \) de notre problème variationnel.

### Cas unidimensionnel (k = 1 et α = 1)

Considérons maintenant le problème unidimensionnel sur l'intervalle \([0,1]\) avec \( k = 1 \) et \( \alpha = 1 \) :
\[ \begin{cases}
-u'' + u = f & \text{dans } [0,1], \\
u(0) = u(1) = 0.
\end{cases} \]

### Utilisation de l'élément fini \( P_1 \) avec un maillage uniforme de pas \( h = \frac{1}{3} \)

#### 1. Espace discret \( V_h \)

Nous utilisons l'espace des fonctions de Lagrange linéaires \( P_1 \) sur le maillage uniforme. Les nœuds du maillage sont \( x_0 = 0 \), \( x_1 = \frac{1}{3} \), \( x_2 = \frac{2}{3} \), et \( x_3 = 1 \).

L'espace d'approximation \( V_h \) est l'ensemble des fonctions continues par morceaux qui sont linéaires sur chaque intervalle \( [x_i, x_{i+1}] \).

#### 2. Calcul des matrices élémentaires

Pour chaque élément \( e = [x_i, x_{i+1}] \), nous devons calculer les matrices de rigidité et de masse élémentaires.

**Matrice de rigidité élémentaire \( K_e \) :**
\[ K_e = \begin{pmatrix}
\int_e \phi_i' \phi_i' \, dx & \int_e \phi_i' \phi_{i+1}' \, dx \\
\int_e \phi_{i+1}' \phi_i' \, dx & \int_e \phi_{i+1}' \phi_{i+1}' \, dx
\end{pmatrix}. \]

Pour un élément \( e = [x_i, x_{i+1}] \) de longueur \( h = \frac{1}{3} \) avec les fonctions de base \( \phi_i \) et \( \phi_{i+1} \), nous avons :
\[ \phi_i(x) = \frac{x_{i+1} - x}{h}, \quad \phi_{i+1}(x) = \frac{x - x_i}{h}. \]

Les dérivées sont :
\[ \phi_i'(x) = -\frac{1}{h}, \quad \phi_{i+1}'(x) = \frac{1}{h}. \]

Ainsi, les termes de la matrice de rigidité sont :
\[ K_e = \begin{pmatrix}
\frac{1}{h} & -\frac{1}{h} \\
-\frac{1}{h} & \frac{1}{h}
\end{pmatrix} \cdot \frac{1}{h} = \begin{pmatrix}
\frac{1}{h} & -\frac{1}{h} \\
-\frac{1}{h} & \frac{1}{h}
\end{pmatrix} \cdot 3 = \begin{pmatrix}
3 & -3 \\
-3 & 3
\end{pmatrix}. \]

**Matrice de masse élémentaire \( M_e \) :**
\[ M_e = \begin{pmatrix}
\int_e \phi_i \phi_i \, dx & \int_e \phi_i \phi_{i+1} \, dx \\
\int_e \phi_{i+1} \phi_i \, dx & \int_e \phi_{i+1} \phi_{i+1} \, dx
\end{pmatrix}. \]

Pour un élément \( e = [x_i, x_{i+1}] \) de longueur \( h = \frac{1}{3} \), nous avons :
\[ \int_e \phi_i \phi_i \, dx = \int_{x_i}^{x_{i+1}} \left( \frac{x_{i+1} - x}{h} \right)^2 \, dx = \frac{h}{3}, \]
\[ \int_e \phi_{i+1} \phi_{i+1} \, dx = \int_{x_i}^{x_{i+1}} \left( \frac{x - x_i}{h} \right)^2 \, dx = \frac{h}{3}, \]
\[ \int_e \phi_i \phi_{i+1} \, dx = \int_{x_i}^{x_{i+1}} \left( \frac{x_{i+1} - x}{h} \right) \left( \frac{x - x_i}{h} \right) \, dx = \frac{h}{6}. \]

Ainsi, la matrice de masse élémentaire est :
\[ M_e = \frac{h}{6} \begin{pmatrix}
2 & 1 \\
1 & 2
\end{pmatrix} = \frac{1}{18} \begin{pmatrix}
2 & 1 \\
1 & 2
\end{pmatrix}. \]

#### 3. Assemblage global

En assemblant les matrices élémentaires pour les trois éléments \([0, \frac{1}{3}], [\frac{1}{3}, \frac{2}{3}], [\frac{2}{3}, 1]\), nous obtenons les matrices globales.

**Matrice de rigidité globale \( K \) :**
\[ K = \begin{pmatrix}
K_{11} & K_{12} & 0 & 0 \\
K_{21} & K_{22} + K_{11} & K_{

12} & 0 \\
0 & K_{21} & K_{22} + K_{11} & K_{12} \\
0 & 0 & K_{21} & K_{22}
\end{pmatrix}. \]

En assemblant les contributions, nous avons :
\[ K = \begin{pmatrix}
3 & -3 & 0 & 0 \\
-3 & 6 & -3 & 0 \\
0 & -3 & 6 & -3 \\
0 & 0 & -3 & 3
\end{pmatrix}. \]

**Matrice de masse globale \( M \) :**
\[ M = \begin{pmatrix}
M_{11} & M_{12} & 0 & 0 \\
M_{21} & M_{22} + M_{11} & M_{12} & 0 \\
0 & M_{21} & M_{22} + M_{11} & M_{12} \\
0 & 0 & M_{21} & M_{22}
\end{pmatrix}. \]

En assemblant les contributions, nous avons :
\[ M = \frac{1}{18} \begin{pmatrix}
2 & 1 & 0 & 0 \\
1 & 4 & 1 & 0 \\
0 & 1 & 4 & 1 \\
0 & 0 & 1 & 2
\end{pmatrix}. \]

### Résumé

1. **Formulation variationnelle :**
\[ \text{Trouver } u \in H^1_0(\Omega) \text{ tel que } \forall v \in H^1_0(\Omega), \quad \int_\Omega k \nabla u \cdot \nabla v \, dx + \int_\Omega \alpha u v \, dx = \int_\Omega f v \, dx. \]

2. **Existence et unicité de la solution :** Assurée par le théorème de Lax-Milgram.

3. **Résolution unidimensionnelle avec \( P_1 \) et maillage uniforme :**
   - Matrices de rigidité et de masse élémentaires calculées et assemblées pour obtenir les matrices globales \( K \) et \( M \).

Cela conclut la correction pour l'exercice 3.
