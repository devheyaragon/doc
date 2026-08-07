# Service de domotique prédictive
### Documentation technique

---

## 1. Objectif et présentation générale

Le Service de domotique prédictive apprend comment votre foyer évolue dans votre logement au fil de la journée et utilise cette connaissance pour anticiper ce qui va se passer ensuite. Plutôt que de s'appuyer sur des horaires fixes ou des règles manuelles, il observe en continu les habitudes d'occupation et construit une image évolutive de vos routines — puis met cette image à profit.

**Le principal cas d'usage de ce service est le maintien à domicile et l'accompagnement des personnes âgées.** Le même modèle d'occupation qui alimente les automatisations de confort (éclairage, climatisation) est aussi ce qui permet de distinguer de manière fiable « cette pièce est simplement calme en ce moment » de « quelque chose semble anormal » — une distinction qu'une alarme à minuterie fixe ne peut pas établir, mais qu'un modèle ayant appris les rythmes réels de votre foyer le peut. Le §3 explique comment fonctionne cet apprentissage sous-jacent ; le §6 décrit la fonctionnalité de maintien à domicile en détail. Le reste de ce document s'applique de la même manière aux deux cas d'usage, puisque le même modèle sous-jacent pilote les deux.

En pratique, cela signifie que le service peut répondre à des questions telles que :

- « Une personne âgée ou vulnérable de la famille se déplace-t-elle dans son logement comme prévu aujourd'hui, ou le calme est-il inhabituel ? »
- « Est-il probable que quelqu'un soit dans le salon dans les 15 prochaines minutes ? »
- « La chambre sera-t-elle occupée dans une heure ? »

Ces réponses sont générées automatiquement et transmises au reste de votre plateforme domotique, qui peut les utiliser pour piloter l'éclairage, la climatisation, la sécurité et la surveillance de sécurité avec un minimum d'intervention de votre part.

---

## 2. Fonctionnement du service

Le service fonctionne comme un processus continu en arrière-plan. Voici ce qui se passe à chaque étape :

**1. Entrée des capteurs.** Les capteurs de mouvement et de présence répartis dans votre logement envoient des mises à jour d'état dès qu'ils détectent un changement — une personne qui entre dans une pièce, qui en sort, ou une pièce qui reste vide. Chaque mise à jour est horodatée et stockée localement.

**2. Construction d'une chronologie.** Le service convertit ces relevés de capteurs individuels en un historique d'occupation minute par minute pour chaque pièce. Si un capteur signale une présence à 9h14 et une absence à 9h47, le service comble cette fenêtre de 33 minutes en conséquence. Cela crée un historique continu et lisible de l'utilisation de chaque espace.

**3. Construction des prédictions à partir de l'historique.** Les 48 dernières heures de la chronologie de chaque pièce constituent l'entrée directe du modèle de prédiction. Le modèle a été entraîné sur des semaines de chronologies similaires issues de votre logement, de sorte que sa sortie reflète implicitement les habitudes récurrentes dans cet historique — par exemple, si une pièce est généralement occupée à un moment précis de la journée. La *prédiction d'occupation* de chaque pièce est générée indépendamment — le modèle n'utilise pas la chronologie d'une pièce pour prédire l'occupation d'une autre pièce. (La fonctionnalité de surveillance de sécurité distincte décrite au §6 croise bien les pièces entre elles, mais dans un but différent.)

**4. Génération des prédictions.** À partir de ce qu'il a appris, le service produit des estimations de probabilité pour l'instant *présent* ainsi que pour deux horizons temporels futurs :

   - **Maintenant** — la pièce est-elle occupée à cet instant ? C'est ce qui déclenche les actions immédiates et réactives, comme allumer une lumière lorsqu'une personne entre dans une pièce.
   - **15 minutes** — occupation à court terme (utile pour l'éclairage et la climatisation réactifs)
   - **1 heure** — occupation à moyen terme (utile pour le préchauffage, le prérefroidissement ou les contrôles de sécurité)

   Chaque prédiction est un nombre compris entre 0 et 1 représentant la probabilité qu'une pièce soit ou sera occupée. Vos règles d'automatisation utilisent ces valeurs pour décider quand agir.

**5. Déclenchement des automatisations.** La plateforme évalue chaque prédiction par rapport à des seuils configurés. Lorsque la probabilité d'occupation franchit un seuil, une action est déclenchée — par exemple, augmenter le chauffage, allumer les lumières ou désactiver une zone de détection.

---

## 3. Comment le service apprend

Le service utilise un modèle d'apprentissage automatique — un moteur de reconnaissance de motifs — entraîné sur l'historique des capteurs propre à votre logement. Il n'est pas préprogrammé avec une connaissance de vos routines ; il l'acquiert par l'observation.

**Le processus d'apprentissage comporte trois étapes :**

| Étape | Quand elle s'applique | Ce que cela signifie pour vous |
|---|---|---|
| **Démarrage** | Premier jour | Les prédictions reposent sur des modèles génériques, pas sur votre logement. À utiliser uniquement à titre de test ; ne pas s'y fier pour des automatisations critiques. |
| **Apprentissage** | Jour 1 à jour 7 | Le modèle commence à intégrer vos données réelles. Les prédictions s'améliorent sensiblement mais peuvent encore varier. Des interventions manuelles sont recommandées pour les actions importantes. |
| **Maturité** | Après le jour 7 | Le modèle dispose de suffisamment de données pour refléter vos habitudes réelles. Une automatisation complète est appropriée. |

**Un exemple concret.** Supposons que votre bureau à domicile soit occupé de façon fiable les matinées en semaine, d'environ 8h30 à midi, et vide le week-end.

- **Démarrage (jour 1) :** À la question « le bureau sera-t-il occupé à 9h ? », le service ne dispose pas encore de données propres à votre logement et se rabat sur un modèle générique — il pourrait répondre quelque chose comme 60 % de probabilité, que l'on soit mardi ou samedi, car il n'a pas encore observé votre propre habitude.
- **Apprentissage (jour 1 à 7) :** Au milieu de cette étape, le service s'est entraîné sur plusieurs matinées réelles. Une requête à 9h en semaine tend désormais correctement vers une valeur élevée (par ex. 75 à 85 %). Une requête à 9h le *week-end* peut encore être imprécise au début, simplement parce que le modèle n'a pas encore vu suffisamment de samedis pour en être sûr — c'est précisément pourquoi les interventions manuelles restent recommandées à ce stade.
- **Maturité (après le jour 7) :** Le service a désormais observé plusieurs semaines complètes. Une requête à 9h en semaine renvoie de manière fiable une probabilité élevée ; une requête à 9h le week-end renvoie de manière fiable une probabilité faible. La distinction entre les matinées de semaine et de week-end — et non simplement « le matin » en général — est ce que signifie concrètement « apprendre vos habitudes ».

Si votre routine réelle change par la suite — par exemple, vous commencez aussi à travailler depuis le bureau le samedi — le réentraînement nocturne décrit ci-dessous permet au modèle de s'adapter automatiquement, sans que vous ayez besoin de le reconfigurer. Comme la fenêtre d'entraînement porte sur 14 jours (soit environ deux occurrences de chaque jour de la semaine), un changement lié à un jour précis peut nécessiter quelques semaines de répétition avant d'être pleinement pris en compte, plutôt que de se refléter dès la première occurrence.

Le service détermine automatiquement dans quelle étape il se trouve — vous n'avez pas besoin de configurer ou de déclencher ces transitions.

**Évolution dans le temps.** Une fois mature, le modèle continue de se mettre à jour chaque nuit (entre 23h et 2h par défaut, lorsque le foyer est généralement inactif). Il s'adapte ainsi naturellement aux changements de votre routine — un nouvel horaire de télétravail, des variations saisonnières d'activité, ou un changement dans la composition du foyer — sans aucune reconfiguration manuelle nécessaire.

**Deux composants, deux façons d'apprendre.** Derrière les prédictions décrites au §2 se trouvent deux éléments distincts d'apprentissage automatique, qui apprennent de façon différente — il est utile de comprendre cette différence pour savoir à quoi s'attendre. Un modèle de base partagé est réentraîné depuis son point de départ initial chaque nuit, en utilisant à chaque fois les 14 derniers jours de vos données. Son rôle est de construire une compréhension générale des habitudes d'occupation de votre logement — ce n'est pas lui qui produit la décision finale sur laquelle agissent vos automatisations. Cette décision finale — les prédictions **maintenant**, **15 minutes** et **1 heure** décrites au §2 — est produite par trois petits composants spécialisés construits au-dessus de ce modèle de base, un par cas de figure. Contrairement au modèle de base, ces trois composants ne repartent pas de zéro chaque nuit : chacun affine un peu plus ce qu'il a déjà appris la nuit précédente. En pratique, cela signifie que ces trois prédictions ont tendance à devenir progressivement plus précises à mesure que le service fonctionne, bien au-delà du jalon de « maturité » indiqué dans le tableau ci-dessus — y compris la prédiction « maintenant » qui pilote vos automatisations les plus immédiates. Cette répartition est délibérée : réinitialiser le modèle de base, plus volumineux, chaque nuit évite qu'il ne dérive progressivement au fil de mois de mises à jour continues, tandis que les composants bien plus petits construits par-dessus peuvent, eux, continuer à progresser en toute sécurité.

---

## 4. Fenêtre de données et historique

Le service utilise deux fenêtres de données différentes pour deux finalités différentes. Comprendre cette distinction permet de se faire une idée précise de ce que le modèle sait et de la rapidité avec laquelle il s'adapte.

**Fenêtre de prédiction — 48 heures.** Lors de la génération d'une prédiction, le service interroge les 48 dernières heures de relevés de capteurs pour la pièce évaluée. Cette chronologie de 48 heures constitue l'entrée directe du modèle de prédiction, et sa durée reste identique quel que soit l'horizon de prévision — qu'il s'agisse de prédire l'occupation maintenant, dans 15 minutes ou dans une heure.

**Fenêtre d'entraînement — 14 jours.** Chaque entraînement nocturne s'appuie sur les 14 derniers jours d'événements de capteurs enregistrés. À partir de ces 14 jours, le pipeline génère un large ensemble d'exemples d'entraînement en faisant glisser une fenêtre d'entrée de 48 heures sur l'ensemble de la chronologie, par pas de 15 minutes. Chaque exemple d'entraînement individuel ne contient donc toujours que 48 heures de contexte d'entrée — la fenêtre de 14 jours sert simplement à générer un ensemble varié et représentatif de ces exemples, couvrant plusieurs cycles de semaine et de week-end.

Ces deux fenêtres répondent à des besoins différents. La **fenêtre de prédiction de 48 heures** donne au modèle le contexte actuel au moment de l'inférence — ce qui s'est réellement passé dans votre logement au cours des deux derniers jours. La **fenêtre d'entraînement de 14 jours** garantit que le modèle a observé suffisamment des rythmes hebdomadaires de votre logement pour comprendre ce qui est habituel, sans être ancré dans des comportements plus anciens qui ne refléteraient plus votre routine actuelle.

La base de données conserve les événements de capteurs pendant 30 jours, puis les supprime. Aucune donnée brute d'événement n'est conservée au-delà de cette période de rétention. (Cette période de rétention de 30 jours est distincte de la fenêtre d'entraînement de 14 jours, et plus longue qu'elle — elle existe pour que la fenêtre d'entraînement puisse regarder en arrière en toute sécurité, sans jamais se heurter à la limite de ce qui est réellement stocké.)

---

## 5. Réutilisation des données et continuité

**Les données sont accumulées, et non supprimées entre les cycles d'entraînement.** Au sein de la fenêtre d'entraînement de 14 jours, les mêmes événements de capteurs peuvent être utilisés lors de plusieurs entraînements nocturnes successifs. Avant chaque exécution, le service rend à nouveau accessible l'intégralité des deux semaines d'événements disponibles — ce qui signifie que chaque entraînement bénéficie de l'historique récent complet, et non uniquement des événements survenus depuis l'exécution précédente.

**Quand le réentraînement a lieu.** Le réentraînement est déclenché selon un calendrier nocturne, pendant la plage calme entre 23h et 2h. Il peut également être déclenché par des événements significatifs, comme lorsque le système passe de l'étape « apprentissage » à l'étape « maturité ». Le processus est entièrement automatique et ne nécessite aucune action de votre part.

**Mode Vacances — suppression des alertes d'absence (manuel).** Si vous savez que vous allez vous absenter, vous pouvez activer le Mode Vacances à l'aide du bouton prévu dans l'interface de suivi. Il s'agit spécifiquement d'un **interrupteur de suppression des alertes d'absence**. Une fois activé, il indique au détecteur d'anomalies qu'une période prolongée de silence dans votre logement est attendue, ce qui évite les fausses notifications de sécurité pendant une absence planifiée.

Le Mode Vacances ne met **pas** en pause l'entraînement du modèle, n'interrompt pas la collecte de données et n'affecte en rien les prédictions. Le service continue d'enregistrer les événements de capteurs et d'exécuter son entraînement nocturne programmé normalement pendant votre absence. Le Mode Vacances doit être activé avant votre départ et désactivé à votre retour.

**Protection de la qualité de l'entraînement lors de journées inhabituelles (automatique).** Indépendamment du Mode Vacances, le pipeline d'entraînement comprend un mécanisme automatique qui détecte lorsque les données du moment risqueraient d'enseigner au modèle des habitudes erronées — par exemple, lors d'un jour férié non déclaré, d'une période de maladie, ou de toute période où l'activité est anormalement basse par rapport à votre routine habituelle pour ce jour de la semaine.

Avant chaque entraînement, le système compare le niveau d'activité global de la journée à une référence statistique construite à partir du même jour de la semaine au cours des semaines précédentes. Si le niveau d'activité est significativement inférieur à la normale pendant trois jours consécutifs ou plus, le système conclut que la période est anormale et soit ignore entièrement l'entraînement (une fois le modèle mature), soit intègre des données de référence synthétiques pour compenser (pendant la phase d'apprentissage). Cela protège le modèle contre l'apprentissage erroné selon lequel un logement vide serait la norme.

Ce mécanisme est entièrement distinct du Mode Vacances. Il fonctionne au sein du pipeline d'entraînement et ne tient compte d'aucune information issue du paramètre Mode Vacances. Activer le Mode Vacances ne le déclenche pas, et il n'a aucun effet sur les alertes d'anomalie. Les deux systèmes répondent à des problèmes différents :

- Le **Mode Vacances** est une commande manuelle que vous activez pour faire taire les alertes de sécurité lorsque vous savez que vous partez.
- La **détection automatique des jours inhabituels** est une protection en arrière-plan qui préserve la précision à long terme du modèle lorsqu'une absence inhabituelle est détectée, sans nécessiter aucune intervention manuelle.

**Cohérence du modèle.** Le modèle de prédiction et ses composants de prévision associés (un par horizon temporel) sont toujours maintenus synchronisés. Si un entraînement produit un modèle qui échoue aux contrôles qualité, le modèle précédent reste en service. Si une mise à jour réussie est ultérieurement annulée pour quelque raison que ce soit, les composants de prévision sont annulés avec elle.

---

## 6. Confidentialité et périmètre des données

**Ce qui est collecté.** Le service collecte uniquement les événements de mouvement et de présence issus des capteurs que vous avez configurés. Chaque événement contient un identifiant de pièce, un horodatage et un état de présence (occupée ou inoccupée). Aucun son, aucune vidéo ni aucune information personnelle identifiable n'est collecté.

**Où les données sont stockées.** Toutes les données de capteurs, les modèles appris et la configuration sont stockés localement sur votre appareil Aragon Maestro. Rien n'est envoyé vers un serveur cloud ou un service externe. L'historique d'occupation de votre logement ne quitte jamais votre réseau domestique.

**Ce qui n'est pas collecté.**

- Le service n'enregistre pas l'identité précise de la personne présente dans une pièce — seulement le fait que la pièce est occupée.
- Il ne surveille ni le contenu des conversations, ni les activités, ni les comportements au-delà de la simple détection de présence.
- Il ne se connecte à aucune source de données tierce.

**Surveillance du maintien à domicile (si activée) — le principal cas d'usage du service (voir §1).** Dans les logements où la fonctionnalité de sécurité pour le maintien à domicile est active, le service surveille en continu les périodes d'inactivité anormalement longues dans l'ensemble des pièces configurées. Ce qui est considéré comme « anormal » n'est pas une durée fixe — cela s'appuie sur le même modèle d'occupation appris décrit tout au long de ce document, ce qui permet au système de distinguer un silence réellement préoccupant d'un simple après-midi calme.

Si un silence prolongé est détecté — c'est-à-dire qu'aucune activité de capteur n'a été enregistrée au-delà de la durée attendue — une alerte est émise. Avant d'émettre une alerte, le service vérifie si une *autre* pièce surveillée a montré une activité récente réelle ; si c'est le cas, l'alerte est retenue, car le fait qu'une personne soit active ailleurs dans le logement constitue en soi une preuve de sa sécurité, même si une pièce en particulier est restée calme. Cela réduit les fausses alertes — par exemple, une pièce dont l'unique capteur est un interrupteur d'éclairage simplement non utilisé pendant les heures de jour ne déclenchera pas à elle seule une fausse alerte de sécurité.

Cette fonctionnalité peut être activée, désactivée ou mise en pause à tout moment depuis les paramètres de la plateforme, et — comme le reste de ce document — fonctionne entièrement à partir de données stockées localement, chaque alerte étant générée sur l'appareil lui-même.

---

*Version du document 1.1 — Corrections apportées : suppression de l'affirmation sur les relations entre pièces ; clarification et distinction entre la fenêtre de prédiction et la fenêtre d'entraînement ; portée du Mode Vacances corrigée pour se limiter à la suppression des alertes ; détection automatique des jours inhabituels distinguée explicitement du Mode Vacances manuel, avec comparaison.*

*Version du document 1.2 — Corrections apportées : période de rétention des données brutes corrigée de 7 à 30 jours ; fenêtre de réutilisation pour l'entraînement corrigée de 7 à 14 jours (et clairement distinguée de la période de rétention, désormais différente, alors qu'il s'agissait auparavant du même nombre) ; suppression de l'horizon de prédiction à 2 heures, qui n'est plus produit par le système (les horizons à 15 minutes et à 1 heure demeurent).*

*Version du document 1.3 — Corrections apportées : l'affirmation du §2 sur « l'absence de relation entre les pièces » a été restreinte aux seules prédictions d'occupation, la fonctionnalité de sécurité pour le maintien à domicile croisant désormais les pièces entre elles (voir ci-dessous) ; le §6 a été mis à jour pour décrire cette vérification croisée et sa raison d'être (réduction des fausses alertes de sécurité). Ajout d'un exemple concret au §3 illustrant ce qui change à chaque étape d'apprentissage, et atténuation d'une affirmation antérieure précisant un délai d'adaptation de « 1 à 2 semaines », afin de ne pas revendiquer une précision qui n'a pas été réellement vérifiée.*

*Version du document 1.4 — Corrections apportées : le maintien à domicile / l'accompagnement des personnes âgées a été explicitement élevé au rang de principal cas d'usage (introduction du §1, question d'exemple en tête de liste, et traitement étendu au §6 avec renvoi vers le §1), plutôt que d'être mentionné brièvement dans la section confidentialité. Ajout d'une explication vérifiée au §3 concernant une distinction architecturale réelle : un modèle de base partagé est réentraîné depuis zéro chaque nuit (fenêtre de 14 jours à chaque fois), tandis que trois composants de prévision spécialisés accumulent l'apprentissage de façon incrémentale nuit après nuit, pour les prédictions « maintenant », « 15 minutes » et « 1 heure ». (Une version précédente de ce paragraphe laissait entendre à tort que la prédiction « maintenant » utilisait directement le modèle de base réentraîné depuis zéro ; corrigé après vérification auprès du code source, qui montre qu'elle est produite par l'un des trois composants à apprentissage cumulatif, au même titre que les prédictions à 15 minutes et à 1 heure.) Le §2 a été mis à jour en conséquence pour mentionner explicitement « maintenant » aux côtés des deux horizons de prévision, plutôt que de ne décrire que des prédictions tournées vers l'avenir. Remplacement de « votre passerelle domestique » par « votre appareil Aragon Maestro » pour l'exactitude du nom de produit.*

*Version du document 1.5 — Ajout : une phrase ajoutée au §3 donnant une explication brève et générale de la raison de cette répartition entre réinitialisation et apprentissage cumulatif, afin que la distinction ne paraisse pas arbitraire. Volontairement concise et dépourvue de détails d'implémentation, conformément au périmètre de ce document — voir la documentation technique pour l'explication technique complète.*
