# ARAGON 2
![Logo.png](./_images/Logo.png)

![PROKNX_coul.png](./_images/PROKNX_coul.png)

***
# SOMMAIRE

[Généralités](#allgemeines)

[Caractéristiques](#eigenschaften)

[Installation](#aufstellung)

[Installation mécanique et électrique](#mechanische)

[Mise en service étape par étape](#inbetriebnahme)

> [Interface de configuration Aragon Master](#verbindung)

> [Réglage de la langue](#sprache)

> [Réglage de la passerelle](#gateway)

> [Mise à jour](#update)

> [Configuration de la passerelle](#konfiggate)

> [Configuration des satellites](#konfigsat)

[Configuration pour SONOS](#sonos)

[Annexe 1: Serveurs et passerelles](#Anhang1)

> [proServ par ProKNX](#proServ)

> [X1 par Gira](#x1)

> [Homeserver par Gira](#homeserver)

> [all-KNX](#allknx)

> [SVS par Jung](#svs)

[Annexe 2: Ce qui est compris](#Anhang2)

> [Commande de lumière](#lights)

> [Commande de volets](#shutter)

> [Scènes](#scenes)

[Annexe 3: Réglages utiles](#Anhang3)

> [Désactiver les commandes globales](#globalcmd)

> [Mettre des fonctions sur la liste noire](#blacklist)

> [Utilisation de AV Control](#av-control)

***

<h1 id="allgemeines">Généralités</h1>

## Description

**Aragon** est le premier système de contrôle vocal basé sur l'IA qui fonctionne sans connexion Internet. La reconnaissance vocale se fait sur l'appareil - rien n'est transmis ou enregistré.

**Votre vie privée est garantie**

Une installation Aragon se compose d'un (et seulement un) **Master** ainsi que de **1 à 10 satellites**. Le Master est obligatoire et responsable de la plupart des tâches, par exemple pour comprendre les commandes vocales et interagir avec le système d'automatisation de la maison. Le Master travaille avec un ou plusieurs satellites. Les satellites sont disponibles en deux versions :
- Satellite2 WL 
dans un boîtier de table avec connexion RJ45 ou Wi-Fi et alimentation via un adaptateur secteur USB 5V
- Satellite2 PoE
au format de boîte de commutation avec connexion PoE. Il est livré avec une façade de haut-parleur de 55x55mm mais sans cadre. Trois couleurs sont disponibles, blanc, noir ou aluminium. Front LS pour LS990 et LS-Zero également possible, veuillez demander séparément.

Toute combinaison de WL et de Satellite PoE est possible. Tous les appareils Aragon doivent être installés dans le même réseau.
***

<h1 id="eigenschaften">Caractéristiques</h1>

## Confidentialité

- Le système de contrôle vocal hors ligne fonctionne également sans connexion Internet. Ce qui est dit à la maison reste à la maison
- Ce qui est dit n'est ni transmis ni enregistré
- La reconnaissance et l'évaluation de la parole se font sur l'appareil
- Aucun compte Internet, aucune "compétence" n'a besoin d'être activée

## Flexibilité

- Entièrement personnalisable. Les mots et phrases personnels peuvent être facilement appris et compris par l'assistant
- Les commandes peuvent être formulées de milliers de façons différentes
- ProKNX peut prendre en compte les besoins spécifiques des clients en matière de vocabulaire
- Le système est interopérable et peut travailler avec d'autres systèmes

***

<h1 id="aufstellung">Installation</h1>

Pour l'emplacement d'installation des satellites ARAGON, les points suivants doivent être pris en compte :
- Il ne devrait pas y avoir de haut-parleur à proximité immédiate (pas plus près de 2m)
- Lors de l'utilisation de plusieurs satellites Aragon, la distance entre eux doit être aussi grande que possible pour éviter que deux appareils ne soient activés en même temps.
- L'utilisation de plusieurs appareils dans une même pièce est possible, mais doit être coordonnée avec nous (ProKNX) à l'avance.
- La distance à la personne qui parle peut être de plus de 7m dans un environnement calme. Ainsi, une pièce de 15m de long peut être couverte par un seul appareil, à condition qu'il soit placé au centre.
- Les bruits ambiants affectent la compréhension. Les orateurs simultanés sont particulièrement perturbants.
- Il est possible de mettre en sourdine les appareils multimédia après avoir reconnu le mot de réveil. Nous offrons une fonction pour cela dans le flux NodeRed. Les haut-parleurs Sonos sont automatiquement mis en sourdine si la zone SONOS choisie est la même que la salle d'installation.

***

<h1 id="mechanische">Installation mécanique et électrique</h1>

## Aragon Master 

(Art. Nr. 139)

L'Aragon Master est conçu pour être installé dans une armoire réseau. Il doit être alimenté en 5VDC, l'adaptateur secteur (fourni) fournit un courant maximal de 3A et est connecté au port USB-C du Master. De plus, une connexion réseau doit être établie avec la sortie RJ45, qui est marquée "LAN".

## Aragon Satellite2 WL 

(Art. Nr. 185)

Aragon Satellite2 WL peut être installé comme un appareil de table. Alternativement, il peut également être vissé au mur ou au plafond à l'aide de la plaque d'adaptateur.
Détachez la plaque d'adaptateur de l'appareil en la tournant dans le sens des aiguilles d'une montre.
Lorsqu'il est installé comme un appareil de table, les quatre pieds en caoutchouc fournis peuvent maintenant être insérés dans les creux des vis.
Lors du choix de l'emplacement, assurez-vous qu'il n'y a pas de haut-parleurs à proximité immédiate (pas plus près de 2 m). Lors de l'utilisation de plusieurs appareils ARAGON, la distance entre eux doit être aussi grande que possible pour éviter que deux appareils ne comprennent le mot d'activation en même temps.

## Aragon Satellite2 PoE 

(Art. Nr. 184)

Lors du choix de l'emplacement de l'installation, il faut veiller à ce qu'il n'y ait pas de haut-parleur à proximité immédiate (pas plus près de 2m). Lors de l'utilisation de plusieurs satellites Aragon, la distance entre eux doit être aussi grande que possible pour éviter que deux appareils ne soient activés en même temps. Un emplacement d'installation très approprié est souvent le milieu du plafond de la pièce, car la distance à la personne qui parle est généralement petite. Juste à côté de la porte de la pièce (par exemple, à côté de l'interrupteur de lumière) n'est souvent pas recommandé, car alors, avec la porte ouverte, l'ARAGON de la pièce voisine pourrait également être activé avec le mot de réveil.
L'appareil est connecté à un CAT5, CAT6 ou CAT7 par des bornes à vis. Le code couleur des désignations est comme suit :

> ![poe.png](./_images/poe.png)

La borne X n'est pas utilisée.
L'Aragon est alimenté par le standard PoE simple, c'est-à-dire IEEE 802.3af avec 48VDC / environ 13W de puissance utilisable. La consommation d'énergie est d'environ 10W.

***

<h1 id="inbetriebnahme">Mise en service étape par étape</h1>

<h2 id="verbindung">Interface de configuration Aragon Master</h2>

Le démarrage de l'Aragon Master prend environ 2 minutes. Il suffit d'appliquer la tension de fonctionnement. Ensuite, vous pouvez le trouver dans le réseau à l'aide d'un navigateur (Firefox, Chrome...) en entrant l'adresse <http://find.heyaragon.com>. Dans le champ de saisie de la fenêtre qui s'ouvre, vous devez indiquer le sous-réseau du réseau, par exemple «192.168.1.»
Après un court moment, le lien (l'adresse IP) de l'Aragon Master est affiché, qui peut simplement être cliqué. Une indication de port spécifique n'est pas nécessaire, car le Master est accessible via le port 80.
La fenêtre de l'interface de configuration du Master permet la mise en service étape par étape. Toutefois, des paramètres prédéfinis doivent d'abord être effectués, qui concernent en particulier la langue de l'utilisateur et la passerelle utilisée pour le couplage du processus. Il est également recommandé d'effectuer des mises à jour du système à ce stade.
Ces paramètres prédéfinis peuvent être atteints directement via divers points de menu de cette page :

![menu.png](./_images/menu.png)

<h2 id="sprache">Configuration de la langue</h2>

Veuillez d'abord vous assurer que la langue de l'appareil est correctement réglée (option de menu "LANGUE")

> **Attention :** La langue définie ici ne change pas seulement la langue de l'interface utilisateur affichée, mais affecte également une mise à jour et la mise à jour du logiciel système.
Veuillez également vous assurer que toute traduction automatique éventuellement activée de la fenêtre du navigateur dans le navigateur que vous utilisez est désactivée.

<h2 id="gateway">Configuration du gateway</h2>

Aragon nécessite la définition d'un gateway par défaut, par lequel il travaille avec l'automatisation du bâtiment. Le gateway actuellement défini est visible sur la première image de la page de configuration, par exemple :

![gira.png](./_images/gira.png)

Si ce gateway ne correspond pas à l'appareil utilisé dans votre installation, veuillez définir le gateway correct pour votre Aragon. Pour ce faire, allez à l'option de menu "À PROPOS"
Vous trouverez la ligne ici :

![gatewyAendern-de.jpg](./_images/gatewyAendern-de.jpg)

La fenêtre qui s'ouvre ensuite permet l'installation de l'un des gateways ou serveurs que nous supportons. Une liste des serveurs, leur description et les fonctions supportées sont décrites dans [Annexe 1 : Serveurs et gateways](#Anhang1).

<h2 id="update">Mise à jour</h2>

L'option de menu "À PROPOS" permet également de mettre à jour différents paquets du maître Aragon.
Sauf accord contraire, nous recommandons d'effectuer les trois mises à jour suivantes :

![ueber-de_.jpg](./_images/ueber-de_.jpg)

**1. Mise à jour du logiciel système Aragon**
La première des trois mises à jour mentionnées ici met à jour le logiciel système du maître Aragon. Cela concerne les versions suivantes mentionnées ici :

![versionen-de.jpg](./_images/versionen-de.jpg)

**2. Mise à jour / Installation de l'application linguistique**
Ce bouton installe ou met à jour le flux NodeRed "ARAGON V2,0" qui est absolument nécessaire pour le fonctionnement d'Aragon. Il prend en charge la communication avec le gateway correspondant (collecte de données, adressage des points de données...) ainsi que l'analyse de la commande reconnue et bien plus encore.

> **Attention :** Si vous avez effectué des modifications dans ce flux, elles seront supprimées et la valeur par défaut sera restaurée. Si vous souhaitez conserver vos modifications, vous devez les sauvegarder manuellement au préalable.

Un contrôle de version est directement visible dans l'un des groupes de ce flux principal :

![flowVersion-de.jpg](./_images/flowVersion-de.jpg)

**3. Mise à jour / Installation de l'assistant vocal**

La troisième entrée installe l'assistant vocal. Cela détermine comment les mots compris doivent être interprétés.
> **Attention :** L'assistant est installé en fonction de la langue de l'interface utilisateur sélectionnée.

Si des modifications dans la compréhension de certaines constructions de phrases sont souhaitées, elles peuvent nous être communiquées. Après ajustement de notre part, l'installation de l'assistant modifié activera ces modifications.

<h2 id="konfiggate">Configuration du Gateway</h2>

![konfiguration-de.jpg](./_images/konfiguration-de.jpg)

- La boîte de dialogue **"Configuration du Gateway"** varie en fonction du Gateway sélectionné. Les détails à ce sujet sont répertoriés séparément pour chaque Gateway dans [Annexe 1 : Serveurs et Gateways](#Anhang1).
Les deux premières cases à cocher doivent devenir vertes. Cela indique que la connexion entre le Master Aragon et le Gateway a été correctement initialisée et que la communication est établie.

- Le bouton **"Voix"** offre une variété de voix via une boîte de dialogue, dont la sortie vocale est générée hors ligne par un TTS interne.
    > **Attention :** La génération du fichier vocal est très intensive en calcul. En règle générale, on peut supposer que le temps de calcul représente un tiers de la durée du texte parlé.

- **L'entraînement de nouveaux mots** est nécessaire après chaque modification de la configuration des données lues via le Gateway. L'"ASR" (Reconnaissance Automatique de la Parole) doit être informé des mots qui sont répertoriés dans le dictionnaire et qui seront finalement compris après une séance d'entraînement effectuée.

<h2 id="konfigsat">Configuration des Satellites</h2>

![satellite-de.jpg](./_images/satellite-de.jpg)

Après avoir appuyé sur le bouton **"Recherche de satellite"**, les adresses IP des satellites trouvés s'affichent en peu de temps dans le menu déroulant ci-dessous.
Sélectionnez maintenant l'une des adresses et appuyez sur le bouton **"...plus d'informations"**.

![konfigsat-de.jpg](./_images/konfigsat-de.jpg)

- En appuyant sur le bouton **"Émettre un signal de test"**, le satellite avec l'adresse IP affichée émettra un son.
- Maintenant, dans la ligne **"Pièce"**, sélectionnez la pièce correspondante où cet Aragon est installé.
- Avec le réglage **"Sensibilité"**, vous pouvez ajuster la sensibilité d'activation du mot d'activation. Une sensibilité plus faible signifie qu'Aragon ne se réveillera pas aussi facilement et que le mot d'activation doit être prononcé très précisément.
- Le système amélioré **"Wakeword System"** permet notamment un plus grand choix de mots d'activation
- Pour le **"Wakeword"** sélectionné, la langue indiquée doit correspondre à la langue sélectionnée, car les lieux d'activation ne sont reconnus que s'ils sont prononcés correctement dans la langue correspondante. Les satellites peuvent tout à fait être configurés avec différents mots d'activation.
- Le bouton **"Wi-Fi"** n'est actif que pour un Aragon WL. Dans la boîte de dialogue qui s'ouvre, vous pouvez sélectionner le point d'accès correspondant à partir de la liste des SSID Wi-Fi. Après avoir entré le mot de passe Wi-Fi, la connexion est établie et le câble réseau peut être débranché.
    > **Attention :** Seul le Wi-Fi 2,4 Ghz est pris en charge

    > Le nom du SSID ne doit contenir aucun caractère spécial ni aucun espace

    > Après s'être connecté au réseau Wi-Fi, le satellite reçoit une nouvelle adresse IP

- **"Ouvrir l'application"** ouvre une nouvelle fenêtre de navigateur avec l'adresse IP du satellite
- **"Redémarrer l'application"** redémarre l'application sur le satellite
- **"Redémarrer"** effectue un redémarrage du satellite.
- **"Éteindre"** arrête le satellite et l'éteint. Un redémarrage n'est possible que par un cycle d'alimentation.
- **"Mise à jour"** effectue une mise à jour du satellite
- **"Obtenir les mots de réveil"** télécharge la collection actuelle de mots de réveil sur l'appareil.
- Le **"Test audio"** permet notamment de tester le microphone.
- La **"Réduction du bruit DeepFilterNet"** est un filtre numérique puissant qui peut notamment éliminer efficacement les bruits ambiants. Les "voix étrangères" ne peuvent être éliminées que difficilement, car elles ne peuvent pas être bien distinguées des données utiles. Si le bruit ambiant est faible, le filtre ne doit pas être activé.

***

<h1 id="sonos">Configuration pour SONOS</h1>

L'intégration d'un système de musique SONOS se fait en **définissant le nom de la zone SONOS identique au nom de la pièce d'un satellite**.

Ensuite, les fonctionnalités suivantes sont possibles :

> **Lorsque le mot d'activation est reconnu, la zone SONOS est mise en sourdine**
Cela a l'avantage que la commande vocale est beaucoup mieux comprise par ARAGON, car le bruit ambiant est réduit

> **Des listes de lecture, des titres de musique et des stations de radio peuvent être lancés par nom**
Cependant, cette fonction nécessite que les morceaux de musique, les listes ou les stations correspondantes soient enregistrés comme favoris SONOS. Directement dans l'application SONOS, il est possible de donner un nom propre à ces favoris (de préférence en allemand), afin qu'ils soient plus facilement compris par ARAGON. Après avoir défini les favoris, une course d'entraînement des mots doit être effectuée.

> **La sortie vocale des satellites ARAGON peut être redirigée vers la zone SONOS respective**
sur demande

Fonctions :

- **Démarrer la musique**
*joue de la musique dans le salon*, *allume la radio dans la cuisine*, *allume "ma musique"* (avec le titre du favori comme "ma musique"), *joue n'importe quoi*

- **Arrêter la musique**
*éteins la musique*, *éteins la radio*, *arrête la musique dans la cuisine*

- **Régler le volume**
*augmente le volume de la musique*, *baisse le volume*, *fais-le un peu plus doux*, *baisse le volume de la radio dans la cuisine*, *règle le volume dans le salon à 30%*

- **Saut de titre**
Le saut de titre ne fonctionne qu'avec une playlist en cours
*prochain titre*, *joue la prochaine chanson*, *joue le titre suivant*, *joue le dernier titre*, *répète le dernier morceau*

- **Shazam**
*quel est le nom de la chanson*, *quel est le nom du chanteur*

***
***

<h1 id="Anhang1">Annexe 1 : Serveurs et passerelles</h1>


| Passerelle | Description |
| ------ | ------ |
|[proServ by ProKNX](#proServ)|Une passerelle qui permet la configuration complète du contrôle vocal via l'ETS. L'opération de l'installation KNX via l'application et la voix peut donc être effectuée uniquement via une configuration ETS|
|[X1 by Gira](#x1)|La configuration d'un Gira X1 accessible sur le réseau est lue et peut être contrôlée par la voix|
|[Homeserver by Gira](#homeserver)|La configuration d'un serveur domestique Gira accessible sur le réseau (à partir de la version 4.10) avec un Quadclient configuré est lue et peut être contrôlée par la voix|
|Philips HUE|La structure du bâtiment et des fonctions d'un pont HUE est lue et peut être contrôlée par la voix|
|KNX IoT|Tout appareil qui supporte le protocole IoT de la 3ème partie KNX peut être lu et contrôlé|
|[all-KNX](#allknx)|Si aucun des serveurs KNX listés ici n'est présent dans votre installation, la communication entre Aragon et le bus KNX peut également être effectuée avec une interface standard KNXNet/IP|
|Passerelle générique|Permet de définir soi-même la communication entre Aragon et d'autres appareils selon une règle (RestfulAPI, commandes http, …)|
|YOUVI by PEAKnx|Supporte le contrôle de la visualisation YOUVI par la voix|
|[SVS by Jung](#svs)|Supporte le contrôle du Smart Visu Server de Jung par la voix|
|LUXORliving by Theben|Supporte le contrôle et l'interrogation d'une installation LUXORliving de Theben par la voix (installation KNX sans ETS)|
|Option ENOCEAN|Cette option permet en plus de l'une des passerelles standard définies ci-dessus l'utilisation de capteurs et d'acteurs qui supportent la norme Enocean|

***

<h2 id="proServ">proServ par ProKNX</h2>

![proserv.jpg](./_images/proserv.jpg)

Nous supposons que le proServ a déjà été configuré et testé pour la visualisation avant la mise en service de la commande vocale. Il faut noter que toutes les fonctions proposées par le proServ ne peuvent pas être utilisées pour la commande vocale et l'interrogation. Voici une liste des fonctions possibles :

- **Commuter**
La fonction est interprétée pour commuter la lumière (par exemple, *"allume/éteins la lumière dans le salon"*). 

    > **Remarque :** - Le nom de la lumière est reconnu exclusivement pour toute l'installation, c'est-à-dire qu'il n'est pas nécessaire de mentionner le nom de la pièce si l'appareil n'existe qu'une seule fois dans l'installation.

    > **Attention :** Ne pas utiliser cette fonction si des agrégats ou des prises commutables doivent être actionnés, car ils seraient alors également allumés/éteints avec la commande "Allume/éteins la lumière". Utilisez la fonction "AUX" pour cela !

- **Dimmer**
La fonction est utilisée pour la gradation de la lumière. La commande vocale permet de *commuter*, de *dimmer de manière relative* (par exemple, *"rendre plus lumineux"*), de *dimmer de manière absolue à une certaine valeur* (par exemple, *"régler la lumière à 50%"*), et de *demander l'état* (par exemple, *"comment est la lumière ?"*). 

    > **Remarque :** - Le nom de la lumière est reconnu exclusivement pour toute l'installation, c'est-à-dire qu'il n'est pas nécessaire de mentionner le nom de la pièce si l'appareil n'existe qu'une seule fois dans l'installation.

- **Jalousie avec retour d'information en octets**
C'est la seule fonction qui permet le contrôle et l'interrogation d'un store par la voix. La fonction suppose que l'acteur a été configuré pour le positionnement et le retour d'information sur la position ! Les objets de communication pour le démarrage et l'arrêt ne sont pas utilisés pour la langue.

- **AUX - Commuter - basculer**
La commande permet les mêmes commandes vocales que la commande "Commuter". La grande différence, cependant, est que cette fonction n'est pas interprétée avec le terme générique "Lumière". Par exemple : *"Éteins la chambre des enfants"* éteindra toutes les fonctions *"Commuter"* et *"Dimmer"*, mais pas une fonction AUX !

- **Statut - Valeur flottante de 2 octets**
Cette fonction permet de demander la valeur par la voix. L'unité spécifiée est également annoncée. Une demande possible serait : *"Quel est le statut de l'humidité de l'air?"*, ou *"quelle est la température?"*

- **Statut - 4 octets non signés**
Cette fonction permet de demander la valeur par la voix. L'unité spécifiée est également annoncée. Une demande possible serait : *"Quel est le statut du compteur d'eau?"*

- **Statut - 4 octets signés**
Cette fonction permet de demander la valeur par la voix. L'unité spécifiée est également annoncée. Une demande possible serait : *"Quelle est la consommation d'énergie?"*

- **Statut - Valeur flottante de 4 octets**
Cette fonction permet de demander la valeur par la voix. L'unité spécifiée est également annoncée. Une demande possible serait : *"Quel est le statut de l'intensité électrique?"*

- **RTR avec mode de fonctionnement 1Bit**
C'est la seule fonction qui permet le contrôle et l'interrogation d'un régulateur de température ambiante par la voix. Indépendamment du nom de la fonction défini, on peut demander *"quel est le thermostat"* ou *"quelle est la température"*. La réponse annonce à la fois la température actuelle et la température de consigne. De plus, la température de consigne peut être réglée, à la fois absolument et relativement (par exemple, *"augmente un peu la température"* ou *"règle le thermostat à 21 degrés"*)

- **Scènes**
...sont des fonctions très puissantes. Ils permettent d'attribuer un numéro de scène à un nom de scène. Lorsque le nom de la scène est prononcé, le numéro correspondant (conforme à KNX "-1") est écrit sur le bus. Aucun "supplément" parlé n'est nécessaire, il suffit de prononcer le nom tel qu'il a été défini (par exemple, *"appelle l'ascenseur"*, *"active le jaune"* ou *"bonne nuit"*)

> **Attention :** Après avoir modifié la configuration du proServ via l'ETS, les nouvelles données doivent être lues par le maître ARAGON, et les mots doivent ensuite être "entraînés". Ce processus est déclenché par un redémarrage de NodeRed **(à sélectionner dans le menu NODE-RED : Reset Node-Red -> Redémarrage)**. Cela prend environ 90 secondes.

***

<h2 id="x1">X1 par Gira</h2>

![GIRA_X1.jpg](./_images/GIRA_X1.jpg)

**Préparations sur le X1 :**
- Dans l'**Assistant de Projet Gira** (GPA), un "utilisateur fixe" nommé "Administrateur" avec le rôle "Administrateur" doit être créé dans la gestion des utilisateurs.

![GPA-benutzer-de.jpg](./_images/GPA-benutzer-de.jpg)

- Comme les **caractères spéciaux** ne sont pas prononcés, ils doivent être supprimés dans le GPA. Cela inclut en particulier les parenthèses, les points, les tirets et les barres obliques. Les accents sont possibles.
- Évitez les **abréviations**, car elles ne sont pas prononcées par la langue (par exemple, au lieu de "HWR" -> "Buanderie")
- Évitez les **énumérations** (au lieu de "Chambre d'enfant 1", "Chambre d'enfant 2"... mieux vaut utiliser "Max", "Moritz"). Écrivez les chiffres en toutes lettres (Salle de bain 1, Salle de bain 2 -> "Salle de bain un", "Salle de bain deux")

Connectez le maître ARAGON uniquement après un GIRA X1 configuré sur le réseau.
Après environ 2 minutes de démarrage, le maître ARAGON peut être recherché sur le réseau via un navigateur : http://find.heyaragon.com.

L'interface de configuration est accessible via l'adresse IP trouvée.

![konfiguration-de.jpg](./_images/konfiguration-de.jpg)

Le bouton "Configuration de la passerelle" ouvre une boîte de dialogue qui permet d'entrer l'adresse IP du X1. Lors de la première mise en service, le maître Aragon trouve cette adresse lui-même.
Dans la même fenêtre, le mot de passe de l'administrateur défini dans le GPA doit être entré.

Après la fermeture de cette boîte de dialogue, le maître Aragon essaie d'établir une connexion avec le Gira X1. La case "Statut de la connexion" devrait maintenant devenir verte.

**Les fonctions suivantes** du X1 sont prises en charge pour le contrôle et l'interrogation par la voix :

![schalter-x1-de.jpg](./_images/schalter-x1-de.jpg)

La fonction est interprétée pour allumer la lumière (par exemple, *"allume la lumière dans le salon"*).

> **Remarque :** - Le nom de la lumière est reconnu exclusivement pour toute l'installation, c'est-à-dire qu'il n'est pas nécessaire de mentionner le nom de la pièce, tant qu'il n'y a qu'un seul appareil dans l'installation.

> **Attention :** Ne pas utiliser cette fonction si des agrégats ou des prises commutables doivent être actionnés, car ils seraient également allumés/éteints avec la commande "Allume la lumière". Utilisez la fonction **"Bouton"** à la place !

![dimmer-x1-de.jpg](./_images/dimmer-x1-de.jpg)

La fonction est utilisée pour la gradation de la lumière. La commande vocale permet *d'allumer*, *de graduer relativement* (par exemple, *"rendre plus lumineux"*), *de graduer absolument à une certaine valeur* (par exemple, *"régler la lumière à 50%"*), et *d'interroger l'état* (par exemple, *"comment est la lumière?"*).

> **Remarque :** - Le nom de la lumière est reconnu exclusivement pour toute l'installation, c'est-à-dire qu'il n'est pas nécessaire de mentionner le nom de la pièce, à condition que l'appareil n'existe qu'une seule fois dans l'installation.

![taster-x1-de.jpg](./_images/taster-x1-de.jpg)

La commande permet les mêmes commandes vocales que la commande "Allumer". La grande différence, cependant, est que cette fonction n'est pas interprétée avec le terme générique "Lumière". Exemple : *"Éteindre la chambre des enfants"* éteindra toutes les fonctions *"Allumer"* et *"Graduer"*, mais pas une fonction "Bouton" !

![shutter-x1-de.jpg]./_images/shutter-x1-de.jpg)

Cette fonction permet le contrôle et l'interrogation d'un store par la voix. La fonction suppose que l'acteur a été configuré pour le positionnement et le retour de position et que les adresses de groupe correspondantes ont été déclarées dans le GPA ! Les adresses pour le démarrage et l'arrêt ne sont pas utilisées pour la voix.

![heizen-x1-de.jpg](./_images/heizen-x1-de.jpg)

Cette fonction permet le contrôle et l'interrogation d'un thermostat d'ambiance par la voix. Indépendamment du nom de la fonction défini, on peut demander *"comment est le thermostat"* ou *"quelle est la température"*. En réponse, la température réelle et la température de consigne actuelle sont annoncées. De même, la température de consigne peut être réglée, à la fois absolument et relativement (par exemple, *"augmenter un peu la température"* ou *"régler le thermostat à 21 degrés"*)

![szene-x1-de.jpg](./_images/szene-x1-de.jpg)

Les scènes sont des fonctions très puissantes. Ils permettent l'attribution d'un numéro de scène à un nom de scène. Lorsque le nom de la scène est prononcé, le numéro correspondant (conforme à KNX "-1") est écrit sur le bus. Aucun "supplément" parlé n'est nécessaire, il suffit de prononcer le nom tel qu'il a été défini (par exemple, *"Appeler l'ascenseur"*, *"activer le jaune"* ou *"bonne nuit"*)

![statusdez-x1-de.jpg](./_images/statusdez-x1-de.jpg)

Cette fonction permet de demander la valeur par langue. L'unité spécifiée est également annoncée. Une demande possible serait : *"Quel est le statut de l'humidité de l'air?"*, ou *"quelle est la température?"*. Les valeurs flottantes de 2 octets et de 4 octets sont prises en charge.

![statusmitvz-x1-de.jpg](./_images/statusmitvz-x1-de.jpg)

Cette fonction permet de demander la valeur par langue. L'unité spécifiée est également annoncée. Une demande possible serait : *"Quelle est la consommation d'énergie?"*

![statusohnevz-x1-de.jpg](./_images/statusohnevz-x1-de.jpg)

Cette fonction permet de demander la valeur par langue. L'unité spécifiée est également annoncée. Une demande possible serait : *"Quel est le statut du compteur d'eau?"*

![prozentwertgeber-x1-de.jpg](./_images/prozentwertgeber-x1-de.jpg)

Avec cette fonction, une valeur prédéfinie stockée dans le GPA peut être envoyée à une adresse de groupe. Par exemple, *"Activez la valeur par défaut"*

> **Attention :** Après avoir modifié la configuration du X1, les nouvelles données doivent être lues par le maître ARAGON, et les mots doivent ensuite être "entraînés". Ce processus est déclenché par un redémarrage de NodeRed **(à sélectionner dans le menu NODE-RED : Reset Node-Red -> Redémarrage)**. Cela prend environ 90 secondes.

> **Messages d'erreur**
Si un message d'erreur apparaît en raison de caractères spéciaux utilisés, veuillez les supprimer dans la configuration du GPA. Le bouton "Entraîner de nouveaux mots" dans la section "Configuration du gateway" permet de lister tous les mots lus. Les caractères spéciaux peuvent être rapidement localisés ici.

***

<h2 id="homeserver">Homeserver par Gira</h2>

![GIRA_Homeserver_.jpg](./_images/GIRA_Homeserver_.jpg)

**Préparations sur le Homeserver :**
- Dans le **Gira Expert**, un utilisateur nommé "Administrateur" doit être créé dans la gestion des utilisateurs.

![Experte.jpg](./_images/Experte.jpg)

Dans le **QuadClient**, un utilisateur pour Aragon doit être créé. **L'option "Fournir pour le service IoT" doit être sélectionnée**

![QC_benutzer.jpg](./_images/QC_benutzer.jpg)

- Comme les **caractères spéciaux** ne sont pas prononcés, ils doivent être supprimés dans le QC. Cela comprend en particulier les parenthèses, les points, les tirets et les barres obliques. Les accents sont possibles.
- Évitez les **abréviations**, car elles ne sont pas prononcées en langue (par exemple, au lieu de "HWR" -> "Buanderie")
- Évitez les **énumérations** (au lieu de "Chambre d'enfant 1", "Chambre d'enfant 2"... mieux vaut utiliser "Max", "Moritz"). Écrivez les chiffres en toutes lettres (Salle de bain 1, Salle de bain 2 -> "Salle de bain un", "Salle de bain deux")

Connectez le ARAGON Master à un serveur domestique GIRA configuré dans le réseau uniquement après. 
Après environ 2 minutes de démarrage, ARAGON Master peut être recherché dans le réseau via un navigateur : http://find.heyaragon.com. 

L'interface de configuration est accessible via l'adresse IP trouvée.

![konfiguration-de.jpg](./_images/konfiguration-de.jpg)

Le bouton "Configuration du Gateway" ouvre une boîte de dialogue qui permet de saisir l'adresse IP du serveur domestique. 
Dans la même fenêtre, le mot de passe administrateur défini par l'expert doit être saisi.

Après la fermeture de cette boîte de dialogue, Aragon Master tente d'établir une connexion avec le serveur domestique Gira. La case "État de la connexion" devrait maintenant devenir verte.

**Les modèles de fonction suivants** du QC sont pris en charge pour le contrôle et l'interrogation par la voix :

**Commutateur plus**

La fonction est interprétée pour commuter la lumière (par exemple, *"allume/éteins la lumière dans le salon"*).

> **Remarque :** - Le nom de la lumière est reconnu exclusivement pour toute l'installation, c'est-à-dire qu'il n'est pas nécessaire de mentionner le nom de la pièce, à condition que l'appareil n'existe qu'une seule fois dans l'installation.

> **Attention :** Ne pas utiliser cette fonction si des agrégats ou des prises commutables doivent être actionnés, car ils seraient également allumés/éteints avec la commande "Allume/éteins la lumière". Utilisez le modèle de fonction **"Bouton plus"** pour cela !

**Variateur plus**

La fonction est utilisée pour la gradation de la lumière. La commande vocale permet *de commuter*, *de graduer de manière relative* (par exemple, *"rend le plus lumineux"*), *de graduer absolument à une certaine valeur* (par exemple, *"règle la lumière à 50%"*), et *d'interroger l'état* (par exemple, *"comment est la lumière ?"*).

> **Remarque :** - Le nom de la lumière est reconnu exclusivement pour toute l'installation, c'est-à-dire qu'il n'est pas nécessaire de mentionner le nom de la pièce, à condition que l'appareil n'existe qu'une seule fois dans l'installation.

**Bouton plus**

La commande permet les mêmes commandes vocales que la commande "Switch plus". La grande différence, cependant, est que ce modèle de fonction n'est pas interprété avec le terme générique "lumière". Par exemple : *"Éteins la chambre des enfants"* éteindra toutes les fonctions *"Switch plus"* et *"Dim plus"*, mais pas une fonction *"Button plus"* !

**Volet roulant, store, fenêtre de toit**

Ce modèle de fonction permet le contrôle et l'interrogation d'un volet roulant par la voix. La fonction suppose que l'acteur a été configuré pour le positionnement et le retour de position et que les adresses de groupe correspondantes ont été déclarées ! Les adresses pour le démarrage et l'arrêt ne sont pas utilisées pour la voix.

**Chauffage plus**

Cette fonction permet le contrôle et l'interrogation d'un régulateur de température ambiante par la voix. Indépendamment du nom de la fonction défini, on peut demander *"quel est le thermostat"* ou *"quelle est la température"*. En réponse, la température actuelle et la température de consigne actuelle sont annoncées. De même, la température de consigne peut être réglée, à la fois absolument et relativement (par exemple, *"augmente un peu la température"* ou *"règle le thermostat à 21 degrés"*)

**Scène plus / Déclencheur**

Les scènes sont des fonctions très puissantes. Ils permettent d'attribuer un numéro de scène à un nom de scène. Lorsque le nom de la scène est prononcé, le numéro correspondant (conforme à KNX "-1") est écrit sur le bus. Aucun "supplément" parlé n'est nécessaire, il suffit de prononcer le nom tel qu'il a été défini (par exemple, *"Appelle l'ascenseur"*, *"active jaune"* ou *"bonne nuit"*)

**Capteur avec limite**

Cette fonction permet d'interroger la valeur par la voix. L'unité spécifiée est également annoncée. Une possible demande serait : *"Quel est le statut de l'humidité ?"*, ou *"quelle est la température ?"*, *"Quelle est la consommation d'énergie ?"*, *"Quel est le statut du compteur d'eau ?"*


> **Attention :** Après avoir modifié la configuration du QC, les nouvelles données doivent être lues par le maître ARAGON, et les mots doivent ensuite être "entraînés". Ce processus est déclenché par un redémarrage de NodeRed **(à sélectionner dans le menu NODE-RED : Reset Node-Red -> Redémarrage)**. Cela prend environ 90 secondes.

> **Messages d'erreur**
Si un message d'erreur apparaît en raison de l'utilisation de caractères spéciaux, veuillez les supprimer dans la configuration de la GPA. Le bouton "Entraîner de nouveaux mots" dans la section "Configuration du Gateway" permet de lister tous les mots lus. Les caractères spéciaux peuvent être rapidement localisés ici.

***

<h2 id="allknx">all-KNX</h2>

![knx-logo_.jpg](./_images/knx-logo_.jpg)

Le maître ARAGON peut être configuré de manière à ce que les commandes et les requêtes soient effectuées via une interface KNX Net/IP à l'aide d'adresses de groupe.

La configuration est stockée dans un fichier au format JSON. Ce fichier peut être créé manuellement et est relativement facile à lire en tant que fichier texte. Nous appelons ce fichier GDF (Gateway Description File).

**Création ou restauration du fichier GDF via Node-Red :**
Le fichier GDF peut être créé en anglais, allemand et français via NodeRed ou le fichier sauvegardé peut être restauré. Pour ce faire, ouvrez Node-Red avec **"ouvre l'éditeur de flux"** et l'utilisateur **"user"** ainsi que le mot de passe imprimé sur l'appareil.

![gdf-nodered_.jpg](./_images/gdf-nodered_.jpg)

En sélectionnant le nœud **"German"**, le fichier JSON peut être affiché de manière lisible et également édité.

![gdf-beispiel.png](./_images/gdf-beispiel.png)

Les modifications apportées sont enregistrées en utilisant le bouton **"Adoption (déploiement)"** en haut à droite.
Ce n'est qu'après avoir activé l'injection manuelle (petite zone bleue à gauche du nœud) que le fichier est actif et écrase la dernière configuration.

Les noms définis doivent maintenant être entraînés. Cela se fait via le menu **"Master" -> "Entraîner de nouveaux mots"**.

> **En alternative à la méthode décrite ci-dessus pour créer la configuration, qui nécessite une connaissance de la syntaxe JSON, nous recommandons l'utilisation de l'éditeur GDF.**

**L'éditeur GDF**

L'éditeur GDF est accessible via le tableau de bord Node-Red. Le lien se trouve sous l'onglet **"NodeRed" -> "Ouvrir le tableau de bord"**.

Après avoir effectué des modifications, celles-ci doivent être appliquées en utilisant le bouton **"SAVE AND APPLY"** en bas de cette page.

Les noms ainsi définis doivent encore être entraînés par ARAGON. Cela se fait via le menu **"Master" -> "Entraînement de nouveaux mots"**.

> **IMPORTANT :** Veuillez sauvegarder ces paramètres de manière permanente dans le cadre de la documentation du projet. Bien qu'ils soient stockés de manière "non volatile" dans l'appareil, ils seront écrasés si l'"Application vocale (Action & Logique)" est réinstallée sous l'onglet "À propos". Le fichier peut être sauvegardé via le lien **"Download active gatewayDescriptionFile.json"** qui se trouve sous le bouton **"SAVE AND APPLY"**.

L'éditeur GDF affiche l'image de la déclaration de variable active. Le fichier GDF sauvegardé peut être copié dans le nœud "German", permettant ainsi une restauration.

![gdf-sichern-de.JPG](./_images/gdf-sichern-de.JPG)

**Types de points de données pour le contrôle vocal**

L'éditeur GDF propose les types de points de données suivants :

- **lights**
Ce sont des appareils commutés qui répondent au nom de Lumière. Ils sont commandés de manière binaire par ON/OFF et ont également un retour binaire (DPT 1.001)
- **dimmers**
Ce sont des appareils qui répondent au nom de Lumière. Ils peuvent être commutés de manière binaire (DPT 1.001) ou réglés via une valeur (DPT 5.001). Le retour est suffisant via la valeur (DPT 5.001).
- **blinds**
Ce sont des appareils qui répondent au nom de Volet roulant. Ils ne peuvent être réglés que par une valeur (DPT 5.001). Un retour est également nécessaire via une valeur (DPT 5.001).
- **heating**
Ce sont des appareils qui répondent au nom de Thermostat. Trois points de données sont nécessaires :
Température actuelle : Température ambiante actuelle, (DPT 9.001)
Température cible : Température de consigne (DPT 9.001)
Température cible actuelle : Température de consigne actuelle (DPT 9.001)
- **scenes**
Les scènes sont définies par un nom qui, lors de son activation, envoie une valeur (numéro de scène) de 1…64 (DPT 17.001) à une adresse de groupe.
- **sensor**
Les capteurs ne peuvent être interrogés. En plus de l'adresse de groupe, le type DPT et l'unité sont également enregistrés. Pour déterminer le type DPT, il est préférable d'utiliser l'ETS, car le type y est affiché pour l'adresse de groupe utilisée.
- **aux**
…. Ce sont des appareils ou des fonctions qui ne doivent pas répondre au nom de Lumière et qui peuvent néanmoins être commutés. Ici, des points de données binaires peuvent être créés en tant que DPT 1.001, qui sont commandés par des instructions de commutation (On/Off). Cependant, il est également possible d'exclure une commutation du point de données en envoyant toujours une valeur fixe (On ou Off) avec la commande. La commande vocale est alors similaire à une scène qui est "activée".

***

<h2 id="svs">SVS par Jung</h2>

![svs.jpg](./_images/svs.jpg)

Veuillez mettre en service le SVS conformément aux instructions du fabricant.

**Mise à jour de JUNG SVS**

Si votre Jung SVS a un numéro de version de firmware de 1.2.1650 ou plus ancien, vous devez le mettre à jour.

1. Sauvegardez la configuration de votre SVS dans l'interface de configuration JUNG (Paramètres -> Système -> Sauvegarde)
2. Allez dans les paramètres et sélectionnez le bouton "Démarrer" dans la section "Mise à jour"
3. -> La mise à jour dure 5 - 10 minutes
4. -> Le système redémarre automatiquement
5. Allez à la gestion des utilisateurs
6. Créez au moins un administrateur
7. **Activez le protocole https** (sans https, aucune communication avec le SVS ne peut être établie!)

Connectez ARAGON Master à un SVS configuré dans le réseau uniquement après. 
Après environ 2 minutes de démarrage, ARAGON Master peut être recherché dans le réseau via un navigateur: http://find.heyaragon.com.

L'interface de configuration est accessible via l'adresse IP trouvée.

![konfiguration-de.jpg](./_images/konfiguration-de.jpg)

Le bouton **"Configuration du Gateway"** ouvre une boîte de dialogue qui permet d'entrer l'adresse IP du SVS. L'adresse est automatiquement entrée lors de la première mise en service si un SVS est trouvé dans le réseau.

Pour le **mot de passe**, veuillez entrer le "mot de passe administrateur". Celui-ci est défini via le logiciel de projet JUNG dans la section "Gestion des utilisateurs".

Les **fonctions** suivantes du SVS sont actuellement prises en charge pour les commandes vocales hors ligne :

- Interrupteur/Dimmer
- Interrupteur
- On/off (n'est pas traité comme "lumière", n'est donc pas pris en compte dans les commandes globales comme "éteindre la lumière dans la pièce")
- Dimmer
- Dimmer/interrupteur
- Moteur
- Volet roulant/Store/Auvent
- Store vénitien (Slider + Bouton)
- Chauffage
- Point de consigne de base
- Scène
- Activer la scène
- Valeur/État
- Affichage flottant 2Byte
- Affichage lux 2Byte
- Affichage flottant 4Byte
- Affichage entier non signé 4Byte
- Affichage entier signé 4Byte
- Émetteur de valeur
- Envoyer 1Byte (0-100%)

> Veillez à ce que le contrôle vocal soit activé dans la fonction respective du SVS

**Messages d'erreur**

Si un message d'erreur apparaît en raison de caractères spéciaux utilisés, veuillez les supprimer dans la configuration. N'utilisez pas d'abréviations dans les noms. Écrivez les chiffres en toutes lettres („Kinderzimmer 2“ -> „Chambre d'enfants deux“).


***
***

<h1 id="Anhang2">Annexe 2 : Ce qui est compris</h1>

La langue offre différentes possibilités d'exprimer une commande. Le soi-disant NLU (Natural Language Understanding) permet à ces différentes constructions de phrases et expressions d'être interprétées par l'ordinateur.

Par exemple : Les expressions suivantes ont toutes le même objectif, et pourtant elles sont fondamentalement différentes :

- *Allume la lampe suspendue*
- *Allumer la lampe suspendue*
- *Mets la lampe suspendue en marche*
- *Tourne la lampe suspendue*

Si l'appareil doit être utilisé dans une certaine pièce, la diversité est multipliée par au moins 4, car le nom de la pièce peut être mentionné avant ou après l'appareil. De plus, si des dimmers sont utilisés, la lumière peut être ajustée de manière absolue et relative.

Toutes ces combinaisons doivent être clairement comprises !

Pour cela, les commandes sont attribuées à ce qu'on appelle des INTENTs. Les Intents suivants sont réalisés :

<h2 id="lights">Commande de lumière</h2>

Commande de circuits d'éclairage individuels :
- *Allume/éteint le/la {FONCTION} dans {ZONE}* -> Le circuit de lumière nommé {FONCTION} dans la pièce nommée {ZONE} est contrôlé
- *Règle le/la {FONCTION} dans {ZONE} à 50%* -> Le circuit de lumière nommé {FONCTION} dans la pièce nommée {ZONE} est réglé à 50%
- *Rends le/la {FONCTION} plus lumineux* -> Le circuit de lumière nommé {FONCTION} dans la zone attribuée au satellite est augmenté de 20%

Commande de plusieurs circuits d'éclairage :
- *Allume/éteint la lumière* -> Tous les circuits d'éclairage de la zone configurée du satellite sont contrôlés
- *Allume/éteint la lumière dans {ZONE}* -> Tous les circuits d'éclairage de la zone mentionnée sont contrôlés
- *Allume/éteint la lumière partout (ou "dans toute la maison", "dans l'appartement")* -> Les circuits d'éclairage de toute l'installation sont contrôlés
- *Rends la lumière plus lumineuse* -> Tous les circuits d'éclairage de la zone configurée du satellite sont augmentés de 20%
- *Règle la lumière à 50%* -> Tous les circuits d'éclairage de la zone configurée du satellite sont réglés à 50%
- *Éteint la {Zone}* -> Tous les circuits d'éclairage de la pièce correspondante sont éteints

> **Reconnaissance unique des noms :** Pour la lumière (et seulement ici), la reconnaissance des noms est étendue à toute l'installation. Donc, si le nom est unique, le nom de la pièce n'a pas besoin d'être mentionné. Cependant, s'il y a plusieurs circuits de lumière avec le même nom dans l'installation, tous ces circuits seront servis.

> **Joker :** Les noms "Lumière", "Éclairage", "Lampes" sont des jokers pour toutes les fonctions d'éclairage (commutées et tamisées) dans la pièce ou dans la maison. "Lumière allumée" cherche donc tous les circuits de lumière dans la pièce et les allume. Il n'est donc pas judicieux de nommer un circuit de lumière avec l'un des noms de joker, car il ne peut pas être adressé individuellement.

> **Les commandes globales** peuvent être désactivées (voir ici : [Désactiver les commandes globales](#globalcmd))

***

<h2 id="shutter">Commande des stores</h2>

Commande de stores individuels :

- *Ouvre/Ferme le volet roulant* -> Le volet nommé "Volet roulant" dans la zone attribuée au satellite est déplacé jusqu'à sa position finale
- *Déploie le store extérieur* -> Le store nommé "Store" dans la zone "Extérieur" est déployé
- *Ouvre/Ferme un peu le store* -> Le store nommé "Store" dans la zone attribuée au satellite est ouvert/fermé de 20%
- *Positionne/Conduis la/le {FONCTION} dans {ZONE} à 20%* -> Le store adressé dans la pièce spécifiée est réglé à 20%. (100% signifie complètement fermé)

Commande de plusieurs stores :

- *Ouvre/Ferme les volets roulants*  -> Toutes les fonctions définies comme volets dans la zone attribuée au satellite sont déplacées jusqu'à leur position finale
- *Ouvre/Ferme les stores dans toute la maison* -> Toutes les fonctions définies comme volets dans toute l'installation sont déplacées jusqu'à leur position finale
- *Ferme la {Zone}* -> Toutes les fonctions définies comme volets dans la pièce correspondante sont fermées

> **Joker :** Les noms "Volets roulants", "Stores", "Rideaux" (seulement au pluriel) sont des jokers pour tous les entraînements motorisés dans la pièce ou dans la maison. "Monter tous les volets roulants" cherche donc toutes les fonctions de volets dans la pièce et les amène à la position finale supérieure. Si une pièce a à la fois des volets roulants et une protection contre les moustiques motorisée, cette commande ouvrirait les deux systèmes.

> **Portes, portails, écrans motorisés** : Ces appareils ne sont pas adressés avec les noms génériques.

> **Les commandes globales** peuvent être désactivées (voir ici : [Désactiver les commandes globales](#globalcmd))

***

<h2 id="scenes">Scènes</h2>

Les scènes sont des commandes universellement applicables. Il faut distinguer :
> **Scènes KNX**
Les membres concernés (canaux d'actuateurs) d'une scène KNX (également appelée scène secondaire) doivent déjà être configurés dans l'ETS.
En temps réel, il peut alors être défini comment chaque canal d'actuateur doit se comporter lors de l'appel d'un numéro de scène.
Pour la plupart des actuateurs, il est également possible de définir dans les paramètres de l'actuateur si un canal d'actuateur est du tout affecté par le numéro de scène, même s'il est associé à l'adresse de groupe de la scène.

> [**Superscènes**](#vocalautomating)
Aragon propose quatre superscènes (voici une liste avec quelques alternatives de commandes) :

|Bon matin|Bonne nuit|Je suis à la maison|Je quitte la maison|
|-----|-----|-----|-----|
|Matin|Dors bien|Je suis de retour|Au revoir|
|Moin|À demain|Nous sommes de retour|Bye bye|

Les **Superscènes** peuvent être très facilement éditées (également par l'utilisateur final). Il suffit d'écrire **la commande comprise par Aragon en clair dans l'une des lignes de commande**. Il est également possible de faire **prononcer un texte à Aragon après l'appel de la superscène**. Pour cela, le texte `SAY` doit être placé devant le texte à lire dans la ligne de commande.

![superscenekonfig-de.jpg](./_images/superscenekonfig-de.jpg)

> **Attention** : Si l'utilisation des superscènes est activée, ces commandes ont la priorité sur les scènes KNX de même nom.

**Désactivation des superscènes :**

De nombreuses applications KNX utilisent déjà les noms des superscènes. Si les superscènes doivent alors être désactivées, veuillez effectuer la modification suivante dans le Main Flow sous NodeRed :

![activate-ss.JPG](./_images/activate-ss.JPG)

![pfeil.JPG](./_images/pfeil.JPG)

![deactivate-ss.JPG](./_images/deactivate-ss.JPG)

**Commandes pour les scènes**
À l'exception des super scènes globales, la commande de scène se réfère toujours à la pièce à laquelle le satellite a été configuré.
De plus, les scènes ont la propriété que le seul nom de la scène prononcé provoque l'exécution de la commande. Cela permet également de générer des commandes très spécifiques :

*Appelle l'ascenseur* devant le satellite au 1er étage déclencherait par exemple le numéro de scène 1

*Appelle l'ascenseur* devant le satellite au 2ème étage déclencherait par exemple le numéro de scène 2


***
***

<h1 id="Anhang3">Annexe 3 : Paramètres utiles</h1>

<h2 id="globalcmd">Désactiver les commandes globales</h2>

Dans les grandes installations (par exemple lors de l'utilisation d'un maître Aragon pour plusieurs unités d'habitation), il peut être nécessaire de désactiver les commandes globales.

Pour cela, copiez le groupe suivant dans un nouveau flux (onglet) et réglez ensuite la valeur du nœud d'injection manuelle sur "false". 

![globalcmd.JPG](./_images/globalcmd.JPG)

Maintenant, il suffit d'exécuter le déploiement !

***

<h2 id="blacklist">Mettre des fonctions sur la liste noire</h2>

En particulier dans les très grandes installations, des centaines de fonctions sont souvent lues à partir du serveur KNX. Beaucoup d'entre elles ne sont pas destinées à être commandées par la voix ou ne doivent pas être commandées par la voix.
Pour exclure ces fonctions pour Aragon, une liste noire peut être créée, qui répertorie les noms des fonctions qui ne doivent pas être commandées.

Dans le flux principal d'Aragon V2.0, le groupe suivant est proposé à cet effet :

![blacklist.JPG](./_images/blacklist.JPG)

Copiez ce groupe dans un nouveau flux et éditez le bloc de fonction avec le fichier JSON.

***

<h2 id="av-control">Utilisation de AV Control</h2>

Les fonctions suivantes permettent de contrôler à la fois la télévision et l'audio dans une zone où ARAGON est également installé. 

Dans le flux principal de NodeRed, un groupe **"CUSTOM MEDIA CONTROL"** est disponible, qui peut être copié dans un nouveau flux.

![media control.png](./_images/media_control.png)

Les commandes suivantes sont disponibles pour le contrôle des appareils multimédias :

- Augmenter le volume (relatif)
- Diminuer le volume (relatif)
- Régler le volume (absolu, 0 - 100%)
- Démarrage du système AV (Allumer/Play)
- Arrêt du système AV (Éteindre/Pause)
- Prochain titre/chaîne
- Titre/chaîne précédente
- Demander ce qui est joué (SONOS uniquement)
- Sélection du titre de la chaîne par nom

Maintenant, ouvrez le nœud de filtre, et commentez la ligne 4 pour l'activer.
