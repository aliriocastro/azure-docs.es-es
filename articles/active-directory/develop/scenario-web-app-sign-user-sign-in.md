---
title: 'Escritura de una aplicación web que permita iniciar o cerrar sesión a los usuarios: Plataforma de identidad de Microsoft | Azure'
description: Obtenga información sobre cómo compilar una aplicación web que permita iniciar o cerrar sesión a los usuarios
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
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: cbec6c5644690f6d7e522294a37c8ea5e2d49e30
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2020
ms.locfileid: "76701525"
---
# <a name="web-app-that-signs-in-users-sign-in-and-sign-out"></a>Aplicación web que inicia sesión de usuarios: Inicio y cierre de sesión

Obtenga información sobre cómo agregar el inicio de sesión al código de una aplicación web que permita iniciar sesión a los usuarios. A continuación, obtenga información sobre cómo permitirles cerrar sesión.

## <a name="sign-in"></a>Inicio de sesión

El inicio de sesión consta de dos partes:

- El botón de inicio de sesión en la página HTML
- La acción de inicio de sesión en el código subyacente en el controlador

### <a name="sign-in-button"></a>Botón de inicio de sesión

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

En ASP.NET Core, el botón de inicio de sesión se expone en `Views\Shared\_LoginPartial.cshtml`. Solo se muestra cuando no hay ninguna cuenta autenticada. Es decir, se muestra cuando el usuario todavía no ha iniciado sesión o ha cerrado la sesión.

```html
@using Microsoft.Identity.Web
@if (User.Identity.IsAuthenticated)
{
 // Code omitted code for clarity
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li><a asp-area="AzureAD" asp-controller="Account" asp-action="SignIn">Sign in</a></li>
    </ul>
}
```

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

En ASP.NET MVC, el botón de inicio de sesión se expone en `Views\Shared\_LoginPartial.cshtml`. Solo se muestra cuando hay una cuenta autenticada. Es decir, se muestra cuando el usuario ha iniciado sesión previamente.

```html
@if (Request.IsAuthenticated)
{
 // Code omitted code for clarity
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li>@Html.ActionLink("Sign in", "SignIn", "Account", routeValues: null, htmlAttributes: new { id = "loginLink" })</li>
    </ul>
}
```

# <a name="javatabjava"></a>[Java](#tab/java)

En el inicio rápido de Java, el botón de inicio de sesión se encuentra en el archivo [main/resources/templates/index.html](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/master/src/main/resources/templates/index.html).

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>HomePage</title>
</head>
<body>
<h3>Home Page</h3>

<form action="/msal4jsample/secure/aad">
    <input type="submit" value="Login">
</form>

</body>
</html>
```

# <a name="pythontabpython"></a>[Python](#tab/python)

En el inicio rápido de Python, no hay ningún botón de inicio de sesión. El código subyacente solicita al usuario de forma automática que inicie sesión al alcanzar la raíz de la aplicación web. Consulte [app.py#L14-L18](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/0.1.0/app.py#L14-L18).

```Python
@app.route("/")
def index():
    if not session.get("user"):
        return redirect(url_for("login"))
    return render_template('index.html', user=session["user"])
```

---

### <a name="signin-action-of-the-controller"></a>Acción `SignIn` del controlador

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

En ASP.NET, seleccionar el botón **Iniciar sesión** en la aplicación web desencadena la acción `SignIn` en el controlador `AccountController`. En las versiones anteriores de las plantillas de ASP.NET Core, el controlador `Account` se incrustó con la aplicación web. Ya no es así porque el controlador ahora forma parte de la arquitectura de ASP.NET Core.

El código para `AccountController` está disponible en el repositorio de ASP.NET Core en [AccountController.cs](https://github.com/aspnet/AspNetCore/blob/master/src/Azure/AzureAD/Authentication.AzureAD.UI/src/Areas/AzureAD/Controllers/AccountController.cs). El control de cuentas desafía al usuario redireccionándole al punto de conexión de la Plataforma de identidad de Microsoft. Para obtener detalles, consulte el método [SignIn](https://github.com/aspnet/AspNetCore/blob/f3e6b74623d42d5164fd5f97a288792c8ad877b6/src/Azure/AzureAD/Authentication.AzureAD.UI/src/Areas/AzureAD/Controllers/AccountController.cs#L23-L31) proporcionado como parte de ASP.NET Core.

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

En ASP.NET, el cierre de sesión se desencadena desde el método `SignOut()` en un controlador (por ejemplo, [AccountController.cs#L16-L23](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect/blob/a2da310539aa613b77da1f9e1c17585311ab22b7/WebApp/Controllers/AccountController.cs#L16-L23)). Este método no forma parte del marco de ASP.NET (lo contrario a lo que sucede en ASP.NET Core). Envía un desafío de inicio de sesión OpenID tras proponer un URI de redirección.

```csharp
public void SignIn()
{
    // Send an OpenID Connect sign-in request.
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
    }
}
```

# <a name="javatabjava"></a>[Java](#tab/java)

En Java, el cierre de sesión se controla llamando directamente al punto de conexión `logout` de la Plataforma de identidad de Microsoft y proporcionando el valor `post_logout_redirect_uri`. Para obtener detalles, consulte [AuthPageController.java#L30-L48](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/java/com/microsoft/azure/msalwebsample/AuthPageController.java#L30-L48).

```Java
@Controller
public class AuthPageController {

    @Autowired
    AuthHelper authHelper;

    @RequestMapping("/msal4jsample")
    public String homepage(){
        return "index";
    }

    @RequestMapping("/msal4jsample/secure/aad")
    public ModelAndView securePage(HttpServletRequest httpRequest) throws ParseException {
        ModelAndView mav = new ModelAndView("auth_page");

        setAccountInfo(mav, httpRequest);

        return mav;
    }

    // More code omitted for simplicity
```

# <a name="pythontabpython"></a>[Python](#tab/python)

A diferencia de otras plataformas, MSAL para Python se encarga de permitir al usuario iniciar sesión desde la página de inicio de sesión. Consulte [app.py#L20-L28](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/e03be352914bfbd58be0d4170eba1fb7a4951d84/app.py#L20-L28).

```Python
@app.route("/login")
def login():
    session["state"] = str(uuid.uuid4())
    auth_url = _build_msal_app().get_authorization_request_url(
        app_config.SCOPE,  # Technically we can use an empty list [] to just sign in
                           # Here we choose to also collect user consent up front
        state=session["state"],
        redirect_uri=url_for("authorized", _external=True))
    return "<a href='%s'>Login with Microsoft Identity</a>" % auth_url
```

El método `_build_msal_app()` se define en [app. py # L81-L88](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/e03be352914bfbd58be0d4170eba1fb7a4951d84/app.py#L81-L88) de la siguiente manera:

```Python
def _load_cache():
    cache = msal.SerializableTokenCache()
    if session.get("token_cache"):
        cache.deserialize(session["token_cache"])
    return cache

def _save_cache(cache):
    if cache.has_state_changed:
        session["token_cache"] = cache.serialize()

def _build_msal_app(cache=None):
    return msal.ConfidentialClientApplication(
        app_config.CLIENT_ID, authority=app_config.AUTHORITY,
        client_credential=app_config.CLIENT_SECRET, token_cache=cache)

def _get_token_from_cache(scope=None):
    cache = _load_cache()  # This web app maintains one cache per session
    cca = _build_msal_app(cache)
    accounts = cca.get_accounts()
    if accounts:  # So all accounts belong to the current signed-in user
        result = cca.acquire_token_silent(scope, account=accounts[0])
        _save_cache(cache)
        return result

```

---

Una vez que el usuario ha iniciado sesión en la aplicación, probablemente quiera permitirle cerrar la sesión.

## <a name="sign-out"></a>Cierre de sesión

Cerrar sesión desde una aplicación web implica más que tan solo quitar la información sobre la cuenta con sesión iniciada del estado de la aplicación web.
La aplicación web también debe redirigir al usuario al punto de conexión `logout` de la Plataforma de identidad de Microsoft para cerrar la sesión. 

Cuando la aplicación web redirige al usuario a punto de conexión `logout`, este punto de conexión borra la sesión del usuario del explorador. Si la aplicación no llegó al punto de conexión `logout`, el usuario se volverá a autenticar en la aplicación sin volver a escribir sus credenciales. La razón es que tendrá una sesión de inicio de sesión único válida con el punto de conexión de la Plataforma de identidad de Microsoft.

Para más información, consulte la sección [Envío de una solicitud de cierre de sesión](v2-protocols-oidc.md#send-a-sign-out-request) en la documentación de la [Plataforma de identidad de Microsoft y protocolo OpenID Connect](v2-protocols-oidc.md).

### <a name="application-registration"></a>Registro de la aplicación

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

Durante el registro de la aplicación, registrará un URI posterior al cierre de sesión. En este tutorial, ha registrado `https://localhost:44321/signout-oidc` en el campo **URL de cierre de sesión** de la sección **Configuración avanzada** de la página **Autenticación**. Para obtener información detallada, consulte [Registrar la aplicación webApp](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-1-MyOrg#register-the-webapp-app-webapp).

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

Durante el registro de la aplicación, registrará un URI posterior al cierre de sesión. En este tutorial, ha registrado `https://localhost:44308/Account/EndSession` en el campo **URL de cierre de sesión** de la sección **Configuración avanzada** de la página **Autenticación**. Para obtener información detallada, consulte [Registrar la aplicación webApp](https://github.com/Azure-Samples/active-directory-dotnet-web-single-sign-out#register-the-service-app-webapp-distributedsignout-dotnet).

# <a name="javatabjava"></a>[Java](#tab/java)

Durante el registro de la aplicación, registrará un URI posterior al cierre de sesión. En este tutorial, ha registrado `http://localhost:8080/msal4jsample/sign_out` en el campo **URL de cierre de sesión** de la sección **Configuración avanzada** de la página **Autenticación**.

# <a name="pythontabpython"></a>[Python](#tab/python)

Durante el registro de la aplicación, no es necesario registrar una dirección URL de cierre de sesión adicional. Se volverá a llamar a la aplicación en su dirección URL principal.

---

### <a name="sign-out-button"></a>Botón de cierre de sesión

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

En ASP.NET Core, el botón de cierre de sesión se expone en `Views\Shared\_LoginPartial.cshtml`. Solo se muestra cuando hay una cuenta autenticada. Es decir, se muestra cuando el usuario ha iniciado sesión previamente.

```html
@using Microsoft.Identity.Web
@if (User.Identity.IsAuthenticated)
{
    <ul class="nav navbar-nav navbar-right">
        <li class="navbar-text">Hello @User.GetDisplayName()!</li>
        <li><a asp-area="AzureAD" asp-controller="Account" asp-action="SignOut">Sign out</a></li>
    </ul>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li><a asp-area="AzureAD" asp-controller="Account" asp-action="SignIn">Sign in</a></li>
    </ul>
}
```

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

En ASP.NET MVC, el botón de inicio de sesión se expone en `Views\Shared\_LoginPartial.cshtml`. Solo se muestra cuando hay una cuenta autenticada. Es decir, se muestra cuando el usuario ha iniciado sesión previamente.

```html
@if (Request.IsAuthenticated)
{
    <text>
        <ul class="nav navbar-nav navbar-right">
            <li class="navbar-text">
                Hello, @User.Identity.Name!
            </li>
            <li>
                @Html.ActionLink("Sign out", "SignOut", "Account")
            </li>
        </ul>
    </text>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li>@Html.ActionLink("Sign in", "SignIn", "Account", routeValues: null, htmlAttributes: new { id = "loginLink" })</li>
    </ul>
}
```

# <a name="javatabjava"></a>[Java](#tab/java)

En el inicio rápido de Java, el botón de cierre de sesión se encuentra en el archivo main/resources/templates/auth_page.html.

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<body>

<form action="/msal4jsample/sign_out">
    <input type="submit" value="Sign out">
</form>
...
```

# <a name="pythontabpython"></a>[Python](#tab/python)

En el inicio rápido de Python, el botón de cierre de sesión se encuentra en el archivo [templates/index.html#L10](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/e03be352914bfbd58be0d4170eba1fb7a4951d84/templates/index.html#L10).

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
</head>
<body>
    <h1>Microsoft Identity Python Web App</h1>
    Welcome {{ user.get("name") }}!
    <li><a href='/graphcall'>Call Microsoft Graph API</a></li>
    <li><a href="/logout">Logout</a></li>
</body>
</html>
```

---

### <a name="signout-action-of-the-controller"></a>Acción `SignOut` del controlador

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

En ASP.NET, seleccionar el botón **Cerrar sesión** en la aplicación web desencadena la acción `SignOut` en el controlador `AccountController`. En las versiones anteriores de las plantillas de ASP.NET Core, el controlador `Account` se incrustó con la aplicación web. Ya no es así porque el controlador ahora forma parte de la arquitectura de ASP.NET Core.

El código para `AccountController` está disponible en el repositorio de ASP.NET Core en [AccountController.cs](https://github.com/aspnet/AspNetCore/blob/master/src/Azure/AzureAD/Authentication.AzureAD.UI/src/Areas/AzureAD/Controllers/AccountController.cs). El control de cuentas:

- Establece un URI de redirección de OpenID a `/Account/SignedOut` para volver a llamar al controlador cuando Azure AD haya completado el cierre de sesión.
- Llama a `Signout()`, que permite que el middleware de OpenID Connect se ponga en contacto con el punto de conexión `logout` de la Plataforma de identidad de Microsoft. A continuación, el punto de conexión:

  - Borra la cookie de sesión del explorador.
  - Vuelve a llamar a la dirección URL de cierre de sesión. De forma predeterminada, la dirección URL de cierre de sesión muestra la página de vista que ha cerrado la sesión [SignedOut. html](https://github.com/aspnet/AspNetCore/blob/master/src/Azure/AzureAD/Authentication.AzureAD.UI/src/Areas/AzureAD/Pages/Account/SignedOut.cshtml). Esta página también se proporciona como parte de ASP.NET Core.

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

En ASP.NET, el cierre de sesión se desencadena desde el método `SignOut()` en un controlador (por ejemplo, [AccountController.cs#L25-L31](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect/blob/a2da310539aa613b77da1f9e1c17585311ab22b7/WebApp/Controllers/AccountController.cs#L25-L31)). Este método no forma parte del marco de ASP.NET (lo contrario de lo que sucede en ASP.NET Core). Este:

- Envía un desafío de cierre de sesión de OpenID.
- Elimina la caché.
- Redirige a la página que quiere.

```csharp
/// <summary>
/// Send an OpenID Connect sign-out request.
/// </summary>
public void SignOut()
{
 HttpContext.GetOwinContext()
            .Authentication
            .SignOut(CookieAuthenticationDefaults.AuthenticationType);
 Response.Redirect("/");
}
```

# <a name="javatabjava"></a>[Java](#tab/java)

En Java, el cierre de sesión se controla llamando directamente al punto de conexión `logout` de la Plataforma de identidad de Microsoft y proporcionando el valor `post_logout_redirect_uri`. Para obtener detalles, consulte [AuthPageController.java#L50-L60](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/java/com/microsoft/azure/msalwebsample/AuthPageController.java#L50-L60)

```Java
@RequestMapping("/msal4jsample/sign_out")
    public void signOut(HttpServletRequest httpRequest, HttpServletResponse response) throws IOException {

        httpRequest.getSession().invalidate();

        String endSessionEndpoint = "https://login.microsoftonline.com/common/oauth2/v2.0/logout";

        String redirectUrl = "http://localhost:8080/msal4jsample/";
        response.sendRedirect(endSessionEndpoint + "?post_logout_redirect_uri=" +
                URLEncoder.encode(redirectUrl, "UTF-8"));
    }
```

# <a name="pythontabpython"></a>[Python](#tab/python)

El código que cierra la sesión del usuario está en [app.py#L46-L52](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/48637475ed7d7733795ebeac55c5d58663714c60/app.py#L47-L48).

```Python
@app.route("/logout")
def logout():
    session.clear()  # Wipe out the user and the token cache from the session
    return redirect(  # Also need to log out from the Microsoft Identity platform
        "https://login.microsoftonline.com/common/oauth2/v2.0/logout"
        "?post_logout_redirect_uri=" + url_for("index", _external=True))
```

---

### <a name="intercepting-the-call-to-the-logout-endpoint"></a>Interceptar la llamada al punto de conexión `logout`

El URI posterior al cierre de sesión permite a las aplicaciones participar en el cierre de sesión global.

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

El middleware OpenID Connect de ASP.NET Core permite a la aplicación interceptar la llamada al punto de conexión `logout` de la Plataforma de identidad de Microsoft al proporcionar un evento de OpenID Connect denominado `OnRedirectToIdentityProviderForSignOut`. Para obtener un ejemplo de cómo suscribirse a este evento (para borrar la caché de tokens), consulte [Microsoft.Identity.Web/WebAppServiceCollectionExtensions.cs#L151-L156](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/faa94fd49c2da46b22d6694c4f5c5895795af26d/Microsoft.Identity.Web/WebAppServiceCollectionExtensions.cs#L151-L156).

```csharp
    // Handling the global sign-out
    options.Events.OnRedirectToIdentityProviderForSignOut = async context =>
    {
        // Forget about the signed-in user
    };
```

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

En ASP.NET, se delega en el middleware la ejecución del cierre de sesión, borrando la cookie de sesión:

```csharp
public class AccountController : Controller
{
 ...
 public void EndSession()
 {
  Request.GetOwinContext().Authentication.SignOut();
  Request.GetOwinContext().Authentication.SignOut(Microsoft.AspNet.Identity.DefaultAuthenticationTypes.ApplicationCookie);
  this.HttpContext.GetOwinContext().Authentication.SignOut(CookieAuthenticationDefaults.AuthenticationType);
 }
}
```

# <a name="javatabjava"></a>[Java](#tab/java)

En el inicio rápido de Java, el URI de redireccionamiento posterior al cierre de sesión solo muestra la página index.html.

# <a name="pythontabpython"></a>[Python](#tab/python)

En el inicio rápido de Python, el URI de redireccionamiento posterior al cierre de sesión solo muestra la página index.html.

---

## <a name="protocol"></a>Protocolo

Si quiere más información sobre el cierre de sesión, lea la documentación del protocolo, que está disponible en [Open ID Connect](./v2-protocols-oidc.md).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Paso a producción](scenario-web-app-sign-user-production.md)
