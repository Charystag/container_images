# Images de conteneurs de développement

Dans ce dépôt, je stocke et partage quelques images personnalisées que je crée pour développer localement dans des conteneurs.

L'objectif est de permettre le développement sans avoir à installer de logiciels supplémentaires — il suffit de lier le conteneur à des répertoires sur votre système de fichiers et d'utiliser un moteur de conteneurs comme  
[`docker`](https://www.docker.com/),  
[`podman`](https://podman.io/)  
ou tout autre.

Même si aucun fichier n'est nommé `Dockerfile`, tous les fichiers de ce dépôt (sauf les scripts) sont de véritables `Dockerfile`s.

## Comment exécuter les images

Vous pouvez construire les images en exécutant la commande suivante (j'utiliserai `docker` pour une meilleure compréhension, mais cela fonctionne exactement de la même manière avec `podman`) :

```bash
docker build -t [nom_de_votre_image] -f chemin_vers_le_dockerfile .
```

Vous avez maintenant une image taguée `nom_de_votre_image` que vous pouvez utiliser pour créer un conteneur de développement.  
Vous pourrez ensuite exécuter ce conteneur avec :

```bash
docker run -it [vos_autres_options] --mount type=bind,src=[votre_répertoire_source],target=[votre_répertoire_cible]
```

Vous pourrez alors modifier vos fichiers avec votre éditeur préféré (le mien est `vim`, désolé, pas désolé).  
Vous pourrez ensuite soit reconstruire votre projet, soit exécuter votre interpréteur favori, soit lancer votre serveur ou toute autre tâche dans le conteneur.

> :warning: Si vous développez un projet destiné à tourner sur un serveur, il faudra probablement rediriger certains ports pour pouvoir faire des requêtes vers ce serveur.

## L’image `base`

L’image de base est celle que j’utilise pour construire toutes les couches suivantes.  
En gros, je sais que dans tous mes conteneurs de développement, j’aurai *au minimum* besoin des outils suivants :
- [curl](https://curl.se/) pour récupérer des URLs *Trop paresseux pour apprendre wget*
- [vim](https://www.vim.org/) pour éditer *Ai-je mentionné que j'utilise vim ?*
- [less](http://www.greenwoodsoftware.com/less/) pour afficher facilement des fichiers *Less, c'est bien plus que [more](https://www.man7.org/linux/man-pages/man1/more.1.html)*
- [tmux](https://github.com/tmux/tmux/wiki) pour le multiplexage *comme Inception, mais avec des fenêtres*
- [man](https://www.man7.org/linux/man-pages/man1/man.1.html) *Le seul* ***véritable*** *homme*
- [git](https://git-scm.com/) *Parce que je fais aussi des commits depuis mes conteneurs*

Donc j'installe ces outils dans mon image de base et je n’ai plus qu’à construire mes autres images en partant de celle-ci.

## Instancier des conteneurs depuis l’image de base

Si vous ne voulez pas construire l’image de base vous-même, vous pouvez exécuter :

```bash
docker pull charystag/development_base
```

Cela téléchargera l’image, que vous pourrez ensuite utiliser pour construire d’autres images ou exécuter des conteneurs.

## Liste des images

- `base` qui contient `curl`, `vim`, `less`, `man`
- `python_base` qui contient uniquement l’interpréteur Python
