---
title: 'API web que llama a otras API web: Plataforma de identidad de Microsoft | Azure'
description: Aprenda a compilar una API web que llama a otras API web.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: d66a08d4e84a3771d6c3fa46b96c975869435452
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76833386"
---
# <a name="a-web-api-that-calls-web-apis-call-an-api"></a>Una API web que llama a las API web: Llamar a una API

Una vez que disponga de un token, puede llamar a una API web protegida. Esto se hace desde el controlador de la API web.

## <a name="controller-code"></a>Código del controlador

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

El código siguiente continúa el código de ejemplo que se muestra en [Una API web que llama a las API web: Adquisición de un token para la aplicación](scenario-web-api-call-api-acquire-token.md). El código se llama en las acciones de los controladores de API. Llama a una API de nivel inferior denominada *todolist*.

Una vez que haya adquirido el token, úselo como token de portador para llamar a la API de nivel inferior.

```csharp
private async Task GetTodoList(bool isAppStarting)
{
 ...
 //
 // Get an access token to call the To Do service.
 //
 AuthenticationResult result = null;
 try
 {
  app = BuildConfidentialClient(HttpContext, HttpContext.User);
  result = await app.AcquireTokenSilent(Scopes, account)
                     .ExecuteAsync()
                     .ConfigureAwait(false);
 }
...

// After the token has been returned by Microsoft Authentication Library (MSAL), add it to the HTTP authorization header before making the call to access the To Do list service.
_httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call the To Do list service.
HttpResponseMessage response = await _httpClient.GetAsync(TodoListBaseAddress + "/api/todolist");
...
}
```

# <a name="javatabjava"></a>[Java](#tab/java)

El código siguiente continúa el código de ejemplo que se muestra en [Una API web que llama a las API web: Adquisición de un token para la aplicación](scenario-web-api-call-api-acquire-token.md). El código se llama en las acciones de los controladores de API. Llama a la API de nivel inferior MS Graph.

Una vez que haya adquirido el token, úselo como token de portador para llamar a la API de nivel inferior.

```Java
private String callMicrosoftGraphMeEndpoint(String accessToken){
    RestTemplate restTemplate = new RestTemplate();

    HttpHeaders headers = new HttpHeaders();
    headers.setContentType(MediaType.APPLICATION_JSON);

    headers.set("Authorization", "Bearer " + accessToken);

    HttpEntity<String> entity = new HttpEntity<>(null, headers);

    String result = restTemplate.exchange("https://graph.microsoft.com/v1.0/me", HttpMethod.GET,
            entity, String.class).getBody();

    return result;
}
```

# <a name="pythontabpython"></a>[Python](#tab/python)
Aún no hay disponible ningún ejemplo de este flujo con MSAL Python.

---

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Una API web que llama a las API web: Paso a producción](scenario-web-api-call-api-production.md)
