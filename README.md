# Search-Sort
Deep dive into search and sort algorithm

- **Array (index, swap, slices)** → Bubble, Selection, Insertion, Cocktail, Gnome, Comb, Odd-Even, Pancake, Bogosort.  
- **Récursion & Divide-and-Conquer** → Merge, Quick (toutes variantes), Strand, Stooge, Shell (gap), Merge-Insertion, Patience.  
- **Tas binaire (heap) en tableau** → Heap Sort.  
- **Arbres (BST)** → Tree Sort.  
- **Graphes (DAG)** → Topological Sort.  
- **Comptage/Buckets/Entiers** → Counting, Pigeonhole, Radix.  
- **Réseaux/Parallèle** → Bitonic, Odd-Even (parallel).  
- **Chaînes/Parsing** → Natural Sort.  
- **Hybride pratique** → Timsort (Python `sort()`/`sorted`).

---

## A. Recherche (Search)

| Ordre | # | Exercice | Prérequis | Concepts clés | Complexité | Niveau |
|---:|---:|---|---|---|---|:--:|
| 1 | 2 | Sequential Search | Array | scan linéaire | O(n) | I |
| 2 | 1 | Binary Search | Array trié | divide & conquer, bornes | O(log n) | I |
| 3 | 3 | Ordered Binary Search | Array trié | variantes binsearch | O(log n) | I |

---

## B. Tris élémentaires (base Array, itératifs)

| Ordre | # | Exercice | Prérequis | Concepts clés | Stable | In-place | Complexité | Niveau |
|---:|---:|---|---|---|:--:|:--:|---|:--:|
| 1 | 6 | Insertion Sort | Array | insertion par décalage | ✅ | ✅ | O(n²) | I |
| 2 | 5 | Selection Sort | Array | sélection min/max | ❌ | ✅ | O(n²) | I |
| 3 | 4 | Bubble Sort | Array | échanges adjacents | ✅ | ✅ | O(n²) | I |
| 4 | 14 | Cocktail Shaker | Array | passes bi-directionnelles | ✅ | ✅ | O(n²) | I |
| 5 | 13 | Gnome Sort | Array | swap + retour | ✅ | ✅ | O(n²) | I |
| 6 | 15 | Comb Sort | Array | gaps décroissants | ❌ | ✅ | ~O(n²) | I |
| 7 | 35 | Odd-Even Sort | Array | phases pair/impair | ✅ | ✅ | O(n²) | I |
| 8 | 36 | Non-Parallel Odd-Even | Array | implémentation séquentielle | ✅ | ✅ | O(n²) | I |
| 9 | 37 | Parallel Odd-Even | Array + threads | échanges parallèles | ✅ | — | — | II |
| 10 | 12 | Bogosort | Array | aléatoire, pédagogie | — | — | — | I (fun) |
| 11 | 18 | Pancake Sort | Array | flips préfixes | ❌ | ✅ | O(n²) | I |

---

## C. Tris “Divide & Conquer” (récursion)

| Ordre | # | Exercice | Prérequis | Concepts clés | Stable | In-place | Complexité | Niveau |
|---:|---:|---|---|---|:--:|:--:|---|:--:|
| 1 | 8 | Merge Sort | Récursion | fusion de runs | ✅ | ❌ | O(n log n) | I |
| 2 | 9 | Quick Sort | Récursion | partition pivot | ❌ | ✅ | O(n log n)* | I |
| 3 | 28 | Recursive Quick Sort | Récursion | variante | ❌ | ✅ | O(n log n)* | I |
| 4 | 31 | Random Pivot Quick Sort | Récursion | pivot aléatoire | ❌ | ✅ | O(n log n) moy. | II |
| 5 | 32 | Multi-key Quick Sort | Récursion + strings | 3-way + clé multiple | — | — | — | II |
| 6 | 39 | Merge-Insertion Sort | Récursion | min de comparaisons | — | — | O(n log n) | II |
| 7 | 26 | Strand Sort | Récursion | runs extraits | ✅ | ❌ | O(n²) pire | II |
| 8 | 27 | Stooge Sort | Récursion | pédagogique (très lent) | — | — | O(n^2.7) | II |
| 9 | 7 | Shell Sort | Array + gaps | insertion à gaps | ❌ | ✅ | ~O(n^1.25..n²) | I |

\* pire cas Quick Sort : O(n²), maîtriser choix de pivot.

---

## D. Tris par structures/algorithmes dédiés

| Ordre | # | Exercice | Prérequis | Concepts clés | Stable | In-place | Complexité | Niveau |
|---:|---:|---|---|---|:--:|:--:|---|:--:|
| 1 | 17 | Heap Sort | **Tas binaire en tableau** | build-heap, heapify | ❌ | ✅ | O(n log n) | I½ |
| 2 | 23 | Tree Sort | **BST** | insertion BST + in-order | ✅ | ❌ | O(n log n)* | II |
| 3 | 22 | Topological Sort | **Graphes (DAG)** | ordre partiel | — | — | O(V+E) | II |
| 4 | 11 | Bitonic Sort | Réseaux/para. | comparateurs | — | — | O(n (log n)²) | II |
| 5 | 34 | Patience Sorting | Piles | LIS + merge | — | — | O(n log n) | II |
| 6 | 10 | Counting Sort | **Entiers bornés** | comptage + positions | ✅ | ❌ | O(n + k) | I |
| 7 | 33 | Pigeonhole Sorting | **Entiers ≈ plage** | buckets | ✅ | ❌ | O(n + k) | I |
| 8 | 19 | Radix Sort | Entiers / clés digit | LSB/MSB passes | ✅ | ❌ | O((n + b)·d) | II |
| 9 | 38 | Natural Sort (strings) | Parsing | comparer nombres nat. | ✅ | ❌ | dépend parsing | II |
| 10 | 25 | Timsort | Merge + Insertion | runs, galloping | ✅ | ❌ | O(n log n) | I |

\* Tree Sort : O(n²) si BST dégénéré sans équilibrage.

---

## Conseils de progression

1. **Recherche** : Séquentielle → Binaire.  
2. **Tris élémentaires** : Insertion → Selection → Bubble → variantes (Cocktail, Gnome, Comb, Odd-Even, Pancake).  
3. **Récursion D&C** : Merge → Quick (pivots) → Shell → variantes (Random/Multi-key/Recursive).  
4. **Structures dédiées** : **Heap Sort** (apprendre *tas binaire en tableau*), Counting/Pigeonhole → Radix.  
5. **Spécialisés** : Tree Sort (BST), Topological (graphes), Bitonic/Patience, Natural, **Timsort** (pratique réelle).

> **Heapsort** : apprends **indexation d’un heap en tableau** (`left=2i+1`, `right=2i+2`, `parent=(i-1)//2`), **heapify**, **build-heap**, **extract-max**. Pas besoin d’un cours complet sur les arbres pour démarrer.

---
