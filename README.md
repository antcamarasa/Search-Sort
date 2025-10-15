# Search-Sort & Sort algorithm
Deep dive into search and sort algorithm

## Sort algorithm
### Cycle Sort.
L'idée générale du cycle sort et de de triéer une liste sous forme de cycle.

Récuper un élément le placer a sa position final et tant que le trou ... n'est pas bouché on continue. Si il est bouché on passe alors a un deuxième cycle.

Ansi de suite, jusqu'a ce que la longeur de notre cycle soit égal à la longeur de notre liste, a ce moment notre liste est trié.

Voici le concept : 
        
        list = [50, 30, 20, 10, 40]
        
        on récupére le premier element 

                  50
                  ^
                  | 
        list = [ ..., 30, 20, 10, 40 ]

Attention :  a aucun moment on ne retire 50 de la lisite c'est une image mentale. 
Une fois l'élément récuperer, on va chercher sa place en le comparant avec tous les autres éléments de la liste (attention a ne pas le comparer avec lui même). 

#### Version sans doublon

       |          CYCLE 1 
       |
       | #============== while : iteration : 1 =================
       | 30 < 50 ? oui, index_to_insert += 1
       | 20 < 50 ? oui, index_to_insert += 1
       | 10 < 50 ? oui, index_to_insert += 1
       | 40 < 50 ? oui, index_to_insert += 1
       | 
       | index_to_insert = 4               
       |                       40
       |                       ^
       |                       |    
       | list[..., 30, 20, 10, 50]
       |
        #============== while : iteration : 2 =================
       | 
       | 30 < 40 ? oui, index_to_insert += 1
       | 20 < 40 ? oui, index_to_insert += 1
       | 10 < 40 ? oui, index_to_insert += 1
       | 50 < 40 ? non
       |
       | index_to_insert = 3
       |
       |                     10
       |                     ^
       |                     |    
       |  list[..., 30, 20, 40, 50]
       |
        #============== while : iteration : 3 =================
       |
       | 30 < 10 ? non
       | 20 < 10 ? non
       | 40 < 10 ? non
       | 50 < 10 ? non
       |
       | index_to_insert = 0 
       | list[10, 30, 20, 40, 50]
       |
       | Le tri est rebouché on a terminé notre premier cycle.
       | ========================================================
       |
       |             CYCLE 2
       |
       | On passe au deuxieme cycle comme cela : 
       |
       |             30
       |             ^
       |             |
       |  list[10, ..., 20, 40, 50]

Ainsi de suite jusqu'a a voir terminé notre cycle. 


#### Version avec doublon

La version avec doublon est similaire. L'idée est si on veux insérer un élement a une position mais qu'a cette position un doublon est présent alors on avance de 1, jusqu'a qui n'y ai plus de doublons ET qu'on ne soit pas au niveau du Trou ! car si on passe au niveau du Trou alors on insére notre élément et on termine le cycle. 


Une fois cela visualiser l'implémentation est beaucoup plus simple.
