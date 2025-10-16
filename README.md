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

---

### Version avec doublon

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

---
### Radix Sort

Il existe deux implémentation possible de l'algorithme radix sort : 
- LSD
- MSD

LSD : (Least significant digit)
- Trie du chiffre le moins significatif au plus significatif (droite → gauche).
- À chaque étape, le tri est stable, donc l’ordre des chiffres déjà triés est conservé.
- Plus simple à implémenter, souvent itératif.

On trie d’abord par unités, puis dizaines, puis centaines.

MDS : (Most significant digit)
- Trie du chiffre le plus significatif au moins significatif (gauche → droite).
- Après chaque tri, on divise le tableau en sous-listes selon le chiffre traité,
- puis on trie récursivement chaque sous-liste.

Plus complexe mais plus efficace sur de très grands ensembles (notamment chaînes)



#### Radix Sort LSD

1. Definition
Le Radix Sort repose sur un tri non comparatif, basé sur le classement par chiffre.

Imaginons la liste suivante : 

        arr = [15, 1, 321]

- Premierement on trouve l'élement le plus grand de la liste => 321.
- Deuxiemement, on calcul le nombre de chiffres qu'il contient => 3.


A partir de ce moment il existe deux implémentations possibles :
- Convertir chaque nombre en string de même longeur
- Version arithmétique (pas de conversion on travaille directement sur les chiffres grâce aux opérations mathématiques %, //)

#### Radix LSD, version conversion en chaine de carractères

Pour rappel nous avons identifié, le chiffre le plus grand => 321. Nous avon égalemebt identifier qu'il contient trois elements. 3, 2 et 1.

Donc notre troisième étape est de convertir les autres element de notre liste sous le même modèle.

        arr = [015, 001, 321]

Attention ! : En python on ne peux pas ajouter de 0 devant un nombre c'est pourquoi on doit le convertir en string.

Le plus dur est fait ! Il nous reste une dernière étape, le coeur du radix sort.

1. On créer des bucket, 10 au total représentant un nombre allant de 0 à 9.

                bucket [[], [], [], [], [], [], [], [], [], []]

2. Pour chaque élément de la liste on ne regarde que le LSD (least significant number)

                015 -> 5 est le Least significant number
                Et on l'ajoute dans le bucket 5.

                bucket [[], [], [], [], [], [015], [], [], [], []]

On continue pour 001 et 321

                bucket [[001, 321], [], [], [], [], [015], [], [], [], []]

Maintenant on applati bucket pour ne recupérer qu'un tableau de valeur :
                
                arr = [001, 015, 321]


3. On continue en incrémentant le least signifcant number de 1, donc on réitère l'opération sur le deuxieme element de chaque nombres.
   
                 bucket [[], [], [], [], [], [], [], [], [], []]

                 bucket [[001, 015], [], [321], [], [], [], [], [], [], []]

                 arr = [001, 015, 321]

4. On fait pareil avec le dernier element qui est le MSD (most singificant digit)

                 bucket [[], [], [], [], [], [], [], [], [], []]

                 bucket [[001, 015], [], [], [321], [], [], [], [], [], []]

                 arr = [001, 015, 321]
5. Le MSD est atteint notre algorithme et terminé et notre liste est trié.

#### Code implémentation (Radix, LSD, String Converting)

Linear : 

    def radix_sort(arr):

    #1 Find the maximum value :
    max_value = max(arr)

    #2 Count how many digits :
    nbr_digit = len(str(max_value))

    #3. Make all element in array = to nbr_digit
    """
     Méthodes itératives classique :
        
        for index, element in enumerate(arr):
            if len(str(element)) == nbr_digit:
                new_arr.append(str(element))
                continue
            else :
                current_count = len(str(element))

            current_element = str(element)
            while current_count != nbr_digit:
                current_element = "0" + current_element
                current_count += 1

            new_arr.append(current_element)


    Methode pythonic, utilisant les formateur de string :
        x → la variable à afficher.
        : → introduit les options de formatage.
        0 → indique que les espaces vides doivent être remplis par des zéros.
        3 → fixe la largeur minimale à 3 caractères.
        d → signifie decimal integer (entier décimal).
    
    """
    formatted = [f"{x:0{nbr_digit}d}" for x in arr]

    index = len(formatted[0]) - 1
    while index >= 0:
        buckets = [[] for _ in range(0, 10)]
        print(buckets)
        for element in formatted:
            buckets[int(element[index])].append(element)

        formatted = [element for elements in buckets for element in elements]
        print(formatted)
        index -= 1


Recursive : 

    def radix_sort_recursive(arr):
            max_value = max(arr)
            nbr_digit = len(str(max_value))
            formatted = [f"{x:0{nbr_digit}d}" for x in arr]

            def helper_recursive(formatted_arr, index):
                if index < 0:
                    return formatted_arr

                buckets = [[] for _ in range(0, 10)]
                print(buckets)
                for element in formatted_arr:
                    buckets[int(element[index])].append(element)

                formatted_arr = [element for elements in buckets for element in elements]
                index -= 1
                return helper_recursive(formatted_arr, index)

            formatted = helper_recursive(formatted, len(formatted[0]) - 1)
            return [int(x) for x in formatted]

#### Radix LSD, version conversion arithmétique. 
Le code est le même, le princiipe également. 


Seulement au lieu de convertir nos nombres en string contenant autant d'éléments que le plus grand élément on fait un opération simple. 

                Nombre // 1 % 10
                
                # diviser par 1 nous donne le nombre sans modification et % 10 prend le dernier element de ce nombre soit :
                321 /1 = 321 et 321 % 10 => 1

                On continue avec le nombre des dizaines :
                Nombre // 10 % 10

                321 // 10 = 32 et 32 % 10 => 2

                On continue :
                Nombre // 100 % 10
                321 // 100 = 3 et 3 % 10 = 3


#### Code implementation (Radix, LSD, arithmétique)

        def radix_sort_arithmetic(arr):
                max_value = max(arr)
                number_digit = len(str(max_value))

                index = 0
                divisor = 1
                
                while index < number_digit:
                        buckets = [[] for _ in range(0, 10)]

                        for element in arr:
                            bucket_position = (element // divisor) % 10
                            buckets[bucket_position].append(element)

                        arr = [element for elements in buckets for element in elements]

                        divisor *= 10
                        index +=1
                
                return arr

