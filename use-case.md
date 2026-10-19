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


 