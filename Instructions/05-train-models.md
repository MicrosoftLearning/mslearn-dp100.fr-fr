---
lab:
  title: Entraîner des modèles
ms.openlocfilehash: dffa9edda34c599dfbd372fe898ddcf955f6f200
ms.sourcegitcommit: 66d8872bc3d24c2121e225be132b56f4df7920ac
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/12/2022
ms.locfileid: "145195768"
---
# <a name="train-models"></a>Effectuer l’apprentissage de modèles

Le Machine Learning a principalement trait aux modèles d’apprentissage que vous pouvez utiliser pour fournir des services prédictifs à des applications. Dans cet exercice, vous allez voir comment utiliser des expériences Azure Machine Learning pour exécuter des scripts d’apprentissage, et comment inscrire les modèles formés qui en résultent.

## <a name="before-you-start"></a>Avant de démarrer

Si ce n’est déjà fait, effectuez l’exercice *[Créer un espace de travail Azure Machine Learning](01-create-a-workspace.md)* pour créer un espace de travail et une instance de calcul Azure Machine Learning, et cloner les notebooks requis.

## <a name="open-jupyter"></a>Ouvrir Jupyter

Bien que vous puissiez utiliser la page **Notebooks** dans Azure Machine Learning studio pour exécuter des notebooks, il est souvent plus productif d’utiliser un environnement de développement de notebook plus complet comme *Jupyter*.

1. Dans [Azure Machine Learning studio](https://ml.azure.com), affichez la page **Calcul** de votre espace de travail, puis, sous l’onglet **Instances de calcul**, démarrez votre instance de calcul si elle n’est pas déjà en cours d’exécution.
2. Une fois l’instance de calcul en cours d’exécution, cliquez sur le lien **Jupyter** pour ouvrir la page d’accueil Jupyter dans un nouvel onglet de navigateur.

> **Conseil** : Vous découvrez Python ? Utilisez l’[aide-mémoire Python](cheat-sheets/dp100-cheat-sheet-python.pdf) pour comprendre le code. Vous découvrez le Machine Learning ? Consultez la [vue d’ensemble du Machine Learning](cheat-sheets/dp100-cheat-sheet-machine-learning.pdf) pour obtenir un aperçu simplifié du processus d’apprentissage automatique dans Azure Machine Learning.

## <a name="train-models-using-the-azure-machine-learning-sdk"></a>Effectuer l’apprentissage de modèles à l’aide du Kit de développement logiciel (SDK) Azure Machine Learning

Dans cet exercice, le code permettant d’effectuer l’apprentissage des modèles est fourni dans un notebook.

1. Dans la page d’accueil Jupyter, accédez au dossier **/users/*votre-nom-utilisateur*/mslearn-dp100** dans lequel vous avez cloné le référentiel de notebooks, puis ouvrez le notebook **Effectuer l’apprentissage de modèles**.
2. Lisez ensuite les notes dans le notebook, en exécutant chaque cellule de code tour à tour.
3. Une fois que vous avez fini d’exécuter le code dans le notebook, dans le menu **Fichier**, cliquez sur **Fermer et arrêter** pour le fermer et arrêter son noyau Python. Fermez ensuite tous les onglets du navigateur Jupyter.

## <a name="clean-up"></a>Nettoyage

Si vous avez fini d’utiliser Azure Machine Learning pour l’instant, dans Azure Machine Learning studio, dans la page **Calcul**, sous l’onglet **Instances de calcul**, sélectionnez votre instance de calcul, puis cliquez sur **Arrêter** pour l’arrêter. Sinon, laissez-la s’exécuter pour le labo suivant.

> **Remarque** : Ainsi, les ressources de calcul ne seront pas facturées dans votre abonnement. Une petite quantité de stockage de données vous est cependant facturée tant que l’espace de travail Azure Machine Learning existe dans votre abonnement. Si vous avez terminé l’exploration d’Azure Machine Learning, vous pouvez supprimer l’espace de travail Azure Machine Learning et les ressources associées. Toutefois, si vous envisagez d’effectuer d’autres labos de cette série, vous devez répéter l’exercice *[Créer un espace de travail Azure Machine Learning](01-create-a-workspace.md)* pour créer l’espace de travail et préparer l’environnement en premier.