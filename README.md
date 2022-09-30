# Git :
   ## Commandes :
        git commit
	    Comme un snapshot du projet, qui consigne les derniers changements
	    effectués
	    Chaque commit référence le commit précédent, sur lequel il se base.

	git branch nomBranche
 	    Les branches sont des références à un commit.
	    Elles sont peu coûteuses en mémoire.
	    Checkout permet de se positionner sur une nouvelle branche,
	    sans quoi le commit avance sans la branche.

			git checkout nomBranche; git commit

	    nous positionne sur nomBranche,
	    sur laquelle on commit nos modifs.

	git merge brancheQuonMerge
	    permet d'intégrer le contenu d'une branche à une autre (d'origine).		   Crée un commit à deux parents (qui eux-mêmes incluent leurs parents		  ).

			git checkout nomBranche: git merge main

	git rebase nomBranche
	   pour faire la même chose que merge, maintient la linéarité des
	   commits. 
	   Rebase une branche crée une copie en amont de main,
		on se repositionne dessus,
		et fait un rebase sur la branche rebased.

	
    ## Déplacements dans Git:
	HEAD : nom symbolique du commit le plus récent dans l'arbre des commit.
	       On se déplace sur head en utilisant le hash d'un commit.

	git log:
		permet de trouver les commits auxquels on peut remonter via
		le commit actuel.
	
	commits relatifs :
	    revenir en arrière avec ^
	    revenir de plusieurs enarrière avec ~<num>
		
		main^ désigne le premier parent de main

	   On peut aussi utiliser HEAD comme référence relative.

		git checkout HEAD~4

	   retourne 4 commits en arrière.
	
	Permettent de réorganiser les branches.
		
	   git branch -f main HEAD~3
	bouge de force la branche main à trois parents derrière HEAD.

	git reset
	git revert
		annulent tous deux des changements.

		git reset fait revenir la branche en arrière
			git reset HEAD~1

		git revert s'utilise pour des cas où l'annulation doit être
		partagée. Introduit un nouveau commit. Permet de push.
