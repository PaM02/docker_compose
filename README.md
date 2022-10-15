L'argument version permet de spécifier à Docker Compose quelle version on souhaite utiliser, et donc d'utiliser ou pas certaines versions. Dans notre cas, nous utiliserons la version 3, qui est actuellement la version la plus utilisée

L'ensemble des conteneurs qui doivent être créés doivent être définis sous l'argument services  . Chaque conteneur commence avec un nom qui lui est propre ; dans notre cas, notre premier conteneur se nommera db

Puis, vous devez décrire votre conteneur ; dans notre cas, nous utilisons l’argument image qui nous permet de définir l'image Docker que nous souhaitons utiliser

Nous aurions pu aussi utiliser l’argument build en lui spécifiant le chemin vers notre fichier Dockerfile ; ainsi, lors de l’exécution de Docker Compose, il aurait construit le conteneur via le Dockerfile avant de l’exécuter.

Pour rappel, nous avons vu précédemment que les conteneurs Docker ne sont pas faits pour faire fonctionner des services stateful, et une base de données est par définition un service stateful. Cependant, vous pouvez utiliser l'argument volumes qui vous permet de stocker l'ensemble du contenu du dossier /var/lib/mysql dans un disque persistant. Et donc, de pouvoir garder les données en local sur notre host

Cette description est présente grâce à la ligne db_data:/var/lib/mysql  . db_data est un volume créé par Docker directement, qui permet d'écrire les données sur le disque hôte sans spécifier l'emplacement exact. Vous auriez pu aussi faire un /data/mysql:/var/lib/mysql qui serait aussi fonctionnel.

Un conteneur étant par définition monoprocessus, s'il rencontre une erreur fatale, il peut être amené à s'arrêter. Dans notre cas, si le serveur MySQL s'arrête, celui-ci redémarrera automatiquement grâce à l'argument restart: always

L'image MySQL fournie dispose de plusieurs variables d'environnement que vous pouvez utiliser ; dans votre cas, nous allons donner au conteneur les valeurs des différents mots de passe et utilisateurs qui doivent exister sur cette base. Quand vous souhaitez donner des variables d'environnement à un conteneur, vous devez utiliser l'argument environment  , comme nous l'avons utilisé dans le fichier docker-compose.yml ci-dessus

Le premier argument, depends_on  , nous permet de créer une dépendance entre deux conteneurs. Ainsi, Docker démarrera le service db avant de démarrer le service WordPress. Ce qui est un comportement souhaitable, car WordPress dépend de la base de données pour fonctionner correctement.

Le second argument, ports  , permet de dire à Docker Compose qu'on veut exposer un port de notre machine hôte vers notre conteneur, et ainsi le rendre accessible depuis l'extérieur.

Quand vous lancerez vos conteneurs avec la commande docker-compose up -d  , vous devriez avoir le résultat suivant 

Vous savez maintenant utiliser les commandes de base de Docker Compose, et créer un fichier docker-compose.yml pour orchestrer vos conteneurs Docker.

Pour rappel, voici les arguments que nous avons pu voir dans ce chapitre :

image qui permet de spécifier l'image source pour le conteneur ;

build qui permet de spécifier le Dockerfile source pour créer l'image du conteneur ;

volume qui permet de spécifier les points de montage entre le système hôte et les conteneurs ;

restart qui permet de définir le comportement du conteneur en cas d'arrêt du processus ;

environment qui permet de définir les variables d’environnement ;

depends_on qui permet de dire que le conteneur dépend d'un autre conteneur ;

ports qui permet de définir les ports disponibles entre la machine host et le conteneur.