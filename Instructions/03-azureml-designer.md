---
lab:
  title: Utiliser le Concepteur Azure Machine Learning
ms.openlocfilehash: ce03f54e5762e66363608b88fd86ec1de5795a33
ms.sourcegitcommit: 48bc4227570b0817702d195aa06fa4dabe1bbdd7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/29/2022
ms.locfileid: "146733074"
---
# <a name="use-azure-machine-learning-designer"></a>Utiliser le Concepteur Azure Machine Learning

Le *concepteur* Azure Machine Learning fournit un environnement de glisser-déposer dans lequel vous pouvez définir un workflow, ou *pipeline*, de composants d’ingestion de données, de transformation et d’entraînement de modèles pour créer un modèle Machine Learning. Vous pouvez ensuite publier ce pipeline en tant que service web que des applications clientes peuvent utiliser à des fins d’*inférence* (génération de prédictions à partir de nouvelles données).

## <a name="before-you-start"></a>Avant de commencer

Si ce n’est déjà fait, effectuez l’exercice *[Créer un espace de travail Azure Machine Learning](01-create-a-workspace.md)* pour créer un espace de travail et une instance de calcul Azure Machine Learning, et cloner les notebooks requis.

## <a name="configure-compute-resources"></a>Configurer des ressources de calcul

Pour utiliser le concepteur Azure Machine Learning, vous avez besoin d’une instance de calcul sur laquelle exécuter l’expérience d’apprentissage du modèle.

1. Connectez-vous à [Azure Machine Learning studio](https://ml.azure.com?azure-portal=true) en utilisant les informations d’identification Microsoft associées à votre abonnement Azure, puis sélectionnez votre espace de travail Azure Machine Learning.
2. Dans Azure Machine Learning studio, affichez la page **Calcul**, puis, sous l’onglet **Instances de calcul**, démarrez votre instance de calcul si elle n’est pas déjà en cours d’exécution. Vous allez utiliser cette instance de calcul pour tester votre modèle formé.
3. Pendant que l’instance de calcul démarre, passez à l’onglet **Clusters de calcul**. Si vous n’avez pas encore de cluster de calcul existant, ajoutez-en un nouveau avec les paramètres suivants. Vous allez utiliser ce cluster pour exécuter le pipeline d’apprentissage.
    - **Région** : *Même région que votre espace de travail*
    - **Priorité de machine virtuelle** : Dédiée
    - **Type de machine virtuelle** : Processeur
    - **Taille de machine virtuelle** : Standard_DS11_v2
    - **Nom de la capacité de calcul** : *entrez un nom unique*
    - **Nombre minimal de nœuds** : 0
    - **Nombre maximal de nœuds** : 2
    - **Secondes d’inactivité avant le scale-down** : 120
    - **Activer l’accès SSH** : non sélectionné

## <a name="review-the-training-dataset"></a>Réviser le jeu de données d’apprentissage

Maintenant que vous disposez de ressources de calcul que vous pouvez utiliser pour exécuter un pipeline d’apprentissage, vous allez avoir besoin de données pour effectuer l’apprentissage du modèle.

1. Dans Azure Machine Learning Studio, affichez la page **Données**. Les jeux de données représentent les tables ou fichiers de données spécifiques que vous comptez utiliser dans Azure Machine Learning.
2. Si vous avez créé précédemment le jeu de données **diabetes dataset**, ouvrez-le. Autrement, créez un jeu de données à partir de fichiers web, en utilisant les paramètres suivants, puis ouvrez-le :
    * **Informations de base** :
        * **URL web** : 
        * **Nom** : diabetes dataset
        * **Type de jeu de données** : Tabulaire
        * **Description** : Données sur le diabète
    * **Paramètres et aperçu** :
        * **Format de fichier** : Délimité
        * **Délimiteur** : Virgule
        * **Encodage** : UTF-8
        * **En-têtes de colonnes** : Utiliser les en-têtes du premier fichier
        * **Ignorer les lignes** : Aucune
    * **Schéma** :
        * Inclure toutes les colonnes autres que **Chemin**
        * Examiner les types détectés automatiquement
    * **Confirmer les détails** :
        * Ne pas profiler ce jeu de données après sa création

4. Affichez la page **Explorer** pour voir un échantillon des données. Ces données sont les détails de patients qui ont été testés pour le diabète. Vous allez les utiliser pour effectuer l’apprentissage d’un modèle qui prédit la probabilité qu’un patient soit testé positif au diabète en fonction des mesures cliniques.

## <a name="create-a-designer-pipeline"></a>Créer un pipeline de concepteur

Pour commencer à utiliser le concepteur, vous devez créer un pipeline et ajouter le jeu de données que vous souhaitez utiliser.

1. Dans Azure Machine Learning Studio, affichez la page **Concepteur** et créez un pipeline.
2. Remplacez le nom du pipeline par défaut (**Pipeline-Created-on-* date***) par **Visual Diabetes Training** en cliquant sur l’icône **&#9881;** à droite pour ouvrir le volet **Paramètres**.
3. Notez que vous devez spécifier une cible de calcul sur laquelle exécuter le pipeline. Dans le volet **Paramètres**, cliquez sur **Sélectionner le type de calcul**, choisissez Cluster de calcul, cliquez sur Sélectionner un cluster de calcul Azure ML, choisissez votre cluster de calcul, puis fermez le volet Paramètres.
4. Sur le côté gauche du concepteur, sélectionnez l’onglet **Données**, puis faites glisser le jeu de données **diabetes dataset** dans le canevas.
5. Sélectionnez le composant **diabetes dataset** sur le canevas. Cliquez dessus avec le bouton droit, puis sélectionnez **Aperçu des données**.
6. Dans le volet DataOutput, sélectionnez l’onglet **Profil**.
7. Examinez le schéma des données. Notez que vous pouvez voir les distributions des différentes colonnes sous forme d’histogrammes. Fermez ensuite la fenêtre de visualisation.

## <a name="add-transformations"></a>Ajouter des transformations

Avant de pouvoir entraîner un modèle, vous devez généralement appliquer certaines transformations de prétraitement aux données.

1. Dans le volet de gauche, sélectionnez l’onglet **Composant**, qui contient un large choix de composants que vous pouvez utiliser pour transformer des données avant l’entraînement du modèle. Vous pouvez rechercher des composants en haut du volet.
2. Recherchez le composant **Normaliser les données** et faites-le glisser dans le canevas, sous le composant **diabetes dataset**. Connectez ensuite la sortie du composant **diabetes dataset** à l’entrée du composant **Normaliser des données**.
3. Sélectionnez le composant **Normaliser les données** et observez ses paramètres ; vous pouvez voir que vous devez spécifier la méthode de transformation et les colonnes à transformer. Ensuite, en laissant la transformation comme **ZScore**, modifiez les colonnes pour inclure les noms de colonnes suivants :
    * PlasmaGlucose
    * DiastolicBloodPressure
    * TricepsThickness
    * SerumInsulin
    * BMI
    * DiabetesPedigree

    **Remarque** : Nous normalisons les colonnes numériques pour les mettre à la même échelle et éviter que des colonnes contenant de grandes valeurs dominent l’apprentissage du modèle. Vous appliqueriez normalement une série complète de transformations de prétraitement comme celle-ci afin de préparer vos données pour l’apprentissage. Cependant, nous allons simplifier les choses dans cet exercice.

4. Nous sommes maintenant prêts à fractionner les données en jeux de données distincts pour l’apprentissage et la validation. Dans le volet de gauche, recherchez le composant **Découper les données** et faites-le glisser dans le canevas sous le composant **Normaliser les données**. Connectez ensuite la sortie *Jeu de données transformées* (à gauche) du composant **Normaliser les données** à l’entrée du composant **Découper les données**.
5. Sélectionnez le composant **Découper les données** et configurez ses paramètres comme suit :
    * **Splitting mode** : Split Rows
    * **Fraction of rows in the first output dataset** : 0.7
    * **Random seed** : 123
    * **Stratified split** : False

## <a name="add-model-training-components"></a>Ajouter des composants d’entraînement de modèle

Une fois les données préparées et découpées en jeux de données d’entraînement et de validation, vous êtes prêt à configurer le pipeline pour entraîner et évaluer un modèle.

1. Recherchez le composant **Entraîner le modèle** et faites-le glisser dans le canevas, sous le composant **Découper les données**. Connectez ensuite la sortie *Jeu de données1 de résultat* (à gauche) du composant **Découper les données** à l’entrée *Jeu de données* (à droite) du composant **Entraîner le modèle**.
2. Le modèle que nous entraînons prédira la valeur **Diabetic**, donc sélectionnez le composant **Entraîner le modèle** et modifiez ses paramètres pour définir la valeur **Étiqueter la colonne** sur **Diabetic** (en respectant exactement la casse et l’orthographe).
3. L’étiquette **Diabétique** que le modèle va prédire étant une colonne binaire (1 pour les patients atteints de diabète, 0 pour les patients non atteints), nous devons effectuer l’apprentissage du modèle à l’aide d’un algorithme de *classification*. Recherchez le composant **Régression logistique à deux classes** et faites glisser-le dans le canevas, à gauche du composant **Découper les données** et au-dessus du composant **Entraîner le modèle**. Ensuite, connectez sa sortie à l’entrée **Modèle non entraîné** (à gauche) du composant **Entraîner le modèle**.
4. Pour tester le modèle formé, nous devons l’utiliser pour déterminer le scoring du jeu de données de validation que nous avons retenu au moment du fractionnement des données d’origine. Recherchez le composant **Scorer le modèle** et faites-le glisser-le dans le canevas, sous le composant **Entraîner le modèle**. Ensuite, connectez la sortie du composant **Entraîner le modèle** à l’entrée **Modèle entraîné** (à gauche) du composant **Scorer le modèle** et faites glisser la sortie **Jeu de données2 de résultat** (à droite) du composant **Découper les données** vers l’entrée **Jeu de données** (à droite) du composant **Scorer le modèle**.
5. Pour évaluer les performances du modèle, nous devons examiner certaines métriques générées par le scoring du jeu de données de validation. Recherchez le composant **Évaluer le modèle** et faites-le glisser dans le canevas, sous le composant **Scorer le modèle**, puis connectez la sortie du composant **Scorer le modèle** à l’entrée **Scorer le jeu de données** (à gauche) du composant **Évaluer le modèle**.

## <a name="run-the-training-pipeline"></a>Exécuter le pipeline d’entraînement

Avec les étapes de flux de données définies, vous êtes maintenant prêt à exécuter le pipeline d’apprentissage et à effectuer l’apprentissage du modèle.

1. Vérifiez que votre pipeline ressemble à ce qui suit :

    ![Pipeline d’apprentissage visuel](images/visual-training.jpg)

2. En haut à droite, cliquez sur **Envoyer**. Ensuite, lorsque vous y êtes invité, créez une expérience nommée **mslearn-designer-train-diabetes**, puis exécutez-la.  Cela aura pour effet d’initialiser le cluster de calcul, puis d’exécuter le pipeline, ce qui peut prendre 10 minutes voire plus. Vous pouvez voir l’état de l’exécution du pipeline en haut à droite du canevas de conception.

    > **Conseil** : si une erreur **GraphDatasetNotFound** se produit, sélectionnez le jeu de données et, dans son volet Propriétés, modifiez la **version** (vous pouvez basculer entre « Toujours utiliser la dernière version » et « 1 »), puis réexécutez le pipeline.
    >
    > Bien qu’il soit en cours d’exécution, vous pouvez afficher le pipeline et l’expérience créés dans les pages **Pipelines** et **Expériences.** Revenez au pipeline **Visual Diabetes Training** sur la page **Concepteur** lorsque vous avez terminé.

3. Une fois le composant **Normaliser les données** terminé, sélectionnez-le, puis, dans le volet **Paramètres**, sous l’onglet **Sorties + journaux**, sous **Sorties de données** dans la section **Jeu de données transformé**, cliquez sur l’icône **Aperçu des données**. Notez que vous pouvez voir les statistiques et les visualisations de distribution pour les colonnes transformées.
4. Fermez les visualisations **Normaliser les données** et attendez que le reste des composants se termine. Visualisez ensuite la sortie du composant **Évaluer le modèle** afin de voir les métriques de performances pour le modèle.

    **Remarque** : La performance de ce modèle n’est pas excellente, notamment parce que nous n’avons effectué qu’une caractérisation et un prétraitement minimes. Vous pouvez essayer différents algorithmes de classification et comparer les résultats (vous pouvez connecter les sorties du composant **Découper les données** aux composants **Entraîner le modèle** et **Scorer le modèle**, et vous pouvez connecter un deuxième modèle scoré au composant **Évaluer le modèle** pour voir une comparaison côte à côte). L’objectif de l’exercice est simplement de vous faire découvrir l’interface du concepteur, pas d’entraîner un modèle parfait.

## <a name="create-an-inference-pipeline"></a>Créer un pipeline d'inférence

Maintenant que vous avez utilisé un *pipeline d’apprentissage* pour effectuer l’apprentissage d’un modèle, vous pouvez créer un *pipeline d’inférence* qui utilise le modèle formé pour prédire des étiquettes pour de nouvelles données.

1. Dans l’onglet **Travaux**, accédez au pipeline terminé. 
2. Sélectionnez **Créer un pipeline d’inférence**, puis cliquez sur le **pipeline d’inférence en temps réel**. Après quelques secondes, une nouvelle version de votre pipeline nommée **Visual Diabetes Training-real time inference** s’ouvre.
3. Renommez le nouveau pipeline **Predict Diabetes**, puis examinez-le. Notez que la transformation de normalisation et le modèle formé ont été encapsulés dans ce pipeline afin que les statistiques issues de vos données d’apprentissage soient utilisées pour normaliser les nouvelles valeurs de données et que le modèle formé soit utilisé pour le scoring des nouvelles données.
4. Notez que le pipeline d’inférence partant du principe que les nouvelles données correspondront au schéma des données d’apprentissage d’origine, le jeu de données **diabetes dataset** du pipeline d’apprentissage est inclus. Toutefois, ces données d’entrée incluent l’étiquette **Diabetic** prédite par le modèle. Or, il n’est pas intuitif de l’inclure dans les nouvelles données de patient pour lesquelles aucune prédiction de diabète n’a encore été effectuée.
5. Supprimez le jeu de données **diabetes dataset** du pipeline d’inférence et remplacez-le par un composant **Entrer des données manuellement**. En le connectant à la même entrée **jeu de données** du composant **Appliquer une transformation** que l’**Entrée du service web**. Modifiez ensuite les paramètres du composant **Entrer des données manuellement** afin d’utiliser l’entrée CSV suivante, qui inclut des valeurs de caractéristiques sans étiquettes pour trois nouvelles observations de patients :

```CSV
PatientID,Pregnancies,PlasmaGlucose,DiastolicBloodPressure,TricepsThickness,SerumInsulin,BMI,DiabetesPedigree,Age
1882185,9,104,51,7,24,27.36983156,1.350472047,43
1662484,6,73,61,35,24,18.74367404,1.074147566,75
1228510,4,115,50,29,243,34.69215364,0.741159926,59
```

5. Le pipeline d’inférence comprend le composant **Évaluer le modèle**, qui n’est pas utile pour la prédiction à partir de nouvelles données, donc supprimez-le.
6. La sortie du composant **Scorer le modèle** comprend toutes les caractéristiques d’entrée ainsi que l’étiquette prédite et le score de probabilité. Pour limiter la sortie uniquement à la prédiction et à la probabilité, supprimez la connexion entre le composant **Scorer le modèle** et la **Sortie du service web**, ajoutez un composant **Exécuter un script Python**. Connectez la sortie du composant **Scorer le modèle** à l’entrée **Jeu de données1** (la plus à gauche) du composant **Exécuter le script Python**, puis connectez la sortie du composant **Exécuter le script Python** à **Sortie du service web**. Modifiez ensuite les paramètres du composant **Exécuter le script Python** pour utiliser le code suivant (en remplaçant tout le code existant) :

```Python
import pandas as pd

def azureml_main(dataframe1 = None, dataframe2 = None):

    scored_results = dataframe1[['PatientID', 'Scored Labels', 'Scored Probabilities']]
    scored_results.rename(columns={'Scored Labels':'DiabetesPrediction',
                                    'Scored Probabilities':'Probability'},
                            inplace=True)
    return scored_results
```
> **Remarque** : Après avoir collé le code dans le composant **Exécuter le script Python**, vérifiez que le code ressemble au code ci-dessus. Les mises en retrait sont importantes dans Python et le composant échoue si elles ne sont pas copiées correctement. 

7. Vérifiez que votre pipeline ressemble à ce qui suit :

    ![Pipeline d’inférence visuelle](images/visual-inference.jpg)

9. Envoyez le pipeline en tant que nouvelle expérience nommée **mslearn-designer-predict-diabetes** sur le cluster de calcul que vous avez utilisé pour l’apprentissage. L’opération peut prendre du temps !

## <a name="deploy-the-inference-pipeline-as-a-web-service"></a>Déployer le pipeline d’inférence comme un service web

Vous disposez maintenant d’un pipeline d’inférence pour l’inférence en temps réel, que vous pouvez déployer en tant que service web utilisable par des applications clientes.

> **Remarque** : Dans cet exercice, vous déployez le service web sur une instance de conteneur Azure (ACI). Ce type de calcul, créé dynamiquement, est utile pour le développement et le test. Pour la production, vous devez créer un *cluster d’inférence* qui fournit un cluster Azure Kubernetes Service (AKS) offrant une meilleure scalabilité et une meilleure sécurité.

1. Si l’exécution du pipeline d’inférence **Prédire un diabète** est toujours en cours, attendez qu’elle se termine. Ensuite, visualisez la sortie **Jeu de données de résultat** du composant **Exécuter le script Python** pour voir les étiquettes prédites et les probabilités pour les trois observations de patients dans les données d’entrée.
2. En haut à droite, cliquez **Déployer** et déployez un nouveau point de terminaison en temps réel à l’aide des paramètres suivants :
    -  **Nom** : designer-predict-diabetes
    -  **Description** : Prédire un diabète.
    - **Type de capacité de calcul** : Instance de conteneur Azure
3. Attendez que le service web soit déployé, ce qui peut prendre plusieurs minutes. L’état du déploiement est indiqué en haut à gauche de l’interface du concepteur.

## <a name="test-the-web-service"></a>Test du service web

Vous pouvez maintenant tester votre service déployé à partir d’une application cliente. En l’occurrence, vous allez utiliser un notebook.

1. Dans la page **Points de terminaison**, ouvrez le point de terminaison en temps réel **designer-predict-diabetes**.
2. Lorsque le point de terminaison **designer-predict-diabetes** s’ouvre, sous l’onglet **Consommer**, notez les valeurs de **Point de terminaison REST** et de **Clé primaire**.
3. Avec la page **Consommer** du service **designer-predict-diabetes** ouverte dans votre navigateur, ouvrez un nouvel onglet de navigateur et ouvrez une deuxième instance d’Azure Machine Learning studio. Ensuite, sous le nouvel onglet, consultez la page **Notebooks**.
4. Dans la page **Notebooks**, sous **Mes fichiers**, accédez au dossier **/users/*votre-nom-utilisateur*/mslearn-dp100** où vous avez cloné le dépôt de notebooks, puis ouvrez le notebook **Obtenir une prédiction du concepteur**.
5. Une fois le notebook ouvert, vérifiez que l’instance de calcul que vous avez créée précédemment est sélectionnée dans la zone **Calcul**, et que son état est **En cours d’exécution**.
6. Dans le notebook, remplacez les espaces réservés **ENDPOINT** et **PRIMARY_KEY** par les valeurs correspondant à votre service, que vous pouvez copier à partir de l’onglet **Consommer** sur la page de votre point de terminaison.
7. Exécutez la cellule de code et affichez le résultat retourné par votre service web.

## <a name="clean-up"></a>Nettoyage

Le service web que vous avez créé est hébergé dans une *instance de conteneur Azure*. Si vous n’envisagez pas d’effectuer d’autres expériences avec celui-ci, vous devez supprimer le point de terminaison afin d’éviter une utilisation d’Azure non nécessaire. Vous devez également arrêter l’instance de calcul tant que vous n’en avez pas à nouveau besoin.

1. Dans Azure Machine Learning Studio, sous l’onglet **Points de terminaison**, sélectionnez le point de terminaison **designer-predict-diabetes**. Sélectionnez cliquez sur le bouton **Supprimer** (&#128465;) et confirmez que vous souhaitez supprimer le point de terminaison.
2. Si vous avez fini d’utiliser Azure Machine Learning pour l’instant, sous l’onglet **Instances de calcul**, sélectionnez votre instance de calcul, puis cliquez sur **Arrêter** pour l’arrêter.

> **Remarque** : Ainsi, les ressources de calcul ne seront pas facturées dans votre abonnement. Une petite quantité de stockage de données vous est cependant facturée tant que l’espace de travail Azure Machine Learning existe dans votre abonnement. Si vous avez terminé l’exploration d’Azure Machine Learning, vous pouvez supprimer l’espace de travail Azure Machine Learning et les ressources associées. Toutefois, si vous envisagez d’effectuer d’autres labos de cette série, vous devez répéter l’exercice *[Créer un espace de travail Azure Machine Learning](01-create-a-workspace.md)* pour créer l’espace de travail et préparer l’environnement en premier.
