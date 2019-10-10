---
title: 'Bedingter Zugriff: Blockieren des Zugriffs nach Standort – Azure Active Directory'
description: Erstellen einer Richtlinien für den bedingten Zugriff, um den Zugriff auf Ressourcen nach IP-Standort zu blockieren
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 08/16/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: c782c8bb2807017053375b45560685acf78161e7
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72169964"
---
# <a name="conditional-access-block-access-by-location"></a>Bedingter Zugriff: Blockieren des Zugriffs nach Standort

Mithilfe der Standortbedingung in Conditional Access können Sie den Zugriff auf Ihre Cloud-Apps auf der Grundlage des Netzwerkstandorts eines Benutzers steuern. Die Standortbedingung wird häufig verwendet, um den Zugriff aus Ländern zu blockieren, von denen Ihrem Unternehmen bekannt ist, dass der Datenverkehr nicht aus diesen Ländern stammen sollte.

## <a name="define-locations"></a>Definieren von Standorten

1. Melden Sie sich beim **Azure-Portal** als globaler Administrator, Sicherheitsadministrator oder Administrator für bedingten Zugriff an.
1. Navigieren Sie zu **Azure Active Directory** > **Bedingter Zugriff**.
1. Wählen Sie **Neuer Standort** aus.
1. Benennen Sie den Standort.
1. Wählen Sie **IP-Bereiche** aus, wenn Sie die spezifischen extern zugänglichen IPv4-Adressbereiche kennen, die diesen Standort definieren, oder wählen Sie **Länder/Regionen**.
   1. Geben Sie die **IP-Bereiche** an, oder wählen Sie die **Länder/Regionen** für den von Ihnen angegebenen Standort aus.
      * Wenn Sie „Länder/Regionen“ ausgewählt haben, können Sie optional auch unbekannte Gebiete einbeziehen.
1. Wählen Sie **Speichern** aus.

Weitere Informationen zur Standortbedingung in Conditional Access finden Sie im Artikel [Was sind Standortbedingungen in Azure Active Directory Conditional Access?](location-condition.md).

## <a name="create-a-conditional-access-policy"></a>Erstellen der Richtlinie für bedingten Zugriff

1. Melden Sie sich beim **Azure-Portal** als globaler Administrator, Sicherheitsadministrator oder Administrator für bedingten Zugriff an.
1. Navigieren Sie zu **Azure Active Directory** > **Bedingter Zugriff**.
1. Wählen Sie **Neue Richtlinie**.
1. Benennen Sie Ihre Richtlinie. Es wird empfohlen, dass Unternehmen einen aussagekräftigen Standard für die Namen ihrer Richtlinien erstellen.
1. Wählen Sie unter **Zuweisungen** die Option **Benutzer und Gruppen** aus.
   1. Wählen Sie unter **Einschließen** die Option **Alle Benutzer** aus.
   1. Wählen Sie **Fertig**aus.
1. Wählen Sie unter **Cloud-Apps oder -aktionen** > **Einschließen** die Option **Alle Cloud-Apps** und dann **Fertig** aus.
1. Unter **Bedingungen** > **Standort**.
   1. Legen Sie **Konfigurieren** auf **Ja** fest.
   1. Wählen Sie unter **Einschließen** die Option **Ausgewählte Standorte** aus.
   1. Wählen Sie den blockierten Standort aus, den Sie für Ihr Unternehmen erstellt haben.
   1. Klicken Sie auf **Auswählen** > **Fertig** > **Fertig**.
1. Wählen Sie unter **Zugriffskontrollen** > **Blockieren** die Option **Auswählen** aus.
1. Bestätigen Sie die Einstellungen und legen Sie **Richtlinie aktivieren** auf **Ein** fest.
1. Wählen Sie **Erstellen** aus, um die Richtlinie zu erstellen und zu aktivieren.

## <a name="next-steps"></a>Nächste Schritte

[Allgemeine Richtlinien für bedingten Zugriff](concept-conditional-access-policy-common.md)

[Simulieren des Anmeldeverhaltens mit dem Was-wäre-wenn-Tool für den bedingten Zugriff](troubleshoot-conditional-access-what-if.md)
