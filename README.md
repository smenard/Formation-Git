
#TP – Formation GIT

##Configuration 
###Nom et email

Une des premières étapes est la configuration du nom et de l'email de l'utilisateur Git. C'est cet email qui apparaîtra ensuite dans tous les commits et qui servira à identifier l'utilisateur. 

```bash
git config --global user.name "John DOE"
git config --global user.email "john.doe@whatever.com"
```
###Configuration de la fin de ligne

C'est une configuration utile surtout pour les utilisateurs Windows. Cela permet de s'assurer que les fichiers se terminent localement par CRLF (pour qu'ils puissent être correctement ouverts dans Notepad par exemple) et que les fichiers se terminent par LF dans le repository. 

**Utilisateur Linux / OSX**

```bash
git config --global core.autocrlf input
git config --global core.safecrlf true
```

**Utilisateur Windows**

```bash
git config --global core.autocrlf true
git config --global core.safecrlf true
```

### Autres configurations

**Coloration des lignes**
```bash
git config --global color.ui true
```

**Editeur par défaut**
```bash
git config --global core.editor vim
```








## Création d'un projet

L'objectif est d'utiliser le versioning avec Git sur un nouveau projet. 

### Initialisation du projet

Dans un premier temps, nous allons créer les répertoires et les fichiers qui vont servir comme base de projet.

```bash
mkdir projet-recette-cuisine
cd projet-recette-cuisine

echo « Recette du pot au feu » >> recette1.txt
```
Ensuite, nous allons initialiser un repository Git. 

```bash
git init
```

Cette commande va créer un répertoire `.git` à la racine du projet. Ce répertoire contient tout le repository ainsi que toutes les versions des fichiers. C'est ici que Git stocke les méta-données et la base de données des objets du projet. Il s'agit de la partie la plus importante de Git, et c’est ce qui est copié lorsque vous clonez un dépôt depuis un autre ordinateur. Git a seulement besoin de ce fichier et ne génère pas d'autres répertoires ou fichiers dans le projet. 

Exemple d'arborescence d'un répertoire .git : 

```
-rw-r--r--   ... HEAD
drwxr-xr-x   ... branches/
-rw-r--r--   ... config
-rw-r--r--   ... description
drwxr-xr-x   ... hooks/
drwxr-xr-x   ... info/
drwxr-xr-x   ... objects/
drwxr-xr-x   ... refs/  
```

## Ajout des fichiers au repository 

Il est important de comprendre que Git gère les fichiers dans trois états différents avec un espace de travail spécifique pour chacun des états. Les fichiers peuvent se trouver dans :
*	Le répertoire de travail
*	L'index
*	L'historique

**Le répertoire de travail** : comme son nom l'indique, c'est le répertoire de travail où l'on crée, modifie, supprime les fichiers et répertoires. Tous les fichiers à ce niveau, ne font pas forcément partie de Git. 

**L'index** : c'est à cet endroit que se trouvent les fichiers à commiter. 

**L'hystorique** : correspond aux fichiers commités et versionnés par Git. 

Pour faire passer un fichier du répertoire de travail à l'index : 
```bash
git add recette1.txt
```

Pour faire passer un fichier de l'index à l'historique :
```bash
git commit -m "Premier commit"
```


## Vérification du status

Il est possible de vérifier le status du repository et voir les fichiers modifiés. Il est également possible de voir les fichiers qui seront ajoutés au prochain commit.

```bash
git status
```

##Ignorer des fichiers

Comme dans tous les SCM, il est possible d'exclure une liste de fichiers du système de versionning. Pour cela, il faut créer un fichier `.gitignore` à la racine du projet.

Exemple de fichier : 

```bash
*.class
*.jar
*.war
*.ear
```
Cet exemple permet d'ignorer tous les fichiers qui terminent par .class, .jar, etc.

Un utilitaire très pratique permet de générer le fichier .gitignore en fonction des technologies utilisées sur le projet. Cet utilitaire est disponible sur le site [Gitignore](http://www.gitignore.io/)


##Modification des fichiers

###Ajout des ingrédients à la recette 

Dans notre exemple, nous allons ajouter des ingrédients à la recette du pot au feu.

```bash
echo « - 500 g de viande de boeuf grasse» >> recette1.txt
echo « - 500 g de viande de boeuf maigre» >> recette1.txt
echo « - 1 os à moelle» >> recette1.txt
echo « - légumes divers» >> recette1.txt

```

###Vérification du status

```bash
git status
```
A cette étape, le fichier est modifié et les modifications n'ont pas été ajoutées à l'index. 




### Ajout des modifications à l'index

Pour ajouter un fichier à l'index : 

```bash
git add recette1.txt
```
Pour ajouter tous les fichier à l'index (même effet dans notre cas)

```bash
git add .
```

La commande `add` permet d'ajouter les modifications à l'index et de préciser à Git que le fichier à été modifié. Cependant, les modifications ne sont pas enregistrées de façon permanente, pour cela il faut passer le fichier dans l'historique. 

### De l'index à l'historique 

Pour ajouter la modification de façon permanente, il faut faire un `commit` des fichiers précédemment ajoutés à l'index.  

```bash
git commit -m 'Ajout des ingrédients'
```




## La commande commit 

Dans Git, il est impossible de faire un commit sans y ajouter un commentaire. Cela permet de garder un historique clair est simple à parcourir. L'option `-m` de la commande `commit`permet d'ajouter ce commentaire dans la même commande. 

Ajoutons d'autres ingrédient à notre fameuse recette : 

```bash
echo « - gros sel» >> recette1.txt
echo « - poivre noirs en grain» >> recette1.txt

git add .
```
 
Ajoutons maintenant les modifications à l'historique : 

```bash 
git commit
```
Cette commande va ouvrir l'éditeur de texte par défaut et permettre à l'utilisateur d'ajouter un commentaire plus long. Un commentaire Git est constitué d'une première ligne qui contient au maximum 50 caractères. Cette phrase peut être suivie d'un saut de ligne et de tous les détails nécessaires pour préciser l'objet de la modification. 


## Suivi des modifications, mais pas des fichiers

Contrairement au SCM classique, Git s'intéresse aux modifications faites à l'intérieur des fichiers et non pas aux fichiers eux-mêmes. C'est-à-dire que quand nous faisons un `git add recette1.txt`, on ne demande pas l'ajout de ce fichier à l'index mais **l'ajout de l'état de ce fichier à l'index**. Le fichier pourra donc être ajouté à l'historique dans cet état, même s'il à été modifié par la suite. 

### Illustration  

Pour illustrer cette fonctionnalité, nous allons ajouter le premier paragraphe à la recette : 

```bash
echo «Ficelez les morceaux de viande pour qu'ils se maintiennent en forme pendant la cuisson. » >> recette1.txt
```
Ajout de la modification à l'index `git add .`

Nous allons à présent faire une deuxième modification :

```bash
echo «Épluchez les carottes, les poireaux et la branche de céleri, puis lavez-les.» >> recette1.txt
```

Nous pouvons vérifier le status `git status`

Il faut noter que le fichier `recette1.txt` apparaît deux fois. Une première fois dans la rubrique « Change to committed » qui représente les fichiers dans l'index, avec dans notre cas la première modification. Le fichier apparaît également une deuxième fois dans « Changes not staged for commit » qui contient la deuxième modification. Les deux modifications peuvent maintenant faire partie de deux commit différents. 

```bash
git commit -m « Ficelez la viande»
git add . 
git commit -m «Épluchez les carottes»
```



## Affichage de l'historique

```bash
git log
```

Cette commande permet d'afficher l'ensemble des modifications qui ont été enregistrées dans l'historique.  


###Personnalisation des logs 

Git fournit une multitude de fonctions pour personnaliser l'affichage des logs. 

**Pour afficher sur une ligne**

```bash
git log --pretty=oneline
```

**Pour filtrer l'affichage**

```bash
git log --pretty=oneline --max-count=2
git log --pretty=oneline --since='5 minutes ago'
git log --pretty=oneline --until='5 minutes ago'
git log --pretty=oneline –author='AMICO Fabien'
git log --pretty=oneline --all
```

**Best of the best**

```bash
git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
```



##Les Alias

Git fournit la possibilité de créer des alias pour simplifier l’utilisation de certaines commandes. 
La création d'alias se fait soit dans le fichier `$HOME/.gitconfig`, soit dans la configuration du projet dans le fichier `.git/config`.

Pour cela, il faut créer une nouvelle rubrique :

```bash
[alias]
  co = checkout
  ci = commit
  st = status
  hist = log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit 
```


## Remonter dans le temps

Comme avec tous les SCM, il est possible de revenir sur une ancienne version d'un fichier ou même du projet entier. 


### Récupérer l'ancienne version d'un fichier

La commande `checkout` permet de copier des fichiers de l'historique ou de l'index vers l'espace de travail. Cette commande permet également de passer d'une branche à une autre. 

```bash
git checkout HEAD~3 recette1.txt
```

Cette commande permet de récupérer le fichier recette1.txt. dans l'espace de travail et dans l'index (donc disponible au prochain commit). La commande n'a pas pour effet de changer le HEAD du projet. 

### Récupérer une ancienne version du projet complet

```bash
git checkout HEAD~3 
```

Quand aucun nom de fichier n'est donné, et que la référence n'est pas une banche (locale) on se retrouve avec une branche anonyme appelée une detached HEAD. Ceci est utile pour se déplacer rapidement dans l'histoire. Supposons que vous souhaitiez compiler la version 1.6.6.1 de Git, vous pouvez faire un git checkout v1.6.6.1 (qui est un tag, et non une branche), compiler, installer, et rebasculer sur une autre branche avec par exemple git checkout master. 

Attention, dans le cas d'une detached HEAD, les commits n'étant pas attachés à une véritable branche, ils peuvent être perdus au changement de branche. 


```bash
git checkout -b HEAD~3 
```
Cette commande force la création d'une nouvelle branche et permet de ne pas se trouver sur une branche anonyme (detached HEAD). 


### Annuler les modifications d'un fichier

La commande `reset' permet de restaurer l'index et l'espace de travail. Elle est également utilisée pour copier des fichiers depuis l'historique vers l'index, sans toucher à l'espace de travail . 

Si un commit est passé en argument, sans nom de fichier, la branche courante est déplacée vers ce commit, et le stage est mis à jour pour correspondre à ce commit. Si l'option –hard est passée en argument, la working copy est aussi mise à jour. Si l'option –soft est passée en argument, aucun des deux n'est mis à jour. 

```bash
git reset --hard HEAD~3 
```


## Les Tags et versions

L'objectif des Tags est d'associer un nom à un commit important pour pouvoir le retrouver plus facilement. Généralement, les gens utilisent cette fonctionnalité pour marquer les états de publication (v1.0 et ainsi de suite) 

### Création d'un Tag

```bash
git tag -a v1.0 -m 'Version 1.0 déployer en Prod' 
```

L’option `-m` permet de spécifier le message de balisage qui sera stocké avec la balise. Si vous ne spécifiez pas de message en ligne pour une balise annotée, Git lance votre éditeur pour pouvoir le saisir. 

### Création d'un Tag avec un ancien commit

```bash
git tag -a v1.2 9 fceb02 
```

### Lister les Tags

```bash
git tag
```

Cette commande liste les Tags dans l’ordre alphabétique. L’ordre dans lequel elles apparaissent n’a pas de rapport avec l’historique. 

Il est possible de lister les Tags et de filtrer l'affichage :

```bash
git tag -l 'v1.4.2.*'
```

### Récupérer un ancien tag

Pour remonter un ancien Tag, il faut utiliser la commande `checkout` comme vu précédement. 

```bash
git checkout v1.2
```

### Supprimer un Tag

```bash
git tag -d v1.2
```



## Annuler une modification

### Modification dans l'espace de travail 

Cet exemple illustre comment annuler une modification présente dans l'espace de travail, qui n'est donc pas encore présente dans l'index. 

```bash

git checkout master
echo « --- modif --- » >> recette1.txt 

git status

```

A cette étape, Git nous précise que le fichier est modifié mais qu'il n'est pas dans l'index. Pour annuler cette modification, il suffit de faire un `checkout` de la dernière version du fichier : 

```bash
git checkout recette1.txt

git status
```

### Modification dans l'index

Pour annuler la modification d'un fichier déjà ajouté à l'index, il faut utiliser la commande `reset`

Exemple :

```bash
git checkout master
echo « --- modif --- » >> recette1.txt 
git add recette1.txt

git status
```

Pour annuler avec la commande `reset` : 

```bash
git reset HEAD recette1.txt
git status
```

Cette commande modifie l'index mais pas l'espace de travail. Pour modifier l'espace de travail, il faut soit utiliser l'option `--hard` de la commande `reset`, soit faire un `checkout` à la suite. Exemple : 

```bash
git checkout recette1.txt
git status
```


### Modification dans l'historique

Pour annuler une modification déjà commitée, il faut générer un commit qui annule le précédent. 

```bash
git revert HEAD
```

La commande `revert` génère un nouveau commit (avec un nouvel id) qui annule le dernier commit. Une autre façon de faire, aurait été d'utiliser la commande `reset  --hard` avec l'id du commit sur lequel on souhaite revenir. Cette deuxième technique est à utiliser avec précaution et de préférence avec des branches locales. 

### Modification du contenu d'un commit

Avec Git, il est possible de modifier le dernier commit et cela en modifiant le descriptif, mais également le contenu. Pour cela, il faut utiliser la commande `amed` 

Exemple : 

```bash
echo « Ajout de a gousse d'ail » >> recette1.txt
git add .
Git commit -m 'Ajout ail'
```

Ooops, nous avons oublié d'ajouter les oignons dans notre commit :

```bash
echo «Ajout des oignons» >> recette1.txt
git add .
git commit --amend -m 'Ajout ail et oignons'
```

## Déplacement et suppression de fichier

### Déplacement / renommage 
Git ne suit pas directement les mouvements des fichiers. Aucune méta-donnée indiquant le renommage n’est stockée par Git. Néanmoins, Git est assez malin pour s’en apercevoir après coup. Pour déplacer ou renommer un fichier il suffit d'éxecuter la commande : 

```bash
git mv recette1.txt recette1-bis.txt

git status

```

### Suppression de fichier
Pour effacer un fichier, il faut l’éliminer des fichiers suivis par Git, ainsi que du système de fichier. La commande `git rm` réalise ces deux actions.

```bash
git rm recette1.tx
```

Une autre situation est de vouloir supprimer un fichier du suivi de version, tout en le conservant dans l'espace de travail. Cela est particulièrement utile lorsqu'on a oublié d'ajouter un fichier dans le `.gitignore`. Cette modification est possible grâce à l'option `--cached` 

```bash
git rm –-cached recette1.txt
```




## Travailler avec les branches


Comme tous les SCM, Git propose un mécanisme de gestion des branches. La différence est qu'avec Git l'utilisation de ce principe à été extrêmement simplifiée et de nombreuses personnes considèrent qu'il s'agit de **la fonctionnalité** qui fait de Git le meilleur SCM. Git favorise le travail avec les branches en permettant leur création et leur fusion rapidement et simplement, et cela même plusieurs fois par jour.

Git travaille, par défaut, sur une branche appelée `master`. Au fur et à mesure des validations, la branche master pointe vers le dernier des commits effectué. À chaque validation, le pointeur de la branche master avance automatiquement.

### Création d'une branche

```bash
git branch dev
git status
``` 

Cette commande permet de créer une nouvelle branche qui sera une « copie de la branche master ». Pour se positionner sur la nouvelle branche, il suffit d'utiliser la commande `git checkout dev`. Il existe un raccourci qui permet de créer la branche et de se positionner dessus instantanément `git checkout -b dev`.

### Lister les branches

Il est possible de lister les branches avec la commande `git branch -a` qui liste toutes les branches, même les branches distantes. 

Il est également possible de voir les branches ainsi que leur état d'avancement grâce à l'alias crée précédemment `git hist`.


### Supression d'une branche

L'option `-D` permet de supprimer une branche locale : 

```bash
git branch -D dev
```

### Fusionner des branches 

Après avoir fait des modifications dans la branche de `dev`, nous voulons intégrer ces modifications sur la branche principale `master` 

Exemple : 

```bash
git checkout dev
echo « Modif depuis branche dev » >> recette1.txt 
git commit -a -m 'Modification depuis branche dev
```

Pour intégrer les modifications dans la branche dev, il faut se positionner dessus puis appeler la commande `merge`

```bash
git checkout master
git merge dev
Updating f3603e0..6f830f4
Fast-forward
 recette1.txt |    2 ++
 1 file changed, 2 insertions(+)
```

Git utilise une stratégie dite **Fast-forward**, cela signifie que le commit de la branche que l'on souhaite fusionner descend directement du commit avec lequel on souhaite le fusionner. Cette fusion est dite rapide car Git fait simplement avancer le pointeur de la branche master. 


Dans le cas de la fusion d'une branche qui diverge de la branche initiale, la stratégie seradite **recursive**. C'est le cas si la branche `master` a continué à progresser indépendamment de la branche `dev`. Dans ce cas, Git crée un nouveau commit pour la fusion des deux branches. 

Exemple : 

```bash
git checkout master

vim recette1.txt # ajout d'une ligne à la fin du fichier
git commit -a -m 'Modif 1 master' 

vim recette1.txt # ajout d'une ligne à la fin du fichier
git commit -a -m 'Modif 2 master' 

git checkout dev

vim recette1.txt # ajout d'une ligne au tout début du fichier
git commit -a -m 'Modif 1 dev'

git checkout master

git merge dev
Auto-merging recette1.txt
Merge made by the 'recursive' strategy.
 recette1.txt |    4 ++++
 1 file changed, 4 insertions(+)
```

Dans ce cas, on voit apparaître un nouveau commit qui représente la fusion des deux branches :

```
dab755d - (HEAD, master) Merge branch 'dev' (5 minutes ago)
```
 
### Rebase / Merge

Il existe une autre commande qui permet de faire la fusion entre deux branches, il s'agit de la commande `rebase` qui fournit un résultat différent au niveau de l'arborescence. Il est important de noter qu'il ne faut jamais utiliser le `rebase` lorsque les commits ont déjà été poussés sur une branche distante. Cette commande est donc à utiliser avec parcimonie.    


## Gestion des conflits 

Si vous avez modifié différemment la même partie d'un fichier mais sur deux branches différentes, il se peut que la fusion ne se passe pas très bien. A la suite de la fusion, les fichiers en conflit sont modifiés avec les modifications des deux branches, c'est alors au développeur de régler le conflit et de choisir les modifications à appliquer. 

Exemple : 
```bash
git checkout master

vim recette1.txt # ajout d'une modification ligne 5
git commit -a -m 'Modif conflit 1 master' 

git checkout dev

vim recette1.txt # ajout d'une modification ligne 5
git commit -a -m 'Modif conflit 1 dev'

git checkout master

git merge dev

Auto-merging recette1.txt
CONFLICT (content): Merge conflict in recette1.txt
Automatic merge failed; fix conflicts and then commit the result.
```

A ce moment là, le fichier est modifié dans l'espace de travail et il faut régler les conflit à la main. Le contenu du fichier doit ressembler à :

```
<<<<<<< HEAD
Super modif master
=======
Super modif dev
>>>>>>> dev
``` 

La première partie contient toutes les modification présentes dans la branche courante (master dans notre cas) et la deuxième partie contient les modifications de la branche à fusionner. C'est alors au développeur d'arbitrer sur ce qu'il faut intégrer ou non. 


##Travailler avec des repo distants

Git est un DSCM (Distributed Source Code Management) et il est nécessaire de partager un dépôt distant pour pouvoir collaborer avec d'autres développeurs.  
 


### Cloner un dépôt distant 

Pour récupérer un dépôt distant, il suffit de le cloner localement. 

```bash
git clone https://github.com/fabienamico/Formation-Git.git
```

Lorsque l'on clone un dépôt distant, Git crée un dépôt local avec, en plus de la branche master, la branche `remotes/origin/master` qui représente le master distant. 

Pour lister les branches distantes, on utilise la commande `git brach -v` ou bien `git remote -vn`

### Suivre l'évolution d'un dépôt 
```bash
git fetch origin
```  

Cette commande va récupérer toutes les branches disponibles sur le repo distant et les copier en local. Cette commande ne modifie pas l'espace de travail local. Il faut ensuite faire un merge pour avoir le repo distant dans la branche  master locale. 

Pour récupérer l'ensemble du repo distant et le fusionner « automatiquement » avec les branches locales, il faut utiliser la commande 

```bash
git pull origin
```

### Envoyer ces modifs sur le repo distant 

Pour pousser les modifications sur un repo distant, il faut dans un premier temps s'assurer qu'il n'y a pas de modifications plus récentes sur celui-ci. Cela se fait ensuite avec un `git pull`.

