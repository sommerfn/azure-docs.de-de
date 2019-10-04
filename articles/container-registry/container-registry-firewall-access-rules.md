---
title: Firewallregeln für den Zugriff auf die Azure-Containerregistrierung
description: Konfigurieren Sie Regeln für den Zugriff auf eine Azure-Containerregistrierung von einem Standort hinter einer Firewall.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 07/17/2019
ms.author: danlep
ms.openlocfilehash: 88b6da4e9bd2938adadadc1ef0e696399fc3c75e
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2019
ms.locfileid: "68828008"
---
# <a name="configure-rules-to-access-an-azure-container-registry-behind-a-firewall"></a>Konfigurieren von Regeln für den Zugriff auf eine Azure-Containerregistrierung von einem Standort hinter einer Firewall

Dieser Artikel erläutert, wie Sie in Ihrer Firewall Regeln konfigurieren, um den Zugriff auf eine Azure-Containerregistrierung zuzulassen. Möglicherweise muss ein Azure IoT Edge-Gerät hinter einer Firewall oder einem Proxyserver auf eine Containerregistrierung zugreifen, um ein Pull für ein Containerimage auszuführen. Oder vielleicht benötigt ein gesperrter Server in einem lokalen Netzwerk Zugriff, um ein Push für ein Image auszuführen.

Wenn Sie stattdessen in einer Containerregistrierung Zugriffsregeln für eingehenden Netzwerkdatenverkehr konfigurieren möchten, um Zugriff nur innerhalb eines virtuellen Azure-Netzwerks oder einem öffentlichen IP-Adressbereich zuzulassen, finden Sie Informationen dazu unter [Beschränken des Zugriffs auf eine Azure-Containerregistrierung aus einem virtuellen Netzwerk](container-registry-vnet.md).

## <a name="about-registry-endpoints"></a>Informationen über Registrierungsendpunkte

Um ein Pull oder ein Push für Images oder andere Artefakt für eine Azure-Containerregistrierung auszuführen, muss ein Client wie z.B. ein Docker-Daemon über HTTPS mit zwei verschiedenen Endpunkten interagieren.

* **REST-API-Endpunkt der Registrierung**: Vorgänge für die Authentifizierungs- und Registrierungsverwaltung werden über den öffentlichen REST-API-Endpunkt der Registrierung verarbeitet. Dieser Endpunkt ist die Anmeldeserver-URL der Registrierung oder ein zugeordneter IP-Adressbereich. 

* **Speicherendpunkt**: Azure [weist Blobspeicher](container-registry-storage.md) in Azure-Speicherkonten im Namen jeder Registrierung zu, um Containerimages und andere Artefakte zu verwalten. Wenn ein Client auf Imageebenen in einer Azure-Containerregistrierung zugreift, erfolgen die Anforderungen über einen Speicherkonto-Endpunkt, der von der Registrierung bereitgestellt wird.

Wenn Ihre Registrierung [georepliziert](container-registry-geo-replication.md) ist, muss ein Client möglicherweise mit den REST- und Speicherendpunkten in einer bestimmten Region oder mehreren replizierten Regionen interagieren.

## <a name="allow-access-to-rest-and-storage-urls"></a>Zulassen des Zugriffs auf REST- und Speicher-URLs

* **REST-Endpunkt**: Erlaubt den Zugriff auf die Registrierungsserver-URL wie z.B. `myregistry.azurecr.io`.
* **Speicherendpunkt**: Erlaubt den Zugriff auf alle Azure-Blobspeicherkonten mithilfe des Platzhalters `*.blob.core.windows.net`.


## <a name="allow-access-by-ip-address-range"></a>Zulassen des Zugriffs nach IP-Adressbereich

Wenn Sie den Zugriff auf bestimmte IP-Adressen zulassen müssen, laden Sie die JSON-Datei [Azure IP Ranges and Service Tags – Public Cloud](https://www.microsoft.com/download/details.aspx?id=56519) (Azure-IP-Bereiche und Diensttags – öffentliche Cloud) herunter.

Suchen Sie in der Datei nach **AzureContainerRegistry**, um die IP-Adressbereiche für ACR-REST-Endpunkte zu finden.

> [!IMPORTANT]
> Die IP-Adressbereiche für Azure-Dienste können sich ändern, Updates werden wöchentlich veröffentlicht. Laden Sie die JSON-Datei regelmäßig herunter, und aktualisieren Sie Ihre Zugriffsregeln, sofern erforderlich. Wenn Ihr Szenario das Konfigurieren von Netzwerksicherheitsgruppen-Regeln in einem virtuellen Azure-Netzwerk für den Zugriff auf eine Azure-Containerregistrierung umfasst, verwenden Sie stattdessen das [Diensttag](#allow-access-by-service-tag) **AzureContainerRegistry**.
>

### <a name="rest-ip-addresses-for-all-regions"></a>REST-IP-Adressen für alle Regionen

```json
{
  "name": "AzureContainerRegistry",
  "id": "AzureContainerRegistry",
  "properties": {
    "changeNumber": 10,
    "region": "",
    "platform": "Azure",
    "systemService": "AzureContainerRegistry",
    "addressPrefixes": [
      "13.66.140.72/29",
    [...]
```

### <a name="rest-ip-addresses-for-a-specific-region"></a>REST-IP-Adressen für eine bestimmte Region

Suchen Sie nach der gewünschten Region, z.B. **AzureContainerRegistry.AustraliaEast**.

```json
{
  "name": "AzureContainerRegistry.AustraliaEast",
  "id": "AzureContainerRegistry.AustraliaEast",
  "properties": {
    "changeNumber": 1,
    "region": "australiaeast",
    "platform": "Azure",
    "systemService": "AzureContainerRegistry",
    "addressPrefixes": [
      "13.70.72.136/29",
    [...]
```

### <a name="storage-ip-addresses-for-all-regions"></a>Speicher-IP-Adressen für alle Regionen

```json
{
  "name": "Storage",
  "id": "Storage",
  "properties": {
    "changeNumber": 19,
    "region": "",
    "platform": "Azure",
    "systemService": "AzureStorage",
    "addressPrefixes": [
      "13.65.107.32/28",
    [...]
```

### <a name="storage-ip-addresses-for-specific-regions"></a>Speicher-IP-Adressen für eine bestimmte Region

Suchen Sie nach einer bestimmten Region, z.B. **Storage.AustraliaCentral**.

```json
{
  "name": "Storage.AustraliaCentral",
  "id": "Storage.AustraliaCentral",
  "properties": {
    "changeNumber": 1,
    "region": "australiacentral",
    "platform": "Azure",
    "systemService": "AzureStorage",
    "addressPrefixes": [
      "52.239.216.0/23"
    [...]
```

## <a name="allow-access-by-service-tag"></a>Zulassen des Zugriffs nach Diensttag

Verwenden Sie in einem virtuellen Azure-Netzwerk Netzwerksicherheitsregeln, um den Datenverkehr von einer Ressource wie z.B. einem virtuellen Computer zu einer Containerregistrierung zu filtern. Um die Erstellung der Azure-Netzwerkregeln zu vereinfachen, verwenden Sie das [Diensttag](../virtual-network/security-overview.md#service-tags) **AzureContainerRegistry**. Ein Diensttag repräsentiert eine Gruppe von IP-Adresspräfixen, mit denen global oder pro Azure-Region auf einen Azure-Dienst zugegriffen werden kann. Das Tag wird automatisch aktualisiert, wenn sich Adressen ändern. 

Erstellen Sie z.B. eine ausgehende Netzwerksicherheitsgruppen-Regel mit dem Ziel **AzureContainerRegistry**, um Datenverkehr zu einer Azure-Containerregistrierung zuzulassen. Um den Zugriff auf das Diensttag nur in einer bestimmten Region zuzulassen, geben Sie die Region in folgendem Format an: **AzureContainerRegistry**.[*Regionsname*].

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu [bewährten Methoden für die Netzwerksicherheit](../security/fundamentals/network-best-practices.md)

* Weitere Informationen zu [Sicherheitsgruppen](/azure/virtual-network/security-overview) in einem virtuellen Azure-Netzwerk



<!-- IMAGES -->

<!-- LINKS - External -->

<!-- LINKS - Internal -->

