TP cryptographie :


PRISE EN MAIN: 


1) La topologie un serveur avec deux clients se nomme étoile. Le serveur étant au centre et les clients autour.

2) Dans les logs du serveur, on peut retrouver tous les messages en clair que les clients s'échangent. Ce n'est donc pas du tout sécurisé. Il n'y a aucune forme de chiffrement des données échangées.

3) C'est un problème car le système est très facilement attaquable. Il viole le principe de Kerckoffs, le système est loin dêtre mathématiquement indéchiffrable étant donné qu'il n'est même pas chiffré.

4) La solution la plus simple serai de mettre en place un chiffrement des données. La méthode la plus que nous ayons vu en cours est le chiffrement par clé publique/privée. On peut résumer ce système de la manière suivante : 
- Chaque client a une paire de clés (une publique et une privé).
- Lorsqu'ils se connectent au serveur, il échangent leur clé privé.
- Pour envoyer un message, ils vont le chiffrer avec la clé public du destinataire.
- Le serveur transmet le message chiffré
- Le destinataire peux déchiffrer le message avec sa clé privé.

Avec cette méthode, si l'on regarde les logs, on aura simplement la vision sur les messages chiffrés.


CHIFFREMENT:


1) urandom est un bon choix pour la cryptographie car elle génère des nombres vraiment aléatoire. Même si il existe des méthodes pouvant générer des nombres avec une quantité de hasard encore plus importante.

2) Si il y a un problème avec ces primitives cryptographiques, les applications qui les utilise sont également brisées, c'est donc risqué.

3) Pour protéger efficacement ses données, les chiffrer est absolument nécessaire mais ce n'est pas suffisant. Un serveur malveillant peut modifier les données en amont et ainsi les rendre inutilisable pour les clients par exemple.

4) Ce qu'il manque ici est un système de vérification de l'intégrité des données.


AUTHENTICATED SYMETRIC ENCRYPTION:


1) Fernet sera forcément moins risqué car il utilise un système d'authenfication (HMAC) en plus du chiffrement des données. On peut savoir si le message a été altéré lorsqu'on le reçoit.

2) Si un serveur attaque avec des messages déja envoyé, on appelle ce type d'attaque des replays. Avec cette méthode, on contourne l'authentification étant donnée que les messages sont déja accepté.

3) Pour s'affrenchir de se problème, on rend tous les messages invalide au bout d'un temps défini.


TTL:


1) Il y a désormais un temps de validité pour les messages ennvoyés.

2) Le message sera considéré trop vieux si l'on retire 45 secondes au TTL.

3) Cette méthode est efficace pour se protéger des replays mais elle reste limité, un serveur est largement assez rapide pour renvoyer un grand nombre de messages pendant la durée défini. 

4) Dans la pratique on peut imaginer que le serveur aurait le temps d'envoyer des replays. On peut imaginer une méthode où lorsqu'un message est lu par un client, il devient instantanément invalide sans définir de période de validité. Ainsi le serveur n'aura pas le temps d'envoyer des replays.


REGARD CRITIQUE:


Concernant les failles qu'il pourrait rester dans la sécurisation de la communication entre les clients, je pense que les points suivant sont à surveiller:
- Le caractère aléatoire des nombres que l'on a généré (clefs,HMAC)
- Si l'authentification est trop longue, les clients risquent de ne pas l'utiliser
- Les clients doivent être très sérieux dans la conservation de leur clé privé. On pourrait imaginer les renouveler pour pallier à ça.
