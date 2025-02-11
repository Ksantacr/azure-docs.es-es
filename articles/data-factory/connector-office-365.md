---
title: Copiar datos de Office 365 mediante Azure Data Factory | Microsoft Docs
description: Obtenga información sobre cómo copiar datos desde Office 365 a almacenes de datos receptores compatibles mediante una actividad de copia de una canalización de Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: jingwang
ms.openlocfilehash: 80ef8870bafa00f3debda99db299018a39d42a82
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66245033"
---
# <a name="copy-data-from-office-365-into-azure-using-azure-data-factory"></a>Copiar datos de Office 365 en Azure mediante Azure Data Factory

Azure Data Factory permite incluir los datos importantes de la organización en el inquilino de Office 365 en Azure de una manera escalable, además de crear aplicaciones de análisis y extraer información en función de estos importantes recursos de datos. La integración con Privileged Access Management proporciona control de acceso seguro para los datos protegidos valiosos en Office 365.  Para más información sobre la conexión de datos de Microsoft Graph, consulte [este vínculo](https://docs.microsoft.com/graph/data-connect-concept-overview).

En este artículo se explica el uso de la actividad de copia de Azure Data Factory para copiar datos de Office 365. El documento se basa en el artículo de [introducción a la actividad de copia](copy-activity-overview.md) que describe información general de la actividad de copia.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Por ahora, dentro de una actividad de copia único solo puede **copiar datos de Office 365 en [Azure Blob Storage](connector-azure-blob-storage.md), [Gen1 de almacenamiento de Azure Data Lake](connector-azure-data-lake-store.md), y [Azure Data Lake Storage Gen2 ](connector-azure-data-lake-storage.md) en formato JSON** (tipo setOfObjects). Si desea cargar Office 365 en otros tipos de almacenes de datos o en otros formatos, puede encadenar la primera actividad de copia con una actividad de copia posterior para cargar más datos en cualquiera de los [almacenes de destino de ADF admitidos](copy-activity-overview.md#supported-data-stores-and-formats); consulte la columna "Se admite como receptor" de la tabla "Almacenes de datos y formatos que se admiten".

>[!IMPORTANT]
>- La suscripción de Azure que contiene la factoría de datos y el almacén de datos del receptor debe estar en el mismo inquilino de Azure Active Directory (Azure AD) que Office 365.
>- Asegúrese de que la región de Azure Integration Runtime utilizada para la actividad de copia y el destino están en la misma región donde se encuentra el buzón de los usuarios del inquilino de Office 365. Haga clic [aquí](concepts-integration-runtime.md#integration-runtime-location) para obtener información sobre cómo se determina la ubicación de Azure IR. Consulte la [tabla aquí](https://docs.microsoft.com/graph/data-connect-datasets#regions) para obtener la lista de las regiones admitidas de Office y las regiones de Azure correspondientes.
>- Autenticación de entidad de servicio es el único mecanismo de autenticación admitido para Azure Blob Storage, Azure Data Lake Storage Gen1 y Gen2 de almacenamiento de Azure Data Lake como almacenes de destino.

## <a name="prerequisites"></a>Requisitos previos

Para copiar datos de Office 365 en Azure, es preciso completar los pasos de requisitos previos siguientes:

- El administrador de inquilinos de Office 365 debe completar las acciones de incorporación como se describe [aquí](https://docs.microsoft.com/graph/data-connect-get-started).
- Cree y configure una aplicación web de Azure AD en Azure Active Directory.  Para obtener instrucciones, vea cómo [crear una aplicación de Azure AD](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application).
- Anote los siguientes valores; los usará para definir el servicio vinculado para Office 365:
    - Identificador de inquilino. Para obtener instrucciones, vea [Obtención del identificador de inquilino](../active-directory/develop/howto-create-service-principal-portal.md#get-values-for-signing-in).
    - Identificador de aplicación y clave de aplicación.  Para obtener instrucciones, vea [Obtención del id. y la clave de autenticación de la aplicación](../active-directory/develop/howto-create-service-principal-portal.md#get-values-for-signing-in).
- Agregue la identidad del usuario que va a realizar la solicitud de acceso a los datos como propietario de la aplicación web de Azure AD (aplicación web de Azure AD > Configuración > Propietarios > Agregar propietario). 
    - La identidad del usuario debe pertenecer a la organización de Office 365 de la que obtendrá los datos. Además, no debe un usuario invitado.

## <a name="approving-new-data-access-requests"></a>Aprobar nuevas solicitudes de acceso a los datos

Si es la primera vez que solicita datos para este contexto (una combinación de los datos de tabla a los que se accede, la cuenta de destino en la que se cargan los datos y la identidad de usuario que realiza la solicitud de acceso a los datos), verá el estado de la actividad de copia como "En curso", y solo al hacer clic en el [vínculo "Detalles" en Acciones](copy-activity-overview.md#monitoring), verá el estado como "RequestingConsent".  Un miembro del grupo de aprobadores de acceso a datos debe aprobar la solicitud en Privileged Access Management antes de pasar a la extracción de datos.

Consulte [aquí](https://docs.microsoft.com/graph/data-connect-tips#approve-pam-requests-via-office-365-admin-portal) cómo el aprobador puede aprobar la solicitud de acceso a los datos y haga clic [aquí](https://docs.microsoft.com/graph/data-connect-pam) para obtener una explicación sobre la integración total con Privileged Access Management, incluida la forma de configurar el grupo de aprobadores de acceso a los datos.

## <a name="policy-validation"></a>Validación de directivas

Si ADF se crea como parte de una aplicación administrada y las asignaciones de directivas de Azure se realizan en los recursos dentro del grupo de recursos de administración, entonces, para cada ejecución de la actividad de copia, ADF comprobará para asegurarse de que se aplican las asignaciones de directivas. Consulte [aquí](https://docs.microsoft.com/graph/data-connect-policies#policies) para obtener una lista de las directivas admitidas.

## <a name="getting-started"></a>Introducción

>[!TIP]
>Para ver un tutorial sobre el uso del conector de Office 365, consulte el artículo [Carga de datos de Office 365](load-office-365-data.md).

Puede crear una canalización con la actividad de copia mediante una de las siguientes herramientas o SDK. Seleccione un vínculo para acceder a un tutorial con instrucciones detalladas sobre cómo crear una canalización con una actividad de copia. 

- [Azure Portal](quickstart-create-data-factory-portal.md)
- [.NET SDK](quickstart-create-data-factory-dot-net.md)
- [SDK de Python](quickstart-create-data-factory-python.md)
- [Azure PowerShell](quickstart-create-data-factory-powershell.md)
- [API DE REST](quickstart-create-data-factory-rest-api.md)
- [Plantilla de Azure Resource Manager](quickstart-create-data-factory-resource-manager-template.md) 

Las secciones siguientes proporcionan detalles sobre las propiedades que se usan para definir entidades de Data Factory específicas del conector de Office 365.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado de Office 365:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type debe establecerse en: **Office365** | Sí |
| office365TenantId | Id. de inquilino de Azure al que pertenece la cuenta de Office 365. | Sí |
| servicePrincipalTenantId | Especifique la información del inquilino en el que reside la aplicación web de Azure AD. | Sí |
| servicePrincipalId | Especifique el id. de cliente de la aplicación. | Sí |
| servicePrincipalKey | Especifique la clave de la aplicación. Marque este campo como SecureString para almacenarlo de forma segura en Data Factory. | Sí |
| connectVia | El entorno de ejecución de integración que se usará para conectarse al almacén de datos.  Si no se especifica, se usará Azure Integration Runtime. | Sin  |

>[!NOTE]
> La diferencia entre **office365TenantId** y **servicePrincipalTenantId** y el valor correspondiente que se debe proporcionar:
>- Si es un desarrollador empresarial que va a desarrollar una aplicación con los datos de Office 365 para usarla en su organización, debe proporcionar el mismo identificador de inquilino para ambas propiedades, que es el identificador de inquilino de AAD de su organización.
>- Si es un ISV que va a desarrollar una aplicación para los clientes, office365TenantId será el identificador de inquilino de AAD (instalador de la aplicación) del cliente y servicePrincipalTenantId será el identificador de inquilino de AAD de la empresa.

**Ejemplo:**

```json
{
    "name": "Office365LinkedService",
    "properties": {
        "type": "Office365",
        "typeProperties": {
            "office365TenantId": "<Office 365 tenant id>",
            "servicePrincipalTenantId": "<AAD app service principal tenant id>",
            "servicePrincipalId": "<AAD app service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<AAD app service principal key>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Propiedades del conjunto de datos

Si desea ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte el artículo sobre [conjuntos de datos](concepts-datasets-linked-services.md). En esta sección se proporciona una lista de las propiedades que admite el conjunto de datos de Office 365.

Para copiar datos de Office 365, se admiten las siguientes propiedades:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del conjunto de datos debe establecerse en: **Office365Table** | Sí |
| tableName | Nombre del conjunto de datos para extraer de Office 365. Haga clic [aquí](https://docs.microsoft.com/graph/data-connect-datasets#datasets) para obtener la lista de conjuntos de datos de Office 365 disponibles para la extracción. | Sí |
| allowedGroups | Predicado de selección de grupo.  Utilice esta propiedad para seleccionar hasta 10 grupos de usuarios para los que se recuperarán los datos.  Si se especifica ningún grupo, se devolverán datos para toda la organización. | Sin  |
| userScopeFilterUri | Cuando `allowedGroups` no se especifica la propiedad, puede usar una expresión de predicado que se aplica en todo el inquilino para filtrar las filas específicas para extraer de Office 365. El formato de predicado debe coincidir con el formato de la consulta de Microsoft Graph API, por ejemplo, `https://graph.microsoft.com/v1.0/users?$filter=Department eq 'Finance'`. | No |
| dateFilterColumn | Nombre de la columna de filtro de fecha y hora. Utilice esta propiedad para limitar el intervalo de tiempo para que Office 365 se extraen los datos. | Sí, si el conjunto de datos tiene una o varias columnas de fecha y hora. Consulte [aquí](https://docs.microsoft.com/graph/data-connect-filtering#filtering) para obtener la lista de conjuntos de datos que requieren este filtro de fecha y hora. |
| startTime | Inicie el valor de fecha y hora para el filtro. | Sí si `dateFilterColumn` se especifica |
| endTime | Finalizar el valor de fecha y hora para el filtro. | Sí si `dateFilterColumn` se especifica |

**Ejemplo**

```json
{
    "name": "DS_May2019_O365_Message",
    "properties": {
        "type": "Office365Table",
        "linkedServiceName": {
            "referenceName": "<Office 365 linked service name>",
            "type": "LinkedServiceReference"
        },
        "structure": [
            {
                "name": "Id",
                "type": "String",
                "description": "The unique identifier of the event."
            },
            {
                "name": "CreatedDateTime",
                "type": "DateTime",
                "description": "The date and time that the event was created."
            },
            {
                "name": "LastModifiedDateTime",
                "type": "DateTime",
                "description": "The date and time that the event was last modified."
            },
            {
                "name": "ChangeKey",
                "type": "String",
                "description": "Identifies the version of the event object. Every time the event is changed, ChangeKey changes as well. This allows Exchange to apply changes to the correct version of the object."
            },
            {
                "name": "Categories",
                "type": "String",
                "description": "The categories associated with the event. Format: ARRAY<STRING>"
            },
            {
                "name": "OriginalStartTimeZone",
                "type": "String",
                "description": "The start time zone that was set when the event was created. See DateTimeTimeZone for a list of valid time zones."
            },
            {
                "name": "OriginalEndTimeZone",
                "type": "String",
                "description": "The end time zone that was set when the event was created. See DateTimeTimeZone for a list of valid time zones."
            },
            {
                "name": "ResponseStatus",
                "type": "String",
                "description": "Indicates the type of response sent in response to an event message. Format: STRUCT<Response: STRING, Time: STRING>"
            },
            {
                "name": "iCalUId",
                "type": "String",
                "description": "A unique identifier that is shared by all instances of an event across different calendars."
            },
            {
                "name": "ReminderMinutesBeforeStart",
                "type": "Int32",
                "description": "The number of minutes before the event start time that the reminder alert occurs."
            },
            {
                "name": "IsReminderOn",
                "type": "Boolean",
                "description": "Set to true if an alert is set to remind the user of the event."
            },
            {
                "name": "HasAttachments",
                "type": "Boolean",
                "description": "Set to true if the event has attachments."
            },
            {
                "name": "Subject",
                "type": "String",
                "description": "The text of the event's subject line."
            },
            {
                "name": "Body",
                "type": "String",
                "description": "The body of the message associated with the event.Format: STRUCT<ContentType: STRING, Content: STRING>"
            },
            {
                "name": "Importance",
                "type": "String",
                "description": "The importance of the event: Low, Normal, High."
            },
            {
                "name": "Sensitivity",
                "type": "String",
                "description": "Indicates the level of privacy for the event: Normal, Personal, Private, Confidential."
            },
            {
                "name": "Start",
                "type": "String",
                "description": "The start time of the event. Format: STRUCT<DateTime: STRING, TimeZone: STRING>"
            },
            {
                "name": "End",
                "type": "String",
                "description": "The date and time that the event ends. Format: STRUCT<DateTime: STRING, TimeZone: STRING>"
            },
            {
                "name": "Location",
                "type": "String",
                "description": "Location information of the event. Format: STRUCT<DisplayName: STRING, Address: STRUCT<Street: STRING, City: STRING, State: STRING, CountryOrRegion: STRING, PostalCode: STRING>, Coordinates: STRUCT<Altitude: DOUBLE, Latitude: DOUBLE, Longitude: DOUBLE, Accuracy: DOUBLE, AltitudeAccuracy: DOUBLE>>"
            },
            {
                "name": "IsAllDay",
                "type": "Boolean",
                "description": "Set to true if the event lasts all day. Adjusting this property requires adjusting the Start and End properties of the event as well."
            },
            {
                "name": "IsCancelled",
                "type": "Boolean",
                "description": "Set to true if the event has been canceled."
            },
            {
                "name": "IsOrganizer",
                "type": "Boolean",
                "description": "Set to true if the message sender is also the organizer."
            },
            {
                "name": "Recurrence",
                "type": "String",
                "description": "The recurrence pattern for the event. Format: STRUCT<Pattern: STRUCT<Type: STRING, `Interval`: INT, Month: INT, DayOfMonth: INT, DaysOfWeek: ARRAY<STRING>, FirstDayOfWeek: STRING, Index: STRING>, `Range`: STRUCT<Type: STRING, StartDate: STRING, EndDate: STRING, RecurrenceTimeZone: STRING, NumberOfOccurrences: INT>>"
            },
            {
                "name": "ResponseRequested",
                "type": "Boolean",
                "description": "Set to true if the sender would like a response when the event is accepted or declined."
            },
            {
                "name": "ShowAs",
                "type": "String",
                "description": "The status to show: Free, Tentative, Busy, Oof, WorkingElsewhere, Unknown."
            },
            {
                "name": "Type",
                "type": "String",
                "description": "The event type: SingleInstance, Occurrence, Exception, SeriesMaster."
            },
            {
                "name": "Attendees",
                "type": "String",
                "description": "The collection of attendees for the event. Format: ARRAY<STRUCT<EmailAddress: STRUCT<Name: STRING, Address: STRING>, Status: STRUCT<Response: STRING, Time: STRING>, Type: STRING>>"
            },
            {
                "name": "Organizer",
                "type": "String",
                "description": "The organizer of the event. Format: STRUCT<EmailAddress: STRUCT<Name: STRING, Address: STRING>>"
            },
            {
                "name": "WebLink",
                "type": "String",
                "description": "The URL to open the event in Outlook Web App."
            },
            {
                "name": "Attachments",
                "type": "String",
                "description": "The FileAttachment and ItemAttachment attachments for the message. Navigation property. Format: ARRAY<STRUCT<LastModifiedDateTime: STRING, Name: STRING, ContentType: STRING, Size: INT, IsInline: BOOLEAN, Id: STRING>>"
            },
            {
                "name": "BodyPreview",
                "type": "String",
                "description": "The preview of the message associated with the event. It is in text format."
            },
            {
                "name": "Locations",
                "type": "String",
                "description": "The locations where the event is held or attended from. The location and locations properties always correspond with each other. Format:  ARRAY<STRUCT<DisplayName: STRING, Address: STRUCT<Street: STRING, City: STRING, State: STRING, CountryOrRegion: STRING, PostalCode: STRING>, Coordinates: STRUCT<Altitude: DOUBLE, Latitude: DOUBLE, Longitude: DOUBLE, Accuracy: DOUBLE, AltitudeAccuracy: DOUBLE>, LocationEmailAddress: STRING, LocationUri: STRING, LocationType: STRING, UniqueId: STRING, UniqueIdType: STRING>>"
            },
            {
                "name": "OnlineMeetingUrl",
                "type": "String",
                "description": "A URL for an online meeting. The property is set only when an organizer specifies an event as an online meeting such as a Skype meeting"
            },
            {
                "name": "OriginalStart",
                "type": "DateTime",
                "description": "The start time that was set when the event was created in UTC time."
            },
            {
                "name": "SeriesMasterId",
                "type": "String",
                "description": "The ID for the recurring series master item, if this event is part of a recurring series."
            }
        ],
        "typeProperties": {
            "tableName": "BasicDataSet_v0.Event_v1",
            "dateFilterColumn": "CreatedDateTime",
            "startTime": "2019-04-28T16:00:00.000Z",
            "endTime": "2019-05-05T16:00:00.000Z",
            "userScopeFilterUri": "https://graph.microsoft.com/v1.0/users?$filter=Department eq 'Finance'"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades que admite el origen de Office 365.

### <a name="office-365-as-source"></a>Office 365 como origen

Para copiar datos desde Office 365, establezca el tipo de origen de la actividad de copia en **Office365Source**. No se admiten propiedades adicionales en la sección **origen** de la actividad de copia.

**Ejemplo:**

```json
"activities": [
    {
        "name": "CopyFromO365ToBlob",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Office 365 input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "Office365Source"
            },
            "sink": {
                "type": "BlobSink"
            }
        }
    }
]
```

## <a name="next-steps"></a>Pasos siguientes
Consulte los [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver la lista de almacenes de datos que la actividad de copia de Azure Data Factory admite como orígenes y receptores.
