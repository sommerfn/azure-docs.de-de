---
title: Vorschreiben der mehrstufigen Authentifizierung (MFA) für den Zugriff von nicht vertrauenswürdigen Netzwerken mit bedingtem Zugriff von Azure Active Directory (Azure AD) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Richtlinie für bedingten Zugriff in Azure Active Directory (Azure AD) für Zugriffsversuche von nicht vertrauenswürdigen Netzwerken konfigurieren.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 12/10/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 39ec09c1ecb94a5ae189317d89cce4bc8f279b48
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73175690"
---
# <a name="how-to-require-mfa-for-access-from-untrusted-networks-with-conditional-access"></a>Gewusst wie: Vorschreiben der Verwendung der MFA für den Zugriff von nicht vertrauenswürdigen Netzwerken mit bedingtem Zugriff   

Azure Active Directory (Azure AD) ermöglicht das einmalige Anmelden bei Geräten, Apps und Diensten an jedem Ort. Benutzer können nicht nur über das Netzwerk Ihres Unternehmens, sondern auch von jeder nicht vertrauenswürdigen Internetadresse aus auf Ihre Cloud-Apps zugreifen. Eine allgemeine bewährte Methode für den Zugriff von nicht vertrauenswürdigen Netzwerken ist die Anforderung der Verwendung der mehrstufigen Authentifizierung (MFA).

Dieser Artikel enthält Informationen, die zum Konfigurieren einer Richtlinie für bedingten Zugriff erforderlich sind, die MFA für den Zugriff von nicht vertrauenswürdigen Netzwerken erfordert. 

## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie mit Folgendem vertraut sind: 

- Den [grundlegenden Konzepten](overview.md) des bedingten Azure AD-Zugriffs 
- Den [Best Practices](best-practices.md) für das Konfigurieren von Richtlinien für bedingten Zugriff im Azure-Portal

## <a name="scenario-description"></a>Beschreibung des Szenarios

Um das Gleichgewicht zwischen Sicherheit und Produktivität zu gewährleisten, kann es ausreichen, dass für die Anmeldung über das Netzwerk Ihrer Organisation lediglich ein Kennwort erforderlich ist. Beim Zugriff über eine nicht vertrauenswürdige Netzwerkadresse besteht jedoch ein erhöhtes Risiko, dass die Anmeldung nicht von berechtigten Benutzern durchgeführt wird. Um dieses Risiko zu vermeiden, können Sie den Zugriff von nicht vertrauenswürdigen Netzwerken blockieren. Alternativ können Sie auch eine mehrstufige Authentifizierung (MFA) verlangen, um sicher zu sein, dass ein Versuch vom legitimen Besitzer des Kontos unternommen wurde. 

Mit dem bedingten Zugriff in Azure AD können Sie diese Anforderung mit einer einzigen Richtlinie erfüllen, die den Zugriff gewährt: 

- Für ausgewählte Cloud-Apps
- Für ausgewählte Benutzer und Gruppen  
- Mit erforderlicher mehrstufiger Authentifizierung 
- Beim Zugriff von: 
   - Einer nicht als vertrauenswürdig eingestuften Adresse

## <a name="implementation"></a>Implementierung

Die Herausforderung dieses Szenarios besteht darin, *den Zugriff von einem nicht vertrauenswürdigen Netzwerk* in eine Bedingung für den bedingten Zugriff umzuwandeln. In einer Richtlinie für bedingten Zugriff können Sie die [Standortbedingung](location-condition.md) so konfigurieren, dass sie Szenarien adressiert, die sich auf Netzwerkadressen beziehen. Die Standortbedingung ermöglicht es Ihnen, benannte Standorte auszuwählen, die logische Gruppierungen von IP-Adressbereichen, Ländern und Regionen sind.  

Typischerweise besitzt Ihre Organisation einen oder mehrere Adressbereiche, z. B. 199.30.16.0 bis 199.30.16.15.
Sie können einen benannten Speicherort wie folgt konfigurieren:

- Festlegen dieses Bereichs (199.30.16.0/28) 
- Zuweisen eines beschreibenden Namens wie **Unternehmensnetzwerk** 

Anstatt zu versuchen, alle Standorte zu definieren, die nicht vertrauenswürdig sind, können Sie:

- Alle Standorte einschließen 

   ![Bedingter Zugriff](./media/untrusted-networks/02.png)

- Alle vertrauenswürdigen Standorte ausschließen 

   ![Bedingter Zugriff](./media/untrusted-networks/01.png)

## <a name="policy-deployment"></a>Richtlinienbereitstellung

Mit dem in diesem Artikel beschriebenen Ansatz können Sie nun eine Richtlinie für den bedingten Zugriff für nicht vertrauenswürdige Standorte konfigurieren. Um die erwartete Funktionsweise der Richtlinie sicherzustellen, empfiehlt es sich, sie zu testen, bevor Sie sie in der Produktionsumgebung verwenden. Idealerweise sollten Sie in einem Testmandanten überprüfen, ob die neue Richtlinie wie erwartet funktioniert. Weitere Informationen finden Sie unter [Wie stellen Sie eine neue Richtlinie bereit?](best-practices.md#how-should-you-deploy-a-new-policy) 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum bedingten Zugriff finden Sie unter [Was ist bedingter Zugriff in Azure Active Directory?](../active-directory-conditional-access-azure-portal.md)
