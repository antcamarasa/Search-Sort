# Search-Sort & Sort algorithm
Deep dive into search and sort algorithm

## Sort algorithm

--- 

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

#### Version finale 

       def cycle_sort_correction(arr):
        n = len(arr)
        cycle_start = 0

        while cycle_start < n - 1:
            search_value = arr[cycle_start]

            # boucle d’un cycle
            while True:
                # 1) Rang : compter sur TOUT le tableau, en s’excluant soi-même
                index_to_insert = 0
                for i in range(n):
                    if i != cycle_start and arr[i] < search_value:
                        index_to_insert += 1

                # 2) Si déjà au bon endroit dans le bloc → cycle trivial
                if index_to_insert == cycle_start:
                    # reboucher le trou et passer au cycle suivant
                    arr[cycle_start] = search_value
                    cycle_start += 1
                    break

                # 3) Skip des doublons (avec bornes et stop si on retombe sur le trou)
                toto = index_to_insert
                tata = n

                titi = arr[index_to_insert]
                ttyty = search_value

                while index_to_insert < n and arr[index_to_insert] == search_value:
                    if index_to_insert == cycle_start:
                        # le trou est revenu au départ → cycle terminé
                        arr[cycle_start] = search_value
                        cycle_start += 1
                        break
                    index_to_insert += 1
                else:
                    # 4) Écriture si on a trouvé une case distincte dans les bornes
                    if index_to_insert < n:
                        tmp = arr[index_to_insert]
                        arr[index_to_insert] = search_value
                        search_value = tmp
                        # on continue le cycle avec le nouvel item porté
                        continue
                    # sinon (fin du tableau après skip) → cycle trivial
                    arr[cycle_start] = search_value
                    cycle_start += 1
                    break

                # si on a quitté le while via “break” dans le skip (retour trou)
                # on sort aussi de la boucle du cycle
                break

---

## Pancake Sort

L'idée générale du pancake sort et de trié une liste uniquement en l'inversant. 

Voici sont déroulé :
1. On trouve l'index de l'élément le plus grans sur la liste
2. On inverse la liste du début jusqu'a son index sans touché aux éléments après notre valeur maximal (cela nous permet de mettre au début de notre liste notre plus grand éléments)
3. Il ne nous reste plus qu'a inversé la totalité de la liste, ainsi le plus grand élément se retrouve a la fin.
4. On reduit la taille de la liste de 1 pour ne plus affecter le dernier element de la liste car c'est le plus grand et il est a sa bonne place. 
5. On continue jusqu'a que le scope de la taille de la liste soit < 1. 

#### Une image vaut mille mots

        index =>  0   1    2   3 
                _________________
        arr =>  | 3 | 2 | 4 | 1 |
                ––––––––––––––––-

        1. On trouve le maximum :
        max_value = 4
        index_of_max_value = 2

        2. On fait une rotation de notre sous liste
        reverse(0, index_of_max_value)
                _________________
        arr =>  | 4 | 2 | 3 | 1 |
                ––––––––––––––––-

        3. Maintenant il nous reste plus qu'a inverser la totalité de la liste, afin que notre current_max_element : 4 soit trié
        
                _________________
        arr =>  | 1 | 3 | 2 | 4 |
                ––––––––––––––––-

        n -= 1
        
On reduit notre scope, afin de ne travailler que sur :
 
                ____________   _____
        arr =>  | 1 | 3 | 2    | 4 |
                ––––––––––––   ––––-
   
        Sans enlever 4 bien-sur, c'est une image mentale.

Pourquoi ? 

Car 4, le plus grand élément de la liste est correctement positionné. 

On, continue ainsi de suite jusqu'a que notre n soit supérieur à 1.


        
