# Gebruik van identifiers in de adresseringsfunctie

Binnen de zorg woren diverse identifiers gebruikt. Sommige van deze identifiers worden uitgegeven door landelijke registers, andere zijn gebonden aan specifieke domeinen, infrastructuren of uitwisselingen.
We moeten daarom onderscheid maken tussen de verschillende soorten identifiers en de context waarin ze (kunnen) worden gebruikt.

## Identifiers voor het uniek identificeren van (zorg)organisaties en personen

Identifiers die worden gebruikt om organisaties en personen uniek te identificeren zijn afkomstig uit officiele bronregisters, vaak met een wettelijke taak. We noemen ze daarom gekwalificeerde identifiers.
Voorbeelden zijn de KVK-nummer voor organisaties, AGB-code voor zorgaanbieders en zorgverleners, URA-code voor zorgaanbieders, UZI-nummer voor zorgverleners.

### Uniekheid

Het uitgangspunt van de organisatie identifiers is dat ze uniek een organisatie kunnen identificeren. Deze identifiers mogen daarom maar aan 1 entiteit in de adresseringsfunctie worden gekoppeld.

Voor personen is deze uniekheids-eis nog niet helemaal duidelijk omdat een persoon bij meerdere organisaties werkzaam kan zijn en daarom meerdere keren in de functie aanwezig kan zijn.

### Koppelen van identifiers

Het is mogelijk dat een organisatie een aantal van deze gekwalificeerde identifiers heeft. Het is daarom mogelijk om meerdere identifiers aan een entiteit in het adresseringsregister te koppelen.
Om deze associatie te kunnen maken stellen we het gebruik van het KVK-nummer voor.

Dit zou de volgende gevolgen hebben:

- Een organisatie die opgenomen wil worden in de adresseringsfunctie moet een KVK inschrijving hebben
- Een bronregister van identifiers moet naast de identifier ook het KVK-nummer aanleveren
- Het is niet nodig om een zorgspecifieke identifier te hebben om opgenomen te worden in de adresseringsfunctie
- Het is mogelijk om organisaties te autehnticeren op basis van het KVK-nummer via eHerkenning

### Beheer van identifiers

Elk van deze identifiers heeft een eigen beheerder. Deze beheerder is verantwoordelijk voor het uitgeven en beheren van de identifiers. De governance van deze identifiers is onderdeel van de adresseringsfunctie. Elke partij die opgenomen is of gebruik maakt van de adresseringsfunctie moet zich houden aan de regels die de beheerder van de identifier stelt.

## Identifiers voor het adresseren van organisaties, afdelingen en personen

Deze identifiers worden uitgegeven door de diverse infrastructuren en uitwisselingen. Deze identifiers worden gebruikt om organisaties, afdelingen en personen te zoeken en adresseren.
Denk hierbij aan EDI adressen, oid's, emailadressen, telefoonnummers, etc. Deze adressen kunnen in het beheer zijn van een infrastructuur of de organisatie zelf.
Het gebruik en garanties van deze identifiers zijn bepaald in de governance van de infrastructuur of uitwisseling.
Deze identifiers dienen alleen gebruikt te worden als beide partijen onderdeel zijn van de betreffende infrastructuur of uitwisseling en volgens de instructies van de beheerder van de identifier.
Deze identifiers zijn mogelijk niet uniek en moeten voor de uniekheid gecombnineerd worden met andere identifiers.
