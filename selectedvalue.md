Gebruik van SELECTEDVALUE en rijcontext in DAX measures

In DAX wordt vaak gebruik gemaakt van SELECTEDVALUE() om een enkele waarde uit een kolom op te halen binnen een filtercontext. Dit werkt goed in situaties waarin de visual of berekening gegarandeerd slechts één waarde per context bevat (bijvoorbeeld één geselecteerd jaar of één geselecteerd item).

Beperking van SELECTEDVALUE

SELECTEDVALUE() geeft alleen een resultaat wanneer er exact één waarde aanwezig is in de huidige filtercontext. Zodra er meerdere waarden aanwezig zijn (bijvoorbeeld in totalen of subtotalen in een tabelvisual), retourneert deze functie BLANK().

Dit gedrag kan leiden tot onverwachte resultaten, zoals:
•	het wegvallen van totalen in visuals 
•	measures die wel werken op detailniveau, maar niet op aggregatieniveau 
•	verschillen tussen rijen en total rows in dezelfde tabel

Oplossing: gebruik van itererende functies (SUMX)
Om consistente resultaten te krijgen op zowel detailniveau als totalen, is het beter om te werken met itererende functies zoals SUMX() in combinatie met directe kolomreferenties.
In plaats van afhankelijk te zijn van SELECTEDVALUE(), wordt de berekening per rij uitgevoerd binnen de tabelcontext. Hierdoor:
•	wordt elke rij individueel geëvalueerd 
•	zijn resultaten onafhankelijk van de visual context 
•	worden totalen automatisch correct berekend als som van alle rijen

Voorbeeld
Minder robuuste aanpak (gevoelig voor contextproblemen):
VAR waarde = SELECTEDVALUE(Tabel[kolom])
RETURN waarde * [maat]
Deze aanpak kan falen in total rows.
________________________________________

Robuuste aanpak (aanbevolen):
SUMX(
    Tabel,
    Tabel[kolom] * [maat]
)
Hierdoor wordt de berekening per rij uitgevoerd en daarna opgeteld, waardoor zowel detailregels als totalen correct worden weergegeven.
________________________________________
Conclusie
SELECTEDVALUE() is geschikt voor enkelvoudige contexten, maar niet voor berekeningen die ook correct moeten werken in totalen of meerdere rijen tegelijk.
Voor betrouwbare en schaalbare measures in Power BI is het gebruik van iteratieve functies zoals SUMX() in combinatie met directe rijcontext in de meeste gevallen de beste keuze.

