---
title: Cuadernos de Jupyter Notebook de ejemplo
titleSuffix: Azure Machine Learning
description: Busque y use cuadernos de Jupyter de ejemplo para explorar el SDK de Python para Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: sample
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.date: 03/05/2020
ms.custom: seodec18
ms.openlocfilehash: 7242b82ee5c43878a33731bd1f02b685020f22b0
ms.sourcegitcommit: bc792d0525d83f00d2329bea054ac45b2495315d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78673611"
---
# <a name="explore-azure-machine-learning-with-jupyter-notebooks"></a>Exploración de Azure Machine Learning con cuadernos de Jupyter

El repositorio de [cuadernos de Azure Machine Learning](https://github.com/azure/machinelearningnotebooks) incluye los ejemplos más recientes del SDK de Python para Azure Machine Learning. Estos cuadernos de Juypter Notebook están diseñados para ayudarle a explorar el SDK y para servir como modelos para sus propios proyectos de aprendizaje automático.

Este artículo le muestra cómo acceder al repositorio desde los siguientes entornos:

- [Instancia de proceso de Azure Machine Learning](#notebookvm)
- [Servidor de Notebook propio](#byo)
- [Data Science Virtual Machine](#dsvm)

> [!NOTE]
> Una vez que haya clonado el repositorio, encontrará los cuadernos del tutorial en la carpeta **tutorials** y los cuadernos de características específicas en la carpeta **how-to-use-azureml**.

<a name="notebookvm"></a>
## <a name="get-samples-on-azure-machine-learning-compute-instance"></a>Obtener ejemplos de la instancia de proceso de Azure Machine Learning

La manera más de fácil de empezar a trabajar con los ejemplos es realizar el [Tutorial: Configuración del entorno y el área de trabajo](tutorial-1st-experiment-sdk-setup.md). Una vez completado, tendrá un servidor de cuadernos en el que se habrá cargado previamente el SDK y el repositorio de ejemplos. No es necesario realizar ninguna descarga ni instalación.

<a name="byo"></a>

## <a name="get-samples-on-your-notebook-server"></a>Obtener ejemplos del servidor de cuadernos

Si desea usar su propio servidor de Jupyter Notebook para desarrollo local, siga estos pasos:

[!INCLUDE [aml-your-server](../../includes/aml-your-server.md)]

Estas instrucciones le permitirán instalar los paquetes básicos del SDK necesarios para los cuadernos del inicio rápido y el tutorial. Otros cuadernos de ejemplo pueden requerir la instalación de componentes adicionales. Para más información, consulte [Install the Azure Machine Learning SDK for Python](https://docs.microsoft.com/python/api/overview/azure/ml/install) (Instalación del SDK de Azure Machine Learning para Python).

<a name="dsvm"></a>
## <a name="get-samples-on-dsvm"></a>Obtener ejemplos de DSVM

Data Science Virtual Machine (DSVM) es una imagen de máquina virtual personalizada diseñada específicamente para realizar ciencia de datos. Si [crea una instancia de DSVM](how-to-configure-environment.md#dsvm), el SDK y el servidor de cuadernos se instalarán y configurarán automáticamente. No obstante, tendrá que crear un área de trabajo y clonar el repositorio de ejemplo.

[!INCLUDE [aml-dsvm-server](../../includes/aml-dsvm-server.md)]

## <a name="next-steps"></a>Pasos siguientes

Explore los [cuadernos de ejemplo](https://aka.ms/aml-notebooks) para descubrir lo que Azure Machine Learning puede hacer o pruebe estos tutoriales:

- [Entrenamiento de un modelo de clasificación de imágenes con el servicio Azure Machine Learning](tutorial-train-models-with-aml.md)

- [Preparación de datos y uso de Machine Learning automatizado para entrenar un modelo de regresión con el conjunto de datos de los taxis de Nueva York](tutorial-auto-train-models.md)
