
#TP – Formation GIT

##Configuration 
###Nom et email

Une des première étapes est de configurer le nom et l'email de l'utilisateur de git. C'est cet email qui apparetra ensuite dans tous les commit qui servira a identifier l'utilisateur. 

```bash
git config --global user.name "John DOE"
git config --global user.email "john.doe@whatever.com"
```
###Configuration de la fin de ligne

C'est une configuration surtout utile pour les utilisateur windows. Cela permet d'être sur que les fichiers se termine localement par CRLF (pour qu'ils puissent être correctement ouvert dans Notepad et autre) et que les fichiers se terminent par LF dans le repository . 

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

**Coloration des ligne**
```bash
git config --global color.ui true
```

**Editeur par défaut**
```bash
git config --global core.editor vim
```








## Création d'un projet

L'objectif est d'utiliser le verioning avec Git sur un nouveau projet. 

### Initialisation du projet

Dans un premier temps nous allons créer les répertoires et fichier qui vont nous servir comme base de projet.

```bash
mkdir projet-recette-cuisine
cd projet-recette-cuisine

echo « Recette du pot au feu » >> recette1.txt
```
Ensuite nous allons initialiser un repository Git. 

```bash
git init
```

Cette commande va créer à la racine du projet un répertoire `.git`qui contient tout le répository avec toute les versions des fichiers. C'est ici que Git stocke les méta-données et la base de données des objets du projet. C’est la partie la plus importante de Git, et c’est ce qui est copié lorsque vous clonez un dépôt depuis un autre ordinateur. Git a seulement besoin de se fichier et ne crée pas d'autre répertoire ou fichier dans le projet. 

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

Il est important de comprendre que git gère les fichiers dans trois états différent avec un espace de travail spécifique pour chacun des états. Les fichiers peuvent se trouver dans :
*	Le répertoire de travail
*	L'Index
*	Le l'historique

**Le répertoire de travail** : Comme son nom l'indique c'est le répertoire d travail ou l'on crée, modifie, supprime les fichiers et répertoire. Tous les fichiers à se niveau ne font pas forcément partie de git. 

**L'index** : C'est l'endroit ou se trouve tous les fichiers qui vont faire partie du prochain commit. 

**L'hystorique** : C'est les fichiers commité et versionné par git. 

Pour passer un fichier du répertoire de travail à l'index : 
```bash
git add recette1.txt
```

Pour passer un fichier de l'index à l'historique :
```bash
git commit -m "Premier commit"
```


## Vérification du status

Il est possible de vérifier le status du repository et de savoir quels sont les fichiers qui sont modifier et, quel sont les fichiers qui vont être ajouté au prochain commit etc.

```bash
git status
```





##Modification des fichiers

###Ajout des ingrédients à la recette 

Pour l'exemple du projet nous allons ajouter des ingrédients à la recette du pot au feu.

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
A cette étape le fichier est modifié et les modifications n'ont pas été ajouté à l'index. 




### Ajout des modifications à l'index

Pour ajouter un fichier à l'index : 

```bash
git add recette1.txt
```
Pour ajouter tous les fichier à l'index (même effet dans notre cas)

```bash
git add .
```

La commande `add`permet d'ajouter les modifications à l'index et de préciser à git que le fichier à été modifier. Par contre les modification ne sont pas enregistré de façons permanente, pour cela il faut passer le fichier dans l'historique. 

### De l'index à l'historique 

Pour ajouter la modification de façon permanente il faut faire un `commit`des fichiers précédemment ajouté à l'index.  

```bash
git commit -m 'Ajout des ingrédients'
```




## La commande commit 

Dans Git il est impossible de faire un commit dans mettre un commentaire. Ce qui permet de garder un historique à peut prêt propre. L'option `-m`de la commande `commit`permet d'ajouter ce commentaire en une seule commande. 

Ajoutons d'autres ingrédient à la fameuse recette : 

```bash
echo « - gros sel» >> recette1.txt
echo « - poivre noirs en grain» >> recette1.txt

git add .
```

Ajoutons maintenant les modifications à l'historique : 

```bash 
git commit
```
Cette commande va ouvrir l'éditeur de texte par défaut et permettre à l'utilisateur d'ajouter un commentaire plus long. Un commentaire Git est constitué d'une première ligne qui contient au maximum 50 caractères. Cette phrase peut être suivit d'un saut de ligne et de tous les détails voulu pour préciser l'objet de la modification. 


## Suivit des modification pas des fichiers

Contrairement au SCM classique Git s'intéresse au modification faite dans les fichiers et non pas au fichiers eux même. C'est à dire que quand nous faisons un `git add recette1.txt` on ne dis pas d'ajouter ce fichier à l'index mais **d'ajouter l'état de ce fichier à l'index**. Il pourra donc être ajouté à l'historique dans cet état même s'il à été modifié par la suite. 

### Illustration  

Pour illustrer cette fonctionnalité nous allons ajouter le premier paragraphe à la recette : 

```bash
echo «Ficelez les morceaux de viande pour qu'ils se maintiennent en forme pendant la cuisson. » >> recette1.txt
```
Ajout de la modification à l'index `git add .`

Maintenant nous allons faire une deuxième modification

```bash
echo «Épluchez les carottes, les poireaux et la branche de céleri, puis lavez-les.» >> recette1.txt
```

Et vérifier le status `git status`

Il faut noter que le fichier `recette1.txt`apparaît deux fois. Une première fois dans la rubrique « Change to committed » qui représente les fichiers dans l'index avec dans notre cas la première modification. Et une deuxième fois dans « Changes not staged for commit » qui contient la deuxième modification. Les deux modification peuvent maintenant faire parti de deux commit différent. 

```bash
git commit -m « Ficelez la viandes»
git add . 
git commit -m «Épluchez les carottes»
```



## Affichage de l'historique

```bash
git log
```

Cette commande permet d'afficher l'ensemble des modifications qui ont été enregistré dans l'historique.  


###Personnalisation des logs 

Git fournit une multitude de fonction pour personnaliser l'affichage des logs. 

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

Git fournit la possibilité de créer des alias pour simplifier l’utilisation de certaine commande. 
La création d'alias se fait soit dans le fichier `$HOME/.gitconfig` soit dans la configuration du projet dans le fichier `.git/config`

Pour cela il faut créer une nouvelle rubrique 

```bash
[alias]
  co = checkout
  ci = commit
  st = status
  hist = log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit 
```


## Remonter dans le temps

Comme avec tous les SCM il est possible de revenir sur une ancienne version d'un fichier ou de tout le projet. 


### Récupérer l'ancienne version d'un fichier

La commande `checkout` permet copier des fichiers de l'historique ou l'indexe vers l'espace de travail, mais également pour passer d'une branche à une autre. 

```bash
git checkout HEAD~3 recette1.txt
```

Cette commande permet de récupérer dans l'espace de travail et dans l'Index (donc disponible au prochain commit ) le fichier recette1.txt. Cette commande n'a pas pour effet de changer le HEAD du projet. 

### Récupérer un ancienne version de tout le projet

```bash
git checkout HEAD~3 
```

Quand aucun nom de fichier n'est donné et que la référence n'est pas une banche (locale) on se retrouve avec une branche anonyme appelée une detached HEAD. Ceci est utile pour se déplacer rapidement dans l'histoire. Supposons que vous souhaitiez compiler la version 1.6.6.1 de git. Vous pouvez faire un git checkout v1.6.6.1(qui est un tag, et non une branche), compiler, installer, et rebasculer sur une autre branche, avec par exemple git checkout master. 

Attention, dans le cas d'une  detached HEAD, les commit n'étant pas ataché à une véritable branche, ils peuvent être perdu au changement de branche. 


```bash
git checkout -b HEAD~3 
```
Cette commande force la création d'une nouvelle branche afin de ne pas se retrouve dans le cas d'une branche anonyme (detached HEAD). 


### Annuler les modifications d'un fichier

La commande `reset' l'index et l'espace de travail. Elle est également utilisée pour copier des fichiers depuis l'historique vers l'index, sans toucher à l'espace de travail . 

Si un commit est passé en argument, sans nom de fichier, la branche courante est déplacée vers ce commit, et le stage est mis à jour pour correspondre à ce commit. Si l'option –hard est passée en argument, la working copy est aussi mise à jour. Si l'option –soft est passée en argument, aucun des deux n'est mis à jour. 

```bash
git reset --hard HEAD~3 
```


## Les Tag et version

L'objectif des Tag est de fournir un nom à un commit important pour pouvoir le retrouver plus facilement. Généralement, les gens utilisent cette fonctionnalité pour marquer les états de publication (v1.0 et ainsi de suite) 

### Création d'un Tag

```bash
git tag -a v1.0 -m 'Version 1.0 déployer en Prod' 
```

L’option `-m` permet de spécifier le message de balisage qui sera stocké avec la balise. Si vous ne spécifiez pas de message en ligne pour une balise annotée, Git lance votre éditeur pour pouvoir le saisir. 

### Création d'un Tag avec un ancien commit

```bash
git tag -a v1.2 9 fceb02 
```

### Lister les Tag

```bash
git tag
```

Cette commande liste les Tags dans l’ordre alphabétique. L’ordre dans lequel elles apparaissent n’a aucun rapport avec l’historique. 

Il est possible de lister les Tags et de filtrer l'affichage

```bash
git tag -l 'v1.4.2.*'
```

### Récupérer un ancien tag

Pour remonter un ancien Tag il faut utiliser la commande `checkout` comme vu précédement. 

```bash
git checkout v1.2
```
