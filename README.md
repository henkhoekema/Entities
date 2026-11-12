# Eigen entity-ID's aanpassen aan jouw eigen Home Assistant-installatie

## Waarom dit hoofdstuk belangrijk is

De voorbeelden in deze repository zijn ontwikkeld en getest in mijn eigen Home Assistant-installatie. Daardoor bevatten ze entity-ID's die specifiek zijn voor mijn omgeving.
Wanneer je een script, automatisering of blueprint uit deze repository overneemt, zul je vrijwel altijd een aantal entity-ID's moeten aanpassen voordat alles correct werkt.
Veel problemen ontstaan niet doordat een script fout is, maar doordat Home Assistant een opgegeven entity simpelweg niet kan vinden.

Een voorbeeld:

```yaml
person.henk
```

Bestaat deze entiteit niet in jouw installatie, dan zal de configuratie niet werken zoals bedoeld.
Je zult deze entity moeten vervangen door de overeenkomstige entity uit jouw eigen Home Assistant-omgeving, bijvoorbeeld:

```yaml
person.jan
```

of

```yaml
person.piet
```

Dit hoofdstuk legt uit wat entity-ID's zijn, hoe Home Assistant ze gebruikt en hoe je jouw eigen entity-ID's kunt vinden.

---

# Wat is een entity?

Home Assistant werkt vrijwel volledig met entiteiten (entities).
Een entity vertegenwoordigt een apparaat, sensor, schakelaar, helper of andere informatiebron binnen Home Assistant.

Voorbeelden zijn:

- een bewegingssensor
- een deurcontact
- een thermometer
- een slimme lamp
- een persoon
- een batterijsensor
- een helper
- een schakelaar

Iedere entity heeft:

- een naam
- een actuele status
- een unieke entity-ID

Bijvoorbeeld:

| Naam | Entity-ID |
|--------|--------|
| Voordeur | `binary_sensor.voordeur` |
| Buitentemperatuur | `sensor.buiten_temperatuur` |
| Jan | `person.jan` |
| Pauzeknop | `input_boolean.bewaking_pauze` |

Home Assistant gebruikt intern vrijwel altijd de entity-ID en niet de zichtbare naam.
Je kunt de entity-ID vergelijken met het kenteken van een auto. Twee auto's kunnen hetzelfde model zijn, maar ieder kenteken is uniek. Zo werkt het ook met entiteiten binnen Home Assistant.

---

# Hoe is een entity-ID opgebouwd?

Een entity-ID bestaat uit twee delen:

```text
domein.naam
```

Voorbeeld:

```text
binary_sensor.voordeur
```

Hierbij is:

- `binary_sensor` het domein
- `voordeur` de unieke naam

Nog een voorbeeld:

```text
person.henk
```

Waarbij:

- `person` het domein is
- `henk` de naam is

Het domein geeft aan wat voor soort entiteit het is. De naam maakt de entiteit uniek binnen dat domein.

---

# Veel voorkomende domeinen

Hieronder staan enkele domeinen die je regelmatig in voorbeelden zult tegenkomen.

## Sensor

Geeft meetwaarden weer.

Voorbeelden:

```yaml
sensor.batterij_percentage
sensor.buiten_temperatuur
sensor.zonnepanelen_vermogen
```

Mogelijke waarden:

```text
23
18.4
542
```

---

## Binary Sensor

Heeft slechts twee toestanden:

```text
on
off
```

Voorbeelden:

```yaml
binary_sensor.voordeur
binary_sensor.beweging_gang
binary_sensor.brievenbus
```

---

## Person

Wordt gebruikt om personen te volgen.

Voorbeelden:

```yaml
person.henk
person.marie
```

Mogelijke statussen:

```text
home
not_home
```

---

## Device Tracker

Wordt vaak gebruikt voor locatie- en aanwezigheidsdetectie.

Voorbeelden:

```yaml
device_tracker.samsung_s25
device_tracker.iphone_jan
```

---

## Input Boolean

Een handmatige aan/uit schakelaar die vaak wordt gebruikt om automatiseringen te activeren of tijdelijk uit te schakelen.

Voorbeelden:

```yaml
input_boolean.vakantie
input_boolean.bewaking_pauze
```

---

## Input Datetime

Wordt gebruikt om een datum of tijd op te slaan.

Voorbeeld:

```yaml
input_datetime.laatste_bezoek
```

---

# Waarom zijn mijn entity-ID's anders?

Iedere Home Assistant-installatie is uniek.
Wanneer een apparaat wordt toegevoegd, maakt Home Assistant automatisch een entity-ID aan op basis van:

- de apparaatnaam
- de gebruikte integratie
- eerdere namen die al bestaan
- eventuele wijzigingen die je later zelf hebt aangebracht

Daardoor kunnen dezelfde apparaten bij verschillende gebruikers totaal andere entity-ID's hebben.

Mijn configuratie:

```yaml
binary_sensor.brievenbus
```

Jouw configuratie:

```yaml
binary_sensor.postvak
```

of:

```yaml
binary_sensor.mailbox
```

of:

```yaml
binary_sensor.postsensor
```

Dit is allemaal correct zolang de entity-ID binnen jouw eigen installatie bestaat.

---

# Hoe vind ik mijn entity-ID's?

## Methode 1: Ontwikkelaarstools (aanbevolen)

Dit is meestal de snelste en meest betrouwbare methode.

Ga naar:

```text
Ontwikkelaarstools → Statussen
```

Bovenaan zie je een zoekveld.

Zoek bijvoorbeeld op:

```text
deur
```

of:

```text
brievenbus
```

of:

```text
batterij
```

Home Assistant toont vervolgens alle entiteiten die overeenkomen.

Bijvoorbeeld:

```text
sensor.telefoon_batterij

binary_sensor.brievenbus

person.jan
```

Deze entiteiten kunnen gebruikt worden in de YAML.

### Extra voordeel

Je ziet hier niet alleen de naam, maar ook direct:

- de huidige status
- het domein
- de attributen
- eventuele foutmeldingen

Daardoor is dit vaak de beste plek om problemen op te lossen.

---

## Methode 2: Via Apparaten en Diensten

Ga naar:

```text
Instellingen → Apparaten en diensten
```

Open vervolgens het apparaat.
Selecteer daarna een entiteit.

Je ziet dan onder andere:

```text
Entity-ID:
sensor.samsung_s25_battery_level
```

Deze naam kun je rechtstreeks overnemen in scripts of automatiseringen.

---

# Entity-ID's vervangen in voorbeelden

Stel dat een voorbeeld in deze repository het volgende bevat:

```yaml
entity_id: person.henk
```

Maar jouw persoon heet:

```yaml
person.jan
```

Dan wijzig je dit naar:

```yaml
entity_id: person.jan
```

Nog een voorbeeld.

In de documentatie staat:

```yaml
entity_id: binary_sensor.brievenbus
```

Maar jouw entiteit heet:

```yaml
binary_sensor.postvak
```

Dan wordt dit:

```yaml
entity_id: binary_sensor.postvak
```

De werking van het script blijft hetzelfde. Alleen de verwijzing naar de juiste entiteit verandert.

---

# Let op bij notificaties

Niet alleen entity-ID's verschillen per installatie.
Ook notificatieservices hebben vaak andere namen.

Voorbeeld uit mijn installatie:

```yaml
notify.henk
```

Bij jou kan dat bijvoorbeeld zijn:

```yaml
notify.mobile_app_samsung_s25
```

of:

```yaml
notify.mobile_app_iphone_van_jan
```

of:

```yaml
notify.gezinsmelding
```

Controleer daarom altijd of een genoemde notificatieservice daadwerkelijk bestaat.

Ga hiervoor naar:

```text
Ontwikkelaarstools → Acties
```

Zoek vervolgens op:

```text
notify
```

Je ziet dan alle beschikbare notificatieservices.

---

# Wat gebeurt er als een entity-ID niet bestaat?

Wanneer Home Assistant een entity niet kan vinden, kunnen verschillende dingen gebeuren.

Bijvoorbeeld:

```yaml
entity_id: person.henk
```

terwijl die entiteit niet bestaat.

Mogelijke gevolgen:

- een automatisering wordt niet gestart
- een script stopt met uitvoeren
- een voorwaarde wordt nooit waar
- meldingen worden niet verzonden
- foutmeldingen verschijnen in de logboeken

Controleer daarom altijd eerst of de gebruikte entiteiten daadwerkelijk bestaan.

---

# Entity-ID wijzigen of hernoemen

Soms wil je een bestaande entity een duidelijkere naam geven.

Dat kan via:

```text
Instellingen → Apparaten en diensten → Entiteiten
```

Open de entiteit en wijzig vervolgens de entity-ID.

Bijvoorbeeld:

Van:

```yaml
binary_sensor.contactsensor_8374
```

Naar:

```yaml
binary_sensor.voordeur
```

Dit maakt scripts vaak veel beter leesbaar.

Let wel op dat alle bestaande automatiseringen en dashboards daarna eventueel aangepast moeten worden.

---

# Veelgemaakte fouten

## Fout 1: Voorbeeldnamen letterlijk overnemen

Bijvoorbeeld:

```yaml
person.henk
```

terwijl die persoon in jouw systeem niet bestaat.

---

## Fout 2: Hoofdletters gebruiken

Dit is fout:

```yaml
Person.Henk
```

Dit is goed:

```yaml
person.henk
```

Entity-ID's zijn hoofdlettergevoelig.

---

## Fout 3: Verkeerde notificatieservice gebruiken

Bijvoorbeeld:

```yaml
notify.henk
```

terwijl jouw telefoon alleen beschikbaar is als:

```yaml
notify.mobile_app_samsung_s25
```

---

## Fout 4: Kijken naar de zichtbare naam in plaats van de entity-ID

De zichtbare naam kan bijvoorbeeld zijn:

```text
Voordeur
```

Maar de daadwerkelijke entity-ID:

```yaml
binary_sensor.voordeur_contact
```

Gebruik altijd de entity-ID.

---

# Handige controle vóór gebruik

Voordat je een script, blueprint of automatisering uit deze repository gebruikt, controleer dan het volgende:

- Bestaan alle genoemde entity-ID's?
- Bestaan alle gebruikte helpers?
- Bestaan alle notificatieservices?
- Zijn alle sensoren aanwezig?
- Zijn alle persoonsentiteiten aanwezig?
- Zijn alle namen aangepast aan jouw situatie?
- Geeft de YAML-controle geen fouten?

Een paar minuten controleren voorkomt vaak uren zoeken naar problemen.

---

# Samenvatting

Een entity-ID is de unieke naam waarmee Home Assistant een apparaat, sensor, helper of persoon herkent. Omdat iedere Home Assistant-installatie anders is, moeten voorbeelden uit deze repository vrijwel altijd worden aangepast aan de eigen situatie.

De snelste manier om jouw entity-ID's te vinden is via:

```text
Ontwikkelaarstools → Statussen
```

Controleer vóór gebruik van een script altijd of alle genoemde entities, notificatieservices en helpers daadwerkelijk bestaan in jouw installatie.
Vrijwel alle problemen die ontstaan bij het overnemen van voorbeelden zijn terug te voeren op ontbrekende of verkeerd ingevulde entity-ID's. Controleer deze daarom altijd als eerste.
