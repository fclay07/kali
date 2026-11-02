# Projet : RSA jouet, et l'attaque qui le casse

## Ce que fait le projet

Le carnet `projet_rsa.ipynb` construit un chiffrement **RSA** entier à partir des
notions du module — exponentiation modulaire rapide, algorithme d'Euclide étendu,
pgcd, test de primalité (Miller-Rabin implémenté à la main) — puis le **casse**
par factorisation et établit **expérimentalement** la taille de clé à partir de
laquelle l'attaque devient hors de portée :

1. boîte à outils arithmétique, chaque brique testée contre une référence
   indépendante (`pow`, `math.gcd`, le crible d'Ératosthène) ;
2. génération de grands nombres premiers (Miller-Rabin validé contre le crible
   sur tous les entiers sous 100 000) ;
3. clés, chiffrement, déchiffrement, aller-retour vérifié sur 2000 messages
   aléatoires et un texte accentué ;
4. attaque : force brute puis **rho de Pollard**, mesures du temps de cassage
   pour des clés de 24 à 72 bits, ajustement de la loi en `2^(b/4)` et
   **extrapolation** : la frontière « un jour de calcul » tombe vers 100–110
   bits pour notre implémentation ;
5. le cas où RSA tombe en une milliseconde quelle que soit la taille : premiers
   trop proches, attaque de **Fermat** ;
6. limites honnêtes : extrapolation fragile, rho de Pollard n'est pas l'état de
   l'art, pas de padding, ne pas utiliser ce code pour chiffrer quoi que ce soit.

## Comment le lancer

Ouvrir `projet_rsa.ipynb` dans Jupyter et tout exécuter (noyau Python 3,
`matplotlib` requis pour le graphique). Le carnet fourni a déjà été exécuté :
les résultats sont visibles sans rien relancer. L'exécution complète prend
quelques minutes (les mesures de temps de cassage dominent).

## Ce qu'on doit en observer

- toutes les clés jusqu'à 72 bits sont effectivement cassées ;
- le graphique en échelle log montre les deux attaques alignées sur leurs lois
  théoriques (`2^(b/2)` pour la force brute, `2^(b/4)` pour le rho, pente mesurée
  ≈ 0,23) ;
- l'extrapolation donne ~100–110 bits pour « un jour de calcul », et l'attaque de
  Fermat factorise en 0,05 ms une clé de 128 bits dont les premiers sont voisins.
