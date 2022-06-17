---
lab:
  title: Utiliser le Machine learning automatisé
ms.openlocfilehash: 70580a25d4bcd3929697874650ea6865262871f4
ms.sourcegitcommit: d2354e40eec31c22eb09381c6a890311cccc30c9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/11/2022
ms.locfileid: "146266837"
---
# <a name="use-automated-machine-learning"></a>Utiliser le Machine learning automatisé

Azure Machine Learning intègre une fonctionnalité de *Machine Learning automatisé* qui tire parti de la scalabilité du calcul cloud pour essayer automatiquement plusieurs techniques de prétraitement et algorithmes d’entraînement de modèle en parallèle afin d’identifier le modèle Machine Learning supervisé le plus performant pour vos données.

Dans cet exercice, vous allez utiliser l’interface visuelle pour le Machine Learning automatisé dans Azure Machine Learning studio

> **Remarque** : Vous pouvez également utiliser le Machine Learning automatisé via le Kit de développement logiciel (SDK) Azure Machine Learning.

## <a name="before-you-start"></a>Avant de commencer

Si ce n’est déjà fait, effectuez l’exercice *[Créer un espace de travail Azure Machine Learning](01-create-a-workspace.md)* pour créer un espace de travail et une instance de calcul Azure Machine Learning, et cloner les notebooks requis.

## <a name="configure-compute-resources"></a>Configurer des ressources de calcul

Pour utiliser le Machine Learning automatisé, vous avez besoin d’une instance de calcul sur laquelle exécuter l’expérience d’apprentissage du modèle.

1. Connectez-vous à [Azure Machine Learning studio](https://ml.azure.com?azure-portal=true) en utilisant les informations d’identification Microsoft associées à votre abonnement Azure, puis sélectionnez votre espace de travail Azure Machine Learning.
2. Dans Azure Machine Learning studio, affichez la page **Calcul**, puis, sous l’onglet **Instances de calcul**, démarrez votre instance de calcul si elle n’est pas déjà en cours d’exécution. Vous allez utiliser cette instance de calcul pour tester votre modèle formé.
3. Pendant que votre instance de calcul démarre, basculez vers l’onglet **Clusters de calcul** et ajoutez un cluster de calcul avec les paramètres suivants. Vous allez exécuter l’expérience de Machine Learning automatisé sur ce cluster pour tirer parti de la possibilité de distribuer les exécutions d’apprentissage sur plusieurs nœuds de calcul :
    - **Emplacement** : *Même emplacement que votre espace de travail*
    - **Niveau de machine virtuelle** : Dédié
    - **Type de machine virtuelle** : Processeur
    - **Taille de machine virtuelle** : Standard_DS11_v2
    - **Nom de la capacité de calcul** : *entrez un nom unique*
    - **Nombre minimal de nœuds** : 0
    - **Nombre maximal de nœuds** : 2
    - **Secondes d’inactivité avant le scale-down** : 120
    - **Activer l’accès SSH** : non sélectionné

## <a name="create-a-dataset"></a>Créer un jeu de données

Maintenant que vous disposez de ressources de calcul pour traiter les données, vous allez avoir besoin d’un moyen de stocker et d’ingérer les données à traiter.

1. Affichez les données séparées par des virgules sur https://aka.ms/diabetes-data dans votre navigateur web. Enregistrez-les dans un fichier local nommé **diabète.csv** (peu importe où l’emplacement).
2. Dans Azure Machine Learning Studio, affichez la page **Jeux de données**. Les jeux de données représentent les tables ou fichiers de données spécifiques que vous comptez utiliser dans Azure Machine Learning.
3. Créez un nouveau jeu de données à partir de fichiers locaux, avec les paramètres suivants :
    * **Informations de base** :
        * **Nom** : jeu de données diabète
        * **Type de jeu de données** : Tabulaire
        * **Description** : Données sur le diabète
    * **Sélection d’un magasin de données et de fichiers** :
        * **Sélectionnez ou créez un magasin de données** : Le magasin de données actuellement sélectionné
        * **Sélectionnez des fichiers pour votre jeu de données** : Accédez au fichier **diabète.csv** que vous avez téléchargé.
        * **Chemin de chargement** : *Laissez la sélection par défaut*
        * **Ignorer la validation des données** : Non sélectionné
    * **Paramètres et aperçu** :
        * **Format de fichier** : Délimité
        * **Délimiteur** : Virgule
        * **Encodage** : UTF-8
        * **En-têtes de colonnes** : seul le premier fichier comporte des en-têtes
        * **Ignorer les lignes** : Aucune
    * **Schéma** :
        * Inclure toutes les colonnes autres que **Chemin**
        * Examiner les types détectés automatiquement
    * **Confirmer les détails** :
        * Ne pas profiler ce jeu de données après sa création
4. Après avoir créé le jeu de données, ouvrez-le, puis affichez la page **Explorer** pour voir un échantillon des données. Ces données sont les détails de patients qui ont été testés pour le diabète. Vous allez les utiliser pour effectuer l’apprentissage d’un modèle qui prédit la probabilité qu’un patient soit testé positif au diabète en fonction des mesures cliniques.

    > **Remarque** : Vous pouvez éventuellement générer un *profil* du jeu de données pour afficher davantage de détails statistiques.

## <a name="run-an-automated-machine-learning-experiment"></a>Exécuter une expérience de machine learning automatisé

Dans Azure Machine Learning, les opérations que vous exécutez sont appelées *expériences*. Suivez les étapes ci-dessous pour exécuter une expérience qui utilise le Machine Learning automatisé pour entraîner un modèle de régression qui prédit des diagnostics de diabète.

1. Dans Azure Machine Learning Studio, consultez la page **ML automatisé** (sous **Auteur**).
2. Créez une exécution de ML automatisé avec les paramètres suivants :
    - **Sélectionner le jeu de données** :
        - **Jeu de données** : jeu de données diabète
    - **Configurer l’exécution** :
        - **Nom de la nouvelle expérience** : mslearn-automl-diabetes
        - **Colonne cible** : Diabétique (*étiquette que le modèle sera entraîné à prédire)*
        - **Sélectionnez le type de calcul** : Cluster de calcul
        - **Sélectionnez le cluster de calcul Azure ML** : *cluster de calcul que vous avez créé précédemment*
    - **Type de tâche et paramètres** :
        - **Type de tâche** : Classification
        - Sélectionnez **Afficher des paramètres de configuration supplémentaires** pour ouvrir **Configurations supplémentaires** :
            - **Métrique principale** : Sélectionnez **AUC pondéré** *(plus sur cette métrique plus tard !)*
            - **Expliquer le meilleur modèle** : sélectionné - *cette option fait en sorte que le Machine Learning automatisé calcule l’importance des caractéristique pour le meilleur modèle, permettant ainsi de déterminer l’influence de chaque caractéristique sur l’étiquette prédite.*
            - **Utilisez tous les modèles pris en charge** : <u>Dé-</u>sélectionnez - nous allons restreindre l’expérience pour essayer quelques algorithmes spécifiques.
            - **Modèles autorisés** : Sélectionnez uniquement **LogisticRegression** et **RandomForest**. Il s’agira des seuls algorithmes essayés dans l’expérience.
            - **Critère de sortie** :
                - **Délai du travail d’entraînement (heures)** : 0,5 - *Cela provoque l’arrêt de l’expérience après un maximum de 30 minutes*.
                - **Seuil de score de métrique** : 0.90 - *cela a pour effet de mettre fin à l’expérience si un modèle obtient une métrique AUC pondérée de 90 % ou plus.*
        - Sélectionnez **Afficher les paramètres de caractérisation** pour ouvrir **Caractérisation** :
            - **Activer la caractérisation** : sélectionné - *cela force Azure Machine Learning à prétraiter automatiquement les caractéristiques avant l’entraînement.*
    - **Sélectionnez le types de validation et de test** :
        - **Type de validation** : Fractionnement apprentissage-validation
        - **Pourcentage de validation de données** : 30
        - **Jeu de données de test** : Aucun jeu de données de test requis

3. Lorsque vous avez terminé de soumettre les détails de l’exécution de ML automatisé, celle-ci démarre automatiquement. Vous pouvez observer l’état de l’exécution dans le volet **Propriétés**.
4. Lorsque l’état d’exécution passe à *En cours d’exécution*, affichez l’onglet **Modèles** et observez à mesure que chaque combinaison possible d’algorithme d’entraînement et d’étapes de prétraitement est tentée, et que les performances du modèle résultant sont évaluées. La page est automatiquement actualisée périodiquement, mais vous pouvez également sélectionner **&#8635; Actualiser**. Quelque dix minutes peuvent s’écouler avant que les modèles commencent à apparaître, car les nœuds de cluster doivent être initialisés avant que l’apprentissage puisse commencer. C’est peut-être le moment de faire une pause café !
5. Attendez la fin de l’expérience.

## <a name="review-the-best-model"></a>Examiner le meilleur modèle

Une fois l’expérience terminée, vous pouvez réviser le modèle le plus performant généré (notez que nous avons utilisé ici des critères de sortie pour arrêter l’expérience ; il se peut que le « meilleur » modèle trouvé par l’expérience ne soit pas le meilleur modèle possible, mais juste le meilleur trouvé compte tenu des contraintes de temps et de métrique de cet exercice).

1. Sous l’onglet **Vue d’ensemble** de l’exécution du Machine Learning automatisé, notez le résumé du meilleur modèle.
2. Sélectionnez le **Nom de l’algorithme** pour le meilleur modèle afin d’afficher l’exécution enfant qui l’a produit.

    Le meilleur modèle est identifié en fonction de la métrique d’évaluation que vous avez spécifiée (*AUC_Weighted*). Pour calculer cette métrique, le processus d’entraînement a utilisé certaines des données pour entraîner le modèle, et appliqué une technique appelée *validation croisée* afin de tester itérativement le modèle entraîné avec des données avec lesquelles il n’a pas été entraîné et de comparer la valeur prédite avec la valeur connue réelle. À partir de ces comparaisons, une *matrice de confusion* contenant les vrais positifs, les faux positifs, les vrais négatifs et les faux négatifs est mise en forme de tableau, et des métriques de classification supplémentaires sont calculées (incluant une courbe de l’opérateur de réception -ROC, Receiving Operator Curve- qui compare les taux de vrais positifs et de faux positifs). L’aire sous cette courbe (AUC) est une métrique courante utilisée pour évaluer les performances de classification.
3. En regard de la valeur *AUC_Weighted*, sélectionnez **Afficher toutes les autres métriques** pour voir les valeurs d’autres métriques d’évaluation possibles pour un modèle de classification.
4. Sélectionnez l’onglet **Métriques** et révisez les métriques de performances que vous pouvez afficher pour le modèle. Celles-ci incluent une visualisation **confusion_matrix** montrant la matrice de confusion pour le modèle validé, et une visualisation **accuracy_table** incluant de graphique ROC.
5. Sélectionnez l’onglet **Explications**, un **ID d’explication**, puis affichez la page **Agréger l’importance**. Cela montre dans quelle mesure chaque composant du jeu de données influence la prédiction de l’étiquette.

## <a name="deploy-a-predictive-service"></a>Déployer un service prédictif

Une fois que vous avez utilisé le Machine Learning automatisé pour entraîner certains modèles, vous pouvez déployer le modèle le plus performant en tant que service afin que des applications clientes puissent l’utiliser.

> **Remarque** : Dans Azure Machine Learning, vous pouvez déployer un service en tant qu’Azure Container Instances (ACI) ou dans un cluster Azure Kubernetes service (AKS). Pour les scénarios de production, un déploiement AKS est recommandé. Vous devez dans ce cas créer une cible de calcul de type *cluster d’inférence*. Dans cet exercice, vous allez utiliser un service ACI, cible de déploiement qui convient pour le test et qui ne demande pas de créer un cluster d’inférence.

1. Sélectionnez l’onglet **Vue d’ensemble** de l’exécution qui a produit le meilleur modèle.
2. À partir de l’option **Déployer**, utilisez le bouton **Déployer sur un service web** pour déployer le modèle avec les paramètres suivants :
    - **Nom** : auto-predict-diabetes
    - **Description** : Prédire un diabète
    - **Type de capacité de calcul** : Instance de conteneur Azure
    - **Activer l’authentification** : Sélectionné
    - **Utilisez des ressources de déploiement personnalisées** : non sélectionné
3. Attendez que le déploiement commence. Cette opération peut prendre quelques secondes. Ensuite, sous l’onglet **Modèle**, dans la section **Résumé du modèle**, observez l’**État du déploiement** pour le service **auto-predict-diabetes**, qui devrait être **En cours d’exécution**. Attendez que cet état passe à **Réussi**. Vous devrez peut-être sélectionner régulièrement **&#8635; Actualiser**.  **REMARQUE** Cela peut prendre un certain temps... Soyez patient !
4. Dans Azure Machine Learning Studio, affichez la page **Points de terminaison** et sélectionnez le point de terminaison en temps réel **auto-predict-diabetes**. Sélectionnez ensuite l’onglet **Consommer** et notez les informations suivantes. Vous avez besoin de ces informations pour vous connecter à votre service déployé à partir d’une application cliente.
    - Point de terminaison REST de votre service
    - Clé primaire de votre service
5. Notez que vous pouvez utiliser le lien &#10697; en regard de ces valeurs pour les copier dans le Presse-papiers.

## <a name="test-the-deployed-service"></a>Tester le service déployé

Maintenant que vous avez déployé un service, vous pouvez le tester à l’aide de code simple.

1. Avec la page **Consommer** du service **auto-predict-diabetes** ouverte dans votre navigateur, ouvrez un nouvel onglet de navigateur et une deuxième instance d’Azure Machine Learning Studio. Ensuite, sous le nouvel onglet, consultez la page **Notebooks**.
2. Dans la page **Notebooks**, sous **Mes fichiers**, accédez au dossier **/users/*your-user-name*/mslearn-dp100** où vous avez cloné le dépôt de notebooks, puis ouvrez le notebook **Get AutoML Prediction**.
3. Une fois le notebook ouvert, vérifiez que l’instance de calcul que vous avez créée précédemment est sélectionnée dans la zone **Calcul**, et que son état est **En cours d’exécution**.
4. Dans le notebook, remplacez les espaces réservés **ENDPOINT** et **PRIMARY_KEY** par les valeurs correspondant à votre service, que vous pouvez copier à partir de l’onglet **Consommer** sur la page de votre point de terminaison.
5. Exécutez la cellule de code et affichez le résultat retourné par votre service web.

## <a name="clean-up"></a>Nettoyage

Le service web que vous avez créé est hébergé dans une *instance de conteneur Azure*. Si vous n’envisagez pas d’effectuer d’autres expériences avec celui-ci, vous devez supprimer le point de terminaison afin d’éviter une utilisation d’Azure non nécessaire. Vous devez également arrêter l’instance de calcul tant que vous n’en avez pas à nouveau besoin.

1. Dans Azure Machine Learning Studio, sous l’onglet **Points de terminaison**, sélectionnez le point de terminaison **auto-predict-diabetes**. Sélectionnez ensuite **Supprimer** (&#128465;) et confirmez que vous souhaitez supprimer le point de terminaison.
2. Dans la page **Calcul**, sous l’onglet **Instances de calcul**, sélectionnez votre instance de calcul, puis sélectionnez **Arrêter**.

> **Remarque** : Ainsi, les ressources de calcul ne seront pas facturées dans votre abonnement. Une petite quantité de stockage de données vous est cependant facturée tant que l’espace de travail Azure Machine Learning existe dans votre abonnement. Si vous avez terminé l’exploration d’Azure Machine Learning, vous pouvez supprimer l’espace de travail Azure Machine Learning et les ressources associées. Toutefois, si vous envisagez d’effectuer d’autres labos de cette série, vous devez répéter l’exercice *[Créer un espace de travail Azure Machine Learning](01-create-a-workspace.md)* pour créer l’espace de travail et préparer l’environnement en premier.
