---
title: azcopy login | Microsoft-Dokumentation
description: Dieser Artikel enthält Referenzinformationen zum Befehl „azcopy login“.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 058506110a8ac4b11f272406a854f72062a1c90d
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72514707"
---
# <a name="azcopy-login"></a>azcopy login

Dient zur Anmeldung bei Azure Active Directory, um den Zugriff auf Azure Storage-Ressourcen zu ermöglichen.

## <a name="synopsis"></a>Zusammenfassung

Melden Sie sich bei Azure Active Directory an, um auf Azure Storage-Ressourcen zugreifen zu können.

Zur Autorisierung für Ihr Azure Storage-Konto müssen Sie Ihrem Benutzerkonto die Rolle **Mitwirkender an Storage-Blobdaten** im Kontext des Speicherkontos, der übergeordneten Ressourcengruppe oder des übergeordneten Abonnements zuweisen.

Dieser Befehl speichert verschlüsselte Anmeldeinformationen für den aktuellen Benutzer mithilfe der integrierten Betriebssystemmechanismen zwischen.

Weitere Informationen finden Sie in den Beispielen.

> [!IMPORTANT]
> Wenn Sie eine Umgebungsvariable über die Befehlszeile festlegen, kann diese Variable in Ihrem Befehlszeilenverlauf gelesen werden. Es empfiehlt sich gegebenenfalls, Variablen mit Anmeldeinformationen aus Ihrem Befehlszeilenverlauf zu löschen. Wenn Sie verhindern möchten, dass Variablen in Ihrem Verlauf angezeigt werden, können Sie den Benutzer mit einem Skript zur Eingabe seiner Anmeldeinformationen auffordern und die Umgebungsvariable festlegen.

```azcopy
azcopy login [flags]
```

## <a name="examples"></a>Beispiele

Interaktive Anmeldung mit allgemeiner standardmäßiger AAD-Mandanten-ID:

```azcopy
azcopy login
```

Interaktive Anmeldung mit einer spezifischen Mandanten-ID:

```azcopy
azcopy login --tenant-id "[TenantID]"
```

Anmeldung mit der systemseitig zugewiesenen Identität einer VM:

```azcopy
azcopy login --identity
```

Anmeldung mit der benutzerseitig zugewiesenen Identität einer VM und einer Client-ID der Dienstidentität:

```azcopy
azcopy login --identity --identity-client-id "[ServiceIdentityClientID]"
```

Anmeldung mit der benutzerseitig zugewiesenen Identität einer VM und einer Objekt-ID der Dienstidentität:

```azcopy
azcopy login --identity --identity-object-id "[ServiceIdentityObjectID]"
```

Anmeldung mit der benutzerseitig zugewiesenen Identität einer VM und einer Ressourcen-ID der Dienstidentität:

```azcopy
azcopy login --identity --identity-resource-id "/subscriptions/<subscriptionId>/resourcegroups/myRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myID"
```

Anmeldung als Dienstprinzipal unter Verwendung eines geheimen Clientschlüssels. Legen Sie die Umgebungsvariable „AZCOPY_SPA_CLIENT_SECRET“ auf den geheimen Clientschlüssel für die geheimnisbasierte Dienstprinzipalautorisierung fest.

```azcopy
azcopy login --service-principal
```

Anmeldung als Dienstprinzipal unter Verwendung eines Zertifikats und eines Kennworts. Legen Sie die Umgebungsvariable „AZCOPY_SPA_CERT_PASSWORD“ auf das Kennwort des Zertifikats für die zertifikatbasierte Dienstprinzipalautorisierung fest.

```azcopy
azcopy login --service-principal --certificate-path /path/to/my/cert
```

Behandeln Sie „/path/to/my/cert“ als Pfad zu einer PEM- oder PKCS12-Datei. AzCopy ruft Ihr Zertifikat nicht aus dem Zertifikatspeicher des Systems ab.

„--certificate-path“ ist bei der zertifikatbasierten Dienstprinzipalautorisierung zwingend erforderlich.

## <a name="options"></a>Optionen

|Option|BESCHREIBUNG|
|--|--|
|--application-id string|Anwendungs-ID der benutzerseitig zugewiesenen Identität. Erforderlich für die Dienstprinzipalautorisierung.|
|--certificate-path string|Pfad zum Zertifikat für die SPN-Authentifizierung. Erforderlich für die zertifikatbasierte Dienstprinzipalautorisierung.|
|-h, --help|Zeigt den Hilfeinhalt für den Befehl „login“ an.|
|--identity|Melden Sie sich mit der Identität des virtuellen Computers an. Diese wird auch als verwaltete Dienstidentität (Managed Service Identity, MSI) bezeichnet.|
|--identity-client-id string|Client-ID der benutzerseitig zugewiesenen Identität.|
|--identity-object-id string|Objekt-ID der benutzerseitig zugewiesenen Identität.|
|--identity-resource-id string|Ressourcen-ID der benutzerseitig zugewiesenen Identität.|
|--service-principal|Melden Sie sich per Dienstprinzipalname (Service Principal Name, SPN) unter Verwendung eines Zertifikats oder Geheimnisses an. Der geheime Clientschlüssel oder das Zertifikatkennwort muss in der entsprechenden Umgebungsvariablen platziert werden. Geben Sie `AzCopy env` ein, um Namen und Beschreibungen von Umgebungsvariablen anzuzeigen.|
|--tenant-id string| Die Azure Active Directory-Mandanten-ID, die für die interaktive OAuth-Geräteanmeldung verwendet werden soll.|

## <a name="options-inherited-from-parent-commands"></a>Von übergeordneten Befehlen geerbte Optionen

|Option|BESCHREIBUNG|
|---|---|
|–cap-mbps uint32|Begrenzt die Übertragungsrate (in Megabit pro Sekunde). Der Schritt-für-Schritt-Durchsatz kann von der Obergrenze geringfügig abweichen. Wenn diese Option auf „null“ festgelegt oder weggelassen wird, ist der Durchsatz nicht begrenzt.|
|–output-type string|Format der Befehlsausgabe. Folgende Optionen sind verfügbar: „text“ und „json“. Der Standardwert lautet „text“.|

## <a name="see-also"></a>Weitere Informationen

- [azcopy](storage-ref-azcopy.md)
