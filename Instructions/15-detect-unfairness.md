---
lab:
  title: Détecter et atténuer la partialité
ms.openlocfilehash: 5e04b41a984fa65b09d3d0a7f249641916438415
ms.sourcegitcommit: 18f734eeb1031a9cb69c3b294632efd2e69324ac
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2021
ms.locfileid: "145195761"
---
# <a name="detect-and-mitigate-unfairness"></a>Détecter et atténuer la partialité

Les modèles Machine Learning peuvent souvent comporter des biais non intentionnels qui se traduisent par un manque d’impartialité. Par exemple, un modèle Machine Learning qui prédit si un patient devrait ou non être testé pour le diabète peut faire cette prédiction plus précisément pour certains groupes d’âge que pour d’autres, avec pour résultat qu’une sous-section de la population de patients est soit privée de contrôles de santé préventifs appropriés, soit soumise à des tests cliniques superflus.

## <a name="before-you-start"></a>Avant de démarrer

Si ce n’est déjà fait, effectuez l’exercice *[Créer un espace de travail Azure Machine Learning](01-create-a-workspace.md)* pour créer un espace de travail et une instance de calcul Azure Machine Learning, et cloner les notebooks requis.

## <a name="open-jupyter"></a>Ouvrir Jupyter

Bien que vous puissiez utiliser la page **Notebooks** dans Azure Machine Learning studio pour exécuter des notebooks, il est souvent plus productif d’utiliser un environnement de développement de notebook plus complet comme *Jupyter*.

1. Dans [Azure Machine Learning studio](https://ml.azure.com), affichez la page **Calcul** de votre espace de travail, puis, sous l’onglet **Instances de calcul**, démarrez votre instance de calcul si elle n’est pas déjà en cours d’exécution.
2. Une fois l’instance de calcul en cours d’exécution, cliquez sur le lien **Jupyter** pour ouvrir la page d’accueil Jupyter dans un nouvel onglet de navigateur.

## <a name="use-fairlearn-and-azure-machine-learning-to-detect-unfairness"></a>Utiliser Fairlearn et Azure Machine Learning pour détecter la partialité

Dans cet exercice, le code permettant d’évaluer les modèles pour l’équité est fourni dans un notebook.

1. Dans la page d’accueil Jupyter, accédez au dossier **/users/*votre-nom-utilisateur*/mslearn-dp100** dans lequel vous avez cloné le dépôt de notebooks, puis ouvrez le notebook **Détecter la partialité**.
2. Lisez ensuite les notes dans le notebook, en exécutant chaque cellule de code tour à tour.
3. Une fois que vous avez fini d’exécuter le code dans le notebook, dans le menu **Fichier**, cliquez sur **Fermer et arrêter** pour le fermer et arrêter son noyau Python. Fermez ensuite tous les onglets du navigateur Jupyter.

## <a name="clean-up"></a>Nettoyage

Si vous avez fini d’utiliser Azure Machine Learning pour l’instant, dans Azure Machine Learning studio, dans la page **Calcul**, sous l’onglet **Instances de calcul**, sélectionnez votre instance de calcul, puis cliquez sur **Arrêter** pour l’arrêter. Sinon, laissez-la s’exécuter pour le labo suivant.

> **Remarque** : Ainsi, les ressources de calcul ne seront pas facturées dans votre abonnement. Une petite quantité de stockage de données vous est cependant facturée tant que l’espace de travail Azure Machine Learning existe dans votre abonnement. Si vous avez terminé l’exploration d’Azure Machine Learning, vous pouvez supprimer l’espace de travail Azure Machine Learning et les ressources associées. Toutefois, si vous envisagez d’effectuer d’autres labos de cette série, vous devez répéter l’exercice *[Créer un espace de travail Azure Machine Learning](01-create-a-workspace.md)* pour créer l’espace de travail et préparer l’environnement en premier.