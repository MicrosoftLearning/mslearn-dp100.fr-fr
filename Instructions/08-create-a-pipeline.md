---
lab:
  title: Créer un pipeline
ms.openlocfilehash: d76c9c66876fcb03aa02d48ee47ae92e327dd8c9
ms.sourcegitcommit: 18f734eeb1031a9cb69c3b294632efd2e69324ac
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2021
ms.locfileid: "145195769"
---
# <a name="create-a-pipeline"></a>Créer un pipeline

Vous pouvez utiliser le Kit de développement logiciel (SDK) Azure Machine Learning pour accomplir toutes les tâches requises pour créer et exploiter d’une solution d’apprentissage automatique dans Azure. Au lieu d’accomplir ces tâches individuellement, vous pouvez utiliser des *pipelines* pour orchestrer les étapes requises pour préparer les données, exécuter des scripts d’apprentissage, inscrire des modèles et effectuer d’autres tâches.

## <a name="before-you-start"></a>Avant de démarrer

Si ce n’est déjà fait, effectuez l’exercice *[Créer un espace de travail Azure Machine Learning](01-create-a-workspace.md)* pour créer un espace de travail et une instance de calcul Azure Machine Learning, et cloner les notebooks requis.

## <a name="open-jupyter"></a>Ouvrir Jupyter

Bien que vous puissiez utiliser la page **Notebooks** dans Azure Machine Learning studio pour exécuter des notebooks, il est souvent plus productif d’utiliser un environnement de développement de notebook plus complet comme *Jupyter*.

1. Dans [Azure Machine Learning studio](https://ml.azure.com), affichez la page **Calcul** de votre espace de travail, puis, sous l’onglet **Instances de calcul**, démarrez votre instance de calcul si elle n’est pas déjà en cours d’exécution.
2. Une fois l’instance de calcul en cours d’exécution, cliquez sur le lien **Jupyter** pour ouvrir la page d’accueil Jupyter dans un nouvel onglet de navigateur.

## <a name="create-and-publish-a-pipeline"></a>Créer et publier un pipeline

Dans cet exercice, le code permettant de créer et publier un pipeline est fourni dans un notebook.

1. Dans la page d’accueil Jupyter, accédez au dossier **/users/*votre-nom-utilisateur*/mslearn-dp100** dans lequel vous avez cloné le référentiel de notebooks, puis ouvrez le notebook **Créer un pipeline**.
2. Lisez ensuite les notes dans le notebook, en exécutant chaque cellule de code tour à tour.
3. Une fois que vous avez fini d’exécuter le code dans le notebook, dans le menu **Fichier**, cliquez sur **Fermer et arrêter** pour le fermer et arrêter son noyau Python. Fermez ensuite tous les onglets du navigateur Jupyter.

## <a name="clean-up"></a>Nettoyage

Si vous avez fini d’utiliser Azure Machine Learning pour l’instant, dans Azure Machine Learning studio, dans la page **Calcul**, sous l’onglet **Instances de calcul**, sélectionnez votre instance de calcul, puis cliquez sur **Arrêter** pour l’arrêter. Sinon, laissez-la s’exécuter pour le labo suivant.

> **Remarque** : Ainsi, les ressources de calcul ne seront pas facturées dans votre abonnement. Une petite quantité de stockage de données vous est cependant facturée tant que l’espace de travail Azure Machine Learning existe dans votre abonnement. Si vous avez terminé l’exploration d’Azure Machine Learning, vous pouvez supprimer l’espace de travail Azure Machine Learning et les ressources associées. Toutefois, si vous envisagez d’effectuer d’autres labos de cette série, vous devez répéter l’exercice *[Créer un espace de travail Azure Machine Learning](01-create-a-workspace.md)* pour créer l’espace de travail et préparer l’environnement en premier.