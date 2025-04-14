# Vertrouwen van gegevens in een mCSD adresseringsfunctie

## 1. Inleiding

Het mCSD profiel geeft ons een manier een adresseringsfunctie te ontwikkelen die diverse bronnen van informatie kan brengen bij diverse consumenten van deze informatie. Het profiel is gebaseerd op de HL7 FHIR standaard en maakt gebruik van de RESTful API's om informatie uit te wisselen. De kracht van dit profiel is dat organisaties zelf in staat zijn gegevens over zichzelf bekend te maken en dat het aanbieden van deze gegevens losgekoppeld is van de consumptie er van. Dit maakt het profiel schaalbaar en flexibel.
Echter is het wel belangrijk dat aanbieders van informatie dit correct doen en ook alleen hun eigen informatie aanbieden.

De adresseringsfunctie is het startpunt voor het vinden van organsiaties en technischce adressen. Als deze gegvens onjuist zijn kan dit leiden tot verkeerde routing van zorginformatie, vertraging in zorgverlening en privacyrisico's.

Het doel van dit document is het vaststellen van de noodzaak van vertrouwen in de informatie van de adresseringsfunctie en het bieden van een uitwerken die dit vertrouwen op een schaalbare en gedistribueerde manier kan waarborgen passend bij het karakter van het mCSD profiel.

## 2. De Noodzaak van Vertrouwen

### 2.1 Oorzaak van Onbetrouwbare Gegevens

Onbetrouwbare gegevens in de adresseringsfuncte kan leiden tot diverse problemen. Foutieve gegevens kunnen op diverse manieren terechtkomen in de addresseringsfuncte:

### Intentionele foutieve gegevens

- Identiteitsfraude, bijvoorbeeld door het opgeven van valse URA waarbij de zorgverlener zich voordoet als een andere zorgverlener
- Verkeerde endpoints kunnen worden geregistreerd om gevoelige zorginformatie te onderscheppen
- Verkeerde kwalificaties kunnen worden geregistreerd om onterecht toegang te krijgen tot bepaalde zorginformatie

### Verouderde gegevens

- Zorgverleners kunnen verhuizen of van werkgever veranderen, waardoor de contactgegevens niet meer kloppen
- Specialisaties kunnen veranderen, waardoor de kwalificaties niet meer kloppen
- Organisaties kunnen fuseren of splitsen, waardoor de registratie van de organisatie niet meer klopt

### Onbedoeld foutieve gegevens

- Typefouten in belangrijke identifiers (AGB-codes, KVK-nummers)
- Onvolledige of inconsistente gegevens door handmatige invoer

### 2.2 Impact van Onbetrouwbare Gegevens

In het mCSD model zijn er update consumers en producers. Als er foutieve gegevens in de adressering terechtkomen, kan dit leiden tot diverse problemen:

- Ongevalideerde updates kunnen hele adresboeken vervuilen
- Cascade-effect waarbij foute gegevens zich verspreiden naar andere systemen
- Moeilijk te traceren oorsprong van foute gegevens
- Risico op onbedoelde overschrijving van correcte gegevens
- Door decentraal karakter lastig de foutieve gegevens te corrigeren

Voor de eindgebruiker kan dit leiden tot:

- Vertraging in (acute) zorgverlening door onjuiste contactgegevens
- Verkeerd gerouteerde medische informatie
- Privacyschendingen door verkeerde adressering
- Verminderd vertrouwen in digitale zorgcommunicatie
- Verhoogde operationele kosten door handmatige verificatie
- Juridische risico's bij gebruik van ongevalideerde gegevens

## 3. Mogelijke Oplossingen

Om de betrouwbaarheid van gegevens in adresseringsfuncties te verbeteren, zijn er diverse oplossingen mogelijk:

### 3.1. **Centrale Validatie en Correctie**

Bijvoorbeeld het inrichten van een data wassstraat. Dit is echter kostbaar en complex, en vereist een centrale autoriteit die de gegevens valideert en corrigeert. Dit is rijmt niet goed met de gedistribueerde aard van het mCSD profiel. Ook creeert het een juridisch risico voor de centrale autoriteit, die aansprakelijk kan worden gesteld voor foutieve gegevens. Ook is het aantal gevalideerde gegevens beperkt tot degene die de wasstraat kan valideren. Ook is het de vraag wat er moet gebeuren met onjuiste gegevens. Moeten deze worden verwijderd of gecorrigeerd? En wie is daar verantwoordelijk voor? Hoe komt een wijziging terecht bij het bronregister?

### 3.2 **Gebruik maken van Authentieke Bronnen of Vertrouwde Uitgevers**

Het rechtstreeks gebruik van authentieke bronnen dan wel vertrouwde uitgevers die zelf de gegevens beheren, valideren en corrigeren
De authentieke bronrergisters zijn zelf verantwoordelijk voor hun eigen gegevens, en kunnen deze valideren en corrigeren.
We hebben het dan over bronregisters zoals het BIG-register, het AGB-register, het KVK-register, het URA-register, etc.
Om deze gegevens te gebruiken in het mCSD profiel kunnen zijn er een aantal mogelijkheden:

### 3.2.1. **Directe Koppeling met Authentieke Bronnen**

Authentieke bronnen nemen zelf de rol van **Update Supplier** aan zodate **Update Consumers** direct via de gegevens ophalen bij de bronregisters. Update consumers moeten de gegevens zelf op basis van een afgesproken identifier consolideren. Het gevolg van deze aanpak is dat alleen bij de update consumers een volledig overzicht ontstaat van de adresseringsgegevens. Afhankelijk van welke authentieke bronnen de update consumer raadpleegt, het beeld per consumer kan verschillen. Als het combineren van de gegevens niet goed wordt gedaan kan er alsnog een verkeerd beeld ontstaan. Dit is echter niet door de eigenaar van de gegevens te controleren. Een ander probleem is dat een authentieke bron een zorg specifiek mCSD Update Supplier moet aanbieden. Dat kunnen we misschien wel verwachten van zorg specifieke bronnen, maar registers zoals bijvoorbeeld het KVK zullen hier misschien niet aan kunnen voldoen.

### 3.2.2. **Gebruik van Vertrouwensbewijzen**

Een andere manier is om de gegevens wel door een **Update Supplier** aan te bieden, maar deze door de authentieke bronregisters te ondertekenen met een digitaal vertrouwensbewijs wat door de **Update Consumer** is te valideren. Hierdoor kan deze zelfstandig vaststellen of de gegevens correct zijn. Hiervoor dient er een uitbreiding op FHIR en het mCSD profiel te worden beschreven waar deze bewijzen kunnen worden aangeboden en gevalideerd. Een voor de hand liggende manier is om te kijken naar de concepten van Self Sovereign Identity (SSI) en Verifiable Credentials (VCs). Het voordeel van deze standaard is dat het voor alle authentieke bronnen relevant gaat worden met de verwachte opkomst van persoonlijke en organisatie wallets.

### 3.3 Evaluatie van de mogelijke oplssingen

Om een beter beeld te krijgen van de gevolgen voor een van de loplossingen werken we oplossing 2 en 3 in dit document verder uit. We zullen zien dat oplossing 3, het gebruik van vertrouwensbewijzen, veel technische complexiteit met zich mee brengt. Dit maakt het ingewikkelder om een implementatie te maken wat de adoptie van de adresseringsfunctie kan belemmeren. Ook is SSI er op gebaseerd dat de verifier en holder direct contact met elkaar hebben, terwijl bij de adresseringsfunctie dit niet altijd het geval is, als bijvoorbeeld de update supplier enkel een dienstverlener is voor de holder, en dus geen toegang heeft tot diens wallet. Een eventuele oplossing door de update consumer direct bij de update supplier de credentials te valideren kan hier een oplossing voor zijn, maar dat zou betekenen dat voor elke combinatie Update Consumer en Update Supplier er een validatie stap moet plaatsvinden. Zodra elke zorgaanbieder mee doet, kan dit een beste impact hebben op de performance van de adresseringsfunctie.

Om bovenstaande redenen gaat de voorkeur nu uit naar optie 2: een directe koppeling tussen update consumer en de authentieke bron. Dit ligt technisch het dichts tegen het mCSD profiel aan en is relatief eenvoudig te implementeren.

## 4. Directe koppeling met authentieke bronnen

Om te zorgen dat een Update Consumer zeker weet dat een Organisation een bepaalde claim kan maken, kan de Update Consumer de claims direct ophalen bij de betreffende authentieke bron. Doordat de update consumer de authentieke bron vertrouwt met een aantal geselecteerde claims, weet deze dat ze kloppen. De authentieke bron vermeld naast de claims ook een afgesproken identifier die de Update Consumer kan gebruiken om de claims samen te voegen met andere Organisation Resources. Zo wordt er een compleet beeld van de organisatie opgebouwd.

### 4.1 Zorgbreede identificerende identifiers

Om te zorgen dat we FHIR resources aan elkaar kunnen matchen hebbben we uniek identifieceren identifiers nodig. Dit valt eigenlijk buiten scope van de adresseringsfunctie en onderdeel van de functie Identificatie en Authenticatie.
Toch willen we graag een voorschot nemen op een invulling hiervan om deze oplossingsrichting te kunnen toentsen. Daarom kiezen we voor adresseringsfunctie de volgende identifiers:

- Zorgorganisatie - URA
- Lokatie - URA

Andere resources zoals HealthcareServices, PractitionerRole, en Endpoint zullen voorlopig nog geen unieke identifier krijgen en daarmee dus ook geen claims uit authentieke bronnen krijgen.

### 4.2 mCSD Update Supplier endpoint registratie

Zoals bijvoorbeeld het CIBG de authentieke bron is van de URA claim, is de zorginstelling zelf de authentieke bron voor de overige claims. De zorginstelling kan deze gegevens aanbieden via een geselecteerde Update Supplier. Een Update Consumer moet voor elke organisatie weten waar de Update Supplier te vinden is. Dit kan door als Update Consumer een eigen administratie bij te houden, maar om de schaalbaarheid te verbeteren is het raadzaam een landelijk register aan te legggen van Organisaties en hun Update Suppliers Endpoints. Dit kan bijvoorbeeld bij het CIBG gebeuren, gezien daar alle organisaties al bekend zijn en ook geauthenticeerd kunnen worden.

### 4.3 Voorbeeld

We gaan uit van een voorbeeld van een zorinstelling met de volgende eigenschappen:

- URA: 123
- Organisatier-naam: Medisch Centrum Oost
- AGB-code: 456
- Organisatie-type: Ziekenhuis
- Update Supplier Endpoint: https://update-supplier.example.nl/organization/een-uuid-11-889

De claims *URA* en *naam* en *update supplier endpoint* worden uitgegeven door het CIBG. De AGB-code en organisatie type worden uitgegeven door Vektis.

Een Update Consumer zal zowel het CIBG als Vektis geconfigureerd hebben als authentieke bron voor bovenstaande claims. Deze bronnen zullen als eerst geraadpleegd worden:

```http
GET https://mcsd.cibg.nl/fhir/organization/_history
```

Geeft:

```json
{
  "resourceType": "Bundle",
  "id": "Example-MCSD",
  "type": "transaction",
  "entry": [
    {
      "fullUrl": "http://mcsd.cibg.nl/fhir/Organization/72f008d4-04cc-4388-89d0-e143d27fe0cf",
      "resource": {
        "resourceType": "Organization",
        "id": "72f008d4-04cc-4388-89d0-e143d27fe0cf",
        "identifier": [
          {
            "system": "https://www.ura.nl",
            "value": "123"
          }
        ],
        "active": true,
        "name": "Medisch Centrum Oost",
        "endpoint": [
          {
            "reference": "https://update-supplier.example.nl/organization/een-uuid-11-889",
            "type": "update-supplier"
          }
        ]
      }
    }
  ]
}
```

```http
GET https://mcsd.vektis.nl/organization/_history
```

Antwoord met de volgende resource-bundle met daarin een organisatie-type en AGB-code. De URA identifier moet altijd aanwezig zijn om de resources aan elkaar te kunnen correleren:

```json
{
  "resourceType": "Bundle",
  "id": "Example-MCSD",
  "type": "transaction",
  "entry": [
    {
      "fullUrl": "http://mcsd.vektis.org/fhir/Organization/f0f31ad0-fad2-44a1-b84e-2d3f06080164",
      "resource": {
        "resourceType": "Organization",
        "id": "f0f31ad0-fad2-44a1-b84e-2d3f06080164",
        "identifier": [
          {
            "system": "https://www.ura.nl",
            "value": "123"
          },
          {
            "system": "https://vektis.nl/agb",
            "value": "456"
          }
        ],
        "active": true,
        "type": [
          {
            "system": "https://vektis.nl/organization-type",
            "code": "Ziekenhuis"
          }
        ]
      }
    }
  ]
}
```

De Update Consumer weet nu het *update supplier endpoint* van de zorginsteling waar rest van de informatie kan worden opgehaald. Dit endpoint geeft een organization resource terug met daarin wederom het URA als identifier en aanvullend een contact entry met telefoonnummer en een bgz-fhir endpoint. Voordat de Update Consumer deze gegevens mag overnemen moet hij controleren of de URA identifier overeenkomt met de verwachte waarde uit het CIBG om te voorkomen dat de Update Supplier zich voordoet als een andere zorginstelling.

```http
GET https://update-supplier.example.nl/organization/een-uuid-11-889/history
```

```json
{
  "resourceType": "Bundle",
  "id": "Example-MCSD",
  "type": "transaction",
  "entry": [
    {
      "fullUrl": "http://update-supplier.example.nl/fhir/een-uuid-11-889",
      "resource": {
        "resourceType": "Organization",
        "id": "een-uuid-11-889",
        "identifier": [
          {
            "system": "https://www.ura.nl",
            "value": "123"
          }
        ],
        "active": true,
        "contact": [
          {
            "telecom": [
              {
                "system": "phone",
                "value": "(+31) 734-677-7777"
              }
            ]
          }
        ],
        "endpoint": [
          {
            "reference": "Endpoint/12",
            "type": "bgz-fhir"
          }
        ]
      }
    }
  ]
}
```

De Update Consumer kan deze resources nu samenvoegen. Dit kan door de URA identifier te gebruiken om de resources aan elkaar te koppelen. De update consumer kan nu een complete resource samenstellen met daarin alle claims van de zorginstelling. De indien de Update Consumer enkel een specifieke toepassing bedient, kan er voor worden gekozen een relevant subset van de ontvangen informatie over te nemen. Bijvoorbeeld: als de Update Consumer een BGZ applicatie bedient, zal een MedMij endpoint niet relevant zijn.

```json
{
  "resourceType": "Organization",
  "id": "42b44c31-d165-4d77-a46d-e0df604166d0",
  "identifier": [
    {
      "system": "https://www.ura.nl",
      "value": "123"
    },
    {
      "system": "https://vektis.nl/agb",
      "value": "456"
    }
  ],
  "active": true,
  "name": "Medisch Centrum Oost",
  "contact": [
    {
      "telecom": [
        {
          "system": "phone",
          "value": "(+31) 734-677-7777"
        }
      ]
    }
  ],
  "endpoint": [
    {
      "reference": "https://update-supplier.example.nl/organization/een-uuid-11-889",
      "type": "update-supplier"
    },
    {
      "reference": "Endpoint/12",
      "type": "bgz-fhir"
    }
  ]
}
```

## 5. Gebruik van vertrouwensbewijzen

In deze sectie werken we de derde optie uit waarbij we vertrouwen in de claims opbouwen door het gebruik van vertrouwensbewijzen. We gebruiken daarvoor de concepten van Self Sovereign Identity (SSI) en Verifiable Credentials (VCs). Dit is een relatief nieuwe technologie die steeds meer wordt toegepast in de zorgsector. Het biedt een gedistribueerde manier om vertrouwen op te bouwen in digitale identiteiten en claims.

### 5.1 Self-Sovereign Identity (SSI)

#### 5.1.1 Basisprincipes

Self-Sovereign Identity (SSI) is gebaseerd op het principe dat individuen of organisaties zelf controle hebben over hun digitale identiteit. Voor (zorg)organisaties betekent dit:

1. **Autonomie**: Zorgorganisaties beheren zelf hun digitale identiteit en bepalen welke informatie ze delen
2. **Verifieerbaarheid**: Claims (zoals AGB-codes, specialisaties, certificeringen) zijn cryptografisch verifieerbaar
3. **Portabiliteit**: Claims kunnen in verschillende zorgnetwerken gebruikt worden
4. **Minimalisatie**: Alleen noodzakelijke informatie wordt gedeeld (bijvoorbeeld alleen relevante certificeringen)
5. **Getrapt vertrouwen**: Vertrouwen komt voort uit een netwerk van vertrouwde authentieke bronnen (zoals het CIBG, Vektis en brancheorganisaties)

In de praktijk betekent dit dat een ziekenhuis bijvoorbeeld de identiteit kan bewijzen met credentials uitgegeven door het UZI-register, de specialisaties kan aantonen met credentials van de zorgautoriteit, en deze bewijzen kan presenteren aan andere zorgpartijen zonder tussenkomst van een centrale autoriteit. In de praktijk komt het er op neer dat gegevens niet meer hoeven worden overgetypt en altijd van vergezeld gaan van een cryptografische handtekening. Hierdoor is handmatige controle niet meer nodig en kan de informatie direct worden verwerkt.

#### 5.1.2 Rollen in het vertrouwensmodel

In het Self-Sovereign Identity model onderscheiden we drie kernrollen:

1. **Uitgever (Issuer)**: Betrouwbare organisaties zoals UZI-register of CIBG die claims uitgeven en ondertekenen, waarmee ze de juistheid van bepaalde claims over een zorgorganisatie bevestigen. Ze fungeren als ankerpunt voor vertrouwen. Deze organisaties geven claims uit waarvoor ze bevoegd zijn. Een uitgeven kan ook verlopen credentials intrekken.

2. **Houder (Holder)**: Personen of zorgorganisatie zelf die de claims ontvangt, beheert en selectief kan delen met anderen. De houder heeft autonomie over welke credentials worden gedeeld en met wie.

3. **Verificateur (Verifier)**: Partijen die de gedeelde claims controleren op echtheid, geldigheid en herkomst. Een verificateur kan bijvoorbeeld een andere zorgaanbieder zijn die de identiteit en endpoints wil valideren.

### 5.2 Technische uitwerking

Nu de concepten van SSI zijn uitgelegd, gaan we in op de technische uitwerking van deze concepten. We maken gebruik van de standaarden die zijn ontwikkeld door de [W3C](https://www.w3.org/TR/did-core/) en de [OpenID Foundation](https://openid.net/). Deze standaarden zijn breed geadopteerd en bieden een solide basis voor het implementeren van SSI in de zorgsector.

#### 5.2.1 Verifiable Credentials

Tot nu toe hebben we het gehad over hoe de identiteit van personenn en organsities opgebouwd zijn uit zogenoemde **claims**.

Een claim is een bewering over een eigenschap of kenmerk van een organisatie. Bijvoorbeeld:

- "Ziekenhuis X heeft AGB-code 12345678"
- "Praktijk Y is gevestigd op adres Z"
- "Organisatie A heeft endpoint B voor gebruik C"

Om een claim digitaal te kunnen verifiëren is er extra metadata nodig zoals een handtekening, informatie over de geldigheid, uitgever en wat voor type claim het is. De claim en deze metadata samen vormen een **Verifiable Credential** (VC). Een VC is een digitaal, cryptografisch ondertekend document dat één of meerdere claims bevat.

Een Verifiable Credential (VC) is een digitaal, cryptografisch ondertekend document dat één of meerdere claims bevat. De VC:

- Is uitgegeven door een vertrouwde partij (Issuer)
- Bevat metadata zoals uitgiftedatum en geldigheidsduur
- Heeft een digitale handtekening die de authenticiteit waarborgt
- Kan worden geverifieerd zonder contact met de uitgever
- Bevat informatie over hoe de verificateur kan controleren of het credential ingetrokken is

Voorbeeld structuur:

```json
{
  "type": ["VerifiableCredential", "ZorgaanbiederCredential"],
  "issuer": "did:web:uzi-register.nl",
  "issuanceDate": "2023-12-15T12:00:00Z",
  "credentialSubject": {
    "id": "did:web:ziekenhuis-x.nl",
    "agbCode": "12345678",
    "organizationType": "Ziekenhuis"
  },
  "proof": {
    "type": "Ed25519Signature2020",
    "created": "2023-12-15T12:00:00Z",
    "verificationMethod": "did:web:uzi-register.nl#key-1",
    "proofPurpose": "assertionMethod",
    "proofValue": "z58...h28"
  }
}
```

Een VC maakt claims dus verifieerbaar en betrouwbaar binnen het gedistribueerde netwerk van de adresseringsfunctie.

#### 5.2.2 Uitwisselprotocol voor Verifiable Credentials

Verifiable credentials moeten worden uitgewisseld tussen uitgever en houder en tussen houder en verificateur. Dit kan op diverse manieren gebeuren, maar de standaarden van de OpenID Foundation heeft momenteel de meeste adoptie bij de diverse internationale wallets.

De twee belangrijkste standaarden zijn:

#### 5.2.3 OpenID4VCI

[OpenID4VCI](https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0.html) staat voor "OpenID for Verifiable Credential Issuance". Het is een standaard voor het uitgeven van Verifiable Credentials door een Issuer aan een Holder. Het protocol is een uitbreiding op OAuth 2.0 en OpenID Connect. Het idee is dat een Verifiable Credential een **Resource** is die kan worden opgevraagd door een **Client**. Hiervoor heeft de **Client** een AccessToken nodig die het op diverse manieren kan verkrijgen. Een wallet kan zelf een verzoek doen om een credential, of een issuer kan een credential aanbieden aan een wallet. Dit zijn respectievelijk de Wallet initiated or Issuer initiated flows.

Als de wallet een AccessToken heeft kan het vervolgens het Verifiable Credential ophalen en opslaan in de wallet.

#### 5.2.4 OpenID4VP

[OpenID4VP](https://openid.net/specs/openid-4-verifiable-presentations-1_0.html) staat voor "OpenID for Verifiable Presentations". Het is een standaard voor het presenteren van Verifiable Credentials als Verifiable Presentation door een Holder aan een Verifier. Het stelt de verifier in staat een set van Verifiable Crededentials op te vragen en vervolgens het eigenaarschap van de houder vast te stellen. Het uitvragen gebeurt door middel van een speciaal query formaat [DIF.PresentationExchange](https://identity.foundation/presentation-exchange/spec/v2.1.1/). De verifier kan vervolgens de presentatie valideren en de claims controleren.

#### 5.2.5 Decentralized Identifiers (DIDs)

Hoe kunnen deze Verifiable Credentials gebruikt worden door een houder om zijn identiteit te bewijzen?
Hiervoor gebruikenn we cryptografische identifiers die volledig onder controle staan van de eigenaar, zonder afhankelijkheid van een centrale autoriteit. Om dit eigenaarschap aan te tonen kan de houder de VC nogmaal ondertekenen met een private key. De verificateur kan dan de VC verifiëren met de publieke sleutel van de houder. De vraag is dan hoe de verificateur de publieke sleutel van de houder kan vinden. Normaliter wordt er gebruik van een Public Key Infrastructure. Binnen het SSI model is hier een decentrale manier voor bedacht. Elke partij in het netwerk heeft een identifier die leidt naar deze publieke sleutel. Deze identifier noemen we een Decentralized Identifier (DID).

Een Decentralized Identifier (DID) is een unieke, permanent geldige identifier die volledig onder controle staat van de eigenaar zonder afhankelijkheid van een centrale autoriteit. Dit verschilt van traditionele identifiers zoals:

- Een URL die afhankelijk is van een domeineigenaar
- Een UZI-nummer dat door het UZI-register wordt uitgegeven
- Een AGB-code die door Vektis wordt beheerd

Een DID:

- Kan door de eigenaar zelf worden aangemaakt
- Is cryptografisch verifieerbaar
- Bevat metadata over hoe er met de identiteit gecommuniceerd kan worden
- Is resolvable naar een DID Document dat publieke sleutels en endpoints bevat
- Blijft geldig ook als onderliggende infrastructuur wijzigt

Voorbeeld van een DID:

- `did:web:ziekenhuis-x.nl`
- `did:web:cibg.nl`

### 5.3 Mapping van SSI concepten naar mCSD

Hoe kunnen de SSI concepten worden toegepast op het mCSD profiel? We kunnen de SSI rollen als volgt mappen op de mCSD rollen:

| Rol mCSD          | Optie 1                 | Optie 2  | Optie 3      |
| ----------------- | ----------------------- | -------- | ------------ |
| Authentieke bron  | Issuer                  | Issuer   | Issuer       |
| (Zorg)organisatie | Holder                  | -        | Holder       |
| Update Supplier   | Authorised Intermediate | Holder   | Intermediate |
| Update Consumer   | Verifier                | Verifier | Verifier     |

De variaties in de opties hebben impact op de volgende aspecten:

- de vertrouwensrelatie tussen (zorg)organisatie en Update Supplier.
- het ontwerp en hoeveel maatwerkt nodig is om de SSI concepten te integreren met het mCSD profiel.
- de hoeveelheid validatie operaties wat moet worden uitgevoerd opdat de Update Consumer de gegevens kan vertrouwen.

#### 5.3.1 Optie 1: Getrapte validatie

In Optie 1 heeft de (zorg)organisatie een eigen wallet. De Update Supplier is een dienstverlener die handelt voor de (zorg)organisatie, maar geen vertrouwde partij is. De noodzaak van beperkt vertrouwen is relevant als er maar een paar van dit soort dienstverleners ontstaan en de Update Consumer bijvoorbeeld niet het eigen EPD is.

Bij een aanpassing van een claim in het adresboek, moet (zorg)organisatie een bewijs presenteren in een Verifiable Presentation aan de Update Supplier welke ze naast de FHIR Resources opslaat in bijvoorbeeld een `Provenance` resource. Bij een synchronisatie stap moet de Update supplier in de bundle met de FHIR resources, ook Verifiable Presentations aanbieden. Om aan te tonen dat de Update Supplier daadwerkerlijk geauthoriseerd is, ondertekent het de bundle zelf met een private key die hoort bij de `DID`. De Update Consumer valideert bij de synchronisatie stap de claims in de Verifiable Presentation, vergelijkt ze met de data in de resources en valideert de handtekening op de Bundle. De (zorg)organisatie hoeft maar eenmalig de data aan de Update Supplier aan te tonen. De Update Consumer kan de "machtiging" van de Update Supplier controleren door de presentatie te valideren. De invulling van `Provencance` resources vergt wel land specifieke profielen wat de implementaties complexer maakt.

#### 5.3.2 Optie 2: Directe validatie met Update Supplier als Holder

In Optie 2 heeft de Update Supplier een vertrouwde rol en heeft zelf (toegang tot) de organisatie wallet. Dit vertrouwen is belangrijk want met de toegang tot de wallet kan de Update Supplier zich voordoen als de (zorg)organisatie bij elke authorisatie stap in het zorg informatie stelsel. Als we landelijk maar een beperkt aantal dienstverleners krijgen is deze optie af te raden.

Omdat de Update Supplier nu zelf ook de holder is, kunnen de claims als Verifiable Credential in plaats van Verifiable Presentation in een provencance worden opgeslagen. De Bundle kan vervolgens als een vorm van een presentation worden ondertekend waardoor de Update Supplier aantoont de Holder te zijn. De Update Supplier moet wel een eigen DID per (zorg)organisatie hebben om te voorkomen dat claims van (zorg)organisaties allemaal in een wallet terechtkomen.

#### 5.3.3 Optie 3: Directe validatie met Organisatie als Holder

In Optie 3 is de (zorg)organisatie zelf de holder en de Update Supplier een intermediate. De Update Supplier heeft geen toegang tot de wallet van de (zorg)organisatie. De Update Supplier voegt aan de claims een hint toe waarmee de Update Consumer zelf bij de (zorg)organisatie een validatie stap kan uitvoeren. Deze optie past het beste bij bestaande invulling van SSI waarbij vertrouwen direct is tussen Holder en Verifier. Ook hoeven er weinig uitbreidingen op het mCSD profiel te worden verricht omdat alleen de hint bij de claim moet worden toegevoegd. Het nadeel van deze aanpak is dat elke Update Consumer, na het binnenkrijgen van nieuwe claims, direct bij de Holder een validatie stap moet uitvoeren. Nu is het wel zo dat deze de bewijzen een langdurig geldig zijn en het adresboek over tijd organisch zal groeien. Alleen bij een nieuwe Update Consumer die geintresseerd is in het gehele adresboek en alle claims wil valideren zal deze validatie periode langer zijn.

#### 5.3.4 Evaluatie van de opties

| Optie | Vertrouwen                                                                                          | Ontwerp                                                                                                                     | Schaalbaarheid                                                            |
| ----- | --------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| 1     | Goed, holder heeft zelf controle over wallet en sleutels en machtigt een vertrouwde Update supplier | Complex, een vertrouwde tussenpartij bestaat niet in SSI. Veel maatwerk op FHIR nodig om concepten als VCs en VPs te mappen | Goed, na synchronizatie alle gegevens aanwezig om validatie uit te voeren |
| 2     | Laag, update supplier heeft toegang tot wallet met alle credentials en sleutels                     | Complex, Veel maatwerk om VCs en VPs te mappen op FHIR resources                                                            | Goed, na synchronizatie alle gegevens aanwezig om validatie uit te voeren |
| 3     | Goed, holder heeft zelf controle over claims en sleutels                                            | Goed, naast een verwijzing naar de claims in de resources is er weinig maatwerk nodig                                       | Matig, elke Update Consumer moet zelf de claims valideren bij de houder   |

### 5.4 Uitwerking Optie 1: Getrapte validatie

#### 5.4.1 Mapping van SSI rollen naar mCSD rollen

##### (Zorg)organisatie

Functioneert als **Holder**

- Beheert eigen organisatie-credentials
- Presenteert deze aan de eigen **Update Supplier**

##### Care Services Update Supplier

Functioneert primair als **Verifier** én als **Holder**:

- Ontvangt presentations van de Zorgorganisatie
- Verifieert de credentials van de Zorgorganisatie
- Publiceert FHIR resources verrijkt met verifiable credentials
- Ondertekent de resources of bundle met een vertrouwensbewijs waarin de authenticiteit van de gegevens wordt bevestigd

##### Care Services Update Consumer

Functioneert primair als **Verifier**:

- Verifieert credentials in ontvangen updates
- Verifieert de presentaties van de credentials
- Verifieert of de presentaties gemaakt zijn voor de **Update Supplier** waar op dat moment de interactie mee is
- Beheert lokale cache van geverifieerde gegevens

##### Care Services Selective Consumer

De Selective consumer heeft een vertrouwensrelatie met de **Selective Supplier**. Het is dus niet nodig om de credentials te verifiëren.

##### Issuers

- UZI-register, CIBG, IGJ etc.
- Geen directe mCSD rol
- Geeft credentials uit aan organisaties
- Beheert revocation status

#### 5.4.2 Gebruik van presentation in FHIR Resources

Een VC kan een of meerdere claims bevatten. Laten we als voorbeeld een VC nemen die de URA-code en de naam van de organisatie bevat:

```json
{
  "type": ["VerifiableCredential", "OrganizationCredential"],
  "issuer": "did:web:cibg.nl",
  "issuanceDate": "2023-12-15T12:00:00Z",
  "credentialSubject": {
    "id": "did:web:ziekenhuis-x.nl",
    "uraCode": "12345678",
    "name": "Ziekenhuis X"
  },
  "proof": {
    "type": "Ed25519Signature2020",
    "created": "2023-12-15T12:00:00Z",
    "verificationMethod": "did:web:cibg.nl#key-1",
    "proofPurpose": "assertionMethod",
    "proofValue": "z58...h28"
  }
}
```

Als we dit proberen te mappen op een FHIR Resource, zou dit er als volgt uit kunnen zien:

```json
{
  "resourceType": "Organization",
  "id": "example-1",
  "identifier": [
    {
      "system": "urn:oid:2.16.840.1.113883",
      "value": "12345678"
    },
    {
      "system": "https://www.w3.org/TR/did-1.0#",
      "value": "did:web:ziekenhuis-x.nl"
    }
  ],
  "name": "Ziekenhuis X"
}
```

De organisatie publiceert een VP in JWT formaat naar de **Update Supplier**:

```json
{
  "iss": "did:web:ziekenhuis-x.nl",
  "aud": "did:web:mcsd.care-services.nl",
  "exp": 1631544000,
  "vp": {
    "type": ["VerifiableCredential", "OrganizationCredential"],
    "issuer": "did:web:cibg.nl",
    "issuanceDate": "2023-12-15T12:00:00Z",
    "credentialSubject": {
      "id": "did:web:ziekenhuis-x.nl",
      "uraCode": "12345678",
      "name": "Ziekenhuis X"
    },
    "proof": {
      "type": "Ed25519Signature2020",
      "created": "2023-12-15T12:00:00Z",
      "verificationMethod": "did:web:cibg.nl#key-1",
      "proofPurpose": "assertionMethod",
      "proofValue": "z58...h28"
    }
  }
}
```

Dit VP bevat een handtekeing gezet door de zorginstelling. De update supplier maakt hier vervolgens een `Provenance` resource van waarin de target verwijst naar zowel het naam veld als de identifier van de organisatie:

```json
{
  "resourceType": "Provenance",
  "id": "prov-1",
  "target": [
    {
      "reference": "Organization/example-1"
    }
  ],
  "recorded": "2023-01-01T00:00:00Z",
  "target": [
    {
      "extension": [
        {
          "url": "http://hl7.org/fhir/StructureDefinition/targetPath",
          "valueString": "Organization.identifier[0].value"
        },
        {
          "url": "https://identity.foundation/presentation-exchange/#jsonpath-syntax-definition",
          "valueString": "$.vc.credentialSubject.uraCode"
        },
        {
          "url": "https://identity.foundation/presentation-exchange/#presentation-definition",
          "valueIdentifier": {
            "system": "https://zorgstelsel.nl#presentation_definitions",
            "value": "123"
          }
        }
      ],
      "reference": "Organization/example-1/_history/1"
    },
    {
      "extension": [
        {
          "url": "http://hl7.org/fhir/StructureDefinition/targetPath",
          "valueString": "Organization.name"
        },
        {
          "url": "https://identity.foundation/presentation-exchange/#jsonpath-syntax-definition",
          "valueString": "$.vc.credentialSubject.name"
        }
      ],
      "reference": "Organization/example-1/_history/1"
    }
  ],
  "activity": [{
    "coding": [{
      "system": "http://terminology.hl7.org/CodeSystem/v3-DocumentCompletion",
      "code": "AU",
      "display": "authenticated"
    }]
  ],
  "agent": [
    {
      "who": {
        "reference": "Organization/example-1",
        "type": "Organization"
      }
    }
  ],
  "signature": [
    {
      "type": [
        {
          "system": "https://www.w3.org/2018/credentials#",
          "code": "VerifiablePresentation"
        }
      ],
      "when": "2025-02-17T15:00:00Z",
      "who": {
        "identifier": {
          "system": "did:web",
          "value": "did:web:ziekenhuis-x.nl"
        }
      },
      "data": "base64(JWT-VP)",
      "targetFormat": "application/jwt",
      "sigFormat": "application/vc+jwt"
    }
  ]
}
```

Om aan te tonen dat de _Update Supplier_ degene is waar de VP voor bedoeld is, kan de _Update Supplier_ een aanvullende `Provenance` aan de `Bundle` toevoegen waarin hij het gehele resultaat ondertekend:

```json
{
  "resourceType": "Provenance",
  "target": [
    {
      "reference": "Bundle/mscd-sync-example"
    }
  ],
  "recorded": "2023-01-01T00:00:00Z",
  "who": {
    "reference": "Organization/mcsd.care-services.nl"
  },
  "signature": [
    {
      "type": "http://hl7.org/fhir/StructureDefinition/Signature",
      "data": "base64(JWT-VP)"
    }
  ]
}
```

Een update `Bundle` komt er dan vervolgens als volgt uit te zien:

```json
{
  "resourceType": "Bundle",
  "id": "mcsd-sync-example",
  "entry": [
    {
      "id": "example-1",
      "resourceType": "Organization"
    },
    {
      "id": " prov-1",
      "resourceType": "Provenance"
    },
    {
      "resourceType": "Provenance",
      "target": [
        {
          "reference": "Bundle/mscd-sync-example"
        }
      ]
    }
  ]
}
```

Elk credential heeft een vastgestelde presentation definition die kan worden gebruikt voor het utivragen van credentials. In het geval van een ura credential is dat de volgende:

```json
{
  "presentation_definition": {
    "id": "1234",
    "input_descriptors": [
      {
        "id": "uraCode",
        "name": "Ura Organisatie Code",
        "purpose": "Vaststellen dat de organisatie een URA code heeft",
        "constraints": {
          "fields": [
            {
              "path": ["$.type"],
              "filter": {
                "type": "string",
                "const": "ZorgaanbiederCredential"
              }
            },
            {
              "id": "name",
              "path": ["$.credentialSubject.name"],
              "filter": {
                "type": "string"
              }
            },
            {
              "path": ["$.issuer"],
              "purpose": "Whe can only accept credentials from a trusted issuer",
              "filter": {
                "type": "string",
                "pattern": "^did:web:cibg.nl$"
              }
            }
          ]
        }
      }
    ]
  }
}
```

Zodra een **Update consumer** een update verricht bij de **Update Supplier** kan deze de **Provenance** resource gebruiken om te verifiëren dat de gegevens authentiek:

- De VC is uitegeven door de juiste authentieke bron door de `issuer` van de VC te controleren
- De holder heeft de VC gepresenteerd aan een **Update Supplier** door signature van de VP te controleren
- De _Update Supplier_ is de juiste partij door de `aud` in de presentation te gebruiken om het DID document van de _Update Supplier_ op te halen en met de publieke sleutel de tweede provenance op de gehele bundle te verifiëren

Er is nog een optimalisatie mogelijk om voor de DID methode van de _Update Supplier_ een `did:jwk` te gebruiken, zodat de resolve stap overgeslagen kan worden. Dit kan omdat de identiteit van de Update Supplier buiten de synchronizatie stap waarschijnlijk niet hoeft te worden vastgesteld. Dit is echter een implementatie detail.

### Validatie

#### Update Supplier

1. Controleer of de issuer van de VC een vertrouwde partij is
2. Controleer of de VC is ondertekend door de issuer
   1. Resolve de DID van de issuer
   2. Verifieer de handtekening van de VC met de publieke sleutel van de issuer
3. Controleer of de VC niet is ingetrokken
   1. Controleer de revocation status van de VC bij de issuer
4. Controleer of de VC nog geldig is (expiration date)
5. Controleer of de VC is gepresenteerd aan de Update Supplier
   1. Verifieer de handtekening van de VP met de publieke sleutel van de holder
   2. Controleer of de aud claim in de VP overeenkomt met de DID van de Update Supplier

#### Update Consumer

Bij een update operatie van de Update Consumer krijgt deze een `Bundle` terug van de Update Supplier. De Update Consumer moet de volgende stappen uitvoeren om de gegevens te valideren:

1. Controleer of de Provenance van de `Bundle` is ondertekend door de Update Supplier
   1. Resolve de DID van de Update Supplier
   2. Verifieer de handtekening van de Provenance met de publieke sleutel van de Update Supplier
2. Voor elk van beschermde attributen in de resources in de bundle:
   1. Vind de bijbehorende provenance entry
   2. Controleer of de VC in de proof is ondertekend door de authentieke de issuer
      1. Resolve de DID van de issuer
      2. Verifieer de handtekening van de VC met de publieke sleutel van de issuer
   3. Controleer of de VC voldoet aan de `presentation_definition` die hoort bij dit attribuut

## 7 Optie 2: Directe validatie met Update Supplier als Holder

Deze variatie is vergelijkbaar als optie 1, maar in plaats van verifiable presentations in de provenance resource, worden de credentials in de provenance resource opgeslagen. De Update Supplier is nu ook de holder van de credentials. Dit heeft als voordeel dat de Zorginstelling geen presentatie hoeft te maken en aan de Update Supplier. Bij het updaten van adresboek identifiers hoeft er geen bevoegd persoon met toegang tot de organisatie wallet betrokken te zijn. Dit kan echter ook een nadeel zijn omdat de Update Supplier nu ook toegang heeft tot alle credentials van de zorgorganisatie.

Een beperking van dit risico is om bij het uitgeven van het credential een extra purpose veld toe te voegen waarmee het gebruik beperkt wordt tot identificatie en niet authenticatie. Zo kan een Update Supplier de credentials niet gebruiken om zich voor te doen als de zorginstelling. Dit is echter een niet standaard oplossing en stelt aanvullende eisen aan o.a. de authentieke bron en de verifiers.

## 8 Optie 3: Directe validatie met Organisatie als Holder

In deze optie is de zorgorganisatie zelf de holder van de credentials en de Update Supplier een intermediate. De Update Supplier heeft geen toegang tot de wallet van de zorgorganisatie.

Zodra een Update Consumer een relevante identifier of claim ontvangt die voor hen vertrouwd moet zijn, kan de Update Consumer direct een validatie stap uitvoeren bij de wallet van de zorgorganisatie. De verifier gebruikt het OpenID4VP protocol om een presentatie van de relevante credentials te verkrijgen, alvorens de gegevens uit de bundle over te nemen in de directory van de selective consumer.

### Vertrouwen

Dus een Update Supplier overhandigd een bundle, met daarin een Organisation. Deze Organisation bevat de eigenschappen URA, DID en naam. Ook is er een wallet endpoint bijgevoegd. De Update Consumer zal in de rol van verifier nu volgens een variatie op de [Cross Device flow](https://openid.net/specs/openid-4-verifiable-presentations-1_0.html#name-cross-device-flow) een Authorization Request verzenden naar de wallet. Via een vastgestelde Presentation Definition vraagt de Verifier om een bewijs van de getoonde attributen in de Organisatie resource. Als de organisatie dit kan overandigen weet je dat de claims inderdaad zijn uitgegeven aan de DID van de organisatie zoals getoond in de Organisation resource. Echte weet je nog niet of de Organisatie ook daadwerkelijk is uitgegeven door de betreffende organisatie. Daaorm moet de Bundle alsnog worden ondertekend met een signature die kan worden gevalideerd met een public key die behoort bij de DID van de Organisation.

## 9 User cases

- Zorgaanbieder registratie
- Validatie van specialisaties
- Updates van contactgegevens

## 10. Best Practices

- Governance modellen
- Update mechanismen
- Versiebeheer
- Privacy overwegingen

## 11. Conclusies en Aanbevelingen

- Samenvatting belangrijkste punten
- Implementatie roadmap
- Volgende stappen

## Bijlagen

A. Technische specificaties
B. Voorbeeld implementaties
C. Referenties en bronnen
