---
title: Azure IoT Hub – IP-Verbindungsfilter | Microsoft-Dokumentation
description: Verwenden der IP-Filterung zum Blockieren von Verbindungen von bestimmten IP-Adressen für Ihren Azure IoT Hub. Sie können Verbindungen von einzelnen IP-Adressen oder Bereichen von IP-Adressen blockieren.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/22/2017
ms.author: robinsh
ms.openlocfilehash: a6bd8a766f3205358a65ef2fd0816643e4261cab
ms.sourcegitcommit: c556477e031f8f82022a8638ca2aec32e79f6fd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2019
ms.locfileid: "68414315"
---
# <a name="use-ip-filters"></a>Verwenden von IP-Filtern

Die Sicherheit ist bei jeder IoT-Lösung, die auf Azure IoT Hub basiert, ein wichtiger Aspekt. Manchmal müssen Sie im Rahmen der Sicherheitskonfiguration die IP-Adressen explizit angeben, über die Geräte eine Verbindung herstellen können. Mit dem Feature *IP-Filter* können Sie Regeln konfigurieren, mit denen Datenverkehr von bestimmten IPv4-Adressen abgelehnt oder zugelassen wird.

## <a name="when-to-use"></a>Einsatzgebiete

Wenn es hilfreich ist, die IoT Hub-Endpunkte für bestimmte IP-Adressen zu blockieren, gibt es zwei spezifische Anwendungsfälle:

* Der IoT Hub sollte nur Datenverkehr aus einem angegebenen Bereich von IP-Adressen empfangen und den anderen Datenverkehr ablehnen. Ein Beispiel hierfür ist, wenn Sie IoT Hub mit [Azure Express Route](https://azure.microsoft.com/documentation/articles/expressroute-faqs/#supported-services) verwenden, um private Verbindungen zwischen IoT Hub und Ihrer lokalen Infrastruktur zu erstellen.

* Sie müssen Datenverkehr von IP-Adressen ablehnen, die vom IoT Hub-Administrator als verdächtig eingestuft wurden.

## <a name="how-filter-rules-are-applied"></a>Anwenden von Filterregeln

Die IP-Filterregeln werden auf IoT Hub-Dienstebene angewendet. Daher gelten die IP-Filterregeln für alle Verbindungen von Geräten und Back-End-Apps mit allen unterstützten Protokollen.

Alle Verbindungsversuche über eine IP-Adresse, die für eine IP-Ablehnungsregel in IoT Hub eine Übereinstimmung ergeben, werden mit dem Statuscode „401 – Nicht autorisiert“ und einer Beschreibung versehen. In der Antwortnachricht wird die IP-Regel nicht erwähnt.

## <a name="default-setting"></a>Standardeinstellung

Das Raster **IP-Filter** im Portal ist für ein IoT Hub standardmäßig leer. Diese Standardeinstellung bedeutet, dass Ihr Hub Verbindungen von allen IP-Adressen akzeptiert. Die Standardeinstellung entspricht einer Regel, bei der der IP-Adressbereich 0.0.0.0/0 zulässig ist.

![Standardeinstellungen der IP-Filterung von IoT Hub](./media/iot-hub-ip-filtering/ip-filter-default.png)

## <a name="add-or-edit-an-ip-filter-rule"></a>Hinzufügen oder Bearbeiten einer IP-Filterregel

Zum Hinzufügen einer IP-Filterregel wählen Sie **+ IP-Filterregel hinzufügen** aus.

![Hinzufügen einer IP-Filterregel zu IoT Hub](./media/iot-hub-ip-filtering/ip-filter-add-rule.png)

Nachdem Sie **IP-Filterregel hinzufügen** ausgewählt haben, füllen Sie die Felder aus.

![Nach dem Auswählen von „IP-Filterregel hinzufügen“](./media/iot-hub-ip-filtering/ip-filter-after-selecting-add.png)

* Geben Sie einen **Namen** für die IP-Filterregel an. Er muss eine eindeutige alphanumerische Zeichenfolge ohne Beachtung von Groß-/Kleinschreibung sein, die bis zu 128 Zeichen umfassen kann. Nur alphanumerische ASCII 7-Bit-Zeichen und die Zeichen `{'-', ':', '/', '\', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '''}` sind zulässig.

* Geben Sie eine einzelne IPv4-Adresse oder einen Block von IP-Adressen in CIDR-Notation ein. In CIDR-Notation steht 192.168.100.0/22 beispielsweise für die 1.024 IPv4-Adressen von 192.168.100.0 bis 192.168.103.255.

* Wählen Sie **Zulassen** oder **Blockieren** als **Aktion** für die IP-Filterregel aus.

Nachdem Sie die Felder ausgefüllt haben, wählen Sie **Speichern** aus, um die Regel zu speichern. Daraufhin wird eine Warnung mit dem Hinweis angezeigt, dass der Updatevorgang läuft.

![Benachrichtigung über das Speichern einer IP-Filterregel](./media/iot-hub-ip-filtering/ip-filter-save-new-rule.png)

Die Option **Hinzufügen** ist deaktiviert, wenn Sie das Maximum von 10 IP-Filterregeln erreichen.

Wenn Sie eine vorhandene Regel bearbeiten möchten, wählen Sie die zu ändernden Daten aus, nehmen Sie die Änderung vor, und wählen Sie **Speichern** aus, um Ihre Bearbeitung zu speichern.

> [!NOTE]
> Das Ablehnen von IP-Adressen kann andere Azure-Dienste (z. B. Azure Stream Analytics, Azure Virtual Machines oder den Geräte-Explorer im Portal) an der Interaktion mit dem IoT Hub hindern.

> [!WARNING]
> Wenn Sie Azure Stream Analytics (ASA) verwenden, um Nachrichten von einer IoT Hub-Instanz mit aktivierter IP-Filterung zu lesen, verwenden Sie den Event Hub-kompatiblen Namen und Endpunkt Ihrer IoT Hub-Instanz in der ASA-Verbindungszeichenfolge.

## <a name="delete-an-ip-filter-rule"></a>Löschen einer IP-Filterregel

Wählen Sie zum Löschen einer IP-Filterregel das Papierkorbsymbol in dieser Zeile und dann **Speichern** aus. Die Regel wird entfernt und die Änderung gespeichert.

![Löschen einer IP-Filterregel von IoT Hub](./media/iot-hub-ip-filtering/ip-filter-delete-rule.png)

## <a name="retrieve-and-update-ip-filters-using-azure-cli"></a>Abrufen und Aktualisieren von IP-Filtern über die Azure-Befehlszeilenschnittstelle

Die IP-Filter für Ihren IoT Hub können über die [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) abgerufen und aktualisiert werden.

Wenn Sie die aktuellen IP-Filter für Ihren IoT Hub abrufen möchten, führen Sie Folgendes aus:

```azurecli-interactive
az resource show -n <iothubName> -g <resourceGroupName> --resource-type Microsoft.Devices/IotHubs
```

Damit wird ein JSON-Objekt zurückgegeben, in dem Ihre vorhandenen IP-Filter unter dem Schlüssel `properties.ipFilterRules` aufgelistet sind:

```json
{
...
    "properties": {
        "ipFilterRules": [
        {
            "action": "Reject",
            "filterName": "MaliciousIP",
            "ipMask": "6.6.6.6/6"
        },
        {
            "action": "Allow",
            "filterName": "GoodIP",
            "ipMask": "131.107.160.200"
        },
        ...
        ],
    },
...
}
```

Wenn Sie einen neuen IP-Filter für Ihren IoT Hub hinzufügen möchten, führen Sie aus:

```azurecli-interactive
az resource update -n <iothubName> -g <resourceGroupName> --resource-type Microsoft.Devices/IotHubs --add properties.ipFilterRules "{\"action\":\"Reject\",\"filterName\":\"MaliciousIP\",\"ipMask\":\"6.6.6.6/6\"}"
```

Wenn Sie einen vorhandenen IP-Filter in Ihrem IoT Hub entfernen möchten, führen Sie aus:

```azurecli-interactive
az resource update -n <iothubName> -g <resourceGroupName> --resource-type Microsoft.Devices/IotHubs --add properties.ipFilterRules <ipFilterIndexToRemove>
```

Beachten Sie, dass `<ipFilterIndexToRemove>` der Reihenfolge von IP-Filtern in `properties.ipFilterRules` Ihres IoT Hub entsprechen muss.

## <a name="retrieve-and-update-ip-filters-using-azure-powershell"></a>Abrufen und Aktualisieren von IP-Filtern über Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Die IP-Filter für Ihren IoT Hub können über [Azure PowerShell](/powershell/azure/overview) abgerufen und festgelegt werden.

```powershell
# Get your IoT Hub resource using its name and its resource group name
$iothubResource = Get-AzResource -ResourceGroupName <resourceGroupNmae> -ResourceName <iotHubName> -ExpandProperties

# Access existing IP filter rules
$iothubResource.Properties.ipFilterRules |% { Write-host $_ }

# Construct a new IP filter
$filter = @{'filterName'='MaliciousIP'; 'action'='Reject'; 'ipMask'='6.6.6.6/6'}

# Add your new IP filter rule
$iothubResource.Properties.ipFilterRules += $filter

# Remove an existing IP filter rule using its name, e.g., 'GoodIP'
$iothubResource.Properties.ipFilterRules = @($iothubResource.Properties.ipFilterRules | Where 'filterName' -ne 'GoodIP')

# Update your IoT Hub resource with your updated IP filters
$iothubResource | Set-AzResource -Force
```

## <a name="update-ip-filter-rules-using-rest"></a>Aktualisieren von IP-Filterregeln mit REST

Möglicherweise müssen Sie den IP-Filter für Ihren IoT Hub auch über den REST-Endpunkt des Azure-Ressourcenanbieters abrufen und ändern. Lesen Sie dazu `properties.ipFilterRules` unter [createorupdate-Methode](https://docs.microsoft.com/rest/api/iothub/iothubresource/createorupdate).

## <a name="ip-filter-rule-evaluation"></a>Auswertung von IP-Filterregeln

IP-Filterregeln werden der Reihenfolge nach angewendet, und die erste Regel, die eine Übereinstimmung mit der IP-Adresse ergibt, bestimmt die Aktion (Zulassen oder Ablehnen).

Wenn Sie beispielsweise Adressen im Bereich 192.168.100.0/22 zulassen und alle anderen Adressen ablehnen möchten, sollte die erste Regel im Raster lauten, dass der Adressbereich 192.168.100.0/22 zulässig ist. Mit der nächsten Regel sollten alle Adressen abgelehnt werden, indem der Bereich 0.0.0.0/0 verwendet wird.

Sie können die Reihenfolge der IP-Filterregeln im Raster ändern, indem Sie auf die drei vertikal angeordneten Punkte am Anfang der Zeile klicken und Drag & Drop nutzen.

Klicken Sie auf **Speichern**, um die neue Reihenfolge der IP-Filterregeln zu speichern.

![Ändern der Reihenfolge der IP-Filterregeln von IoT Hub](./media/iot-hub-ip-filtering/ip-filter-rule-order.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [Vorgangsüberwachung](iot-hub-operations-monitoring.md)
* [IoT Hub-Metriken](iot-hub-metrics.md)
