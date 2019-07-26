---
title: Rollenschema in Azure Cloud Services | Microsoft-Dokumentation
ms.custom: ''
ms.date: 12/07/2016
services: cloud-services
ms.service: cloud-services
ms.topic: reference
caps.latest.revision: 12
author: georgewallace
ms.author: gwallace
ms.openlocfilehash: 481301333ada39297bf2813bbea5f096c2abd3ad
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2019
ms.locfileid: "68360654"
---
# <a name="azure-cloud-services-config-role-schema"></a>Rollenschema der Azure Cloud Services-Konfiguration

Das `Role`-Element der Konfigurationsdatei gibt die Anzahl der Rolleninstanzen, die für jede Rolle im Dienst bereitgestellt werden, die Werte aller Konfigurationseinstellungen und die Fingerabdrücke für alle einer Rolle zugeordneten Zertifikate an.

Weitere Informationen zum Azure-Dienstkonfigurationsschema finden Sie unter [Azure Cloud Services Config Schema (.cscfg File)](schema-cscfg-file.md) (Azure Cloud Services-Konfigurationsschema (CSCFG-Datei)). Weitere Informationen zum Azure-Dienstdefinitionsschema finden Sie unter [Azure Cloud Services Definition Schema (.csdef File)](schema-csdef-file.md) (Azure Cloud Services-Definitionsschema (CSDEF-Datei)).

##  <a name="Role"></a> Role-Element
Das folgende Beispiel zeigt das `Role`-Element und seine untergeordneten Elemente.

```xml 
<ServiceConfiguration>
  <Role name="<role-name>" vmName="<vm-name>">
    <Instances count="<number-of-instances>"/>
    <ConfigurationSettings>
      <Setting name="<setting-name>" value="<setting-value>" />
    </ConfigurationSettings>
    <Certificates>
      <Certificate name="<certificate-name>" thumbprint="<certificate-thumbprint>" thumbprintAlgorithm="<algorithm>"/>
    </Certificates>
  </Role>
</ServiceConfiguration>
```

In der folgenden Tabelle sind die Attribute des `Role`-Elements beschrieben.

| Attribut | BESCHREIBUNG |
| --------- | ----------- |
| name   | Erforderlich. Gibt den Namen der Rolle an. Der Name muss mit dem in der Dienstdefinitionsdatei angegebenen Namen für die Rolle übereinstimmen.|
| vmName | Optional. Gibt den DNS-Namen für einen virtuellen Computer an. Der Name darf höchstens 10 Zeichen enthalten.|

Die folgende Tabelle beschreibt die untergeordneten Elemente des `Role`-Elements.

| Element | BESCHREIBUNG |
| ------- | ----------- |
| Instanzen | Erforderlich. Gibt die Anzahl von Instanzen an, die für die Rolle bereitgestellt werden sollen. Die Anzahl von Instanzen wird durch ein Integer für das `count`-Attribut definiert.|
| Einstellung   | Optional. Gibt einen Einstellungsnamen und -Wert in einer Auflistung von Einstellungen für eine Rolle an. Der Name der Einstellung wird durch eine Zeichenfolge für das `name`-Attribut und der Wert der Einstellung durch eine Zeichenfolge für das `value`-Attribut definiert.|
| Zertifikat | Optional. Gibt den Namen, den Fingerabdruck und den Algorithmus eines Dienstzertifikats an, das der Rolle zugeordnet werden soll. Der Name des Zertifikats wird durch eine Zeichenfolge für das `name`-Attribut definiert. Der Zertifikatfingerabdruck wird durch eine Zeichenfolge von Hexadezimalzahlen ohne Leerzeichen für das `thumbprint`-Attribut definiert. Die Hexadezimalzahlen müssen durch Ziffern und Großbuchstaben dargestellt werden. Der Zertifikatalgorithmus wird durch eine Zeichenfolge für das `thumbprintAlgorithm`-Attribut definiert.|

## <a name="see-also"></a>Siehe auch
[Azure Cloud Services Config Schema (.cscfg File)](schema-cscfg-file.md) (Azure Cloud Services-Konfigurationsschema (CSCFG-Datei))