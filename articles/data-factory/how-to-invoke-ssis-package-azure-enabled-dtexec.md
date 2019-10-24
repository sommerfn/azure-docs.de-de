---
title: Ausführen von SSIS-Paketen (SQL Server Integration Services) mit Azure-fähigem Hilfsprogramm „dtexec“ | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie SSIS-Pakete (SQL Server Integration Services) mit Azure-fähigem Hilfsprogramm „dtexec“ ausführen.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 09/21/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 740e53728356755bcc42e1e0aafb64992b30e113
ms.sourcegitcommit: 961468fa0cfe650dc1bec87e032e648486f67651
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72249029"
---
# <a name="run-sql-server-integration-services-ssis-packages-with-azure-enabled-dtexec-utility"></a>Ausführen von SSIS-Paketen (SQL Server Integration Services) mit Azure-fähigem Hilfsprogramm „dtexec“
In diesem Artikel wird das Azure-fähige Befehlszeilenhilfsprogramm **dtexec** (**AzureDTExec**) beschrieben.  Es wird verwendet, um SSIS-Pakete unter der Azure-SSIS Integration Runtime (IR) in Azure Data Factory (ADF) auszuführen.

Das traditionelle Hilfsprogramm **dtexec** wird mit SQL Server bereitgestellt. Weitere Informationen finden Sie in der Dokumentation zum Hilfsprogramm [dtexec](https://docs.microsoft.com/sql/integration-services/packages/dtexec-utility?view=sql-server-2017).  Es wird häufig von externen Orchestratoren/Planern wie Active Batch, Control-M usw. aufgerufen, um SSIS-Pakete lokal auszuführen.  Das moderne Hilfsprogramm **AzureDTExec** wird mit dem SSMS-Tool (SQL Server Management Studio) bereitgestellt.  Es kann auch von externen Orchestratoren/Planern aufgerufen werden, um SSIS-Pakete in Azure auszuführen.  Es erleichtert das Lift & Shift bzw. die Migration Ihrer SSIS-Pakete in die Cloud.  Wenn Sie nach der Migration weiterhin externe Orchestratoren/Planer für Ihre täglichen Vorgänge verwenden möchten, können diese jetzt **AzureDTExec** anstelle von **dtexec** aufrufen.

**AzureDTExec** führt Ihre Pakete als Aktivitäten vom Typ „SSIS-Pakete ausführen“ in ADF-Pipelines aus. Weitere Informationen finden Sie im Artikel [Ausführen von SSIS-Paketen als ADF-Aktivitäten](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).  Es kann über SSMS konfiguriert werden, um eine AAD-Anwendung (Azure Active Directory) zu verwenden, die Pipelines in Ihrer ADF generiert.  Sie kann auch für den Zugriff auf Dateisysteme/Dateifreigaben/Azure Files konfiguriert werden, in denen Sie Ihre Pakete speichern.  Basierend auf den Werten, die Sie für die Aufrufoptionen angeben, generiert **AzureDTExec** eine eindeutige ADF-Pipeline mit der darin enthaltenen Aktivität vom Typ „SSIS-Paket ausführen“.  Wenn Sie **AzureDTExec** mit denselben Werten für seine Optionen aufrufen, wird die vorhandene Pipeline erneut ausgeführt.

## <a name="prerequisites"></a>Voraussetzungen
Um **AzureDTExec** zu verwenden, laden Sie das neueste SSMS (Version 18.3 oder höher) [hier](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017) herunter und installieren Sie es.

## <a name="configure-azuredtexec-utility"></a>Konfigurieren des Hilfsprogramms AzureDTExec
Wenn Sie SSMS auf Ihrem lokalen Computer installieren, wird auch **AzureDTExec** installiert.  Um dessen Einstellungen zu konfigurieren, starten Sie SSMS mit der Option **Als Administrator ausführen**, und wählen Sie den kaskadierten Dropdownmenüeintrag **Tools -> Zu Azure migrieren -> Azure-fähiges DTExec konfigurieren** aus.

![Konfigurieren des Menüs für das Azure-fähige dtexec](media/how-to-invoke-ssis-package-azure-enabled-dtexec/ssms-azure-enabled-dtexec-menu.png)

Diese Aktion öffnet das Fenster **AzureDTExecConfig**, das mit Administratorrechten geöffnet werden muss, damit es in die Datei **AzureDTExec.settings** schreiben kann.  Wenn Sie SSMS nicht als Administrator ausgeführt haben, öffnet sich ein Fenster für die Benutzerkontensteuerung (UAC), in dem Sie Ihr Administratorkennwort eingeben können, um Ihre Berechtigungen zu erhöhen.

![Konfigurieren der Einstellungen für das Azure-fähige dtexec](media/how-to-invoke-ssis-package-azure-enabled-dtexec/ssms-azure-enabled-dtexec-settings.png)

Im Fenster **AzureDTExecConfig** können Sie Ihre Konfigurationseinstellungen wie folgt eingeben:

- **ApplicationId**: Geben Sie die eindeutige Kennung der AAD-App ein, die Sie mit den richtigen Berechtigungen zum Generieren von Pipelines in Ihrer ADF erstellen. Weitere Informationen finden Sie im Artikel [Erstellen einer AAD-App und eines Dienstprinzipals über das Azure-Portal](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal)

- **AuthenticationKey**: Geben Sie den Authentifizierungsschlüssel für Ihre AAD-App ein.

- **TenantId**: Geben Sie die eindeutige Kennung des AAD-Mandanten ein, unter dem Ihre AAD-App erstellt wird.

- **SubscriptionId**: Geben Sie die eindeutige Kennung des Azure-Abonnements ein, unter der Ihre ADF erstellt wurde.

- **Ressourcengruppe**: Geben Sie den Namen der Azure-Ressourcengruppe ein, in der Ihre ADF erstellt wurde.

- **DataFactory**:  Geben Sie den Namen Ihrer ADF ein, in der eindeutige Pipelines mit der Aktivität vom Typ „SSIS-Paket ausführen“ basierend auf den Werten der Optionen generiert werden, die beim Aufruf von **AzureDTExec** bereitgestellt wurden.

- **IRName**: Geben Sie den Namen der Azure-SSIS IR in Ihrer ADF ein, in der die Pakete, die in ihrem UNC-Pfad (Universal Naming Convention) beim Aufruf von **AzureDTExec** angegeben sind, ausgeführt werden.

- **PackageAccessDomain**: Geben Sie die Anmeldeinformation für die Domäne für den Zugriff auf Ihre Pakete in dem beim Aufruf von **AzureDTExec** angegebenen UNC-Pfad ein.

- **PackageAccessUserName**:  Geben Sie die Anmeldeinformation für den Benutzernamen für den Zugriff auf Ihre Pakete in dem beim Aufruf von **AzureDTExec** angegebenen UNC-Pfad ein.

- **PackageAccessPassword**: Geben Sie die Anmeldeinformation für das Kennwort für den Zugriff auf Ihre Pakete in dem beim Aufruf von **AzureDTExec** angegebenen UNC-Pfad ein.

- **LogPath**:  Geben Sie den UNC-Pfad des Protokollordners ein, in den die Protokolldateien Ihrer Paketausführungen in der Azure-SSIS IR geschrieben werden.

- **LogLevel:**  Geben Sie den ausgewählten Protokollierungsbereich aus den vordefinierten Optionen **Null**/**Standard**/**Ausführlich**/**Leistung** für Ihre Paketausführungen in der Azure-SSIS IR ein.

- **LogAccessDomain**: Geben Sie die Anmeldeinformation für die Domäne ein, um beim Schreiben von Protokolldateien in Ihrem UNC-Pfad auf Ihren Protokollordner zuzugreifen, wenn **LogPath** angegeben ist und **LogLevel** nicht **Null** ist.

- **LogAccessUserName**: Geben Sie die Anmeldeinformation für den Benutzernamen ein, um beim Schreiben von Protokolldateien in Ihrem UNC-Pfad auf Ihren Protokollordner zuzugreifen, wenn **LogPath** angegeben ist und **LogLevel** nicht **Null** ist.

- **LogAccessPassword**: Geben Sie die Anmeldeinformation für das Kennwort ein, um beim Schreiben von Protokolldateien in Ihrem UNC-Pfad auf Ihren Protokollordner zuzugreifen, wenn **LogPath** angegeben ist und **LogLevel** nicht **Null** ist.

- **PipelineNameHashStrLen**: Geben Sie die Länge der Hashzeichenfolgen ein, die aus den Werten der Optionen generiert werden sollen, die Sie beim Aufruf von **AzureDTExec** bereitstellen.  Die Zeichenfolgen werden verwendet, um eindeutige Namen für ADF-Pipelines zu bilden, die Ihre Pakete in der Azure-SSIS IR ausführen.  Eine Länge von 32 Zeichen ist normalerweise ausreichend.

Wenn Sie planen, Ihre Pakete und Protokolldateien in lokalen Dateisystemen/Dateifreigaben zu speichern, sollten Sie Ihre Azure-SSIS IR mit einem VNet verknüpfen, das mit Ihrem lokalen Netzwerk verbunden ist, damit es Ihre Pakete abrufen und Ihre Protokolldateien schreiben kann. Weitere Informationen finden Sie im Artikel [Verknüpfen der Azure-SSIS IR mit einem VNet](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network).

Um zu vermeiden, dass vertrauliche Werte, die in die Datei **AzureDTExec.settings** geschrieben wurden, als Nur-Text angezeigt werden, codieren wir sie in Zeichenfolgen mit Base64-Codierung.  Wenn Sie **AzureDTExec** aufrufen, werden alle Base64-codierten Zeichenfolgen wieder in ihre ursprünglichen Werte decodiert.  Sie können die Datei **AzureDTExec.settings** weiter sichern, indem Sie die Konten einschränken, die darauf zugreifen können.

## <a name="invoke-azuredtexec-utility"></a>Aufrufen des Hilfsprogramms AzureDTExec
Sie können **AzureDTExec** über die Eingabeaufforderung aufrufen und die relevanten Werte für bestimmte Optionen in Ihrem Anwendungsfallszenario angeben.

Das Hilfsprogramm wird in `{SSMS Folder}\Common7\IDE\CommonExtensions\Microsoft\SSIS\150\Binn` installiert. Sie können den Pfad der Umgebungsvariablen „PATH“ hinzufügen, damit es von überall aufgerufen werden kann.

```dos
> cd "C:\Program Files (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE\CommonExtensions\Microsoft\SSIS\150\Binn"
> AzureDTExec.exe  ^
  /F \\MyStorageAccount.file.core.windows.net\MyFileShare\MyPackage.dtsx  ^
  /Conf \\MyStorageAccount.file.core.windows.net\MyFileShare\MyConfig.dtsConfig  ^
  /Conn "MyConnectionManager;Data Source=MyDatabaseServer.database.windows.net;User ID=MyAdminUsername;Password=MyAdminPassword;Initial Catalog=MyDatabase"  ^
  /Set \package.variables[MyVariable].Value;MyValue  ^
  /De MyEncryptionPassword
```

Der Aufruf von **AzureDTExec** bietet ähnliche Optionen wie der Aufruf von **dtexec**. Weitere Informationen finden Sie in der Dokumentation zum Hilfsprogramm [dtexec](https://docs.microsoft.com/sql/integration-services/packages/dtexec-utility?view=sql-server-2017).  Nachfolgend sind die Optionen aufgeführt, die derzeit unterstützt werden:

- **/F[ile]** : Lädt ein Paket, das in Dateisystem/Dateifreigabe/Azure Files gespeichert ist.  Als Wert für diese Option können Sie den UNC-Pfad für Ihre Paketdatei in Dateisystem/Dateifreigabe/Azure Files mit der Erweiterung DTSX angeben.  Wenn der angegebene UNC-Pfad Leerzeichen enthält, müssen Sie den gesamten Pfad in Anführungszeichen einschließen.

- **/Conf[igFile]** : Gibt eine Konfigurationsdatei an, aus der Werte extrahiert werden sollen.  Mit dieser Option können Sie eine Laufzeitkonfiguration für Ihr Paket festlegen, die sich von der zur Entwurfszeit angegebenen Konfiguration unterscheidet.  Sie können verschiedene Einstellungen in einer XML-Konfigurationsdatei speichern und diese dann vor der Paketausführung laden.  Weitere Informationen finden Sie im Artikel [SSIS-Paketkonfigurationen](https://docs.microsoft.com/sql/integration-services/packages/package-configurations?view=sql-server-2017).  Als Wert für diese Option können Sie den UNC-Pfad für Ihre Konfigurationsdatei in Dateisystem/Dateifreigabe/Azure Files mit der „dtsConfig“-Erweiterung angeben.  Wenn der angegebene UNC-Pfad Leerzeichen enthält, müssen Sie den gesamten Pfad in Anführungszeichen einschließen.

- **/Conn[ection]** : Gibt Verbindungszeichenfolgen für vorhandene Verbindungs-Manager in Ihrem Paket an.  Mit dieser Option können Sie Laufzeitverbindungszeichenfolgen für vorhandene Verbindungs-Manager in Ihrem Paket festlegen, die sich von den zur Entwurfszeit angegebenen unterscheiden.  Als Wert für diese Option können Sie es wie folgt angeben: `connection_manager_name_or_id;connection_string [[;connection_manager_name_or_id;connection_string]...]`.

- **/Set**: Setzt die Konfiguration eines Parameters, einer Variablen, einer Eigenschaft, eines Containers, eines Protokollanbieters, eines Foreach-Enumerators oder einer Verbindung in Ihrem Paket außer Kraft.  Diese Option kann mehrfach angegeben werden.  Als Wert für diese Option können Sie es wie folgt angeben: `property_path;value`. `\package.variables[counter].Value;1` setzt z. B. den Wert der Variablen `counter` durch 1 außer Kraft.  Sie können den Assistenten für die Paketkonfiguration verwenden, um den Wert von `property_path` für Elemente in Ihrem Paket zu suchen, zu kopieren und einzufügen, deren Wert Sie außer Kraft setzen möchten. Weitere Informationen finden Sie in der Dokumentation zum [Assistenten für die Paketkonfiguration](https://docs.microsoft.com/sql/integration-services/package-configuration-wizard-ui-reference?view=sql-server-2014)

- **/De[crypt]** : Legt das Entschlüsselungskennwort für Ihr Paket fest, das mit der Schutzebene **EncryptAllWithPassword**/**EncryptSensitiveWithPassword** konfiguriert ist.

> [!NOTE]
> Wenn Sie **AzureDTExec** mit neuen Werten für seine Optionen aufrufen, wird eine neue Pipeline generiert, mit Ausnahme der Option **/De[cript]** .

## <a name="next-steps"></a>Nächste Schritte

Sobald eindeutige Pipelines mit der Aktivität vom Typ „SSIS-Paket ausführen“ in ihnen generiert und nach dem Aufruf von **AzureDTExec** ausgeführt werden, können sie im ADF-Portal überwacht werden. Weitere Informationen finden Sie im Artikel [Ausführen von SSIS-Paketen als ADF-Aktivitäten](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).

> [!WARNING]
> Es wird davon ausgegangen, dass die generierte Pipeline nur von **AzureDTExec** verwendet wird. Die Eigenschaften/Parameter können sich in Zukunft ändern. Daher sollten Sie diese nicht für andere Zwecke ändern oder wiederverwenden, da **AzureDTExec** dadurch unterbrochen werden kann. Wenn das der Fall ist, können Sie die Pipeline jederzeit löschen, und **AzureDTExec** generiert beim nächsten Aufruf eine neue Pipeline.
