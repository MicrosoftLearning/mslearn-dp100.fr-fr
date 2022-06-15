---
lab:
  title: Exécuter des expériences
ms.openlocfilehash: 62b6b79c99142d4311c2d9c0db4b02cfd710feb6
ms.sourcegitcommit: 66d8872bc3d24c2121e225be132b56f4df7920ac
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/12/2022
ms.locfileid: "145195782"
---
# <a name="run-experiments"></a>Exécuter des expériences

Les expériences sont au cœur du travail d’un scientifique des données. Dans Azure Machine Learning, une *expérience* est utilisée pour exécuter un script ou un pipeline, et génère généralement des sorties et des métriques d’enregistrements. Dans cet exercice, vous allez utiliser le Kit de développement logiciel (SDK) Azure Machine Learning pour exécuter du code Python en guide d’expérience.

## <a name="before-you-start"></a>Avant de démarrer

Si ce n’est déjà fait, effectuez l’exercice *[Créer un espace de travail Azure Machine Learning](01-create-a-workspace.md)* pour créer un espace de travail et une instance de calcul Azure Machine Learning, et cloner les notebooks requis.

## <a name="open-jupyter"></a>Ouvrir Jupyter

Bien que vous puissiez utiliser la page **Notebooks** dans Azure Machine Learning studio pour exécuter des notebooks, il est souvent plus productif d’utiliser un environnement de développement de notebook plus complet comme *Jupyter*. Heureusement, votre instance de calcul Azure Machine Learning inclut une installation de Jupyter.

> **Conseil** : Jupyter Notebook est un outil open source couramment utilisé pour la science des données. Vous pouvez consulter la [documentation](https://jupyter-notebook.readthedocs.io/en/stable/notebook.html) si vous ne le connaissez pas cette bien .

1. Dans [Azure Machine Learning studio](https://ml.azure.com), affichez la page **Calcul** de votre espace de travail, puis, sous l’onglet **Instances de calcul**, démarrez votre instance de calcul si elle n’est pas déjà en cours d’exécution.
2. Une fois l’instance de calcul en cours d’exécution, cliquez sur le lien **Jupyter** pour ouvrir la page d’accueil Jupyter dans un nouvel onglet de navigateur. Veillez à ouvrir *Jupyter* et non *JupyterLab*.

> **Conseil** : Vous découvrez Python ? Utilisez l’[aide-mémoire Python](cheat-sheets/dp100-cheat-sheet-python.pdf) pour comprendre le code.

## <a name="verify-the-azure-machine-learning-sdk-is-installed"></a>Le Kit de développement logiciel (SDK) Azure Machine Learning est installé.

Le Kit de développement logiciel (SDK) Azure Machine Learning est installé par défaut sur votre instance de calcul. Pour vérifier l’installation, procédez comme suit.

1. Dans l’environnement de notebook Jupyter, créez **un terminal.** Cela aura pour effet d’ouvrir un nouvel onglet avec une interface de commande.
2. Entrez la commande suivante pour vérifier que le Kit de développement logiciel (SDK) Azure ML est installé :

    ```bash
    pip show azureml-sdk
    ```

    Notez la version du package du Kit de développement logiciel (SDK) installé.

3. Le package du Kit de développement logiciel (SDK) **azureml-sdk** fournit les principales les plus importantes nécessaires pour utiliser Azure Machine Learning. Toutefois, il existe des packages supplémentaires qui contiennent d’autres bibliothèques utiles non incluses dans le package du Kit de développement logiciel (SDK) principal. Utilisez la commande suivante pour vérifier que le package **azureml-widgets**, qui contient des bibliothèques permettant d’afficher les informations d’Azure Machine Learning dans les notebooks, est également installé :

    ```bash
    pip show azureml-widgets
    ```

4. Fermez l’onglet **Terminal** et revenez à l’onglet contenant la page d’accueil Jupyter.

> **Plus d’informations** : Pour plus de détails sur l’installation du Kit de développement logiciel (SDK) Azure ML et de ses composants facultatifs, consultez la [documentation du Kit de développement logiciel (SDK) Azure ML](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

## <a name="run-experiments-in-a-notebook"></a>Exécuter des expériences dans un notebook

Les expériences dans Azure Machine Learning doivent être lancées à partir d’une sorte de couche *de contrôle*, souvent un script ou un programme. Dans cet exercice, vous allez utiliser un notebook pour contrôler les expériences.

1. Dans la page d’accueil Jupyter, accédez au dossier **/users/*votre-nom-utilisateur*/mslearn-dp100** dans lequel vous avez cloné le dépôt de notebooks, puis ouvrez le notebook **Exécuter des expériences**.
2. Lisez ensuite les notes dans le notebook, en exécutant chaque cellule de code tour à tour.
3. Une fois que vous avez fini d’exécuter le code dans le notebook, dans le menu **Fichier**, cliquez sur **Fermer et arrêter** pour le fermer et arrêter son noyau Python. Fermez ensuite tous les onglets du navigateur Jupyter.

## <a name="clean-up"></a>Nettoyage

Si vous avez fini d’utiliser Azure Machine Learning pour l’instant, dans Azure Machine Learning studio, dans la page **Calcul**, sous l’onglet **Instances de calcul**, sélectionnez votre instance de calcul, puis cliquez sur **Arrêter** pour l’arrêter. Sinon, laissez-la s’exécuter pour le labo suivant.

> **Remarque** : Ainsi, les ressources de calcul ne seront pas facturées dans votre abonnement. Une petite quantité de stockage de données vous est cependant facturée tant que l’espace de travail Azure Machine Learning existe dans votre abonnement. Si vous avez terminé l’exploration d’Azure Machine Learning, vous pouvez supprimer l’espace de travail Azure Machine Learning et les ressources associées. Toutefois, si vous envisagez d’effectuer d’autres labos de cette série, vous devez répéter l’exercice *[Créer un espace de travail Azure Machine Learning](01-create-a-workspace.md)* pour créer l’espace de travail et préparer l’environnement en premier.