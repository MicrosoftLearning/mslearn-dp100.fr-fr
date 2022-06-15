---
lab:
  title: Création d’un espace de travail Microsoft Azure Machine Learning
ms.openlocfilehash: c2f33973281221da1a4871f19fbaff8f127d028f
ms.sourcegitcommit: 8601551af6c32a4c75fd9ecffce750583c2ab4b8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/01/2022
ms.locfileid: "145195789"
---
# <a name="create-and-explore-an-azure-machine-learning-workspace"></a>Créer et explorer un espace de travail Azure Machine Learning

Dans cet exercice, vous allez créer et explorer un espace de travail Azure Machine Learning.

## <a name="create-an-azure-machine-learning-workspace"></a>Création d’un espace de travail Microsoft Azure Machine Learning

Comme son nom l’indique, un espace de travail est un emplacement centralisé pour gérer toutes les ressources Azure ML dont vous avez besoin pour travailler sur un projet Machine Learning.

1. Dans le [portail Azure](https://portal.azure.com), créez une ressource **Machine Learning**, en spécifiant les paramètres suivants :

    - **Abonnement** : *votre abonnement Azure*
    - **Groupe de ressources** : *créez ou sélectionnez un groupe de ressources*
    - **Nom de l’espace de travail** : *entrez un nom unique pour votre espace de travail*
    - **Région** : *sélectionnez la région géographique la plus proche de vous*
    - **Compte de stockage** : *notez le nouveau compte de stockage par défaut à créer pour votre espace de travail*
    - **Coffre de clés** : *notez le nouveau coffre de clés par défaut à créer pour votre espace de travail*
    - **Application Insights** : *notez la nouvelle ressource Application Insights par défaut à créer pour votre espace de travail*
    - **Registre de conteneurs** : aucun (*un registre est créé automatiquement la première fois que vous déployez un modèle sur un conteneur*)

    > **Remarque** : Lorsque vous créez un espace de travail Azure Machine Learning, vous pouvez utiliser des options avancées pour restreindre l’accès via un *point de terminaison privé*, et spécifier des clés personnalisées pour le chiffrement des données. Nous n’utiliserons pas ces options dans cet exercice mais vous devriez avoir conscience de leur existence.

2. Une fois l’espace de travail et ses ressources associées créés, affichez l’espace de travail dans le portail.

## <a name="explore-azure-machine-learning-studio"></a>Explorer Azure Machine Learning Studio

Vous pouvez gérer certaines ressources de l’espace de travail dans le portail Azure mais, pour les scientifiques des données, cet outil contient beaucoup d’informations et de liens non pertinents liés à la gestion des ressources générales d’Azure. *Azure Machine Learning studio* fournit un portail web dédié pour travailler avec votre espace de travail.

1. Dans le volet Portail Azure de votre espace de travail Azure Machine Learning, cliquez sur le lien pour lancer studio. Ou bien, dans un nouvel onglet de navigateur, ouvrez [https://ml.azure.com](https://ml.azure.com). Si vous y êtes invité, connectez-vous en utilisant le compte Microsoft que vous avez utilisé dans la tâche précédente, puis sélectionnez votre abonnement et votre espace de travail Azure.

    > **Astuce** Si vous avez plusieurs abonnements Azure, vous devez choisir le *répertoire* Azure dans lequel l’abonnement est défini, puis l’abonnement, et enfin l’espace de travail.

2. Affichez l’interface Azure Machine Learning studio pour votre espace de travail, via la quelle vous pouvez gérer toutes les ressources de votre espace de travail.
3. Dans Azure Machine Learning studio, utilisez l’icône &#9776; en haut à gauche pour afficher et masquer les différentes pages de l’interface. Vous pouvez utiliser ces pages pour gérer les ressources dans votre espace de travail.

## <a name="create-a-compute-instance"></a>Créer une instance de calcul

L’un des avantages d’Azure Machine Learning est la possibilité de créer une ressource de calcul basée sur le cloud, sur laquelle vous pouvez exécuter des expériences et des scripts d’apprentissage à grande échelle.

1. Dans Azure Machine Learning studio, affichez la page **Calcul**. C’est ici que vous allez gérer les cibles de calcul pour vos activités de science des données. Vous pouvez créer quatre sortes de ressources de calcul :
    - **Instances de calcul** : stations de travail de développement sur lesquelles les scientifiques des données peuvent travailler avec des données et des modèles.
    - **Clusters de calcul** : clusters scalables de machines virtuelles pour le traitement à la demande du code d’expérimentation.
    - **Clusters d’inférence** : cibles de déploiement pour les services prédictifs qui utilisent vos modèles entraînés.
    - **Capacité de calcul attachée** : liens vers d’autres ressources de calcul Azure, telles que des machines virtuelles ou des clusters Azure Databricks.

    Pour cet exercice, vous allez créer une instance de calcul afin de pouvoir exécuter du code dans votre espace de travail.

2. Sous l’onglet **Instances de calcul**, ajoutez une nouvelle instance de calcul avec les paramètres suivants. Vous allez l’utiliser comme station de travail pour exécuter du code dans les notebooks.
    - **Nom de la capacité de calcul** : *entrez un nom unique*
    - **Emplacement** : *Même emplacement que votre espace de travail*
    - **Type de machine virtuelle** : Processeur
    - **Taille de machine virtuelle** : Standard_DS11_v2
    - **Nombre total de quotas disponibles** : affiche les cœurs dédiés disponibles.
    - **Afficher les paramètres avancés** : notez les paramètres suivants, mais ne les sélectionnez pas : 
        - **Activer l’accès SSH** : désélectionnez *(vous pouvez utiliser cette option pour permettre un accès direct à la machine virtuelle en utilisant un client SSH)*
        - **Activer un réseau virtuel** : désélectionnez *(vous utiliserez généralement cette option dans un environnement d’entreprise pour améliorer la sécurité réseau)*
        - **Affecter à un autre utilisateur** : désélectionnez *(vous pouvez utiliser cette option pour affecter une instance de calcul à un scientifique des données)* 3.Attendez que l’instance de calcul démarre et qu’elle soit à l’état **En cours d’exécution**.

> **Remarque** : Les instances de calcul et les clusters sont basés sur des images de machines virtuelles Azure standard. Pour cet exercice, l’image *Standard_DS11_v2* est recommandée pour obtenir un équilibre optimal entre coûts et performances. Si votre abonnement s’accompagne d’un quota qui ne couvre pas cette image, choisissez-en une autre. Gardez cependant à l’esprit qu’une image plus grande peut entraîner des coûts plus élevés, tandis qu’une plus petite risque de ne pas suffire pour effectuer les tâches. Vous pouvez également demander à votre administrateur Azure d’étendre votre quota.

## <a name="clone-and-run-a-notebook"></a>Cloner et exécuter un notebook

Une grande partie de l’expérimentation de science des données et d’apprentissage automatique est réalisée en exécutant du code dans des *notebooks*. Votre instance de calcul inclut des environnements de notebook Python complets (*Jupyter* et *JuypyterLab*) que vous pouvez utiliser pour accomplir un travail approfondi. Toutefois, pour effectuer une édition de notebook de base, vous pouvez utiliser la page **Notebooks** intégrée dans Azure Machine Learning Studio.

1. Dans Azure Machine Learning studio, affichez la page **Notebooks**.
2. Si un message décrivant de nouvelles fonctionnalités s’affiche, fermez-le.
3. Sélectionnez **Terminal** ou l’icône **Ouvrir un terminal** pour ouvrir un terminal, puis vérifiez que son **Calcul** est défini sur votre instance de calcul et que le chemin d’accès actuel est le dossier **/utilisateurs/votre-nom-utilisateur** .
4. Entrez la commande suivante pour cloner un dépôt Git contenant des notebooks, des données et d’autres fichiers pour votre espace de travail :

    ```bash
    git clone https://github.com/MicrosoftLearning/mslearn-dp100 mslearn-dp100
    ```

4. Une fois la commande exécutée, dans le volet **Fichiers**, cliquez sur **&#8635;** pour actualiser l’affichage et vérifier qu’un nouveau dossier **/ustilisateurs/*votre-nom-utilisateur*/mslearn-dp100** a été créé. Ce dossier contient plusieurs fichiers de notebook **.ipynb**.
5. Fermez le volet du terminal, ce qui met fin à la session.
6. Dans le dossier **/utilisateurs/*votre-nom-utilisateur*/mslearn-dp100**, ouvrez le notebook **Prise en main avec notebooks**. Ensuite, lisez les notes et suivez les instructions qu’elles contiennent.

> **Conseil** : Pour exécuter une cellule de code, sélectionnez la cellule que vous souhaitez exécuter, puis utilisez le bouton **&#9655;** pour l’exécuter. 

> **Vous découvrez Python ?** Utilisez l’[aide-mémoire Python](cheat-sheets/dp100-cheat-sheet-python.pdf) pour comprendre le code.

> **Vous découvrez le Machine Learning ?** Consultez la [vue d’ensemble du Machine Learning](cheat-sheets/dp100-cheat-sheet-machine-learning.pdf) pour obtenir un aperçu simplifié du processus d’apprentissage automatique dans Azure Machine Learning.

## <a name="stop-your-compute-instance"></a>Arrêter votre instance de calcul

Si vous avez fini d’explorer Azure Machine Learning pour l’instant, vous devriez arrêter votre instance de calcul pour éviter des frais inutiles dans votre abonnement Azure.

1. Dans Azure Machine Learning Studio, sur la page **Calcul**, sélectionnez votre instance de calcul.
2. Cliquez sur **Arrêter** pour arrêter votre instance de calcul. Quand celle-ci est arrêtée, son état passe à **Arrêtée**.

> **Remarque** : Ainsi, les ressources de calcul ne seront pas facturées dans votre abonnement. Une petite quantité de stockage de données vous est cependant facturée tant que l’espace de travail Azure Machine Learning existe dans votre abonnement. Si vous avez terminé l’exploration d’Azure Machine Learning, vous pouvez supprimer l’espace de travail Azure Machine Learning et les ressources associées. Toutefois, si vous envisagez d’effectuer d’autres labos de cette série, vous devrez répéter ce labo pour créer l’espace de travail et préparer l’environnement en premier.
