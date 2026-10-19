## Aktør
Hotelmanageren 
medarbjeder

## mål
bemande vagt


# Use-Case 1
Scope: Personaleplanlægning 
Level: User goal
primary actor: Hotelmanageren
Stakeholders and interests: 
    Hotelmanageren: vil gerne bemande et ubemandet vagt med en medarbejder.
    Medarbjeder: vil gerne have tildelt en vagt med passende rolle.
preconditions: 
    ubemandet vagt
postconditions: 
    vagt er bemandet med passende rolle og tidsrum. 
Main success:
    1. Hotelmanageren vil gerne bemande en vagt
    2. Hotelmanageren vælger den ledig vagt
    3. Hotelmanageren tildeler vagt til gyldig medarbjeder
    4. systemet skal registrerer vagten 
    5. systemet viser resultatet af tildeling
Extensions:
    a. Medarbejderen har en overlappende vagt:
        1. Hotelmanageren vælger Noah til V01 kl. 07.00–15.00.
        2. Systemet opdager, at Noah allerede er tildelt V04
        kl. 12.00–15.00.
        3. Systemet afviser tildelingen og forklarer, at vagterne overlapper.
        4. V01 forbliver ubemandet, og Noah beholder V04.
        5. Hotelmanageren kan vælge en anden medarbejder i trin 3.

    b. Medarbejderen har ikke den krævede rolle:
        1. Hotelmanageren vælger Ida til V01, som kræver en receptionist.
        2. Systemet opdager, at Ida har rollen køkken.
        3. Systemet afviser tildelingen og forklarer, at medarbejderen
        ikke har den krævede rolle.
        4. V01 forbliver ubemandet, og de eksisterende tildelinger er uændrede.
        5. Hotelmanageren kan vælge en anden medarbejder i trin 3.



## Use case 1: Bemande vagt

### seMuligeMedarbejdere

| Definition | Beskrivelse |
|---|---|
| Operation | `seMuligeMedarbejdere(vagtId)` |
| Cross reference | Bemande vagt |
| Præbetingelser | Vagten med `vagtId` eksisterer og er ubemandet. |
| Postbetingelser | Systemet returnerer medarbejdere med den krævede rolle, som er til rådighed og ikke har overlappende vagter. |
| Objekter | Ingen objekter oprettes eller slettes. |
| Attributværdier | Ingen attributværdier ændres. |
| Links | Ingen links oprettes eller fjernes. |
| Resultat | En liste over mulige medarbejdere returneres. Domænets tilstand er uændret. |

### tildelMedarbejder

| Definition | Beskrivelse |
|---|---|
| Operation | `tildelMedarbejder(vagtId, medarbejder)` |
| Cross reference | Bemande vagt |
| Præbetingelser | Vagten og medarbejderen eksisterer. Vagten er ubemandet. Medarbejderen har den krævede rolle, er til rådighed og har ingen overlappende vagter. |
| Postbetingelser | Medarbejderen er tilknyttet vagten, som nu er bemandet. |
| Objekter | Ingen objekter oprettes eller slettes. |
| Attributværdier | Ingen attributværdier ændres. Vagtens `vagtId`, `dato`, `starttid` og `sluttid` bevares. |
| Links | Et nyt link oprettes mellem `Medarbejder` og `Vagt`. Eksisterende links bevares. |
| Resultat | Tildelingen bekræftes. Ved ugyldig tildeling forbliver domænets tilstand uændret. |
 


# Use-Case 2
Scope: Personaleplanlægning 
Level: User goal
primary actor: Hotelmanageren
Stakeholders and interests: 
    Hotelmanageren: vil gerne ændre en bemandet vagt 

    Medarbjeder: vil gerne have ændret en vagt med passende rolle.
preconditions: 
    en bemandet vagt bliver meldt afbud på
postconditions: 
    vagt er bemandet af ny medarbjeder med passende rolle og tidsrum eller er blevet ubemandet. 
Main success:  
    1. Hotelmanageren vil gerne ændre en bemandet vagt
    2. Hotelmanageren vælger en bemandet vagt
    3. Hotelmanageren tildeler vagt til en ny gyldig medarbjeder
    4. systemet skal registrerer vagten 
    5. systemet viser resultatet af tildeling
Extensions:
a. Medarbejderen har en overlappende vagt:
    1. Hotelmanageren vælger Sara til at overtage V04 kl. 12.00–15.00.
    2. Systemet opdager, at Sara allerede har V01 kl. 07.00–15.00.
    3. Systemet afviser ændringen og forklarer, at vagterne overlapper.
    4. Noah beholder V04, og Sara beholder V01.
    5. Hotelmanageren kan vælge en anden medarbejder i trin 3.

b. Medarbejderen har ikke den krævede rolle:
    1. Hotelmanageren vælger Ida til at overtage V04.
    2. Systemet opdager, at Ida har rollen køkken,
       mens V04 kræver en receptionist.
    3. Systemet afviser ændringen og forklarer den manglende rolle.
    4. Noah beholder V04, og de øvrige tildelinger er uændrede.
    5. Hotelmanageren kan vælge en anden medarbejder i trin 3.

## OCs: Use case 2: Skift medarbejder på en vagt

### vælgVagt

| Definition | Beskrivelse |
|---|---|
| Operation | `vælgVagt(vagtId)` |
| Cross reference | Skift medarbejder på en vagt |
| Præbetingelser | Vagten med `vagtId` eksisterer i systemet. |
| Postbetingelser | Oplysninger om den valgte vagt er returneret. |
| Objekter | Ingen objekter oprettes eller slettes. |
| Attributværdier | Ingen attributværdier ændres. |
| Links | Ingen links oprettes eller fjernes. |
| Resultat | Den valgte vagts dato, starttid, sluttid, krævede rolle og nuværende medarbejder returneres. Domænets tilstand er uændret. |

### skiftMedarbejder

| Definition | Beskrivelse |
|---|---|
| Operation | `skiftMedarbejder(vagtId, medarbejder)` |
| Cross reference | Skift medarbejder på en vagt |
| Præbetingelser | Vagten med `vagtId` eksisterer og er tildelt en medarbejder. Den foreslåede medarbejder eksisterer i systemet. |
| Postbetingelser | Ved gyldig tildeling er vagten overdraget til den foreslåede medarbejder. Ved ugyldig tildeling er domænets tilstand uændret. |
| Objekter | Ingen objekter oprettes eller slettes. De eksisterende `Vagt`- og `Medarbejder`-objekter bevares. |
| Attributværdier | Vagtens `vagtId`, `dato`, `starttid` og `sluttid` forbliver uændrede. Den krævede rolle ændres ikke. |
| Links | Ved gyldig tildeling fjernes linket mellem vagten og den tidligere medarbejder, og et nyt link oprettes mellem vagten og den nye medarbejder. Alle øvrige links bevares. |
| Resultat | Tildelingen godkendes, hvis medarbejderen har den krævede rolle, er til rådighed i vagtens tidsrum og ikke har overlappende vagter. Ellers afvises tildelingen, og den eksisterende tilknytning bevares. Ingen objekter, attributværdier eller links ændres ved afvisning. |



