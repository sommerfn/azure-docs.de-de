---
title: Referenzen für FarmBeats
description: ''
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: 8e8e4524034f0a296045691309b065f8547bdaa0
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73797235"
---
# <a name="references"></a>Referenzen

Im Folgenden finden Sie eine Zusammenstellung aus Hinweisen und Anleitungen für die Azure FarmBeats-APIs.

## <a name="rest-api"></a>REST-API

Die Azure FarmBeats-APIs bieten Benutzern im Agrarsektor eine standardisierte RESTful-Schnittstelle mit JSON-basierten Antworten, damit die Funktionen von Azure FarmBeats optimal genutzt werden können:

- APIs zum Abrufen von Daten aus Sensoren, Kameras, Drohnen oder Satelliten zu Wettersituationen und Bodenbeschaffenheit.
- Normalisierung und Kontextualisierung von Daten von allen gängigen Datenanbietern.
- Schemabasierte Zugriffs- und Abfragefunktionen für alle erfassten Daten.
- Automatische Generierung von Metadaten, die auf ackerbauspezifischen Merkmalen basieren und abgefragt werden können.  
- Automatisch generierte Zeitreihenaggregate für eine schnelle Modellerstellung.
- Integrierte Azure Data Factory-Engine (ADF) zum einfachen Erstellen von benutzerdefinierten Pipelines für die Datenverarbeitung.

## <a name="application-development"></a>Anwendungsentwicklung

Die APIs enthalten eine technische Swagger-Dokumentation. Informationen zu allen APIs und den entsprechenden Anforderungen/Antworten finden Sie unter [Swagger](https://aka.ms/FarmBeatsDatahubSwagger).

Im Folgenden finden Sie eine Zusammenfassung aller Objekte und Ressourcen im FarmBeats-Datenhub:

Bauernhof | „Bauernhof“ entspricht einem physischen Ort, der innerhalb des FarmBeats-Systems von Interesse ist. Jeder Bauernhof besitzt einen Namen und eine eindeutige Bauernhof-ID.
--- | ---|
Gerät  | „Gerät“ entspricht einem physischen Gerät auf dem Bauernhof. Jedes Gerät verfügt über eine eindeutige Geräte-ID. Ein Gerät wird einem Bauernhof in der Regel mit einer Bauernhof-ID bereitgestellt.
DeviceModel  | „DeviceModel“ entspricht den Metadaten des Geräts, beispielsweise Hersteller und Gerätetyp (Gateway oder Knoten).
Sensor  | „Sensor“ entspricht einem physischen Sensor, der Werte aufzeichnet. Ein Sensor ist in der Regel mit einem Gerät mit einer Geräte-ID verbunden.
SensorModel  | „SensorModel“ entspricht den Metadaten des Sensors. Hierzu zählen beispielsweise der Hersteller, der Sensortyp (analog oder digital) und die Sensormessung (wie Umgebungstemperatur, Luftdruck usw.).
Telemetrie  | Telemetrie ermöglicht es, Telemetrienachrichten für einen bestimmten Sensor und Zeitbereich zu lesen.
Auftrag  | „Auftrag“ entspricht jedem Workflow aus Aktivitäten, die im FarmBeats-System ausgeführt werden, um eine gewünschte Ausgabe zu erhalten. Jedem Auftrag ist eine Auftrags-ID und ein Auftragstyp zugeordnet.
JobType  | „JobType“ entspricht den verschiedenen Auftragstypen, die vom System unterstützt werden. Dies umfasst system- und benutzerdefinierte Auftragstypen.
ExtendedType  | „ExtendedType“ entspricht der Liste mit system- und benutzerdefinierten Typen im System. Dies hilft dabei, einen neuen Sensor-, Szenen- oder Szenendateityp im FarmBeats-System einzurichten.
Partner  | „Partner“ entspricht dem Partner für die Integration von Sensoren oder Bildern in FarmBeats.
Szene  | „Szene“ entspricht einer beliebigen generierten Ausgabe im Kontext eines landwirtschaftlichen Betriebs. Jeder Szene ist eine Szenen-ID, eine Szenenquelle, ein Szenentyp und eine Bauernhof-ID zugeordnet. Jeder Szenen-ID können mehrere Szenendateien zugeordnet sein.
SceneFile |„SceneFile“ entspricht allen Dateien, die für eine einzelne Szene generiert werden. Einer einzelnen Szenen-ID können mehrere SceneFile-IDs zugeordnet sein.
Regel  |„Regel“ entspricht einer Bedingung für Daten in Bezug auf den Bauernhof, die zum Auslösen einer Warnung verwendet wird. Jede Regel befindet sich im Kontext der Daten eines Bauernhofs.
Warnung  | „Warnung“ entspricht einer Benachrichtigung, die generiert wird, wenn eine Regelbedingung erfüllt ist. Jede Warnung befindet sich im Kontext einer Regel.
RoleDefinition  | „RoleDefinition“ definiert zulässige und nicht zulässige Aktionen für eine Rolle.
RoleAssignment  |„RoleAssignment“ entspricht der Zuweisung einer Rolle zu einem Benutzer oder Dienstprinzipal.

**Datenformat**

JSON (JavaScript Object Notation) ist ein gängiges, sprachunabhängiges Datenformat, das eine einfache Textdarstellung beliebiger Datenstrukturen bereitstellt. Weitere Informationen finden Sie unter „json.org“.

## <a name="authentication-and-authorization"></a>Authentifizierung und Autorisierung

HTTP-Anforderungen an die REST-API werden durch Azure Active Directory (Azure AD) geschützt.
Um eine authentifizierte Anforderung an die REST-APIs zu senden, erfordert der Clientcode eine Authentifizierung mit gültigen Anmeldeinformationen, bevor Sie die API aufrufen können. Die Authentifizierung zwischen den verschiedenen Akteuren wird von Azure AD koordiniert. Ihr Client erhält ein Zugriffstoken als Nachweis der Authentifizierung. Das Token wird dann im HTTP-Autorisierungsheader der REST-API-Anforderungen gesendet. Weitere Informationen zur Azure AD-Authentifizierung finden Sie unter [Azure Active Directory](https://portal.azure.com) für Entwickler.

Das Zugriffstoken muss in nachfolgenden API-Anforderungen im Headerabschnitt gesendet werden:

```
headers = {"Authorization": "Bearer " + **access_token**}
```

**HTTP-Anforderungsheader**

Im Folgenden sind die gängigsten Anforderungsheader aufgeführt, die bei einem API-Aufruf des Azure FarmBeats-Datenhubs angegeben werden müssen:


**Kopfzeile** | **Beschreibung und Beispiel**
--- | ---
Content-Type  | Das Anforderungsformat (Content-Type: application/<format>) für die Azure FarmBeats-Datenhub-APIs ist JSON. Content-Type: application/json
Authorization  | Gibt das Zugriffstoken an, das zum Ausführen eines API-Aufrufs erforderlich ist. **Autorisierung: Bearer <Zugriffstoken>**
Akzeptieren | Das Antwortformat. Für Azure FarmBeats-Datenhub-APIs ist das Format JSON: **Accept: application/json**.

**API-Anforderungen**

Zum Senden einer REST-API-Anforderung kombinieren Sie die HTTP-Methode (GET, POST, PUT oder DELETE), die URL zum API-Dienst, den Ressourcen-URI (zum Abfragen, Übermitteln von Daten, Aktualisieren oder Löschen) und mindestens einen HTTP-Anforderungsheader miteinander.

Der URL zum API-Dienst ist Ihr Datenhub-URL (https://\<Name-Ihrer-Datenhub-Website>.azurewebsites.net). Optional können Sie auch Abfrageparameter in GET-Aufrufe einfügen, um die Daten in den Antworten zu filtern, zu sortieren und ihre Größe zu beschränken.

Die Beispielanforderung unten dient zum Abrufen der Liste mit den Geräten:

```
curl -X GET "https://microsoft-farmbeats.azurewebsites.net/Device" -H "Content-Type: application/json" -H "Authorization: Bearer <Access-Token>”
```

Für die meisten GET-, POST- und PUT-Aufrufe ist ein JSON-Anforderungstext erforderlich.

Die folgende Beispielanforderung dient zum Erstellen eines Geräts (mit Eingabe im JSON-Format im Anforderungstext).

```json
curl -X POST "https://microsoft-farmbeats.azurewebsites.net/Device" -H  "accept: application/json" -H  "Content-Type: application/json" -H "Authorization: Bearer <Access-Token>" -d "{  \"deviceModelId\": \"ID123\",  \"hardwareId\": \"MHDN123\",  \"reportingInterval\": 900,  \"name\": \"Device123\",  \"description\": \"Test Device 123\",}"
```

**Abfrageparameter**

Bei REST-**GET**-Aufrufen können Sie die Daten in einer API-Antwort filtern, sortieren und ihre Größe beschränken, indem Sie mindestens einen Abfrageparameter in den Anforderungs-URI einfügen. Informationen zu den Abfrageparametern finden Sie in der Dokumentation zur API und zu den einzelnen GET-Aufrufen.
Beim Abfragen der Geräteliste (GET-Aufruf in „/Device“) können die folgenden Abfrageparameter angegeben werden:  

![FarmBeats-Projekt](./media/for-references/query-parameters-device-1.png)

**Fehlerbehandlung**

Azure FarmBeats-Datenhub-APIs geben die standardmäßigen HTTP-Fehler zurück. Hier finden Sie die häufigsten Fehlercodes:


 |Fehlercode             | BESCHREIBUNG |
 |---                    | --- |
 |200                    | Erfolgreich |
 |201                    | Erfolgreich erstellt |
 |400                    | Ungültige Anforderung; Die Anforderung enthält einen Fehler. |
 |401                    | Nicht autorisiert. Der Aufrufer der API ist nicht für den Zugriff auf die Ressource autorisiert. |
 |404                    | Ressource nicht gefunden. |
 |5XX                    | Interner Serverfehler. Die mit „5XX“ beginnenden Fehlercodes weisen auf einen Fehler auf dem Server hin. Weitere Informationen finden Sie im folgenden Abschnitt und in den Serverprotokollen. |


Zusätzlich zu den standardmäßigen HTTP-Fehlern geben Azure FarmBeats-Datenhub-APIs auch interne Fehler im folgenden Format zurück:

```
{
  "message": "<More information on the error>",
  "status": "<error code>”,
  "code": "<InternalErrorCode>",
  "moreInfo": "<Details of the error>"
}
```

Beispiel: Beim Erstellen eines Bauernhofs wurde das obligatorische Feld „Name“ in der Eingabenutzlast nicht angegeben. Daraus resultiert die folgende Fehlermeldung:

```
{
  "message": "Model validation failed",
  "status": 400,
  "code": "ModelValidationFailed",
  "moreInfo": "[\"The Name field is required.\"]"
}
```

## <a name="adding-users-or-app-registrations-to-azure-active-directory"></a>Hinzufügen von Benutzern oder App-Registrierungen zu Azure Active Directory

 Auf Azure FarmBeats-APIs kann von einem Benutzer oder einer App-Registrierung in Azure Active Directory zugegriffen werden. Um eine App-Registrierung in Ihrer Azure Active Directory-Instanz zu erstellen, führen Sie folgende Schritte aus:  

1. Wechseln Sie im [Azure-Portal](https://portal.azure.com) zu **Azure Active Directory** > **App-Registrierungen** > **Neue Registrierung**. Alternativ dazu können Sie auch ein vorhandenes Konto verwenden.
2. Stellen Sie bei einem neuen Konto Folgendes sicher:

    - Geben Sie einen Namen ein.
    - Wählen Sie **Nur Konten in diesem Organisationsverzeichnis (einzelner Mandant)** aus.
    - Übernehmen Sie in den restlichen Feldern die Standardwerte.
    - Wählen Sie **Registrieren** aus.

3. Gehen Sie in der neuen oder vorhandenen App-Registrierung unter **Übersicht** folgendermaßen vor:

    - Erfassen Sie die **Client-ID** und die **Mandanten-ID**.
    - Wechseln Sie zu **Zertifikate und Geheimnisse**, um einen neuen geheimen Clientschlüssel zu generieren, und erfassen Sie den **geheimen Clientschlüssel**.
    - Wechseln Sie zurück zur Übersicht, und klicken Sie auf den Link neben **Anwendung im lokalen Verzeichnis verwalten**.
    - Wechseln Sie zu **Eigenschaften**, um die **Objekt-ID** zu erfassen.

4. Wechseln Sie zu Ihrem [Datenhub-Swagger](https://<yourdatahub>.azurewebsites.net/swagger/index.html), und führen Sie die folgenden Schritte aus:
    - Navigieren Sie zu **RoleAssignment-API**.
    - Führen Sie eine **POST**-Anweisung aus, um eine Rollenzuweisung für die soeben erstellte **Objekt-ID** zu erstellen. Die JSON-Eingabe lautet wie folgt:

  > [!NOTE]
  > Weitere Informationen zum Hinzufügen von Benutzern und der AD-Registrierung finden Sie unter [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).

Nachdem Sie die oben beschriebenen Schritte ausgeführt haben, kann Ihre App-Registrierung (der Client) die Azure FarmBeats-APIs mithilfe eines Zugriffstokens über die Bearerauthentifizierung aufrufen.  

Senden Sie das Zugriffstoken in nachfolgenden API-Anforderungen im Headerabschnitt:

```
headers = {"Authorization": "Bearer " + **access_token**, "Content-Type" : "application/json" }
```
