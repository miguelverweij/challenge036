# challenge036

``` mermaid
sequenceDiagram
    participant U as Gebruiker
    participant S as SharePoint Site (UI)
    participant L as SharePoint Lijsten
    participant F as Power Automate Flow

    U->>S: Bezoek pagina & selecteer dienst (Boeken)
    S->>L: Schrijf dienstboeking weg (CRUD actie)
    L->>F: Trigger flow bij nieuwe boeking
    F->>L: Valideer/werk gegevens bij indien nodig
    F->>U: Verstuur bevestiging e-mail
    U->>S: Vernieuwt pagina en ziet geboekte dienst

```

# Datamodel
``` mermaid
classDiagram
  
namespace RhythmOfBusiness {
  
    %% RoB Calendar Events
    class DienstenCalendarEvents {
        DienstTypeID: Lookup DienstType
        ReeksID: Lookup Reeks
        Naam dienst: Enkele tekstregel
        Datum: Datum en tijd
        Buddy nodig: Ja/Nee
    }
  
    %% RoB Calendar Refiners
    class Refiners {
        DienstTypeID: Lookup DienstType
        ReeksID: Lookup Reeks
        Naam dienst: Enkele tekstregel
        Datum: Datum en tijd
        Buddy nodig: Ja/Nee
    }
  
    %% RoB Calendar Refiner Values
    class RefinerValues {
        DienstTypeID: Lookup DienstType
        ReeksID: Lookup Reeks
        Naam dienst: Enkele tekstregel
        Datum: Datum en tijd
        Buddy nodig: Ja/Nee
    }
  
    %% RoB Calendar Approvers
    class Approvers {
        DienstTypeID: Lookup DienstType
        ReeksID: Lookup Reeks
        Naam dienst: Enkele tekstregel
        Datum: Datum en tijd
        Buddy nodig: Ja/Nee
    }
  
    %% RoB Calendar Configuration
    class CalendarConfiguration {
        DienstTypeID: Lookup DienstType
        ReeksID: Lookup Reeks
        Naam dienst: Enkele tekstregel
        Datum: Datum en tijd
        Buddy nodig: Ja/Nee
    }
}
  
class AirportStar {
    Nummer: Enkele tekstregel (auto-nummer: STAR-XXXXXX)
    Persoon: Persoon of groep
    Toegangspastype: Keuze
    Rijbewijzen: Keuze-meervoudig
    Procesvoorkeur: Keuze-meervoudig
    Mobiele telefoon: Enkele tekstregel
}
  
class ResponseStarAanmelding {
    Procesvoorkeur: Keuze-meervoudig
    Leidinggevende geïnformeerd: Ja/Nee
    Communicatie-app geïnstalleerd: Ja/Nee
    Mobiele telefoon: Enkele tekstregel
}
  
class BusStarAanmelding {
    AirportStar: Lookup AirportStar (om de velden te tonen op de view)
    Aanvraag goedgekeurd op: Datum en tijd
    Werkt reguliere uren: Ja/Nee
    Bewaartermijn: Keuze (is dit nodig?)
    Goedgekeurd door leidinggevende: Ja/Nee
    Ervaring met grote voertuigen: Ja/Nee
    Details ervaring grote voertuigen: Meerdere tekstregels
    Ervaring airside: Ja/Nee
    Details airside ervaring: Meerdere tekstregels
}
  
class SnowStarAanmelding {
    AirportStar: Lookup AirportStar (om de velden te tonen op de view)
    Aanvraag goedgekeurd op: Datum en tijd
    Werkt reguliere uren: Ja/Nee
    Heeft piketdiensten: Ja/Nee
    Binnen één uur aanwezig: Ja/Nee
    Goedgekeurd door leidinggevende: Ja/Nee
    Fysiek en mentaal geschikt: Ja/Nee
    Motivatie: Meerdere tekstregels
    Heeft leaseauto: Ja/Nee
    Bewaartermijn: Keuze (is dit nodig?)
}
  
class DienstType {
    Naam: Enkele tekstregel
    Beschrijving: Meerdere tekstregels
    Team of losse dienst: Keuze
    Categorie: Keuze
    Logo: Enkele tekstregel
    Beantwoordadres: Enkele tekstregel
    Website: Enkele tekstregel
    Telefoonnummer: Enkele tekstregel
    Toegestane toegangspastypen: Keuze-meervoudig
    Rijbewijs B vereist: Ja/Nee
    Interne medewerker vereist: Ja/Nee
    Maximum aantal deelnemers: Nummer
    Duur min: Nummer
    Maximale vooruitboekingstijd uren: Nummer
    Minimale startvoorloop uren: Nummer
    Locatie: Enkele tekstregel
}
  
class Boekingen {
    AirportStarID: Lookup AirportStar
    DienstID: Lookup Dienst
    Geannuleerd: Ja/Nee
}
  
%% RoB Relaties
RefinerValues "1" --> "0..*" DienstenCalendarEvents
Refiners "1" --> "0..*" RefinerValues
RefinerValues "1" --> "0..*" Approvers

%% Relaties
AirportStar "1" --> "0..*" Boekingen : kan boekingen hebben
DienstenCalendarEvents "1" --> "0..*" Boekingen : kan boekingen hebben

AirportStar "1" --> "0..1" ResponseStarAanmelding : aangemeld voor
AirportStar "1" --> "0..1" BusStarAanmelding : aangemeld voor
AirportStar "1" --> "0..1" SnowStarAanmelding : aangemeld voor

DienstType "1" --> "1..*" DienstenCalendarEvents : template voor dienst
```

```stl
solid cube_corner
  facet normal 0.0 -1.0 0.0
    outer loop
      vertex 0.0 0.0 0.0
      vertex 1.0 0.0 0.0
      vertex 0.0 0.0 1.0
    endloop
  endfacet
  facet normal 0.0 0.0 -1.0
    outer loop
      vertex 0.0 0.0 0.0
      vertex 0.0 1.0 0.0
      vertex 1.0 0.0 0.0
    endloop
  endfacet
  facet normal -1.0 0.0 0.0
    outer loop
      vertex 0.0 0.0 0.0
      vertex 0.0 0.0 1.0
      vertex 0.0 1.0 0.0
    endloop
  endfacet
  facet normal 0.577 0.577 0.577
    outer loop
      vertex 1.0 0.0 0.0
      vertex 0.0 1.0 0.0
      vertex 0.0 0.0 1.0
    endloop
  endfacet
endsolid
```
