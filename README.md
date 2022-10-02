<h1 align="center">Memo Git</h1>

- **HEAD** :  
nom symbolique du commit le plus récent dans l'arbre des commits. On peut déplacer HEAD et le détacher en utilisant le nom d'un commit plutôt que celui d'une branche.

- **git commit** :  
Comme un snapshot du projet, qui consigne les derniers changements effectués  
Chaque commit référence le commit précédent, sur lequel il se base.

- **git branch** :  
Les branches sont des références à un commit.
Elles sont peu coûteuses en mémoire.
  ```
    git branch nomBranche
  ```

- **git checkout** :  
Checkout permet de se positionner sur une nouvelle branche,
sans quoi le commit fait avancer la branche sur laquelle on se trouve déjà, sans la branche.

  ```
    git checkout branch
  ```
  
- **git merge** :  
  permet d'intégrer le contenu de la branche sur laquelle on se trouve à une autre branche. Crée un commit à deux parents (qui eux-mêmes incluent leurs parents).

  ```git
    git checkout nomBranche; git merge main
  ```

- **git rebase** :  
  ```
    git rebase nomBranche
  ```
  permet de faire la même chose que merge, en maintenant la linéarité de l'arbre des commits.  
  Rebase une branche crée une copie en amont de main:
	- on se repositionne dessus,
	- et fait un rebase sur la branche rebased.

- **git log** :  
permet de trouver les commits auxquels on peut remonter via le commit actuel.
	
- **commits relatifs** :  
Les commits relatifs simplifient la navigation :
  
  - `^` permet de revenir en arrière d'un commit :  
  `main^` désigne le premier parent de main,  
  `main^^` désigne le parent du parent, etc.

  - `~<num>` permet de revenir en arrière de plusieurs commits,  
    ce qui permet d'éviter une prolifération de `^`.

    - On peut aussi utiliser `HEAD` comme référence relative à la place d'un nom de branche :  
    `git checkout HEAD~4` positionne 4 commits en arrière de `HEAD`.
	
  Ils permettent aussi de réorganiser facilement les branches :
		
  `git branch -f main HEAD~3` bouge de force la branche main trois parents derrière `HEAD`.

- **git reset / git revert** :  
  annulent tous deux des changements.
  - `git reset` fait revenir la branche en arrière :  
    ```git
      git reset HEAD~1
    ```

  - `git revert` s'utilise dans les cas où l'annulation doit être partagée. Il introduit un nouveau commit et permet de push.

- **git tag** :  
  - Il existe deux types de tags :
    - les lightweight (pour les tags privés) ;
    - les annotés (pour les tags publics) ;
      - les tags annotés contiennent plus de métadonnées (comme le nom, l'email, la date).
    
  - Par défaut, le tag est créé sur le commit auquel fait référence `HEAD`.
    ```
      git tag -a v1.0 -m "my version 1.0"
    ```

  - Pour envoyer les tags sur un repo distant, `git push`tel quel ne fonctionne pas.
    - S'il n'y a qu'un tag, on peut le spécifier :
    ```
      git push origin v1.0
    ```
    - S'il y en a plusieurs, on peut tous les envoyer d'un coup :
    ```
      git push origin --tags
    ```
