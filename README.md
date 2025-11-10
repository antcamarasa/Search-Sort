# 🔍 SEARCH & SORT ALGORITHMS — SUMMARY
## 🔎 SEARCH ALGORITHMS
- [Linear Search](#linear-search)
- [Binary Search](#binary-search)
- [Jump Search](#jump-search)
- [Interpolation Search](#interpolation-search)
- [Exponential Search](#exponential-search)
- [Fibonacci Search](#fibonacci-search)

## ⚙️ SORTING ALGORITHMS 
- [Bubble Sort](#bubble-sort)
- [Selection Sort](#selection-sort)
- [Insertion Sort](#insertion-sort)
- [Shell Sort](#shell-sort)
- [Cycle Sort](#cycle-sort)
- [Comb Sort](#comb-sort)
- [Gnome Sort](#gnome-sort)
- [Merge Sort](#merge-sort)
- [Quick Sort](#quick-sort)
- [Heap Sort](#heap-sort)
- [Counting Sort](#counting-sort)
- [Bucket Sort](#bucket-sort)
- [Radix Sort](#radix-sort)

## Bonus
- [Complexity Table (O-notation)](#complexity-table)
- [When to Use Which Algorithm](#when-to-use-wh)


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
### Counting Sort
Au lieu de comparer les éléments entre eux (comme le font le Merge Sort ou le Quick Sort), le Counting Sort utilise un tableau auxiliaire pour compter la fréquence de chaque valeur de la liste d'entrée.

#### Les quatres étapes clés
1. Compter la fréquence
On crée un tableau de comptage (count array, Count) dont la taille est égale a la valeur maximal du tableau + 1 (pour y inclure les valeurs 0).

        ##########################################################################
        idx    0   1   2   3   4   5   6   7   8   9   10  11  12   13  14  15  16 
        __________________________________________________________________________   
        A   = [2,  1,  1,  0,  2,  5,  4,  0,  2,  8,  7,  7,   9,  2,  0,   1,  9]

Le plus grand élément du tableau est 9. Donc on va créer un tableau de 10 éléments et compter les occurences de chacun des éléments du tableau.

        ###########################################################################
        idx        0   1   2   3   4   5   6   7   8   9
        count = [  3,  3,  4,  0,  1,  1,  0,  2,  1,  2 ]

2. ➕ Calculer le Cumul
On transforme le tableau de comptage Count en un tableau de comptage cumulé.

        ###########################################################################
        idx        0   1   2   3   4   5   6   7   8   9
        count = [  3,  3,  4,  0,  1,  1,  0,  2,  1,  2 ]
        c_cumul=[  3,  6,  10, 10, 11, 12, 12, 14, 15, 17]
        
   
4. ➡️ Placer les Éléments
C'est l'étape la plus délicate, qui assure la stabilité du tri.
- On crée un tableau de sortie (Output Array, B) de la même taille que la liste d'entrée A.
- On parcourt la liste d'entrée à l'envers (de la droite a la gauche).
- Pour chaque index d'élement dans arr correspond à sa position dans c-cumul -1 (explication ci dessous)

        ##########################################################################
        idx    0   1   2   3   4   5   6   7   8   9   10  11  12   13  14  15  16 
        __________________________________________________________________________   
        A   = [2,  1,  1,  0,  2,  5,  4,  0,  2,  8,  7,  7,   9,  2,  0,   1,  9]
        |                                                                        ^
        |                                                                        |
        B   = [ ,  ,    ,   ,   ,   ,   ,   ,   ,   ,   ,   ,   ,   ,    ,    ,   ]

        #################################################
        |  idx        0   1   2   3   4   5   6   7   8   9
        |  c_cumul=[  3,  6,  10, 10, 11, 12, 12, 14, 15, 17]
        |
        |  Current    = 9 # Current est égale a l'index dans count
        |  position_to_insert_current = c_cumul[9] = 17
        |
        |  Maintenant deux étapes :
        |  1. On décremente c_cumul[current] -= 1 #  c_cumul=[  3,  6,  10, 10, 11, 12, 12, 14, 15, !!16!!]
        |  2. On va placer current = 9 à position_to_insert_current
           B   = [ ,  ,    ,   ,   ,   ,   ,   ,   ,   ,   ,   ,   ,   ,    ,    ,  9]

         /////////////////////////  FIN DE LA PREMIERE ITERATION ///////////////////////// 
        |  On décréement :
        |  ##########################################################################
        |  idx    0   1   2   3   4   5   6   7   8   9   10  11  12   13  14  15  16 
        |  __________________________________________________________________________   
        |  A   = [2,  1,  1,  0,  2,  5,  4,  0,  2,  8,  7,  7,   9,  2,  0,   1,  9]
        |                                                                       ^
        |                                                                       |
        |  B   = [ ,  ,    ,   ,   ,   ,   ,   ,   ,   ,   ,   ,   ,   ,    ,    ,  9]
        |
        |  ##################################################
        |  c_cumul=[  3,  6,  10, 10, 11, 12, 12, 14, 15, 16]
            
        |  Current    = 1 # Current est égale a l'index dans count
        |  position_to_insert_current = c_cumul[1] = 6
        |
        |  Maintenant deux étapes :
        |  1. On décremente c_cumul[current] -= 1 #  c_cumul=[  3,  !!5!!,  10, 10, 11, 12, 12, 14, 15, 16]
        |  2. On va placer current = 1 à position_to_insert_current
           B   = [  ,  ,    ,   ,   ,  1,   ,   ,   ,   ,   ,   ,   ,   ,    ,    ,  9]
        /////////////////////////  FIN DE LA DEUXIEME ITERATION ///////////////////////// 
        
En résumé : 
#### 🧩 1️⃣ Les trois tableaux mentaux

A → ton tableau d’origine (les données à trier)
B → le tableau trié (la “destination”)
C → le comptage brut (combien de fois chaque chiffre apparaît)
C_cumul → le cumul de C (combien d’éléments ≤ à chaque chiffre)
          
#### ⚙️ 2️⃣ Ce que signifie vraiment chaque case de C_cumul

C_cumul[x] = “le nombre total d’éléments dont le chiffre est ≤ x”.

Autrement dit, les chiffres ≤ x occupent les C_cumul[x] premières cases du tableau trié B.

Quand tu traites 321 (chiffre = 1) :

        C_cumul[1] = 2 → donc “les chiffres ≤ 1 remplissent les 2 premières cases” →
        → la dernière case libre pour ce chiffre est index 1 (2 - 1).
        -> Puis tu décrémentes C_cumul[1] à 1 pour le suivant.

| Tableau | Signification     | Exemple              |
| ------- | ----------------- | -------------------- |
| A       | données           | `[15, 1, 321]`       |
| C       | comptage          | `[0, 2, 0, 0, 0, 1]` |
| C_cumul | territoire cumulé | `[0, 2, 2, 2, 2, 3]` |
| B       | résultat          | `[1, 321, 15]`       |


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


--- 

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


---

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


---

#### Radix Sort, MSD(most significant digit)

Maintenant, l'idée est de faire l'inverse, cet algorithm est légèrement plus complexes mais bien meilleure en performance sur de grosses liste a trié, donc indispensable. Rien de complexe a part un compréhensions de la pile d'appel récursif. 

Le principe est le même on définit deux variables : 
- L'élément maximum de notre liste
- Son nombre de digit

Cela nous servira a fair l'opération arithmétique suivant : 

                nombre_digit = 3
                current_value = 351

                # Objectif récupérer le most significant value donc :
                351 // 10 puissance (numbre_digit - 1) % 10 => 3

                Pour nombre_digit = 2
                351 // 10 puissance (numbre_digit - 1) % 10 => 5

                Pour nombre_digit = 1
                351 // 10 puissance (numbre_digit - 1) % 10 => 1

Ceci est le calcul le plus complexe de l'algo donc relativement simple. 

Maintenant, il faut comprendre la pile d'appel récursif, on definit un fonction récursive qui va avoir pour objectif :
- Insérer dans une liste de bucket chaque nombres de ma liste en fonction de leur MSD

                  [[], [], [], [329, 355], [457, 436], [], [657], [720], [839], []]
                  clean = [[329, 355], [457, 436], [657], [720], [839]]
                  accumulateur_local = [] # c'est cet acumulateur qui va permettre de propager vers le haut de la pile les nombre triées.      
  
- Appeler récursivement chaque sous tableau, ([329, 355], [457, 436], [657], [720], [839]) ...

                  
                  [[], [], [329], [], [], [355], [], [], [], []]
                  clean  = [[329], [355]]
                  accumulateur_local = []
  
- Appeler récursivement chaque sous tableau

                  [329] => Cas de base on retourne 329
                  Et on le stock dans accumulateur_local = [] de la scope précédent donc la scope

                  clean  = [[329], [355]]
                  accumulateur_local = [329]

                  On appelle récursivment [355] => cas de base on retourne 355 dans la scope précedente
                  clean  = [[329], [355]]
                  accumulateur_local = [329, 355]

                  Et ici on retourne accumulateur local a l'appel précédent et on l'ajoute dans accumulateur local de la scope supérieur
                  
                  clean = [[329, 355], [457, 436], [657], [720], [839]]
                  accumulateur_local = [329, 355]

                  On continue avec [457, 436], [657], [720], [839]

Ainsi on obtient une liste trié !
                        
                  [329, 355, 436, 457, 657, 720, 839]


#### Implémentation du code 


        def radix_sort_msd_second_implementation(arr):
            max_value = max(arr)
            number_digit = len(str(max_value))

            def helper_recursive(current_arr, current_msd):
                if len(current_arr) <= 1:
                    return current_arr
                elif current_msd < 1:
                    return current_arr

                buckets = [[] for _ in range(0, 10)]
                for element in current_arr:
                     msd_value = element // (10 ** (current_msd - 1)) % 10
                     buckets[msd_value].append(element)

                clean_bucket = [bucket for bucket in buckets if bucket]
                accumulateur = []

                current_msd -= 1
                for element in clean_bucket:
                    tmp = helper_recursive(element, current_msd)
            
                    # Permet d'aplatir mon tableau final, sinon j'ai des tableau imbriqué pour simplifier 
                    # Juste => accumulateur.append(tmp)
                    if isinstance(tmp, list):
                        accumulateur.extend(tmp)
                    else :
                        accumulateur.append(tmp)
        
                
                # Indispensable remonte au appel parent le résultat obtenu dans les appels enfant.
                return accumulateur

            return helper_recursive(arr, number_digit)


# Shell Sort
Idée : on trie la liste via plusieurs tris par insertion sur des sous-séquences espacées d’un pas (gap) qu’on réduit (÷2) jusqu’à 1.

Liste de départ :
     
     index =>   0     1    2    3    4    5    6    7   8
          __________________________________________________
     arr   => | 57 | 46 | 43 | 27 | 14 | 41 | 45 | 21 | 70 |
          ——————————————————————————————————————————————————

     #######################################   
     #Passe 1 — gap = len(arr)//2 = 9//2 = 4

             On trie chaque sous-séquence d’indices pris tous les 4 :
             - start = 0 : indices [0, 4, 8] → valeurs [57, 14, 70] 
            |
            |
            |           Il faut visualiser cela :
            |
            |                 ######################### 
            |                 index =>   0    4   8
            |                 _________________________
            |                 arr   => | 57 | 14 | 70 |
            |                               |
            |                         Trié  | Non Trié
            |                 —————————————————————————
            |
            |                 On compare 14 avec 57 => 57 | 57 | 70
            |                 Fin de notre liste donc on s'arrête et on insérer notre élément courant 14.
            |                 
            |                 #########################
            |                 index =>   0    4   8
            |                 _________________________
            |                 arr   => | 14 | 57 | 70 |
            |    
            |            ####################################################
            |            A ce stade notre liste vaut :
            |            index =>   0     1    2    3    4    5    6    7   8
            |            ______________________________________________________
            |            arr   => | 14 | 46 | 43 | 27 | 57 | 41 | 45 | 21 | 70 |
            |            ——————————————————————————————————————————————————————
            |
             - start=1 : indices [1, 5] → valeurs [46, 41] → insertion à pas : 41 remonte avant 46
             - start=2 : indices [2, 6] → valeurs [43, 45] → déjà en ordre
             - start=3 : indices [3, 7] → valeurs [27, 21] → insertion à pas : 21 remonte avant 27
             
    
    #Passe 2 — gap = gap//2 = 4//2 = 2
        
        On trie maintenant les deux familles de sous-séquences modulo 2 :
             - start=0 : indices [0,2,4,6,8] → insertion à pas sur [14, 43, 57, 45, 70]
             - start=1 : indices [1,3,5,7] → insertion à pas sur [41, 21, 46, 27] (21 et 27 “remontent”)
             |
             |           On imagine  : [41, 21, 46, 27]
             |           
             |           #1 Current  = start + gap => 21 (on va comparer jusqu'au début) current(21) et current - gap(41)
             |                        => 21 < 41 
             |                        => [21, 41, 46, 27] => Fin on place current à position => [21, 41, 46, 27]
             |
             |                        Ansi de suite jusqu'a ; 21, 27, 41, 46
        
                                
  Impplémentation classique : 

                def shellSort(alist):
                    sublistcount = len(alist)//2
                    while sublistcount > 0:
                      for start_position in range(sublistcount):
                        gap_InsertionSort(alist, start_position, sublistcount)
                
                      print("After increments of size",sublistcount, "The list is",nlist)
                
                      sublistcount = sublistcount // 2
        
                def gap_InsertionSort(nlist,start,gap):
                    for i in range(start+gap,len(nlist),gap):
                        current_value = nlist[i]
                        position = i
        
                        while position>=gap and nlist[position-gap]>current_value:
                            nlist[position]=nlist[position-gap]
                            position = position-gap
        
                        nlist[position]=current_value
        

                nlist = [14,46,43,27,57,41,45,21,70]
                shellSort(nlist)
                print(nlist)

Impplémentation récursive :
TODO
     
## RADIX SORT

