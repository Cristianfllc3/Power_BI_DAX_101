# Power_BI_DAX_101
Référence d'utilisation de Markdown[^1].   
 Power BI DAX Functions Tutorial [Full Course] [^2].


**Lors d'un entretien d'embauche, ils ont demandé pourquoi les dates seront répétées si les dates de commande étaient différentes pour différents clients :**
``` 
Max Order Date = 
MAX('Internet Sales'[Order Date])) 
```

![image](https://user-images.githubusercontent.com/72107370/236263392-1fb86571-0142-4e13-aaab-a1972c31b121.png)

## Ma réponse au niveau conceptuel était,
> que s'est-il passé parce que le filtre de ligne n'appliquait pas le filtre de contenu.  

*La réponse du personnel technique était parce que la relation avec l'autre table est manquante.*  

Je ne voulais pas débattre à l'époque, mais j'ai reçu plus tard la réponse que je n'avais pas obtenu le poste en raison d'un manque de connaissances techniques. :(

### Je joins 3 exemples qui démontrent mon propos, et cela reste un souvenir de cette interview :

1 - Une option consiste à créer une mesure, puis à l'utiliser dans la nouvelle colonne.

Mesure:
``` 
Max_Date_Measure = MAX('Internet Sales'[Order Date])
```

Max Date Using Measure:
``` 
Max Date Using Measure = [Max_Date_Measure]
```

![image](https://user-images.githubusercontent.com/72107370/236270276-9fb22e5f-85ea-483b-aa4c-0df147ded90c.png)

2 - La deuxième option encapsule la formule Max dans la formule Calculer, cela active le filtre de ligne dans le filtre de contenu.
``` 
Max Order Date (Calculate) = 
CALCULATE(
    MAX('Internet Sales'[Order Date])
    )
``` 
![image](https://user-images.githubusercontent.com/72107370/236270961-5d6aafc0-7a32-4e8d-b321-6bfb67b9e96c.png)


3 - Enfin, en utilisant les fonctions X, en l'occurrence MAXX, qui effectue une itération en utilisant la table de relation et la valeur de recherche comme paramètres.
*C'est la solution commentée par l'évaluateur technique, mais en soi ce n'était pas la plus simple.*

``` 
Max Order Date (MAXX) = 
 MAXX(
     RELATEDTABLE('Internet Sales'),
      'Internet Sales'[Order Date]
      )
``` 
![image](https://user-images.githubusercontent.com/72107370/236272054-a047cf07-023a-4072-b9b5-a3dbfa30b373.png)




[^1]:https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet.
[^2]:https://www.youtube.com/watch?v=QJw4HkagVWc
