---
title: '.NET SDK: Operaciones de administración en Azure Data Lake Storage Gen1 cuentas | Microsoft Docs'
description: Con Azure Data Lake Storage Gen1 y el SDK de .NET se realizan operaciones de administración de cuentas en Data Lake Storage Gen1
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: ea57d5a9-2929-4473-9d30-08227912aba7
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 8da40aa04381542c8c750c8d7e33c9a29879371d
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65900881"
---
# <a name="account-management-operations-on-azure-data-lake-storage-gen1-using-net-sdk"></a>Operaciones de administración de cuentas en Azure Data Lake Storage Gen1 mediante el SDK de .NET.
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [API DE REST](data-lake-store-get-started-rest-api.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

En este artículo aprenderá a realizar operaciones de administración de cuentas en Azure Data Lake Storage Gen1 con el SDK de .NET. Las operaciones de administración de cuentas incluyen, por ejemplo, la creación o eliminación de cuentas de Data Lake Storage Gen1, o su enumeración en una suscripción de Azure.

Para obtener instrucciones sobre cómo realizar operaciones de administración de datos en Data Lake Storage Gen1 con el SDK de .NET, consulte [Filesystem operations on Data Lake Storage Gen1 using .NET SDK](data-lake-store-data-operations-net-sdk.md) (Operaciones de sistema de archivos en Azure Data Lake Storage Gen1 con el SDK de .NET).

## <a name="prerequisites"></a>Requisitos previos
* **Visual Studio 2013 o versiones posteriores**. Las instrucciones siguientes usan Visual Studio de 2019.

* **Una suscripción de Azure**. Consulte [Obtención de una versión de evaluación gratuita](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-a-net-application"></a>Creación de una aplicación .NET
1. En Visual Studio, seleccione el **archivo** menú, **New**y, a continuación, **proyecto**.
2. Elija **aplicación de consola (.NET Framework)** y, a continuación, seleccione **siguiente**.
3. En **nombre del proyecto**, escriba `CreateADLApplication`y, a continuación, seleccione **crear**.

4. Agregue los paquetes NuGet al proyecto.

   1. Haga clic con el botón derecho en el Explorador de soluciones y haga clic en **Administrar paquetes de NuGet**.
   2. En la pestaña **Administrador de paquetes NuGet**, asegúrese de que la opción **Origen del paquete** esté establecida en **nuget.org** y que esté activada la casilla **Incluir versión preliminar**.
   3. Busque e instale los siguientes paquetes NuGet:

      * `Microsoft.Azure.Management.DataLake.Store` - En este tutorial se usa v2.1.3 (versión preliminar).
      * `Microsoft.Rest.ClientRuntime.Azure.Authentication` - En este tutorial se usa v2.2.12.

        ![Incorporación de un origen de NuGet](./media/data-lake-store-get-started-net-sdk/data-lake-store-install-nuget-package.png "Creación de una cuenta de Azure Data Lake")
   4. Cierre el **Administrador de paquetes NuGet**.
5. Abra **Program.cs**, elimine el código existente e incluya las siguientes instrucciones para agregar referencias a espacios de nombres.

        using System;
        using System.IO;
        using System.Linq;
        using System.Text;
        using System.Threading;
        using System.Collections.Generic;
        using System.Security.Cryptography.X509Certificates; // Required only if you are using an Azure AD application created with certificates
                
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.Store.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

6. Declare las variables y proporcione los valores de los marcadores de posición. Además, asegúrese de que la ruta de acceso local y el nombre de archivo que proporcione existen en el equipo.

        namespace SdkSample
        {
            class Program
            {
                private static DataLakeStoreAccountManagementClient _adlsClient;
                
                private static string _adlsAccountName;
                private static string _resourceGroupName;
                private static string _location;
                private static string _subId;

                private static void Main(string[] args)
                {
                    _adlsAccountName = "<DATA-LAKE-STORAGE-GEN1-NAME>.azuredatalakestore.net"; 
                    _resourceGroupName = "<RESOURCE-GROUP-NAME>"; 
                    _location = "East US 2";
                    _subId = "<SUBSCRIPTION-ID>";                    
                }
            }
        }

En las restantes secciones de este artículo, se puede ver cómo se utilizan los métodos .NET disponibles para realizar operaciones como la autenticación, la carga de archivos, etc.

## <a name="authentication"></a>Authentication

* Para la autenticación del usuario final para la aplicación, consulte [End-user authentication with Data Lake Storage Gen1 using .NET SDK](data-lake-store-end-user-authenticate-net-sdk.md) (Autenticación del usuario final con Data Lake Storage Gen1 mediante el SDK de .NET).
* Para la autenticación entre servicios para la aplicación, consulte [Service-to-service authentication with Data Lake Storage Gen1 using .NET SDK](data-lake-store-service-to-service-authenticate-net-sdk.md) (Autenticación entre servicios con Data Lake Storage Gen1 mediante el SDK de .NET).

## <a name="create-client-object"></a>Creación del objeto de cliente
El fragmento de código siguiente crea el objeto de cliente de la cuenta de Data Lake Storage Gen1, que se usa para emitir solicitudes de administración de cuentas para el servicio, como, por ejemplo, crear o eliminar una cuenta.

    // Create client objects and set the subscription ID
    _adlsClient = new DataLakeStoreAccountManagementClient(armCreds) { SubscriptionId = _subId };
    
## <a name="create-a-data-lake-storage-gen1-account"></a>Creación de una cuenta de Data Lake Storage Gen1
El fragmento de código siguiente crea una cuenta de Data Lake Storage Gen1 en la suscripción de Azure que proporcionó al crear el objeto de cliente de la cuenta de Data Lake Storage Gen1.

    // Create Data Lake Storage Gen1 account
    var adlsParameters = new DataLakeStoreAccount(location: _location);
    _adlsClient.Account.Create(_resourceGroupName, _adlsAccountName, adlsParameters);

## <a name="list-all-data-lake-storage-gen1-accounts-within-a-subscription"></a>Enumerar todas las cuentas de Data Lake Storage Gen1 de una suscripción
Agregue el método siguiente a la definición de la clase. En el siguiente fragmento de código se enumeran todas las cuentas de Data Lake Storage Gen1 de una suscripción de Azure dada.

    // List all Data Lake Storage Gen1 accounts within the subscription
    public static List<DataLakeStoreAccountBasic> ListAdlStoreAccounts()
    {
        var response = _adlsClient.Account.List(_adlsAccountName);
        var accounts = new List<DataLakeStoreAccountBasic>(response);

        while (response.NextPageLink != null)
        {
            response = _adlsClient.Account.ListNext(response.NextPageLink);
            accounts.AddRange(response);
        }

        return accounts;
    }

## <a name="delete-a-data-lake-storage-gen1-account"></a>Eliminación de una cuenta de Data Lake Storage Gen1
En el siguiente fragmento de código se elimina la cuenta de Data Lake Storage Gen1 que creó anteriormente.

    // Delete Data Lake Storage Gen1 account
    _adlsClient.Account.Delete(_resourceGroupName, _adlsAccountName);

## <a name="see-also"></a>Vea también
* [Filesystem operations on Data Lake Storage Gen1 using .NET SDK](data-lake-store-data-operations-net-sdk.md) (Operaciones del sistema de archivos en Data Lake Storage Gen1 mediante el SDK de .NET).
* [Data Lake Storage Gen1 .NET SDK Reference](https://docs.microsoft.com/dotnet/api/overview/azure/data-lake-store?view=azure-dotnet) (Referencia del SDK de .NET de Data Lake Storage Gen1)

## <a name="next-steps"></a>Pasos siguientes
* [Protección de datos en Data Lake Storage Gen1](data-lake-store-secure-data.md)
