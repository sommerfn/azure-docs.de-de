---
author: mmacy
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: marsma
ms.openlocfilehash: 6cb0ef848bdeab35c971ebd1a0b14eca1dfe3001
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73522877"
---
#### <a name="applicationstabapplications"></a>[Anwendungen](#tab/applications/)

1. Wählen Sie auf der Übersichtsseite **Registrierte App** die Option **Einstellungen** aus.
1. Wählen Sie unter **API-ZUGRIFF** die Option **Erforderliche Berechtigungen** aus.
1. Wählen Sie die Option **Windows Azure Active Directory**.
1. Wählen Sie unter **ANWENDUNGSBERECHTIGUNGEN** die Option **Verzeichnisdaten lesen und schreiben** aus.
1. Wählen Sie **Speichern** aus.
1. Wählen Sie **Berechtigungen erteilen** und dann **Ja** aus. Es kann einige Minuten dauern, bis die Berechtigungen vollständig verteilt sind.

#### <a name="app-registrations-previewtabapp-reg-preview"></a>[App-Registrierungen (Vorschau)](#tab/app-reg-preview/)

1. Wählen Sie unter **Verwalten** die Option **API-Berechtigungen**.
1. Wählen Sie unter **Konfigurierte Berechtigungen** die Option **Berechtigung hinzufügen** aus.
1. Wählen Sie **Azure Active Directory Graph** aus.
1. Wählen Sie **Anwendungsberechtigungen**.
1. Erweitern Sie **Verzeichnis**, und aktivieren Sie dann das Kontrollkästchen **Directory.ReadWrite.All**.
1. Wählen Sie **Berechtigungen hinzufügen** aus. Warten Sie ein paar Minuten, bevor Sie mit dem nächsten Schritt fortfahren.
1. Wählen Sie **Administratorzustimmung für (Name Ihres Mandanten) erteilen** aus.
1. Wählen Sie ein Mandantenadministratorkonto aus.
1. Wählen Sie **Akzeptieren** aus.
1. Wählen Sie **Aktualisieren** aus, und überprüfen Sie dann, ob „Gewährt für...“ unter **STATUS** angezeigt wird. Es kann einige Minuten dauern, bis die Berechtigungen verteilt wurden.