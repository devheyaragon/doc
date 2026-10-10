# Configuration du serveur IA cloud

Le serveur IA cloud permet à votre Aragon Maestro de comprendre les
commandes vocales qu'il ne comprend pas lui-même. Vous pouvez alors parler
librement, aussi dans une autre langue que celle du Maestro, et obtenir une
courte réponse parlée. Les commandes sont comprises par un service d'IA sur
Internet, Scaleway Generative APIs, qui fonctionne à Paris (France).
Scaleway le facture sur votre propre compte.

Sans Internet, ou tant que le service ne répond pas, le Maestro comprend
les commandes lui-même comme avant.

Les boutons de la console Scaleway sont cités tels qu'ils apparaissent dans
la console en anglais.

## Ce qu'il vous faut avant de commencer

- **Un compte Scaleway.** Les particuliers peuvent en ouvrir un ; le
  paiement se fait par carte ou par prélèvement SEPA. Inscription sur
  [console.scaleway.com](https://console.scaleway.com).
- **Aragon Control MCP démarré** sur votre Maestro. Le serveur IA cloud
  l'utilise pour exécuter les commandes dans la maison. Vous le trouvez
  dans la section **Aragon Control MCP** de la page du Maestro. L'accès à
  distance pour les assistants IA n'est pas nécessaire pour cela.
- **Un logiciel Maestro à jour.**

## Étape 1 – Un accès propre pour le Maestro (recommandé)

Scaleway peut donner à une *application* un accès propre qui ne peut
utiliser que les modèles d'IA. La clé que vous saisissez dans le Maestro ne
peut alors rien faire d'autre dans votre compte Scaleway.

1. Dans la console Scaleway, cliquez sur **IAM & API keys** dans le menu
   déroulant en haut à droite.
2. Ouvrez l'onglet **Applications** et cliquez sur
   **Create application**.
3. Saisissez un nom, par exemple *Aragon Maestro*, et cliquez sur
   **Create application**.
4. Ouvrez l'onglet **Policies** et cliquez sur **Create policy**.
5. Saisissez un nom, par exemple *Aragon Maestro AI*, et choisissez
   l'application que vous venez de créer comme **principal**.
6. Ajoutez une règle : choisissez le permission set
   **GenerativeApisModelAccess** (sous Generative APIs – Serverless),
   cliquez sur **Next**, choisissez votre Project comme portée (scope) et
   terminez la policy.

Vous pouvez sauter cette étape et créer la clé pour votre propre
utilisateur (étape 2, « Bearer »), mais cette clé a alors tous les droits
de votre utilisateur.

## Étape 2 – Créer la clé API

1. Dans **IAM & API keys**, ouvrez l'onglet **API keys** et cliquez sur
   **+ Generate API key**.
2. **Bearer :** choisissez l'application de l'étape 1 (ou vous-même).
3. **Description :** par exemple *Aragon Maestro*.
4. **Expiration :** choisissez quand la clé cesse de fonctionner. Une clé
   sans expiration n'a jamais besoin d'être renouvelée ; une date de fin
   limite le risque si la clé est un jour divulguée. Si vous choisissez une
   date de fin, notez-la dans votre agenda. Votre Organisation Scaleway
   peut imposer une durée maximale.
5. **Object Storage :** choisissez **No, skip for now**.
6. Cliquez sur **Generate API key**.
7. Copiez la **secret key**. Elle n'est affichée qu'une seule fois. (L'access
   key, la plus courte, qui commence par *SCW*, n'est pas nécessaire.)

## Étape 3 – L'activer dans le Maestro

Ouvrez la page du Maestro et allez à la section **Serveur IA cloud**.

1. **Clé API :** collez la secret key et cliquez sur **Enregistrer**. La
   page affiche alors « Une clé est définie. » La clé est enregistrée sur le
   Maestro et n'est plus jamais affichée.
2. **Langue parlée à la maison :** choisissez la langue que la plupart des
   personnes parlent au Maestro et cliquez sur **Enregistrer**. Les
   commandes courtes sont alors bien mieux comprises ; les autres langues
   fonctionnent toujours.
3. Activez **Utiliser le serveur IA cloud**. Après un instant, l'état
   affiche **Prêt**.
4. Facultatif : **Toujours utiliser le serveur IA cloud.** Activez ceci
   lorsque le foyer parle une autre langue que celle du Maestro. Chaque
   commande vocale est alors envoyée au service d'IA, même celles que le
   Maestro aurait comprises lui-même ; sinon le Maestro peut prendre des
   mots étrangers pour une commande. L'application vocale redémarre lorsque
   vous modifiez ce réglage.

Essayez : dites une commande avec vos propres mots, par exemple « mets un
peu plus de lumière ici » ou « ferme les volets du salon ».

## Ce que cela coûte

Scaleway facture à l'usage. Prix en octobre 2026, hors taxes :

- Reconnaissance vocale (*whisper-large-v3*) : 0,003 € par minute d'audio.
- Compréhension de la commande (*qwen3-235b-a22b-instruct-2507*) : 0,75 €
  par million de tokens en entrée et 2,25 € par million de tokens en
  sortie.
- Offre gratuite : les premiers 1 000 000 tokens et 60 minutes d'audio.

Une commande vocale utilise environ 5 000 tokens et quelques secondes
d'audio, soit environ 0,004 €. À 50 commandes par jour, cela fait environ
6 € par mois. Avec **Toujours utiliser le serveur IA cloud**, chaque
commande compte.

Scaleway ne peut pas arrêter le service à un plafond de dépenses, mais vous
pouvez configurer des **Billing alerts** dans la console Scaleway pour être
averti par e-mail. Prix actuels :
[scaleway.com/en/pricing/model-as-a-service](https://www.scaleway.com/en/pricing/model-as-a-service/).

## Ce qui est envoyé à Scaleway

Pour chaque commande envoyée au service d'IA : l'enregistrement de ce que
vous avez dit, son texte et les noms de vos pièces, appareils et scènes,
pour que le service d'IA choisisse le bon. Les questions sur la météo, un
calcul ou autre envoient aussi le résultat de cette recherche. Le service
fonctionne à Paris (France).

## Quand la clé ne fonctionne plus

Si la clé a expiré ou a été supprimée, l'état sur la page du Maestro
indique : « Activé, mais le service d'IA refuse la clé API. Elle a peut-être
expiré. » Le Maestro traite alors les commandes lui-même.

1. Créez une nouvelle clé comme à l'étape 2.
2. Collez-la dans le champ **Clé API** et cliquez sur **Enregistrer**. La
   clé est vérifiée immédiatement.
3. Dans la console Scaleway, supprimez l'ancienne clé (onglet
   **API keys**, menu à côté de la clé, **Delete**).

## Messages d'état

| État | Que faire |
|---|---|
| Prêt | Rien. |
| Désactivé | Activer **Utiliser le serveur IA cloud**. |
| Activé, mais aucune clé API n'est définie | Saisir la clé (étape 3). |
| Activé, mais le service d'IA refuse la clé API | Créer une nouvelle clé (voir ci-dessus). |
| Activé, mais Aragon Control MCP n'est pas démarré | Le démarrer dans la section **Aragon Control MCP**. |
| Activé, mais le service d'IA n'a pas pu être joint à l'instant | Vérifier la connexion Internet. Le Maestro fonctionne seul en attendant et réessaie plus tard. |
| Ne répond pas | Redémarrer le Maestro. Si cela persiste, contacter votre installateur. |
