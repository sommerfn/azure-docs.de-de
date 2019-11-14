---
title: 'Erstellen eines Mandantenmodells (Vorschau): Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: Generieren Sie automatisch ein Mandantenmodell (Custom Speech mit Office 365-Daten), das Ihre Office 365-Daten nutzt, um eine optimale Spracherkennung für organisationsspezifische Begriffe bereitzustellen, die sowohl sicher als auch regelkonform ist.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: tutorial
ms.date: 10/26/2019
ms.author: erhopf
ms.openlocfilehash: c8a2855ce9cd320be3aea8b3b4a05f3b3eb39976
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73578222"
---
# <a name="create-a-tenant-model-preview"></a>Erstellen eines Mandantenmodells (Vorschau)

Ein Mandantenmodell (Custom Speech mit Office 365-Daten) ist ein abonnierbarer Dienst für Office 365-Unternehmenskunden, der automatisch ein kundenspezifisches Spracherkennungsmodell anhand der Office 365-Daten einer Organisation generiert. Das entstehende Modell ist für Fachbegriffe, Jargon und Namen von Personen optimiert und zeichnet sich durch hohe Sicherheit und Konformität aus.

> [!IMPORTANT]
> Wenn sich Ihre Organisation für ein Mandantenmodell registriert, kann der Speech-Dienst auf das Sprachmodell Ihrer Organisation zugreifen, das auf der Grundlage von E-Mails und Dokumenten öffentlicher Office 365-Gruppen generiert wird. Diese E-Mails und Dokumente sind für jeden in Ihrer Organisation sichtbar. Der Office 365-Administrator Ihrer Organisation kann die Nutzung des organisationsweiten Sprachmodells über das Office 365-Verwaltungsportal aktivieren und deaktivieren.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Registrieren für die Nutzung eines Mandantenmodells im Microsoft 365 Admin Center
> * Abrufen eines Abonnementschlüssels für Speech
> * Erstellen eines Mandantenmodells
> * Bereitstellen eines Mandantenmodells
> * Verwenden eines Mandantenmodells mit dem Speech SDK

## <a name="enroll-using-the-microsoft-365-admin-center"></a>Registrieren im Microsoft 365 Admin Center

Bevor Sie Ihr Mandantenmodell bereitstellen können, müssen Sie sich zunächst im Microsoft 365 Admin Center registrieren. Diese Aufgabe kann nur von Ihrem Microsoft 365-Administrator ausgeführt werden.

1. Melden Sie sich beim [Microsoft 365 Admin Center](https://admin.microsoft.com ) an.
2. Wählen Sie im linken Bereich **Einstellungen** und dann **Apps** aus.

   ![Registrierung für das Mandantenmodell](media/tenant-language-model/tenant-language-model-enrollment.png)

3. Wechseln Sie zu **Azure Speech-Dienste**, und wählen Sie diesen Eintrag aus.

   ![Registrierung für das Mandantenmodell 2](media/tenant-language-model/tenant-language-model-enrollment-2.png)

4. Aktivieren Sie das Kontrollkästchen, und speichern Sie Ihre Einstellungen.

Wenn Sie das Mandantenmodell deaktivieren müssen, kehren Sie zu diesem Bildschirm zurück. Deaktivieren Sie das Kontrollkästchen, und speichern Sie diese Einstellungen.

## <a name="get-a-speech-subscription-key"></a>Abrufen eines Abonnementschlüssels für Speech

Um ein Mandantenmodell mit dem Speech SDK verwenden zu können, benötigen Sie eine Speech-Ressource und den zugehörigen Abonnementschlüssel.

1. Melden Sie sich beim [Azure-Portal](https://aka.ms/azureportal) an.
2. Wählen Sie **Ressource erstellen**.
3. Geben in der Suchleiste Folgendes ein: **Speech**.
4. Wählen Sie **Speech** aus, und klicken Sie auf **Erstellen**.
5. Folgen Sie den Anweisungen auf dem Bildschirm, um Ihre Ressource zu erstellen. Stellen Sie Folgendes sicher:
   * **Standort** ist entweder auf **eastus** oder **westus** festgelegt.
   * **Tarif** ist auf **S0** festgelegt.
6. Klicken Sie auf **Create**.
7. Ihre Ressource wird binnen weniger Minuten erstellt. Den Abonnementschlüssel finden Sie im Abschnitt **Übersicht** für Ihre Ressource.

## <a name="create-a-model"></a>Modellerstellung

Nachdem Ihr Administrator das Mandantenmodell für Ihre Organisation aktiviert hat, können Sie ein Sprachmodell basierend auf Ihren Office 365-Daten erstellen.

1. Melden Sie sich bei [Speech Studio](https://speech.microsoft.com/) an.
2. Klicken Sie rechts oben auf das Zahnradsymbol (Einstellungen), und wählen Sie dann **Tenant Model settings** (Mandantenmodelleinstellungen) aus.

   ![Einstellungsmenü](media/tenant-language-model/tenant-language-settings.png)

3. An dieser Stelle werden Sie in einer Meldung informiert, ob Sie für die Erstellung eines Mandantenmodells berechtigt sind.
   > [!NOTE]
   > Office 365 Enterprise-Kunden in Nordamerika sind berechtigt, ein Mandantenmodell (Englisch) zu erstellen. Für Kunden vom Typ „Kunden-Lockbox“ (Customer Lockbox, CLB), „Kundenschlüssel“ (Customer Key, CK) oder „Office 365 Government“ ist dieses Feature nicht verfügbar. Gehen Sie folgendermaßen vor, um zu bestimmen, ob Sie ein Kunde des Typs „Kunden-Lockbox“ oder „Kundenschlüssel“ sind:
   > * [Kunden-Lockbox](https://docs.microsoft.com/office365/securitycompliance/controlling-your-data-using-customer-key#FastTrack)
   > * [Kundenschlüssel](https://docs.microsoft.com/microsoft-365/compliance/customer-lockbox-requests)
   > * [Office 365 Government](https://www.microsoft.com/microsoft-365/government)

4. Wählen Sie als Nächstes **Abonnieren** aus. Sie erhalten eine E-Mail mit Anweisungen, sobald Ihr Mandantenmodell bereit ist.

## <a name="deploy-your-model"></a>Bereitstellen Ihres Modells

Wenn Ihr Mandantenmodell bereit ist, führen Sie die folgenden Schritte aus, um Ihr Modell bereitzustellen:

1. Klicken Sie in der empfangenen Bestätigungs-E-Mail auf die Schaltfläche **Modell anzeigen**, oder melden Sie sich bei [Speech Studio](https://speech.microsoft.com/) an.
2. Klicken Sie rechts oben auf das Zahnradsymbol (Einstellungen), und wählen Sie dann **Tenant Model settings** (Mandantenmodelleinstellungen) aus.

   ![Einstellungsmenü](media/tenant-language-model/tenant-language-settings.png)

3. Klicken Sie auf **Bereitstellen**.
4. Wenn Ihr Modell bereitgestellt wird, ändert sich der Status in **Bereitgestellt**.

## <a name="use-your-model-with-the-speech-sdk"></a>Verwenden des Modells mit dem Speech SDK

Nachdem Sie Ihr Modell bereitgestellt haben, können Sie es mit dem Speech SDK verwenden. In diesem Abschnitt verwenden Sie den bereitgestellten Beispielcode, um den Speech-Dienst über die Authentifizierung bei Azure AD aufzurufen.

Schauen wir uns den Code an, mit dem Sie das Speech SDK in C# aufrufen. In diesem Beispiel führen Sie die Spracherkennung mithilfe eines Mandantenmodells durch. Diese Anleitung setzt voraus, dass Ihre Plattform bereits eingerichtet ist. Wenn Sie Hilfe bei der Einrichtung benötigen, siehe [Schnellstart: Erkennen von Sprache, C# (.NET Core)](quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=dotnetcore).

Kopieren Sie diesen Code in Ihr Projekt:

```csharp
namespace PrincetonSROnly.FrontEnd.Samples
{
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Net.Http;
    using System.Text;
    using System.Text.RegularExpressions;
    using System.Threading.Tasks;
    using Microsoft.CognitiveServices.Speech;
    using Microsoft.CognitiveServices.Speech.Audio;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Newtonsoft.Json.Linq;

    // Note: ServiceApplicationId is a fixed value.  No need to change.

    public class TenantLMSample
    {
        private const string EndpointUriArgName = "EndpointUri";
        private const string SubscriptionKeyArgName = "SubscriptionKey";
        private const string UsernameArgName = "Username";
        private const string PasswordArgName = "Password";
        private const string ClientApplicationId = "f87bc118-1576-4097-93c9-dbf8f45ef0dd";
        private const string ServiceApplicationId = "18301695-f99d-4cae-9618-6901d4bdc7be";

        public static async Task ContinuousRecognitionWithTenantLMAsync(Uri endpointUri, string subscriptionKey, string audioDirPath, string username, string password)
        {
            var config = SpeechConfig.FromEndpoint(endpointUri, subscriptionKey);

            // Passing client specific information for obtaining LM
            if (string.IsNullOrEmpty(username) || string.IsNullOrEmpty(password))
            {
                config.AuthorizationToken = await AcquireAuthTokenWithInteractiveLoginAsync().ConfigureAwait(false);
            }
            else
            {
                config.AuthorizationToken = await AcquireAuthTokenWithUsernamePasswordAsync(username, password).ConfigureAwait(false);
            }

            var stopRecognition = new TaskCompletionSource<int>();

            // Creates a speech recognizer using file as audio input.
            // Replace with your own audio file name.
            using (var audioInput = AudioConfig.FromWavFileInput(audioDirPath))
            {
                using (var recognizer = new SpeechRecognizer(config, audioInput))
                {
                    // Subscribes to events
                    recognizer.Recognizing += (s, e) =>
                    {
                        Console.WriteLine($"RECOGNIZING: Text={e.Result.Text}");
                    };

                    recognizer.Recognized += (s, e) =>
                    {
                        if (e.Result.Reason == ResultReason.RecognizedSpeech)
                        {
                            Console.WriteLine($"RECOGNIZED: Text={e.Result.Text}");
                        }
                        else if (e.Result.Reason == ResultReason.NoMatch)
                        {
                            Console.WriteLine($"NOMATCH: Speech could not be recognized.");
                        }
                    };

                    recognizer.Canceled += (s, e) =>
                    {
                        Console.WriteLine($"CANCELED: Reason={e.Reason}");
                        if (e.Reason == CancellationReason.Error)
                        {
                            Exception exp = new Exception(string.Format("Error Code: {0}\nError Details{1}\nIs your subscription information updated?", e.ErrorCode, e.ErrorDetails));
                            throw exp;
                        }

                        stopRecognition.TrySetResult(0);
                    };

                    recognizer.SessionStarted += (s, e) =>
                    {
                        Console.WriteLine("\n    Session started event.");
                    };

                    recognizer.SessionStopped += (s, e) =>
                    {
                        Console.WriteLine("\n    Session stopped event.");
                        Console.WriteLine("\nStop recognition.");
                        stopRecognition.TrySetResult(0);
                    };

                    // Starts continuous recognition. Uses StopContinuousRecognitionAsync() to stop recognition.
                    await recognizer.StartContinuousRecognitionAsync().ConfigureAwait(false);

                    // Waits for completion.
                    // Use Task.WaitAny to keep the task rooted.
                    Task.WaitAny(new[] { stopRecognition.Task });

                    // Stops recognition.
                    await recognizer.StopContinuousRecognitionAsync().ConfigureAwait(false);
                }
            }
        }

        public static void Main(string[] args)
        {
            var arguments = new Dictionary<string, string>();
            string inputArgNamePattern = "--";
            Regex regex = new Regex(inputArgNamePattern);
            if (args.Length > 0)
            {
                foreach (var arg in args)
                {
                    var userArgs = arg.Split("=");
                    arguments[regex.Replace(userArgs[0], string.Empty)] = userArgs[1];
                }
            }

            var endpointString = arguments.GetValueOrDefault(EndpointUriArgName, $"wss://westus.online.princeton.customspeech.ai/msgraphcustomspeech/conversation/v1");
            var endpointUri = new Uri(endpointString);

            if (!arguments.ContainsKey(SubscriptionKeyArgName))
            {
                Exception exp = new Exception("Subscription Key missing! Please pass in a Cognitive services subscription Key using --SubscriptionKey=\"your_subscription_key\"" +
                    "Find more information on creating a Cognitive services resource and accessing your Subscription key here: https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account?tabs=multiservice%2Cwindows");
                throw exp;
            }

            var subscriptionKey = arguments[SubscriptionKeyArgName];
            var username = arguments.GetValueOrDefault(UsernameArgName, null);
            var password = arguments.GetValueOrDefault(PasswordArgName, null);

            var audioDirPath = Path.Combine(Path.GetDirectoryName(System.Reflection.Assembly.GetExecutingAssembly().Location), "../../../AudioSamples/DictationBatman.wav");
            if (!File.Exists(audioDirPath))
            {
                Exception exp = new Exception(string.Format("Audio File does not exist at path: {0}", audioDirPath));
                throw exp;
            }

            ContinuousRecognitionWithTenantLMAsync(endpointUri, subscriptionKey, audioDirPath, username, password).GetAwaiter().GetResult();
        }

        private static async Task<string> AcquireAuthTokenWithUsernamePasswordAsync(string username, string password)
        {
            var tokenEndpoint = "https://login.microsoftonline.com/common/oauth2/token";
            var postBody = $"resource={ServiceApplicationId}&client_id={ClientApplicationId}&grant_type=password&username={username}&password={password}";
            var stringContent = new StringContent(postBody, Encoding.UTF8, "application/x-www-form-urlencoded");
            using (HttpClient httpClient = new HttpClient())
            {
                var response = await httpClient.PostAsync(tokenEndpoint, stringContent).ConfigureAwait(false);

                if (response.IsSuccessStatusCode)
                {
                    var result = await response.Content.ReadAsStringAsync().ConfigureAwait(false);

                    JObject jobject = JObject.Parse(result);
                    return jobject["access_token"].Value<string>();
                }
                else
                {
                    throw new Exception($"Requesting token from {tokenEndpoint} failed with status code {response.StatusCode}: {await response.Content.ReadAsStringAsync().ConfigureAwait(false)}");
                }
            }
        }

        private static async Task<string> AcquireAuthTokenWithInteractiveLoginAsync()
        {
            var authContext = new AuthenticationContext("https://login.windows.net/microsoft.onmicrosoft.com");
            var deviceCodeResult = await authContext.AcquireDeviceCodeAsync(ServiceApplicationId, ClientApplicationId).ConfigureAwait(false);

            Console.WriteLine(deviceCodeResult.Message);

            var authResult = await authContext.AcquireTokenByDeviceCodeAsync(deviceCodeResult).ConfigureAwait(false);

            return authResult.AccessToken;
        }
    }
}
```

Als Nächstes müssen Sie das Projekt neu erstellen und über die Befehlszeile ausführen. Es gibt einige Parameter, die Sie ändern müssen, bevor Sie den Befehl ausführen.

1. Ersetzen Sie `<Username>` und `<Password>` durch die Werte eines gültigen Mandantenbenutzers.
2. Ersetzen Sie `<Subscription-Key>` durch den Abonnementschlüssel Ihrer Speech-Ressource. Diesen Wert finden Sie im [Azure-Portal](https://aka.ms/azureportal) im Abschnitt **Übersicht** für Ihre Speech-Ressource.
3. Ersetzen Sie `<Endpoint-Uri>` durch den unten stehenden Endpunkt. Achten Sie darauf, dass Sie `{your-region}` durch die Region ersetzen, in der Ihre Speech-Ressource erstellt wurde. Diese Regionen werden unterstützt: `westus`, `westus2` und `eastus`. Informationen zu Ihrer Region finden Sie im [Azure-Portal](https://aka.ms/azureportal) im Abschnitt **Übersicht** für Ihre Speech-Ressource.
   ```
   "wss://{your region}.online.princeton.customspeech.ai/msgraphcustomspeech/conversation/v1".
   ```
4. Führen Sie den folgenden Befehl aus:
   ```bash
   dotnet TenantLMSample.dll --Username=<Username> --Password=<Password> --SubscriptionKey=<Subscription-Key> --EndpointUri=<Endpoint-Uri>
   ```

## <a name="next-steps"></a>Nächste Schritte

* [Speech Studio](https://speech.microsoft.com/)
* [Speech SDK](speech-sdk.md)
