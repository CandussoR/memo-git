<h1 align="center">Memo Git</h1>

## Dépôts locaux
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
- **Delete a branch**:
  - To delete a local branch :
    ```
    git branch -d <nombranche>
    ```
  - To delete a remote branch :
    ```
    git push origin -d <nombranche>
    ```
    
## Dépôts distants
- **git clone** :  
  permet de copier localement un repo distant.
  Crée une branche distante ; détache la tête si on se rend sur celle-ci.
  Les branches distantes sont nommées par convention `<nom depo distant>/<nom branche>`.
  o/branche ne se met à jour que quand le dépôt distant est mis à jour.

- **git fetch** :  
  permet de rapporter des données depuis un dépôt distant.
  Les branches distantes se mettent à jour en concordance avec la nouvelle version du repo distant.
  - télécharge les commits du repo distant absent du repo local ;
  - met à jour les branches distantes.
  - **ne met pas à jour les branches locales**, ne change rien aux fichiers locaux.

- **git pull** :  
  fait à la fois un `fetch` et un `merge`.

- **git push** :
  envoie les changements dans un repo local sur le repo distant.
  En situation de travail par équipe, si notre commit repose sur un commit qui n'est plus le dernier du repo distant, impossible de push.
  Deux solutions :  
  - Résoudre avec fetch et rebase ;
     ```
      git fetch; git rebase <branche distante>; git push
    ```
    D'abord on récupère les données, on rebase nos modifs et les envoie.
    Un raccourci de ces commandes est `git pull --rebase`.
  - Utiliser merge.
    ```
      git fetch; git rebase <branche distante>; git push
    ```

- **git fork**:
  Github fait une copie d'un repo qui est entièrement à soi, et vers lequel on peut push des modifications.

## Dans une grande équipe
  Le push de commit sur main peut être verrouillé.  
  Processus : créer une branche, push, pull request.  
  En cas d'oubli de la branche, créer une autre branche "feature", push, reset la branche main.

## Du HTTPS au SSH
  On peut modifier l'accès au repo du HTTPS au SSh avec :  
  `git remote set-url origin git@github.com:USERNAME/REPOSITORY.git`  
  Git arrête à ce moment de demander le nom d'utilisateur et le mot de passe, et demande une passphrase s'il y en a une.

## Utilisateur Git
- **git config** :
  On peut voir les paramètres de configuration de git à l'aide de `git config --list`.  
  Pour modifier des paramètres d'utilisateur (username et email, ici), on utilise `git config --global user.name "<username>"` et `git config --global user.email "thisis@example.com"`.
  
 
## En cas de détachage de tête
- Si la tête se retrouve détaché d'une branche sur laquelle on travaille pourtant, il faut remettre la branche distante à niveau avec :
  ```
  git branch -f <branche-en-arrière> HEAD
  ```

## Du repo local au repo github
- `git init` pour installer git dans notre projet ;
- `git add . && git commit -m "Initial Commit (ou autre)"` ;
- on crée son repo Github ;
- on récupère sa clé SSH ;
- on revient à la ligne de commande ;
- `git remote add origin <clé SSH>` ;
- `git push origin master`.

## Ajouter tous les fichiers sauf...
- Il existe maintenant une commande pour ajouter tous les fichiers ayant subi des modifications sauf certains (je te vois `public/css/app/css`) :
  ```
  git add --all -- ':!path/to/file1' ':!path/to/file2' ':!path/to/folder1/*'
  ```

## Modifier le message du dernier commit 
- Pour la doc, cf [Github](https://docs.github.com/en/pull-requests/committing-changes-to-your-project/creating-and-editing-commits/changing-a-commit-message) :
   ```
   git commit --amend
   ```
   permet de modifier le message du dernier commit. S'il est déjà poussé en ligne :  
   ```
   git push --force-with-lease origin <nom-de-branche>
   ```  
   Mais cette dernière opération nn'est vraiment pratique que si personne n'a pull le dernier commit, sans quoi chacun semble devoir faire des modifs à la mano. Cf la doc.
   
## Mettre à jour les branches locales par rapport à un repo distant
- ... quand des branches ont été supprimées :  
  ```git
  git remote update origin --prune
  ```
