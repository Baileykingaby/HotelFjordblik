# Hotel Fjordblik – Personaleplanlægning (opgave A)

## Aktører og mål

**Primær aktør:** Hotelmanageren.

**Interessenter:**

- **Hotelmanageren:** Vil bemande ledige vagter og ændre eksisterende bemanding uden at bryde planlægningsreglerne.
- **Medarbejderen:** Vil kun tildeles vagter, der passer til vedkommendes rolle og rådighed og ikke overlapper andre vagter.

**Mål:**

1. Bemand en ledig vagt.
2. Skift medarbejder på en bemandet vagt.

## Fælles antagelser og regler

- En medarbejder har **én rolle** i denne afgrænsede case, og hver vagt kræver én bestemt rolle.
- Medarbejderens rådighed skal dække **hele vagtens tidsrum på den pågældende dato**.
- To vagter på samme dato overlapper, når `startA < slutB` **og** `startB < slutA`. En vagt må derfor begynde præcis, når en anden slutter.
- En vagt kan være tildelt **højst én medarbejder**, mens en medarbejder kan have flere vagter, som ikke overlapper.
- En vagt er bemandet, når der findes et bemandingslink mellem `Vagt` og `Medarbejder`. Der er ikke behov for en selvstændig statusattribut.
- Hvis et forslag bryder flere regler, viser systemet mindst én konkret afvisningsårsag, eksempelvis den første konstaterede fejl.
- `seMuligeMedarbejdere(vagtId)` viser en medarbejderoversigt med oplysninger om rolle og rådighed. Oversigten kan også vise medarbejdere, der ikke opfylder kravene, så ugyldige forslag kan vælges og afvises af systemet.
- Systemet registrerer kun ændringer i bemandingen, når **alle relevante regler** er opfyldt. Ved afvisning ændres ingen objekter, attributværdier eller links.

---

# Use case 1 – Bemand en ledig vagt

| Felt | Beskrivelse |
|---|---|
| **Navn** | Bemand en ledig vagt |
| **Mål** | Hotelmanageren tildeler en ledig vagt til en medarbejder, der opfylder kravene. |
| **Scope** | Personaleplanlægning |
| **Level** | User goal |
| **Primær aktør** | Hotelmanageren |
| **Interessenter og interesser** | **Hotelmanageren:** Vil bemande en ledig vagt korrekt. **Medarbejderen:** Vil kun tildeles en vagt med passende rolle og rådighed og uden overlap. |
| **Præbetingelser** | Hotelmanageren har adgang til personaleplanlægningen. Der findes en ledig vagt og registrerede medarbejdere i systemet. |
| **Postbetingelser** | **Ved succes:** Den valgte vagt er bemandet med den valgte medarbejder. **Ved afvisning:** Bemandingen er uændret. |

## Hovedscenarie (Main success scenario)

1. Hotelmanageren ønsker at bemande en ledig vagt.
2. Hotelmanageren vælger en ledig vagt.
3. Systemet viser vagtens dato, starttid, sluttid og krævede rolle.
4. Hotelmanageren anmoder om at se medarbejderoversigten via `seMuligeMedarbejdere(vagtId)`, og systemet viser medarbejdere med relevante oplysninger om rolle og rådighed.
5. Hotelmanageren foreslår en medarbejder til vagten via `tildelMedarbejder(vagtId, medarbejder)`.
6. Systemet kontrollerer, at vagten stadig er ubemandet, at medarbejderen har den krævede rolle, er til rådighed under hele vagten og ikke har overlappende vagter.
7. Systemet registrerer tildelingen ved at oprette et bemandingslink mellem vagten og medarbejderen.
8. Systemet bekræfter tildelingen.

## Alternative forløb (Extensions)

**6a. Medarbejderen har en overlappende vagt**

1. Systemet konstaterer, at den foreslåede medarbejder allerede er tildelt en vagt, der overlapper tidsrummet for den valgte vagt.
2. Systemet afviser tildelingen og forklarer overlappet.
3. Den valgte vagt forbliver ubemandet, og de eksisterende tildelinger bevares.
4. Hotelmanageren kan vende tilbage til trin 5 og foreslå en anden medarbejder.

**6b. Medarbejderen har ikke den krævede rolle**

1. Systemet konstaterer, at den foreslåede medarbejders rolle ikke svarer til vagtens krævede rolle.
2. Systemet afviser tildelingen og forklarer rollefejlen.
3. Den valgte vagt forbliver ubemandet, og de eksisterende tildelinger bevares.
4. Hotelmanageren kan vende tilbage til trin 5 og foreslå en anden medarbejder.

**6c. Medarbejderens rådighed dækker ikke hele vagten**

1. Systemet konstaterer, at medarbejderen ikke er til rådighed under hele vagtens tidsrum.
2. Systemet afviser tildelingen og forklarer, at rådigheden er utilstrækkelig.
3. Ingen tildelinger ændres.
4. Hotelmanageren kan vende tilbage til trin 5 og foreslå en anden medarbejder.

**6d. Vagten er allerede blevet bemandet**

1. Systemet konstaterer, at vagten ikke længere er ledig.
2. Systemet afviser tildelingen og forklarer, at vagten allerede er bemandet.
3. Den eksisterende bemanding bevares uændret.
4. Hotelmanageren kan vælge en anden ledig vagt fra trin 2.

## Operationskontrakter – Use case 1

### `seMuligeMedarbejdere`

| Definition | Beskrivelse |
|---|---|
| **Operation** | `seMuligeMedarbejdere(vagtId)` |
| **Cross reference** | Bemand en ledig vagt |
| **Præbetingelser** | Vagten med `vagtId` eksisterer i systemet. |
| **Postbetingelser** | En medarbejderoversigt med oplysninger om rolle og rådighed er returneret. Oversigten kan også omfatte medarbejdere, der ikke opfylder kravene. |
| **Objekter** | Ingen objekter oprettes eller slettes. |
| **Attributværdier** | Ingen attributværdier ændres. |
| **Links** | Ingen links oprettes eller fjernes. |
| **Resultat** | Systemet viser medarbejderoversigten. Domænets tilstand er uændret. |

### `tildelMedarbejder`

| Definition | Beskrivelse |
|---|---|
| **Operation** | `tildelMedarbejder(vagtId, medarbejder)` |
| **Cross reference** | Bemand en ledig vagt |
| **Præbetingelser** | Vagten med `vagtId` og den foreslåede medarbejder eksisterer i systemet. Tildelingens gyldighed kontrolleres under operationen. |
| **Postbetingelser** | **Ved godkendelse:** Medarbejderen er tilknyttet den valgte vagt. **Ved afvisning:** Domænets tilstand er uændret. |
| **Objekter** | Ingen objekter oprettes eller slettes. |
| **Attributværdier** | Ingen attributværdier ændres. Vagtens `vagtId`, `dato`, `starttid`, `sluttid` og krævede rolle bevares. |
| **Links** | **Ved godkendelse:** Ét bemandingslink mellem `Medarbejder` og `Vagt` oprettes; øvrige links bevares. **Ved afvisning:** Ingen links ændres. |
| **Resultat** | Tildelingen godkendes kun, hvis vagten er ubemandet, medarbejderens rolle passer, rådigheden dækker hele vagten, og medarbejderen ikke har overlappende vagter. Ellers afvises forslaget med en begrundelse, og ingen objekter, attributværdier eller links ændres. |

---

# Use case 2 – Skift medarbejder på en vagt

| Felt | Beskrivelse |
|---|---|
| **Navn** | Skift medarbejder på en vagt |
| **Mål** | Hotelmanageren erstatter den medarbejder, der er tildelt en vagt, med en anden medarbejder, som opfylder kravene. |
| **Scope** | Personaleplanlægning |
| **Level** | User goal |
| **Primær aktør** | Hotelmanageren |
| **Interessenter og interesser** | **Hotelmanageren:** Vil ændre bemandingen uden at efterlade vagten ubemandet. **Medarbejderne:** Vil have ændringer, der respekterer roller, rådighed og øvrige vagter. |
| **Præbetingelser** | Hotelmanageren har adgang til personaleplanlægningen. Der findes en bemandet vagt og registrerede medarbejdere i systemet. Et afbud kan være årsagen til skiftet, men er ikke et krav. |
| **Postbetingelser** | **Ved succes:** Vagten er tildelt den nye medarbejder i stedet for den tidligere. **Ved afvisning:** Den tidligere medarbejder beholder vagten, og alle øvrige tildelinger er uændrede. |

## Hovedscenarie (Main success scenario)

1. Hotelmanageren ønsker at ændre bemandingen af en eksisterende vagt.
2. Hotelmanageren vælger en bemandet vagt via `vælgVagt(vagtId)`.
3. Systemet viser vagtens dato, starttid, sluttid, krævede rolle og nuværende medarbejder.
4. Hotelmanageren foreslår en ny medarbejder til vagten via `skiftMedarbejder(vagtId, medarbejder)`.
5. Systemet kontrollerer, at medarbejderen har den krævede rolle, er til rådighed under hele vagten og ikke har overlappende vagter.
6. Systemet registrerer skiftet ved at fjerne bemandingslinket til den tidligere medarbejder og oprette et link til den nye. Den tidligere tilknytning fjernes først, når den nye tildeling kan gennemføres.
7. Systemet bekræfter, at medarbejderen på vagten er skiftet.

## Alternative forløb (Extensions)

**5a. Den foreslåede medarbejder har en overlappende vagt**

1. Systemet konstaterer, at den foreslåede medarbejder allerede er tildelt en vagt, der overlapper den valgte vagt.
2. Systemet afviser skiftet og forklarer overlappet.
3. Den tidligere medarbejder beholder vagten, og alle øvrige tildelinger bevares.
4. Hotelmanageren kan vende tilbage til trin 4 og foreslå en anden medarbejder.

**5b. Den foreslåede medarbejder har ikke den krævede rolle**

1. Systemet konstaterer, at den foreslåede medarbejders rolle ikke svarer til vagtens krævede rolle.
2. Systemet afviser skiftet og forklarer rollefejlen.
3. Den tidligere medarbejder beholder vagten, og alle øvrige tildelinger bevares.
4. Hotelmanageren kan vende tilbage til trin 4 og foreslå en anden medarbejder.

**5c. Den foreslåede medarbejders rådighed dækker ikke hele vagten**

1. Systemet konstaterer, at medarbejderen ikke er til rådighed under hele vagtens tidsrum.
2. Systemet afviser skiftet og forklarer, at rådigheden er utilstrækkelig.
3. Den tidligere medarbejder beholder vagten, og alle øvrige tildelinger bevares.
4. Hotelmanageren kan vende tilbage til trin 4 og foreslå en anden medarbejder.

## Operationskontrakter – Use case 2

### `vælgVagt`

| Definition | Beskrivelse |
|---|---|
| **Operation** | `vælgVagt(vagtId)` |
| **Cross reference** | Skift medarbejder på en vagt |
| **Præbetingelser** | Vagten med `vagtId` eksisterer i systemet. |
| **Postbetingelser** | Oplysninger om den valgte vagt er returneret. |
| **Objekter** | Ingen objekter oprettes eller slettes. |
| **Attributværdier** | Ingen attributværdier ændres. |
| **Links** | Ingen links oprettes eller fjernes. |
| **Resultat** | Systemet returnerer vagtens dato, starttid, sluttid, krævede rolle og nuværende medarbejder. Domænets tilstand er uændret. |

### `skiftMedarbejder`

| Definition | Beskrivelse |
|---|---|
| **Operation** | `skiftMedarbejder(vagtId, medarbejder)` |
| **Cross reference** | Skift medarbejder på en vagt |
| **Præbetingelser** | Vagten med `vagtId` eksisterer og er tildelt en medarbejder. Den foreslåede medarbejder eksisterer i systemet. Skiftets gyldighed kontrolleres under operationen. |
| **Postbetingelser** | **Ved godkendelse:** Vagten er tildelt den foreslåede medarbejder i stedet for den tidligere. **Ved afvisning:** Den tidligere medarbejder beholder vagten, og domænets tilstand er uændret. |
| **Objekter** | Ingen objekter oprettes eller slettes. De eksisterende `Vagt`- og `Medarbejder`-objekter bevares. |
| **Attributværdier** | Ingen attributværdier ændres. Vagtens `vagtId`, `dato`, `starttid`, `sluttid` og krævede rolle bevares. |
| **Links** | **Ved godkendelse:** Linket mellem vagten og den tidligere medarbejder fjernes, og et nyt link til den nye medarbejder oprettes. Alle øvrige links bevares. **Ved afvisning:** Ingen links ændres. |
| **Resultat** | Skiftet godkendes kun, hvis den foreslåede medarbejder har den krævede rolle, er til rådighed under hele vagten og ikke har overlappende vagter. Ved afvisning forklarer systemet årsagen, og den eksisterende bemanding bevares. Det gamle bemandingslink fjernes først, når den nye tildeling kan gennemføres. |

---

## Afgrænsning af casens konkrete eksempel

Use cases og operationskontrakter ovenfor er **generiske**. Følgende konkrete oplysninger hører til opgavens eksempel og bruges ved udarbejdelsen af objektbillederne:

- **Før tildeling:** V01 er en ubemandet receptionistvagt kl. 07.00–15.00. V04 er bemandet med Noah.
- **Efter tildeling:** Sara er tildelt V01; Noah har fortsat V04.
- **Før skift:** Sara har V01, Noah har V04, og Emil findes i systemet uden tildelte vagter.
- **Efter skift:** Sara beholder V01; Emil overtager V04 fra Noah.
- **Antagelse om V04:** Opgavegrundlaget er modstridende. Her antages V04 at være en receptionistvagt kl. 12.00–15.00, oprindeligt bemandet med Noah.
- Alle vagter og rådighedsperioder i det konkrete eksempel gælder **samme workshopdag**.
