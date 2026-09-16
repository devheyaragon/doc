# Connecter votre assistant IA à Aragon Control MCP

Ce guide vous accompagne pour connecter un assistant IA — Claude, ChatGPT
ou une autre application compatible — à votre serveur Aragon Control MCP,
afin de contrôler votre maison en parlant ou en écrivant à votre assistant
(« Allume la lumière du salon », « Ferme les volets du bureau »).

## Ce dont vous avez besoin avant de commencer

- **Aragon Control MCP installé et actif** sur votre appareil Aragon
  Maestro. Si ce n'est pas encore fait, consultez les instructions
  d'installation fournies avec votre logiciel.
- **L'adresse (URL) de votre serveur et votre mot de passe propriétaire.**
  Vous trouverez les deux dans la section Aragon Control MCP de votre
  application Aragon Maestro — elle affiche l'adresse de votre serveur et
  vous permet d'y définir le mot de passe que vous utiliserez pour vous
  connecter lorsqu'un assistant demande à se connecter. Ce n'est pas le
  mot de passe de votre compte Claude/ChatGPT/etc.

## La seule règle qui compte partout

Chaque fois qu'un client vous demande une **URL de serveur**, ajoutez
toujours `/mcp` à la fin de votre adresse :

```
https://nom-de-votre-appareil.votre-tailnet.ts.net/aragon-control/mcp
```

Oublier `/mcp` est l'erreur de configuration la plus fréquente — l'assistant
affichera alors une erreur de connexion ou de type « introuvable » qui
donne l'impression que quelque chose est cassé, alors qu'il manque
simplement le dernier élément de l'adresse. Vérifiez ce point en premier
si une connexion échoue.

---

## Claude (claude.ai et Claude Desktop)

1. Ouvrez **Paramètres → Connecteurs → Ajouter un connecteur
   personnalisé.**
2. **URL du serveur :** saisissez votre adresse avec `/mcp` à la fin,
   comme ci-dessus.
3. **Authentification :** laissez l'option détectée automatiquement
   (« Toujours requis » / « Se connecter maintenant ») — aucun changement
   nécessaire ici.
4. **Client OAuth :** choisissez **« Aucun ID client — en enregistrer un
   automatiquement »** (peut aussi être intitulé « Enregistrement
   automatique »). Ne choisissez **pas** « Utiliser les métadonnées client
   hébergées par Anthropic », même si cette option est marquée
   « Recommandé » — elle ne fonctionne pas avec ce serveur.
5. Laissez le champ **En-têtes de requête supplémentaires** vide.
6. Validez. Vous serez redirigé vers une page de connexion **Aragon
   Maestro** — saisissez-y votre **mot de passe propriétaire** (ce n'est
   pas une connexion à un compte Claude). Une fois connecté, vous serez
   renvoyé vers Claude, la connexion établie.

Une fois connecté, vous pouvez parler à votre assistant en langage naturel
— en anglais ou, si votre assistant le prend en charge, dans une autre
langue que vous parlez, car l'assistant traduit votre demande avant
qu'elle n'atteigne votre système Maestro. Demandez-lui de lister vos
pièces, de vérifier l'état d'une lumière, ou d'allumer ou d'éteindre
quelque chose.

**Si une connexion qui fonctionnait auparavant affiche « Problème de
connexion » ou « Connexion impossible » :** ne vous contentez pas de
cliquer sur Reconnecter — essayez plutôt de supprimer le connecteur et de
le rajouter entièrement si quelques tentatives de reconnexion ne
résolvent pas le problème.

**Remarque :** la page de connexion vers laquelle vous êtes redirigé
porte la marque « Aragon Maestro », alors que le connecteur lui-même est
nommé « Aragon Control MCP » dans votre liste de connecteurs. C'est normal
— la page de connexion concerne le système domotique auquel vous accordez
l'accès, pas le nom du connecteur.

**Si vous aviez précédemment configuré ce connecteur dans Claude Desktop
via un fichier de configuration modifié manuellement** (une méthode plus
ancienne et non officielle) : supprimez cette ancienne entrée et
redémarrez complètement Claude Desktop avant d'ajouter le connecteur
officiel comme décrit ci-dessus. Faire fonctionner les deux en même temps
peut causer des problèmes.

## ChatGPT

Les connecteurs personnalisés sont disponibles dans les paramètres de
connecteurs du **mode développeur** de ChatGPT. Utilisez la même URL de
serveur (avec `/mcp`) et choisissez l'enregistrement OAuth
automatique/dynamique, comme ci-dessus.

**Exigence de forfait :** avec un forfait personnel ChatGPT Plus ou Pro,
les connecteurs MCP personnalisés peuvent être limités à la lecture de
l'état (par exemple, lister les appareils) sans pouvoir rien modifier —
allumer ou éteindre une lumière peut alors échouer silencieusement. Le
contrôle complet (lecture et écriture) nécessite un forfait ChatGPT
Business, Enterprise ou Edu. Si les commandes semblent se connecter et
lire correctement mais ne contrôlent rien en pratique, vérifiez d'abord
votre forfait avant de penser à un problème de configuration.

**Si la création du connecteur échoue à la première tentative** avec une
erreur du type « ne prend pas en charge Dynamic Client Registration » :
supprimez le connecteur et ajoutez-le à nouveau. Il a été observé que
cela échoue une première fois puis fonctionne immédiatement à la seconde
tentative sans aucun autre changement — cela semble venir du fait que
ChatGPT lui-même ne termine pas son processus de découverte la première
fois, plutôt que d'un problème avec votre serveur ou votre adresse.

**Si une commande dans votre propre langue ne fonctionne pas du premier
coup** (« Jag kunde inte tända lamporna just nu — anslutningen till
hemstyrningen misslyckades » / « impossible d'allumer les lumières pour le
moment — la connexion au système domotique a échoué »), essayez la même
demande une fois en anglais, puis réessayez dans votre propre langue. Il a
été observé que la première commande non anglaise échoue puis fonctionne
immédiatement ensuite — y compris pour d'autres commandes dans votre
propre langue — une fois qu'une commande en anglais est passée. Si une
commande continue d'échouer après cela, il faut alors le considérer comme
un vrai problème plutôt que cet effet de préchauffage.

## Perplexity

Perplexity prend en charge les connecteurs distants personnalisés selon
le même type de connexion que ci-dessus (OAuth avec enregistrement
automatique du client).

**Exigence de forfait :** la configuration d'un connecteur MCP
*personnalisé* dans Perplexity nécessite un **compte Perplexity payant**
— ce n'est pas disponible sur l'offre gratuite.

## Mistral Le Chat

À l'heure actuelle, la connexion à Mistral Le Chat **n'est pas prise en
charge** — l'ajout du connecteur échoue pendant la configuration avec une
erreur « connexion au serveur impossible ». Il a été confirmé qu'il
s'agit d'un problème du côté de Mistral, et non de votre serveur Aragon
Control MCP (le serveur fonctionne correctement avec d'autres assistants
au même moment). Si vous utilisez Mistral Le Chat, merci de revenir
vérifier une fois que Mistral aura résolu ce problème, ou d'utiliser en
attendant l'un des assistants pris en charge ci-dessus.

## Grok

Selon la documentation officielle de xAI, le forfait **gratuit** de Grok
inclut les connecteurs, et des connecteurs MCP personnalisés peuvent être
ajoutés en saisissant une URL de serveur — cela ne nécessiterait donc pas
forcément un forfait SuperGrok payant, contrairement à ce qui avait été
supposé précédemment. **Cela n'a pas encore été testé de bout en bout
avec Aragon Control MCP**, à considérer donc comme non confirmé plutôt
que comme une garantie. Si vous l'essayez, les mêmes étapes devraient
s'appliquer : saisissez votre URL de serveur avec `/mcp` à la fin et
choisissez l'enregistrement automatique/dynamique du client OAuth.
Merci de nous faire savoir ce qu'il se passe si vous testez cela, afin
que cette section puisse être mise à jour avec un résultat confirmé.

## Autres assistants

Si votre assistant prend en charge la connexion à des serveurs MCP
distants personnalisés avec OAuth 2.0/2.1 et l'enregistrement automatique
(dynamique) du client, les mêmes étapes devraient s'appliquer : saisissez
votre URL de serveur avec `/mcp` à la fin, choisissez l'enregistrement
automatique du client OAuth, et connectez-vous avec votre mot de passe
propriétaire Aragon Maestro lorsque cela vous est demandé. Si vous
rencontrez un problème avec un assistant non listé ici, merci de nous
contacter pour nous indiquer ce qui s'est passé.
