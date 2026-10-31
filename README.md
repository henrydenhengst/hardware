# FreeBoot Hardware Migratie Monitor

Dit is de centrale verzamelplek voor hardwareprofielen van computers die zijn gemigreerd naar Linux.
Elk bestand is een momentopname van een machine, vastgelegd met lshw of een ander tool.

## Hoe wordt een profiel gemaakt?

Op elke gemigreerde laptop of desktop computer voeren we uit:
```
sudo apt install lshw hwinfo -y
sudo lshw > [hostname].txt
```

Het resulterende txt-bestand wordt toegevoegd aan deze repo.

## Wat meten we?

- CPU, RAM, schijven, netwerkkaarten, moederbord etc.

Zo kunnen we de footprint van ons werk in kaart brengen: welke hardware redden we van 'planned obsolescence'.

## Bijdragen medewerkers FreeBoot:

1. Fork deze repo
2. Voeg toe jouw [hostname].txt
3. Dien een pull request in

Of stuur het bestand door naar de coördinator als je geen Git ervaring hebt.

## Doelstellingen per maand

- 10+ gemigreerde systemen geregistreerd
- Overzicht van onze bespaarde e-waste

---

>Vrijheid begint bij transparantie en bij de hardware die we een tweede leven geven.
