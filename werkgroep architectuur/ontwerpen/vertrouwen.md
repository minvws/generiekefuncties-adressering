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

Authentieke bronnen nemen zelf de rol van **Update provider** aan zodate **Update Consumers** direct via de gegevens ophalen bij de bronregisters. Update consumers moeten de gegevens zelf op basis van een afgesproken identifier consolideren. Het gevolg van deze aanpak is dat alleen bij de update consumers een volledig overzicht ontstaat van de adresseringsgegevens. Afhankelijk van welke authentieke bronnen de update consumer raadpleegt, het beeld per consumer kan verschillen. Als het combineren van de gegevens niet goed wordt gedaan kan er alsnog een verkeerd beeld ontstaan. Dit is echter niet door de eigenaar van de gegevens te controleren.

### 3.2.2. **Gebruik van Vertrouwensbewijzen**

Een andere manier is om de gegevens door de authentieke bronregisters te ondertekenen met een vertrouwensbewijs en onder beheer van de update supplier te combineren en zo consistent aanbieden. De update consumers kunnen adv het bewijs verifiëren dat de gegevens authentiek zijn. Omdat deze aanpak een aantal voordelen t.o.v. de eerste optie zullen we hem daarom verder uitwerken in de rest van dit document.

## 4. Self-Sovereign Identity (SSI) Concepten

### 4.1 Basisprincipes van Self-Sovereign Identity (SSI)

Self-Sovereign Identity (SSI) is gebaseerd op het principe dat individuen of organisaties zelf controle hebben over hun digitale identiteit. Voor zorgorganisaties betekent dit:

1. **Autonomie**: Zorgorganisaties beheren zelf hun digitale identiteit en bepalen welke informatie ze delen
2. **Verifieerbaarheid**: Claims (zoals AGB-codes, specialisaties, certificeringen) zijn cryptografisch verifieerbaar
3. **Persistentie**: De identiteit blijft bestaan, onafhankelijk van individuele dienstverleners of systemen
4. **Portabiliteit**: Claims kunnen in verschillende zorgnetwerken gebruikt worden
5. **Minimalisatie**: Alleen noodzakelijke informatie wordt gedeeld (bijvoorbeeld alleen relevante certificeringen)
6. **Gedistribueerd vertrouwen**: Vertrouwen komt voort uit een netwerk van erkende partijen (zoals IGJ, CIBG, brancheorganisaties)

In de praktijk betekent dit dat een ziekenhuis bijvoorbeeld de identiteit kan bewijzen met credentials uitgegeven door het UZI-register, de specialisaties kan aantonen met credentials van de zorgautoriteit, en deze bewijzen kan presenteren aan andere zorgpartijen zonder tussenkomst van een centrale autoriteit. In de praktijk komt het er op neer dat gegevens niet meer hoeven worden overgetypt en altijd van vergezeld gaan van een cryptografische handtekening. Hierdoor is handmatige controle niet meer nodig en kan de informatie direct worden verwerkt.

### 4.2 Rollen in het vertrouwensmodel

### Basisrollen SSI

In het Self-Sovereign Identity model onderscheiden we drie kernrollen:

1. **Uitgever (Issuer)**: Betrouwbare organisaties zoals UZI-register of CIBG die claims uitgeven en ondertekenen, waarmee ze de juistheid van bepaalde claims over een zorgorganisatie bevestigen. Ze fungeren als ankerpunt voor vertrouwen. Deze organisaties geven claims uit waarvoor ze bevoegd zijn. Een uitgeven kan ook verlopen credentials intrekken.

2. **Houder (Holder)**: Personen of zorgorganisatie zelf die de claims ontvangt, beheert en selectief kan delen met anderen. De houder heeft autonomie over welke credentials worden gedeeld en met wie.

3. **Verificateur (Verifier)**: Partijen die de gedeelde claims controleren op echtheid, geldigheid en herkomst. Een verificateur kan bijvoorbeeld een andere zorgaanbieder zijn die de identiteit en endpoints wil valideren.

## 5. Technische implementatie SSI

### 5.1 Verifiable Credentials

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

### 5.2 Decentralized Identifiers (DIDs)

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
`did:web:ziekenhuis-x.nl`
`did:web:cibg.nl`

## 6. Gebruik van Verifiable Credentials in mCSD

### 6.1 Mapping van SSI rollen naar mCSD rollen

Hoe kunnen de SSI concepten worden toegepast op het mCSD profiel? We kunnen de SSI rollen als volgt mappen op de mCSD rollen:

#### Zorgorganisatie

Functioneert als **Holder**

- Beheert eigen organisatie-credentials
- Presenteert deze aan de eigen **Update Supplier**

#### Care Services Update Supplier

Functioneert primair als **Verifier** én als **Holder**:

- Ontvangt presentations van de Zorgorganisatie
- Verifieert de credentials van de Zorgorganisatie
- Publiceert FHIR resources verrijkt met verifiable credentials
- Ondertekent de resources of bundle met een vertrouwensbewijs waarin de authenticiteit van de gegevens wordt bevestigd

#### Care Services Update Consumer

Functioneert primair als **Verifier**:

- Verifieert credentials in ontvangen updates
- Verifieert de presentaties van de credentials
- Verifieert of de presentaties gemaakt zijn voor de **Update Supplier** waar op dat moment de interactie mee is
- Beheert lokale cache van geverifieerde gegevens

#### Care Services Selective Consumer

De Selective consumer heeft een vertrouwensrelatie met de **Selective Supplier**. Het is dus niet nodig om de credentials te verifiëren.

#### Trusted Issuers (Externe Rol)

- UZI-register, CIBG, IGJ etc.
- Geen directe mCSD rol
- Geeft credentials uit aan organisaties
- Beheert revocation status

### 6.2 Gebruik van presentation in FHIR Resources

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
        }
      ],
      "reference": "Organization/example-1/_history/1"
    },
    {
      "extension": [
        {
          "url": "http://hl7.org/fhir/StructureDefinition/targetPath",
          "valueString": "Organization.name"
        }
      ],
      "reference": "Organization/example-1/_history/1"
    }
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
      }
    }
  ],
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

Zodra een **Update consumer** een update verricht bij de **Update Supplier** kan deze de **Provenance** resource gebruiken om te verifiëren dat de gegevens authentiek:

- De VC is uitegeven door de juiste authentieke bron door de `issuer` van de VC te controleren
- De holder heeft de VC gepresenteerd aan een **Update Supplier** door signature van de VP te controleren
- De _Update Supplier_ is de juiste partij door de `aud` in de presentation te gebruiken om het DID document van de _Update Supplier_ op te halen en met de publieke sleutel de tweede provenance op de gehele bundle te verifiëren

Er is nog een optimalisatie mogelijk om voor de DID methode van de _Update Supplier_ een `did:jwk` te gebruiken, zodat de resolve stap overgeslagen kan worden. Dit kan omdat de identiteit van de Update Supplier buiten de synchronizatie stap waarschijnlijk niet hoeft te worden vastgesteld. Dit is echter een implementatie detail.

## 7. Use Cases

- Zorgaanbieder registratie
- Validatie van specialisaties
- Updates van contactgegevens

## 8. Best Practices

- Governance modellen
- Update mechanismen
- Versiebeheer
- Privacy overwegingen

## 9. Conclusies en Aanbevelingen

- Samenvatting belangrijkste punten
- Implementatie roadmap
- Volgende stappen

## Bijlagen

A. Technische specificaties
B. Voorbeeld implementaties
C. Referenties en bronnen
