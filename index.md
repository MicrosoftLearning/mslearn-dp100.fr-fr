---
title: Instructions hébergées en ligne
permalink: index.html
layout: home
ms.openlocfilehash: a2eb157b1d188655f4cfbcc575befec4a2e9c623
ms.sourcegitcommit: 18f734eeb1031a9cb69c3b294632efd2e69324ac
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2021
ms.locfileid: "145195788"
---
# <a name="azure-machine-learning-exercises"></a>Exercices Azure Machine Learning

Ce référentiel contient les exercices pratiques de labo pour le cours Microsoft [DP-100 *Conception et implémentation d’une solution de science des données sur Azure*](https://docs.microsoft.com/learn/certifications/courses/dp-100t01) et les [modules auto-rythmés sur Microsoft Learn](https://docs.microsoft.com/learn/paths/build-ai-solutions-with-azure-ml-service/) équivalents. Les exercices sont conçus pour accompagner les supports de cours d'apprentissage et vous permettre de vous exercer à utiliser les technologies qu'ils décrivent.

Pour accomplir ces exercices, vous devez disposer d’un abonnement Microsoft Azure. Si votre instructeur ne vous en a pas fourni un, vous pouvez vous inscrire pour un essai gratuit sur [https://azure.microsoft.com](https://azure.microsoft.com).

{% assign labs = site.pages | where_exp:"page", "page.url contains '/instructions'" %}
| Exercices |
| ------- | 
{% for activity in labs  %}| [{{ activity.lab.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
