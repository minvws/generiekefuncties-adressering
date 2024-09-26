# Adresboek casussen

## Inleiding

Dit document beschrijft de casussen voor de generieke functie
adressering. Qua protocol en datamodel is gekozen voor de FHIR mCSD
standaard. Dit document beschrijft ook hoe de casussen zich vertalen
naar het FHIR mCSD model.

Het primaire doel, wat ook terugkomt in de casussen, is de geprioriteerde
uitwisselingen mogelijk maken. Dit houdt in de casussen in die betrekking hebben
op 1<sup>e</sup> plateau van de Nationale Visie Strategie voor de zorg.

Door FHIR mCSD te gebruiken is het denkbaar ook andere casussen te ondersteunen.
Deze vallen in principe buiten scope tenzij deze expliciet in dit document
opgenomen zijn. Hierdoor kunnen de technische oplossingsvoorstellen getoetst
worden aan concrete doelen. Dit laat onverlet dat uitbreiding in de toekomst
mogelijk blijft.


## Uitgangspunten

De casussen hebben als doel om zowel de direct voorziene gebruikscasussen
mogelijk te maken als om een adresboek op te leveren wat ingezet kan worden voor
nu nog niet voorziene casussen.

## Casussen

De volgende casussen beschrijven gezamenlijk de scope van de generieke
functie adressering.

### Opzoeken

#### Endpoint zoeken op basis van URA en soort gegevens

Gebruikers van het nationale adresboek moeten in staat zijn om op basis
van een URA en een gewenste gegevens soort (bijvoorbeeld beeld) de
relevante endpoints te vinden. De endpoints dienen een beschrijving te
hebben waarmee een applicatie het juiste endpoint kan kiezen voor het
gebruiksdoel.

#### Organisatie opzoeken op basis van naam

Het moet mogelijk zijn om een organisatie te vinden op basis van de naam
van de organisatie.

#### Organisatie zoeken op identificerende eigenschappen

Het opzoeken van een organisatie moet mogelijk zijn via identificerende
kenmerken. Een identificerende eigenschap is bijvoorbeeld een URA of AGB-code.
Om ook toekomstige casussen te ondersteunen dient het adresboek het mogelijk te
maken dat er op elke identificerende eigenschap gezocht kan worden. Een
identificerende eigenschap bestaat uit een type (AGB, URA, KvK etc.) en een
waarde (het nummer of code wat bij het type hoort).

#### Ophalen van organisatie gegevens

Een organisatie heeft gegevens zoals adres, vestigingen etc. Deze dienen
opvraagbaar te zijn. De onderstaande lijst geeft een overzicht van de
gegevens die tenminste via het adresboek beschikbaar dienen te
zijn:

- Vestigingen

  - Contact informatie

- Technische endpoints

- Diensten

- Medewerkers

- Samenwerkingen met andere organisaties

#### Organisatie opzoeken op basis van samenwerking

Zorgaanbieders kunnen met elkaar samenwerken in een georganiseerd verband. Het
gaat hier bijvoorbeeld om samenwerkingen tussen zorgorganisaties zoals
bijvoorbeeld in een gezondheidscentrum waarbij huisartsen, apotheker,
fysiotherapeuten zich gezamenlijk organiseren.


#### Organisatie opzoeken op basis van dienstverlening

Om samenwerking in de zorg te bevorderen moeten organisaties elkaar kunnen
vinden op dienstverlening. Dit kan bijvoorbeeld gebruikt worden om te kunnen
zoeken op organisaties die thuiszorg kunnen organiseren.


#### Zorgverleners opzoeken via een zorgaanbieder

Voor applicaties waarbij contact met een specifiek persoon gemaakt worden (chat
etc.) is het gewenst dat er een lijst is van zorgverleners die bij een
zorgaanbieder werkzaam zijn.


### Registratie

#### Zorgorganisatie aanmelden bij het nationale adresboek

Het nationale adresboek vereist dat een zorgaanbieder zich hierbij kan
aanmelden.

#### Zorgorganisatie registreert gegevens

Een zorgaanbieder kan gegevens over de organisatie registreren. Deze
gegevens corresponderen met de casus “Ophalen van organisatie gegevens”.

#### IT-leverancier registreert gegevens

IT-leveranciers beheren de applicaties en diensten die als endpoint
beschikbaar worden gesteld. Deze endpoints moeten door een leverancier
bijgewerkt kunnen zonder handmatige tussenkomst van een zorgaanbieder.


### Buiten de doelstellingen

Naast de eerder genoemde casussen zijn er een aantal punten die expliciet buiten
de doelstelling van het adresboek vallen.

#### Actualiteit van informatie

Het adresboek is bedoeld voor informatie waarbij een verversingssnelheid van
eens per dag als afdoende beschouwd kan worden. Hoewel het systeem in de
praktijk mogelijk vaker geactualiseerd zal worden mag hier door gebruikmakende
partijen niet vanuit gegaan worden.

#### Juistheid van data

De data voor het adresboek wordt via verschillende partijen aangeleverd. Deze data wordt, met uitzondering van de URA en KvK, niet op juistheid gecontroleerd. Dit betekent dat een organisatie invalide endpoints of een niet bestaande adres kan registreren.

## Datamodel FHIR mCSD

Het FHIR mCSD datamodel bied de volgende
datatypes:

- Organization

- Affiliation

- Endpoint

- Jurisdiction

- Facility

- Location

- Practitioner

- Service

## Betrokken entiteiten per casus

Dit hoofdstuk beschrijft per casus welke datatypes
nodig zijn. De onderstaande tabel geeft een overzicht van de datatypes
uit het FHIR mCSD waarbij aangeven is of deze terug komt in één of
meerdere casussen.

| Datatype     | Nodig voor casussen |
| ------------ | ------------------- |
| Organization | Ja                  |
| Affiliation  | Ja                  |
| Endpoint     | Ja                  |
| Jurisdiction | Nee                 |
| Facility     | Ja                  |
| Location     | Ja                  |
| Practitioner | Ja                  |
| Service      | Ja                  |

## Opzoeken

### Endpoint zoeken op basis van URA en soort gegevens

Endpoint, Organization

### Organisatie opzoeken op basis van naam

Organization

### Ophalen van organisatie gegevens

Organization, Facility, Location

## Registratie

### Zorgorganisatie meld zich aan bij het nationale adresboek

Organization, Endpoint

### Zorgorganisatie registreert gegevens

Organization, Facility, Location, Affiliation, Endpoint

### IT leverancier registreert gegevens

Organization, Endpoint

# Casussen vertaald naar datamodel opties

## Opzoeken

### Endpoint zoeken op basis van URA en soort gegevens

Voor deze casus is het mogelijk om de endpoints bij een of meerdere
organisaties te plaatsen. De mogelijke opties worden hier beschreven.
Het is mogelijk deze opties te combineren voor het totale beeld van een
organisatie.

#### Endpoints direct op de zorgorganisatie

De URA is een eigenschap van het Organization datatype. Het adresboek
kan daardoor zoeken naar deze eigenschap. Op basis van de Organization
kunnen alle aan deze organisatie gekoppelde Endpoint datatypes worden
geleverd.

#### Endpoints via een IT leverancier

Nadat de organisatie van de zorgaanbieder is gevonden (via de URA)
kunnen de relevante leveranciers worden opgehaald. Een leverancier is
een Organization welke via een Affiliation gekoppeld is aan de
Organization van de zorgaanbieder.

Door gebruik te maken van een eigen `valueset` voor de rol kan de IT-organisatie
als `connectivity-supplier` worden gekenmerkt.

### Organisatie opzoeken op basis van naam

Het Organization datatype bied een naam attribuut waarop het adresboek kan
zoeken.

### Ophalen van organisatie gegevens

Gegevens ophalen van een organisatie is via het ID van een Organization
datatype direct mogelijk. Alle eventueel gerelateerde datatypes kunnen
direct via de in het FHIR mCSD datamodel beschreven relaties worden
benaderd.

## Registratie

### Zorgorganisatie meld zich aan bij het nationale adresboek

Organization, Endpoint

### Zorgorganisatie registreert gegevens

Organization, Facility, Location, Affiliation, Endpoint

### IT leverancier registreert gegevens

Een IT leverancier kan via verschillende opties data registreren. Deze
sectie beschrijft de mogelijkheden.

#### Uit naam van de organisatie

In deze optie levert de IT leverancier alle gegevens namens de
zorgaanbieder. Dit houd in dat deze casus op dezelfde wijze wordt
ingevuld als dat de zorgaanbieder dit zelf zou
doen.

#### Eigen deel van het datamodel

Het is mogelijk het FHIR mCSD datamodel zo te gebruiken dat een IT
leverancier een eigen Organization datatype krijgt. Deze kan dan direct
door de IT leverancier worden voorzien van data (waaronder Endpoint
data).
