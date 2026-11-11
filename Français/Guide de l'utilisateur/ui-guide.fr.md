# Aragon Maestro : guide de l'interface utilisateur

Ce guide explique la page web du Maestro : ce que fait chaque élément de menu
et chaque section, et où les trouver. Il est écrit pour vous et pour les
assistants IA qui répondent aux questions sur Aragon. Les noms en **gras**
sont les mots affichés sur la page en français ; la page existe aussi en
anglais et en allemand (menu **LANGUE**).

Il décrit la page d'Aragon Maestro 2.6.4 et est mis à jour quand la page
change.

## Ouvrir la page

| Où vous êtes | Ce que vous ouvrez |
| ------------- | ------------------------------ |
| À la maison | Tapez `find.heyaragon.com` dans le navigateur et indiquez votre réseau (par exemple `192.168.1.`). La recherche affiche le Maestro ; cliquez dessus. Vous pouvez aussi taper directement l'adresse IP du Maestro. |
| En déplacement | Avec la **Connexion à distance** configurée (menu **SYSTÈME**), ouvrez l'adresse Tailscale du Maestro. |

La page a une barre de menus en haut et une longue page en dessous. Les
éléments de menu sautent à une section de la page, ouvrent une boîte de
dialogue ou ouvrent une page séparée dans un nouvel onglet.

## La barre de menus

### COMMANDE VOCALE

| Élément | Ce qu'il fait |
| ------------- | ------------------------------ |
| **Voir ce qui a été entendu** | Ouvre une page qui montre ce qu'Aragon a entendu et comment il l'a compris. Utile quand une commande n'est pas exécutée. |
| **Guide des commandes vocales** | Saute à la section **UTILISATION** : exemples de commandes pour l'éclairage, les stores, les scènes, la musique et les Super Scènes, avec de courtes vidéos. |
| **Assistant IA (Q&R)** | Saute aux questions, tâches et rappels que vous pouvez demander à l'assistant IA intégré. |
| **Apprentissage de nouveaux mots** | Apprend à Aragon les noms de vos pièces, appareils et scènes. Lancez-le après avoir ajouté ou renommé une pièce ou un appareil dans la passerelle. Cela prend une à deux minutes. Les favoris SONOS peuvent être inclus. |
| **Dépannage** | Saute à la section **DEPANNAGE**. |
| **Redémarrer l'application vocale** | Redémarre l'application vocale sans redémarrer le Maestro. La première chose à essayer quand la commande vocale ne réagit plus. |
| **Configurer en mode rapide ou réfléchi** | Règle combien de temps Aragon attend que vous ayez fini de parler (**Réflexion**). Valeurs suggérées : expérimenté 0-1, moyen 1-3, débutant 2-4. |
| **Affiner la reconnaissance** | Règle le **Seuil de confiance Kaldi** de la reconnaissance vocale intégrée (0,8-0,85 recommandé ; plus bas est plus rapide, plus haut est plus précis). Non utilisé tant que **Toujours utiliser l'IA cloud** est activé. |

### AUTOMATISATION

| Élément | Ce qu'il fait |
| ------------- | ------------------------------ |
| **Super Scènes** | Saute à la section **SUPER SCENES** (Bonjour, Bonne nuit, Je suis à la maison, Je pars). |
| **Règles d'automatisation** | Saute à la section **RÈGLES D'AUTOMATISATION** : des règles programmées écrites en langage courant. |
| **Surveillance de présence** | Ouvre la page de surveillance de présence dans un nouvel onglet. |
| **Événements Audio** | Ouvre les graphiques des sons détectés par les satellites (tableau de bord Node-RED). |

### CONFIGURATION

| Élément | Ce qu'il fait |
| ------------- | ------------------------------ |
| **Configuration de la passerelle** | Ouvre la boîte de dialogue de votre passerelle domotique : son adresse IP (**Recherche de passerelle** liste celles trouvées), le nom d'utilisateur et le mot de passe si nécessaire, et l'**Etat** de la connexion. Après une modification, comptez jusqu'à deux minutes. |
| **Satellites** | Saute à la section **SATELLITES**. |
| **Sélectionner la voix** | Ouvre la boîte de dialogue pour choisir la voix du Maestro. **Dites-le !** joue un essai. Option : envoyer les réponses parlées à l'enceinte SONOS de la même pièce. |

### NODE-RED

| Élément | Ce qu'il fait |
| ------------- | ------------------------------ |
| **Ouvrir Node-RED** | Ouvre l'éditeur Node-RED dans un nouvel onglet. |
| **Ouvrir Dashboard** | Ouvre le tableau de bord Node-RED. |
| **Node-RED library GitHub** | Ouvre la bibliothèque d'exemples Node-RED d'Aragon. |
| **Réinitialiser Node-RED** | Redémarre Node-RED (**Redémarrer**) ou le réinstalle (**Réinitialisation complète**). Une réinitialisation complète peut aussi effacer tous les paramètres sauf la passerelle. |
| **Paramétrer le mot de passe de Node-RED** | Remet le mot de passe Node-RED à la valeur d'usine. |

### SYSTÈME

| Élément | Ce qu'il fait |
| ------------- | ------------------------------ |
| **À propos et mises à jour** | Ouvre la page des versions logicielles et des mises à jour (voir plus bas). |
| **Connexion à distance** | Saute à la section **CONNEXION À DISTANCE**. |
| **Aragon Control MCP** | Saute à la section **ARAGON CONTROL MCP**. |
| **Accès support** | Saute à la section **ACCÈS SUPPORT**. |
| **IA cloud** | Saute à la section **IA CLOUD**. |
| **Chat IA Aragon** | Ouvre le chat Aragon dans un nouvel onglet. Avec l'IA cloud activée, le chat peut aussi commander les appareils et, s'il est activé sous **MCP Servers**, vérifier le Maestro. |
| **Talk to Home** | Ouvre une page où vous tapez ou dites une commande (saisie vocale dans Chrome, Safari et Samsung Internet). Elle montre comment la commande a été comprise. |
| **Vikunja To-Do App** | Ouvre l'application de tâches et de rappels. |
| **Netdata Dashboard** | Ouvre des graphiques en direct du processeur, de la mémoire et du réseau du Maestro. |
| **Fichiers journaux** | Affiche les fichiers journaux du Maestro. |
| **Redémarrer l'Aragon** | Redémarre le Maestro après confirmation. Il faut quelques minutes avant qu'Aragon soit de nouveau prêt. |
| **Eteindre l'Aragon** | Arrête le Maestro après confirmation. Attendez que le voyant rouge d'alimentation (PWR) soit complètement éteint (jusqu'à 3 minutes) avant de débrancher. |

### AIDE et LANGUE

| Élément | Ce qu'il fait |
| ------------- | ------------------------------ |
| **Aide et documentation** | Ouvre les guides et vidéos d'Aragon, enregistrés sur le Maestro. |
| **HeyAragon.com** | Ouvre le site web d'Aragon. |
| **find.HeyAragon.com** | Trouve les Maestros de votre réseau. |
| **LANGUE** | English, Français ou Deutsch : la langue de la page. Si elle diffère de la langue de l'assistant vocal, la page affiche un avertissement, car certaines fonctions ne marchent alors pas correctement. |

## La page, de haut en bas

| Section | Ce que vous y trouvez |
| ------------- | ------------------------------ |
| Colonne de gauche | **A propos** (vie privée), **La technologie** (un Maestro et 1 à 10 satellites, WL ou PoE, tous dans le même réseau). |
| **Pour commencer** | Rappel : donnez à vos pièces et appareils des noms appropriés dans la passerelle, pour pouvoir les commander à la voix. |
| **MAESTRO** | Comment installer le Maestro, puis **Configuration de la passerelle** : le bouton **Configuration de la passerelle** et l'état de la connexion, **Sélectionner la voix**, et **Apprentissage de nouveaux mots** pour les nouveaux noms d'appareils ou de pièces. |
| **SATELLITES** | Configurer les satellites (voir plus bas). |
| **UTILISATION** | Comment parler à Aragon (mot clé, une courte pause, puis la commande) et la liste des commandes vocales, avec vidéos ; en dessous, l'assistant IA pour les questions, tâches et rappels. |
| **DEPANNAGE** | **Redémarrer l'application vocale**, **Voir ce qui a été entendu**, **Configurer en mode rapide ou réfléchi**, **Affiner la reconnaissance**, et **Télécharger** pour les fichiers journaux. |
| **RÈGLES D'AUTOMATISATION** | **Gérer les règles** et **Paramètres de localisation** (voir plus bas). |
| **SUPER SCENES** | Les quatre Super Scènes et **Redémarrer les Super Scènes** (voir plus bas). |
| **CONNEXION À DISTANCE** | **Activer la connexion à distance** / **Désactiver la connexion à distance** (Tailscale ; créez d'abord un compte Tailscale gratuit), et d'autres sous-réseaux à joindre à distance (CIDR, par exemple `192.168.9.0/24`) avec **Enregistrer**. |
| **ARAGON CONTROL MCP** | Permet à un assistant IA comme Claude ou ChatGPT de commander votre maison. L'adresse de connexion avec **Copier**, le service avec **Redémarrer le service**, **Démarrer le service** ou **Arrêter le service**, le **Guide d'installation**, le **Mot de passe propriétaire** (celui que vous tapez sur la page de connexion du Maestro, pas votre mot de passe Claude ou ChatGPT) et **Republier l'adresse**. Nécessite la **Connexion à distance**. |
| **ACCÈS SUPPORT** | Permet à un assistant IA de vérifier votre Maestro ; il peut seulement lire. **Activer pour 48 heures** (avec le mot de passe propriétaire), **Désactiver maintenant**, l'adresse du connecteur avec **Copier**, le dernier accès, la ligne pour le chat Aragon, et le **Guide d'utilisation**. Tant qu'il est activé, un bandeau en haut de chaque page le signale. |
| **IA CLOUD** | Un service d'IA sur Internet (Scaleway, Paris) qui comprend les commandes vocales et y répond : **Utiliser l'IA cloud**, **Toujours utiliser l'IA cloud**, le **Guide d'installation**, la **Clé API** de votre compte Scaleway, et **Langue et voix** : **Langue parlée à la maison**, **D'autres langues sont aussi parlées à la maison**, **Parler la langue parlée à la maison**, et la **Voix de la langue parlée à la maison** (**Installer**, **Utiliser**, **Supprimer**, **Tester la voix**). |
| **VOIX EN LIGNE** | Une voix naturelle de KugelAudio, payée à l'usage : **Utiliser la voix en ligne (KugelAudio)**, la **Clé API KugelAudio**, **Voix par langue** et **Tester**. Tout ce que dit le Maestro est alors envoyé à KugelAudio sous forme de texte. |
| **RECHERCHE WEB** | Recherches sur Internet via Mojeek, payées à l'usage avec votre propre clé : **Utiliser la recherche web (Mojeek)** et la **Clé API Mojeek**. |
| Pied de page | Licences open source et contact. |

## Boîtes de dialogue et pages en détail

### Satellites

Dans la section **SATELLITES** :

1. Branchez le satellite (câble PoE, ou alimentation USB et Ethernet pour le
   modèle WL). Après environ une minute, il émet un son.
2. Cliquez sur **Rafraîchir le Satellite**. La liste en dessous affiche les
   satellites trouvés, par adresse IP.
3. Sélectionnez un satellite et cliquez sur **Afficher les détails**. La
   boîte de dialogue **Paramètres du satellite** s'ouvre.
4. **Événements Sonores** : choisissez les pièces où les sons sont détectés.
   Il faut au moins un satellite dont le **Système de mots-clés** est réglé
   sur centralisé.

Dans **Paramètres du satellite** :

| Réglage ou bouton | Ce qu'il fait |
| ------------- | ------------------------------ |
| **Pièce** | La pièce où se trouve le satellite. |
| **Volume** | Le volume du satellite. |
| **Mot clé** | Le mot qui réveille le satellite. Choisissez-en un dont la langue correspond à celle du satellite. |
| **Système de mots-clés**, **Prononciation mot clé**, **Sensibilité:** | Comment le mot clé est détecté. |
| **Réponse d'un seul appareil** | Seul le premier satellite qui vous a entendu répond. |
| **Émettre un son à l'activation** | Un bip quand le satellite se réveille : attendez-le avant de parler. |
| **Pas de retour visuel** | Pas de lumière bleue. |
| **Utiliser la suppression de bruit DeepFilterNet** | Meilleure compréhension dans les pièces bruyantes. |
| **Wi-Fi** | Connecte le satellite au Wi-Fi (2,4 GHz uniquement). |
| **Ouvrir l'application**, **Redémarrer l'application**, **Redémarrer**, **Éteindre** | Ouvre la page du satellite, redémarre son application, le redémarre ou l'éteint (pour le rallumer, débranchez puis rebranchez le câble). |
| **Mise à jour** | Met à jour le logiciel du satellite (plusieurs minutes, ne pas interrompre). |
| **Télécharger mots-clés** | Charge les mots clés disponibles. |
| **Test audio**, **Jouer un son de test** | Enregistre 5 secondes et les rejoue, ou joue un son de test. |

### Super Scènes

Les boutons **Bonjour**, **Bonne nuit**, **Je suis à la maison** et **Je
pars** ouvrent chacun une boîte de dialogue. Pour chacun, saisissez jusqu'à
trois commandes vocales exécutées quand vous dites la Super Scène, et si
besoin une condition (**Quand**, par exemple jour ou nuit). **Restaurer les
valeurs par défaut** rétablit les commandes d'origine d'une scène ;
**Redémarrer les Super Scènes** réinitialise les quatre.

### Règles d'automatisation

**Gérer les règles** ouvre la liste des règles. **Ajouter une règle**, tapez
la règle en langage courant (par exemple « Ferme tous les volets à 22 h le
week-end »), cliquez sur **Valider**, puis sur **Enregistrer**. Tout ce que
vous pouvez commander à la voix peut devenir une règle ; les règles se
répètent, ce ne sont pas des actions uniques. Pour les règles au lever ou au
coucher du soleil, indiquez la grande ville la plus proche sous **Paramètres
de localisation** → **Modifier**. Écrivez les règles dans la langue de
l'assistant vocal.

### À propos et mises à jour

La page ouverte par **SYSTÈME** → **À propos et mises à jour** :

| Partie | Ce qu'elle fait |
| ------------- | ------------------------------ |
| **1. Aragon est à jour** / **1. Une ou plusieurs mises à jour sont disponibles** | La mise à jour d'Aragon, avec **Mise à jour**. Ne redémarrez pas et n'éteignez pas le Maestro pendant une mise à jour. |
| **2. Application vocale (action et logique)** | Installe l'application vocale (**Installer**), par exemple quand une nouvelle version est affichée. |
| **3. Assistant vocal (Compréhension du langage naturel)** | Installe l'assistant vocal (**Installer**), par exemple pour changer sa langue quand la page signale des langues différentes. |
| **4. Changer de passerelle (Avancé)** | Passe à un autre type de passerelle. |
| **5. Oublier la passerelle (Avancé)** | Efface tous les paramètres de passerelle (IP, noms d'utilisateur, mots de passe). |
| **Afficher/Masquer Mise à jour du système** | **6. Mise à jour du système** : met à jour tous les fichiers système. Seulement en cas de problème. |
| **Afficher/Masquer la mise à jour Piper/TTS** | Met à jour les fichiers de la synthèse vocale (gros téléchargement). |
| **Afficher/Masquer l'heure système** | Fuseau horaire, synchronisation de l'heure réseau, date et heure. |
| **Paramètres ZRAM** | Mémoire compressée utilisée comme espace d'échange. |
| **Afficher/Masquer les licences** | Les logiciels open source utilisés dans Aragon. |
| **RETOUR** | Retour à la page principale. |

## Comment faire pour... ?

| Tâche | Où |
| ------------- | ------------------------------ |
| Redémarrer le Maestro | **SYSTÈME** → **Redémarrer l'Aragon**, puis confirmer. Si seule la commande vocale est bloquée, essayez d'abord **COMMANDE VOCALE** → **Redémarrer l'application vocale**. |
| Éteindre le Maestro | **SYSTÈME** → **Eteindre l'Aragon** ; ne débranchez que lorsque le voyant rouge PWR est éteint. |
| Changer le mot clé d'un satellite | **SATELLITES** : **Rafraîchir le Satellite**, sélectionner le satellite, **Afficher les détails**, **Mot clé**. |
| Commander à la voix une nouvelle pièce ou un nouvel appareil | Le nommer dans la passerelle, puis **COMMANDE VOCALE** → **Apprentissage de nouveaux mots**. |
| Voir pourquoi une commande n'a pas été comprise | **COMMANDE VOCALE** → **Voir ce qui a été entendu**. |
| Changer la voix du Maestro | **CONFIGURATION** → **Sélectionner la voix**. |
| Mettre à jour Aragon | **SYSTÈME** → **À propos et mises à jour**. |
| Obtenir les fichiers journaux | **DEPANNAGE** → **Télécharger**, ou **SYSTÈME** → **Fichiers journaux**. |
| Joindre le Maestro à distance | **SYSTÈME** → **Connexion à distance**. |
| Commander la maison depuis Claude ou ChatGPT | **SYSTÈME** → **Aragon Control MCP**, et son **Guide d'installation**. |
| Faire vérifier le Maestro par le support ou un assistant IA | **SYSTÈME** → **Accès support** → **Activer pour 48 heures**. |
| Utiliser un service d'IA pour les commandes vocales | **SYSTÈME** → **IA cloud**, et son **Guide d'installation**. |
| Discuter avec Aragon | **SYSTÈME** → **Chat IA Aragon**. |
| Changer la langue de la page | **LANGUE**. |
| Trouver les guides | **AIDE** → **Aide et documentation**, ou doc.heyaragon.com. |
