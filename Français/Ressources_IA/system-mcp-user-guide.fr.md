# Aragon System MCP : faire vérifier votre Maestro par un assistant IA

Avec l'Aragon System MCP, un assistant IA comme Claude peut examiner votre
Aragon Maestro et trouver pourquoi quelque chose ne fonctionne pas : un
satellite qui ne réagit pas, des commandes vocales sans effet, un système
lent après une mise à jour. Vous décrivez le problème avec vos propres mots ;
l'assistant vérifie lui-même le Maestro et explique ce qu'il trouve.

Il ne fait que lire. Il ne modifie jamais un réglage, ne redémarre rien et
n'actionne aucun appareil. Il reste désactivé tant que vous ne l'activez pas,
et se désactive de lui-même au bout de 48 heures.

## Ce qu'il peut vérifier

| Domaine | Ce que l'assistant peut voir |
| ----------- | ------------------------------------------------------------ |
| État | Le contrôle complet du Maestro : paquets, services, ports, modèles vocaux, l'assistant IA, l'IA cloud, espace disque et mémoire |
| Erreurs | Avertissements et erreurs dans les journaux de chaque service Aragon, avec le moment de chacun |
| Changements | Changements de langue, entraînement vocal, réglages de l'IA cloud enregistrés, redémarrages (et leur cause), connexions, la dernière mise à jour et ce qu'elle a changé |
| Performances | Processeur, mémoire, swap et température des dernières heures, et le programme à l'origine d'un pic |
| Parole | Nombre de commandes vocales envoyées à l'IA cloud, rapidité des réponses, nouvelles tentatives (nombres uniquement) |
| Passerelle KNX | Type et adresse de la passerelle, état de la connexion, les appareils et pièces qu'Aragon en a chargés |
| Satellites | Pièce, mot d'activation, firmware et connexion réseau de chaque satellite ; les satellites que le Maestro ne trouve pas |
| Guides | Les guides et vidéos d'Aragon, pour que la réponse vous renvoie à la bonne page |

## Ce qui est partagé, et ce qui ne l'est jamais

L'assistant voit des informations système : états, versions, réglages sans
leurs valeurs secrètes, et lignes de journal. Avec Claude, ces informations
vont à Anthropic ; avec un autre assistant, à l'entreprise qui le fournit ;
dans le chat Aragon, au service d'IA cloud.

Jamais partagés :

- **Mots de passe et clés.** Ils sont retirés de tout ce que le connecteur
  renvoie, y compris le nom d'utilisateur et le mot de passe de votre
  passerelle KNX.
- **Ce qui a été dit à la maison.** La parole n'est rapportée qu'en nombres
  et en durées, jamais en texte.
- **Les enregistrements.** Le connecteur n'a aucun accès à l'audio.

## Ce dont vous avez besoin avant de commencer

- **Remote Connect configuré** sur votre Maestro, avec un mot de passe
  propriétaire. C'est le même mot de passe que pour Aragon Control MCP. Si ce
  n'est pas encore fait, consultez [Connecter votre assistant IA à Aragon
  Control MCP](mcp-client-setup-guide.fr.md).
- **Un assistant IA qui prend en charge les connecteurs personnalisés**, par
  exemple Claude (claude.ai ou Claude Desktop) ou ChatGPT avec le Developer
  mode activé.

## Étape 1 : l'activer

1. Ouvrez la page du Maestro et choisissez **Système → Accès support** dans le
   menu (la section se trouve sous Connexion à distance).
2. Cliquez sur **Activer pour 48 heures** et saisissez votre mot de passe
   propriétaire.
3. La section indique jusqu'à quand l'accès est activé, ainsi que l'adresse
   de votre connecteur avec un bouton de copie.

Vous pouvez cliquer sur **Désactiver maintenant** à tout moment. L'assistant
perd alors immédiatement l'accès. Le réactiver ouvre une nouvelle période de
48 heures et déconnecte tout assistant connecté auparavant : il doit se
reconnecter. Changer le mot de passe propriétaire dans la section Aragon
Control MCP a le même effet.

## Étape 2 : connecter votre assistant

L'adresse de votre connecteur se termine par `/aragon-system/mcp` :

```
https://nom-de-votre-appareil.votre-tailnet.ts.net/aragon-system/mcp
```

Copiez-la depuis la section Accès support. Oublier `/mcp` à la fin est
l'erreur de configuration la plus fréquente.

**Claude (claude.ai et Claude Desktop)**

1. Ouvrez **Paramètres → Connecteurs → Ajouter un connecteur personnalisé**
   et donnez-lui un nom, par exemple « Aragon System ».
2. **URL du serveur :** votre adresse, avec `/mcp` à la fin.
3. **Client OAuth :** choisissez **« Aucun ID client — en enregistrer un
   automatiquement »**. Ne choisissez **pas** « Utiliser les métadonnées
   client hébergées par Anthropic ».
4. Laissez **En-têtes de requête supplémentaires** vide et validez.
5. Sur la page de connexion **Aragon Maestro**, saisissez votre **mot de
   passe propriétaire** (pas votre mot de passe Claude). Vous êtes renvoyé
   vers Claude, connecté.

**ChatGPT :** activez le **Developer mode** dans **Settings → Security and
login**, puis ajoutez un connecteur avec la même adresse et l'enregistrement
OAuth automatique.

Aragon Control MCP et le System MCP sont deux connecteurs distincts, avec deux
adresses. Vous pouvez avoir les deux : l'un pilote votre maison, l'autre la
vérifie.

## Étape 3 : poser votre question

Ouvrez une nouvelle conversation, vérifiez que le connecteur y est activé, et
décrivez le problème comme vous le feriez à une personne. Par exemple :

- « Mon Maestro ne réagit plus aux commandes vocales. Trouve pourquoi et
  dis-moi ce que tu as vérifié. »
- « Le satellite du salon ne réagit pas. Qu'est-ce qui ne va pas ? »
- « Les commandes vocales n'allument aucune lumière. Ma passerelle KNX
  est-elle connectée ? »
- « Depuis la dernière mise à jour, quelque chose ne marche plus. Qu'est-ce
  qui a changé ? »
- « Y a-t-il eu quelque chose d'inhabituel sur mon Maestro hier entre 18 h et
  22 h ? »
- « Comment changer le mot d'activation d'un satellite ? »

L'assistant choisit lui-même les vérifications à lancer. Une bonne réponse
indique ce qui a été vérifié, ce qui est normal et ce qui ne l'est pas, et
renvoie au guide Aragon adapté.

**Voir chaque vérification avant qu'elle ne s'exécute :** comme les
vérifications ne font que lire, Claude peut les lancer sans demander. Pour
approuver chacune, ouvrez les paramètres du connecteur dans Claude et réglez
les outils voulus, par exemple **Recent errors** et **Support report**, sur
**Approbation requise** (dans l'interface anglaise **Needs approval**).

## Dans le chat Aragon (avec l'IA cloud)

Les mêmes vérifications fonctionnent aussi dans la page de chat du Maestro,
sans Claude, lorsque l'IA cloud est activée. Cela ne dépend pas de
l'interrupteur Accès support et ne nécessite pas Remote Connect.

1. Ouvrez le chat depuis le menu du Maestro, puis ouvrez **Serveurs MCP**.
2. Activez **Aragon System** et démarrez une **Nouvelle conversation**.
3. Posez votre question comme à l'étape 3.

Tant qu'il est activé, chaque message envoyé à l'IA cloud coûte un peu plus,
car les descriptions des vérifications accompagnent chaque message. Pour
arrêter, désactivez-le et démarrez une nouvelle conversation ; ouvrir le chat
depuis le menu du Maestro le désactive à nouveau. Tant que les vérifications
sont proposées, le chat ne peut pas ouvrir de pages web.

## Envoyer un rapport au support Aragon

Demandez à l'assistant : *« Fais un rapport de support et donne-le-moi sous
forme de texte à envoyer au support Aragon. »* Le rapport contient les mêmes
faits que ceux vus par l'assistant, avec la date et l'heure, pour que le
support voie exactement ce qui a été vérifié. Faites un second rapport après
une correction pour montrer ce qui a changé.

## Si la connexion échoue

- **« Connexion impossible » ou « service indisponible » :** l'accès support
  est désactivé, ou ses 48 heures sont écoulées. Réactivez-le sur la page du
  Maestro et reconnectez-vous.
- **La connexion fonctionnait et demande maintenant de se reconnecter :**
  l'accès support a été réactivé ou le mot de passe propriétaire a été
  changé, ce qui déconnecte les connexions précédentes. Connectez-vous avec
  votre mot de passe propriétaire.
- **Se reconnecter ne suffit pas :** supprimez le connecteur dans Claude et
  ajoutez-le à nouveau.
- **La section indique « Remote Connect n'est pas connecté » :** le Maestro
  n'est pas connecté à Tailscale pour le moment. L'accès support refonctionne
  de lui-même dès que Remote Connect est connecté ; inutile de le désactiver
  puis de le réactiver.
- **L'adresse ne répond pas du tout :** Remote Connect doit être configuré et
  joignable. Vérifiez l'état de la Connexion à distance sur la page du
  Maestro.

## Limites

- L'assistant explique ; il ne répare pas. C'est vous ou le support Aragon
  qui faites la modification.
- Il ne peut pas vérifier un Maestro éteint ou sans connexion Internet.
- Il voit la passerelle KNX, les satellites et le flux Node-RED du point de
  vue du Maestro : s'ils répondent et ce qu'ils signalent, pas leurs propres
  réglages internes.
