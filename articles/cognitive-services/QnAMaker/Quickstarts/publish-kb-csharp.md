---
title: 'Inicio rápido: Publicación de la base de conocimiento en QnA Maker con REST y C#'
description: Esta guía de inicio rápido basada en REST de C# publica la base de conocimiento y crea un punto de conexión al que se puede llamar en su aplicación o bot de chat.
ms.date: 02/08/2020
ROBOTS: NOINDEX,NOFOLLOW
ms.custom: RESTCURL2020FEB27
ms.topic: conceptual
ms.openlocfilehash: 0a2f64795d82928e33a10c7e9d162f2333cdf7bf
ms.sourcegitcommit: f5e4d0466b417fa511b942fd3bd206aeae0055bc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78851558"
---
# <a name="quickstart-publish-a-knowledge-base-in-qna-maker-using-c"></a>Inicio rápido: Publicación de una base de conocimiento en QnA Maker con C#

En esta guía de inicio rápido basada en REST se describe la publicación mediante programación de una base de conocimiento (KB). La publicación inserta la versión más reciente de la base de conocimiento en un índice de Azure Cognitive Search dedicado y crea un punto de conexión que se puede llamar en su aplicación o bot de chat.

En esta guía de inicio rápido se llama a las siguientes API de QnA Maker:
* [Publicar](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/publish): esta API no requiere ninguna información en el cuerpo de la solicitud.

## <a name="prerequisites"></a>Prerrequisitos

* Versión de [**Visual Studio Community Edition**](https://www.visualstudio.com/downloads/) más reciente.
* Debe tener un [servicio QnA Maker](../How-To/set-up-qnamaker-service-azure.md). Para recuperar la clave y el punto de conexión (que incluye el nombre del recurso), seleccione **Inicio rápido** en el recurso en Azure Portal.
* Identificador de la base de conocimiento (KB) de QnA Maker encontrado en la dirección URL en el parámetro de cadena de consulta de `kbid`, como se muestra a continuación.

    ![Identificador de base de conocimiento de QnA Maker](../media/qnamaker-quickstart-kb/qna-maker-id.png)

    Si aún no tiene una base de conocimiento, puede crear una de ejemplo para usar en este inicio rápido: [Creación de una base de conocimiento](create-new-kb-csharp.md)

> [!NOTE]
> Los archivos de la solución completa están disponibles en el repositorio [**Azure-Samples/cognitive-services-qnamaker-csharp** de GitHub](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/tree/master/documentation-samples/quickstarts/publish-knowledge-base).

## <a name="create-knowledge-base-project"></a>Creación del proyecto de base de conocimiento

1. Abra Visual Studio 2019 Community Edition.
1. Cree un proyecto de **aplicación de consola (.Net Core)** y asígnele el nombre `QnaMakerQuickstart`. Acepte los valores predeterminados del resto de la configuración.

## <a name="add-required-dependencies"></a>Incorporación de las dependencias necesarias

En la parte superior de Program.cs, reemplace la instrucción using por las líneas siguientes para agregar las dependencias necesarias al proyecto:

[!code-csharp[Add the required dependencies](~/samples-qnamaker-csharp/documentation-samples/quickstarts/publish-knowledge-base/QnAMakerPublishQuickstart/Program.cs?range=1-2 "Add the required dependencies")]

## <a name="add-required-constants"></a>Incorporación de las constantes necesarias

En la clase **Program**, agregue las constantes necesarias para acceder a QnA Maker.

[!code-csharp[Add the required constants](~/samples-qnamaker-csharp/documentation-samples/quickstarts/publish-knowledge-base/QnAMakerPublishQuickstart/Program.cs?range=8-34 "Add the required constants")]

## <a name="add-the-main-method-to-publish-the-knowledge-base"></a>Adición del método Main para publicar la base de conocimiento

Tras las constantes requeridas, agregue el siguiente código, que realiza una solicitud HTTPS a QnA Maker API para publicar una base de conocimiento y recibe la respuesta:

[!code-csharp[Add HTTP Post request and response](~/samples-qnamaker-csharp/documentation-samples/quickstarts/publish-knowledge-base/QnAMakerPublishQuickstart/Program.cs?range=36-56 "Add HTTP Post request and response")]

La llamada API devuelve un estado 204 en caso de publicación correcta sin ningún contenido en el cuerpo de la respuesta.

## <a name="build-and-run-the-program"></a>Compilar y ejecutar el programa

Compile y ejecute el programa. Este enviará automáticamente la solicitud a QnA Maker API para publicar la base de conocimiento y, después, la respuesta se imprimirá en la ventana de la consola.

Una vez publicada la base de conocimiento, puede realizar consultas en ella desde el punto de conexión con una aplicación cliente o un bot de chat.

[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

Después de publicar la base de conocimiento, es preciso que la dirección URL del punto de conexión [ genere una respuesta](./get-answer-from-knowledge-base-csharp.md).

> [!div class="nextstepaction"]
> [Referencia de QnA Maker (V4) REST API](https://go.microsoft.com/fwlink/?linkid=2092179)