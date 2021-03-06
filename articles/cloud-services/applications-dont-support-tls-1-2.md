---
title: Solucionar problemas causados por aplicaciones que no admiten TLS 1.2 | Microsoft Docs
description: Solucionar problemas causados por aplicaciones que no admiten TLS 1.2
services: cloud-services
documentationcenter: ''
author: mimckitt
manager: vashan
editor: ''
tags: top-support-issue
ms.assetid: ''
ms.service: cloud-services
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: ''
ms.date: 01/17/2020
ms.author: tagore
ms.openlocfilehash: a9d15a94421694583562f433c20413fcc84697c7
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2020
ms.locfileid: "78270862"
---
# <a name="troubleshooting-applications-that-dont-support-tls-12"></a>Solucionar problemas de aplicaciones que no admiten TLS 1.2
En este artículo se describe cómo habilitar los protocolos de TLS más antiguos (TLS 1.0 y 1.1), además de cómo aplicar los conjuntos de cifrado heredados para admitir los protocolos adicionales en los roles de trabajo y web del servicio en la nube de Windows Server 2019. 

Somos conscientes de que, aunque estamos llevando a cabo pasos para dejar de usar TLS 1.0 y TLS 1.1, es posible que nuestros clientes tengan que admitir los protocolos y los conjuntos de cifrado más antiguos hasta que puedan planear su desuso.  Si bien no se recomienda volver a habilitar estos valores heredados, ofrecemos esto como orientación para ayudar a los clientes. Animamos a los clientes a evaluar el riesgo de regresión antes de implementar los cambios descritos en este artículo. 

> [!NOTE]
> Los lanzamientos de la familia 6 del SO invitado aplican TLS 1.2 al deshabilitar los cifrados de 1.0/1.1. 


## <a name="dropping-support-for-tls-10-tls-11-and-older-cipher-suites"></a>Eliminación de la compatibilidad con TLS 1.0, TLS 1.1 y los conjuntos de cifrado anteriores 
En respuesta a nuestro compromiso de usar el mejor cifrado posible, Microsoft anunció planes para comenzar la migración para abandonar TLS 1.0 y 1.1 en junio de 2017.   Desde ese anuncio inicial, Microsoft anunció la intención de deshabilitar la Seguridad de la capa de transporte (TLS) 1.0 y 1.1 de forma predeterminada en las versiones admitidas de Microsoft Edge e Internet Explorer 11 en el primer semestre de 2020.  Los anuncios similares de Apple, Google y Mozilla indican la dirección en la que se dirige el sector.   

## <a name="tls-configuration"></a>Configuración de TLS  
La imagen de servidor en la nube de Windows Server 2019 se configura con TLS 1.0 y TLS 1.1 deshabilitado en el nivel del Registro. Esto significa que las aplicaciones implementadas en esta versión de Windows Y que usan la pila de Windows para la negociación TLS no permitirán la comunicación con TLS 1.0 y TLS 1.1.   

El servidor también incluye un conjunto limitado de conjuntos de cifrado: 

```
    TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256 
    TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384 
    TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 
    TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 
    TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256 
    TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384 
    TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256 
    TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384 
```

## <a name="step-1-create-the-powershell-script-to-enable-tls-10-and-tls-11"></a>Paso 1: Crear el script de PowerShell para habilitar TLS 1.0 y TLS 1.1 

Use el código siguiente como ejemplo para crear un script que habilite los protocolos y conjuntos de cifrado más antiguos. A los efectos de esta documentación, este script se denominará: **TLSsettings.ps1**. Almacene este script en el escritorio local para facilitar el acceso en pasos posteriores. 


```Powershell
#******************* FUNCTION THAT ACTUALLY UPDATES KEYS; WILL RETURN REBOOT FLAG IF CHANGES *********************** 
 
Function Set-CryptoSetting {  
    param (  
        $regKeyName,  
        $value,  
        $valuedata,  
        $valuetype       
    )  
    
    $restart = $false 
  
    # Check for existence of registry key, and create if it does not exist  
    If (!(Test-Path -Path $regKeyName)) {  
        New-Item $regKeyName | Out-Null  
    }  
 
    # Get data of registry value, or null if it does not exist  
    $val = (Get-ItemProperty -Path $regKeyName -Name $value -ErrorAction SilentlyContinue).$value  
 
    If ($val -eq $null) {  
        # Value does not exist - create and set to desired value  
        New-ItemProperty -Path $regKeyName -Name $value -Value $valuedata -PropertyType $valuetype | Out-Null  
        $restart = $true 
    } 
 
    Else {  
        # Value does exist - if not equal to desired value, change it  
        If ($val -ne $valuedata) {  
            Set-ItemProperty -Path $regKeyName -Name $value -Value $valuedata  
            $restart = $true  
        }  
    }  
 
    $restart  
}  
 
#*************************************************************************************************************** 
 
#****************************** CIPHERSUITES FOR OS VERSIONS WINDOWS 10 AND ABOVE ****************************** 
 
function Get-BaseCipherSuitesWin10Above() 
{ 
        $cipherorder += "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256," 
        $cipherorder += "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384," 
        $cipherorder += "TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256," 
        $cipherorder += "TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384," 
        $cipherorder += "TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256," 
        $cipherorder += "TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384," 
        $cipherorder += "TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256," 
        $cipherorder += "TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384," 
 
# Legacy cipher suites 
        $cipherorder += "TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA_P256," 
        $cipherorder += "TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA_P256," 
        $cipherorder += "TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P256," 
        $cipherorder += "TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256," 
        $cipherorder += "TLS_RSA_WITH_AES_256_GCM_SHA384,"  
        $cipherorder += "TLS_RSA_WITH_AES_128_GCM_SHA256,"  
        $cipherorder += "TLS_RSA_WITH_AES_256_CBC_SHA256,"  
        $cipherorder += "TLS_RSA_WITH_AES_128_CBC_SHA256,"  
        $cipherorder += "TLS_RSA_WITH_AES_256_CBC_SHA," 
        $cipherorder += "TLS_RSA_WITH_AES_128_CBC_SHA" 
 
 return $cipherorder 
} 
 

#*************************************************************************************************************** 
 
 
#********************************************** REGISTRY KEYS **************************************************** 
 
 
function Get-RegKeyPathToEnable() 
{ 
    $regKeyPath = @( 
        "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2",         
        "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client",  
        "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server" , 
        "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1",  
        "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client", 
        "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server" , 
        "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0",  
        "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client",  
        "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server" 
    ) 
    return $regKeyPath 
} 
 
#*************************************************************************************************************** 
 
$localRegistryPath = @() 
 
# Enable TLS 1.2, TLS 1.1 and TLS 1.0 
$localRegistryPath += Get-RegKeyPathToEnable 
 
#******************* CREATE THE REGISTRY KEYS IF THEY DON'T EXIST******************************** 
 
# Check for existence of the registry keys, and create if they do not exist  
For ($i = 0; $i -lt $localRegistryPath.Length; $i = $i + 1) {  
   Write-Host "Checking for existing of key: $($localRegistryPath[$i]) Severity Level: Information"
   If (!(Test-Path -Path $localRegistryPath[$i])) {
        New-Item $localRegistryPath [$i] | Out-Null
    Write-Host "Creating key: $($localRegistryPath[$i]) Severity Level: Information"
    }
}  
 
#********************************* EXPLICITLY Enable TLS12,  TLS11 and TLS10********************************* 
 
For ($i = 0; $i -lt $localRegistryPath.Length; $i = $i + 1) { 
    if ($localRegistryPath[$i].Contains("Client") -Or $localRegistryPath[$i].Contains("Server")) { 
      Write-Host "Enabling this key: $($localRegistryPath[$i]) Severity: Information "
        $result = Set-CryptoSetting $localRegistryPath[$i].ToString() Enabled 1 DWord   
        $result = Set-CryptoSetting $localRegistryPath[$i].ToString() DisabledByDefault 0 DWord  
        $reboot = $reboot -or $result 
    } 
} 
 
#**************************************** SET THE CIPHER SUITE ORDER******************************** 
 
$cipherlist = @() 
 
# Set cipher suite order 
$cipherlist += Get-BaseCipherSuitesWin10Above 
$CipherSuiteRegKey = "HKLM:\SOFTWARE\Policies\Microsoft\Cryptography\Configuration\SSL\00010002"  
 
if (!(Test-Path -Path $CipherSuiteRegKey))  
{  
    New-Item $CipherSuiteRegKey | Out-Null  
    $reboot = $True  
    Write-Host "Creating key: $($CipherSuiteRegKey) Severity: Information "
}  
 
#Set-ItemProperty -Path $CipherSuiteRegKey -Name Functions -Value $cipherorder  
Set-ItemProperty -Path $CipherSuiteRegKey -Name Functions -Value $cipherlist  
#********************************************* REBOOT ******************************************* 
 
Write-Host "A reboot is required in order for changes to effect"  
Write-Host "Rebooting now..."  
shutdown.exe /r /t 5 /c "Crypto settings changed" /f /d p:2:4
```

## <a name="step-2-create-a-command-file"></a>Paso 2: Crear un archivo de comandos 

Cree un archivo CMD denominado **RunTLSSettings. cmd** con lo siguiente. Almacene este script en el escritorio local para facilitar el acceso en pasos posteriores. 

```cmd
PowerShell -ExecutionPolicy Unrestricted %~dp0TLSsettings.ps1
REM This line is required to ensure the startup tasks does not block the role from starting in case of error.  DO NOT REMOVE!!!! 
EXIT /B 0
```

## <a name="step-3-add-the-startup-task-to-the-roles-service-definition-csdef"></a>Paso 3: Agregar la tarea de inicio a la definición de servicio del rol (csdef) 

Agregue el siguiente fragmento de código al archivo de definición de servicio existente. 


```
    <Startup> 
        <Task executionContext="elevated" taskType="simple" commandLine="RunTLSSettings.cmd"> 
        </Task> 
    </Startup> 
```

Este es un ejemplo que muestra tanto el rol de trabajo como el rol web. 

```
<?xmlversion="1.0"encoding="utf-8"?> 
<ServiceDefinitionname="CloudServiceName"xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition"schemaVersion="2015-04.2.6"> 
    <WebRolename="WebRole1"vmsize="Standard_D1_v2"> 
        <Sites> 
            <Sitename="Web"> 
                <Bindings> 
                    <Bindingname="Endpoint1"endpointName="Endpoint1"/> 
                </Bindings> 
            </Site> 
        </Sites> 
        <Startup> 
            <Taske xecutionContext="elevated" taskType="simple" commandLine="RunTLSSettings.cmd"> 
            </Task> 
        </Startup> 
        <Endpoints> 
            <InputEndpointname="Endpoint1"protocol="http"port="80"/> 
        </Endpoints> 
    </WebRole> 
<WorkerRolename="WorkerRole1"vmsize="Standard_D1_v2"> 
    <Startup> 
        <Task executionContext="elevated" taskType="simple" commandLine="RunTLSSettings.cmd"> 
        </Task> 
    </Startup> 
</WorkerRole> 
</ServiceDefinition> 
```

## <a name="step-5-add-the-scripts-to-your-cloud-service"></a>Paso 5: Agregar scripts al servicio en la nube 

1) En Visual Studio, haga clic con el botón derecho en su WebRole.
2) Seleccione **Agregar**.
3) Seleccione **Elemento existente**.
4) En el explorador de archivos, vaya al escritorio donde almacenó los archivos **TLSsettings.PS1** y **RunTLSSettings.cmd** 
5) Seleccione los dos archivos para agregarlos a su proyecto de Cloud Services.

## <a name="step-6-enable-copy-to-output-directory"></a>Paso 6: Habilitar la copia en el directorio de salida

Para asegurarse de que los scripts se cargan con cada actualización insertada desde Visual Studio, la configuración de *Copiar en el directorio de salida* debe establecerse en *Copiar siempre*.

1) En su WebRole, haga clic con el botón derecho en RunTLSSettings.cmd.
2) Seleccione **Propiedades**.
3) En la pestaña de propiedades, cambie *Copiar en el directorio de salida* a *Copiar siempre"*
4) Repita los pasos para **TLSsettings. ps1**.

## <a name="step-7-publish--validate"></a>Paso 7: Publicar y validar

Ahora que se han completado los pasos anteriores, publique la actualización en el servicio en la nube existente. 

Puede usar [SSLLabs](https://www.ssllabs.com/) para validar el estado de TLS de sus puntos de conexión. 

 
