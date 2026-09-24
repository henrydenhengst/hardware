# FreeBoot Hardware Migratie Monitor

Dit is de centrale verzamelplek voor hardwareprofielen van computers die zijn gemigreerd naar Linux.
Elk bestand is een momentopname van een machine, vastgelegd met lshw of een ander tool.

Nog lang niet alles is getest. Het zou best kunnen dat er in deze scripts in de readme nog fouten zitten.

## Hoe wordt een profiel gemaakt?

Op elke gemigreerde laptop of desktop computer voeren we uit:
```
sudo apt install lshw hwinfo -y
sudo lshw > [hostname].txt
```

Voor een iPad met jailbreak, run dit script vanaf je computer.

Maak hiervoor een bashscript aan: ipad_lshw.sh

```
#!/bin/sh
#
# iPad Hardware Inventory Script
# Voor gejailbreakte iPads
#

HOSTNAME=$(hostname | tr ' ' '_' | tr -cd '[:alnum:]_-')
DATE=$(date +"%Y%m%d_%H%M%S")

OUT="/tmp/ipad_hardware_${HOSTNAME}_${DATE}.txt"

echo "============================================" > "$OUT"
echo " iPad Hardware Inventory Report" >> "$OUT"
echo " Hostname: $HOSTNAME" >> "$OUT"
echo " Datum: $(date)" >> "$OUT"
echo "============================================" >> "$OUT"


echo "" >> "$OUT"
echo "############################################" >> "$OUT"
echo "# Systeeminformatie" >> "$OUT"
echo "############################################" >> "$OUT"

uname -a >> "$OUT" 2>&1

sw_vers >> "$OUT" 2>&1


echo "" >> "$OUT"
echo "############################################" >> "$OUT"
echo "# Hardware Identificatie" >> "$OUT"
echo "############################################" >> "$OUT"

echo "--- Model ---" >> "$OUT"
sysctl hw.model >> "$OUT" 2>&1

echo "--- Machine ---" >> "$OUT"
sysctl hw.machine >> "$OUT" 2>&1

echo "--- Board ---" >> "$OUT"
sysctl hw.board >> "$OUT" 2>&1


echo "" >> "$OUT"
echo "############################################" >> "$OUT"
echo "# CPU informatie" >> "$OUT"
echo "############################################" >> "$OUT"

sysctl hw.ncpu >> "$OUT" 2>&1
sysctl hw.cpufrequency >> "$OUT" 2>&1
sysctl machdep.cpu >> "$OUT" 2>&1


echo "" >> "$OUT"
echo "############################################" >> "$OUT"
echo "# Geheugen" >> "$OUT"
echo "############################################" >> "$OUT"

sysctl hw.memsize >> "$OUT" 2>&1
sysctl vm.swapusage >> "$OUT" 2>&1


echo "" >> "$OUT"
echo "############################################" >> "$OUT"
echo "# Opslag" >> "$OUT"
echo "############################################" >> "$OUT"

df -h >> "$OUT" 2>&1


echo "" >> "$OUT"
echo "############################################" >> "$OUT"
echo "# Mount punten" >> "$OUT"
echo "############################################" >> "$OUT"

mount >> "$OUT" 2>&1


echo "" >> "$OUT"
echo "############################################" >> "$OUT"
echo "# IORegistry Hardware Tree" >> "$OUT"
echo "############################################" >> "$OUT"

ioreg -l >> "$OUT" 2>&1


echo "" >> "$OUT"
echo "############################################" >> "$OUT"
echo "# USB / Lightning informatie" >> "$OUT"
echo "############################################" >> "$OUT"

ioreg -p IOUSB -l >> "$OUT" 2>&1


echo "" >> "$OUT"
echo "############################################" >> "$OUT"
echo "# Batterij informatie" >> "$OUT"
echo "############################################" >> "$OUT"

ioreg -rn AppleSmartBattery >> "$OUT" 2>&1


echo "" >> "$OUT"
echo "############################################" >> "$OUT"
echo "# Netwerk interfaces" >> "$OUT"
echo "############################################" >> "$OUT"

ifconfig >> "$OUT" 2>&1


echo "" >> "$OUT"
echo "############################################" >> "$OUT"
echo "# Actieve processen" >> "$OUT"
echo "############################################" >> "$OUT"

ps aux >> "$OUT" 2>&1


echo "" >> "$OUT"
echo "############################################" >> "$OUT"
echo "# Kernel extensies / Jailbreak componenten" >> "$OUT"
echo "############################################" >> "$OUT"

kextstat >> "$OUT" 2>&1


echo "" >> "$OUT"
echo "############################################" >> "$OUT"
echo "# Jailbreak omgeving" >> "$OUT"
echo "############################################" >> "$OUT"

ls -la /Applications >> "$OUT" 2>&1
ls -la /Library/MobileSubstrate/DynamicLibraries >> "$OUT" 2>&1


echo "" >> "$OUT"
echo "############################################" >> "$OUT"
echo "# Volledige sysctl dump" >> "$OUT"
echo "############################################" >> "$OUT"

sysctl -a >> "$OUT" 2>&1


echo "" >> "$OUT"
echo "============================================" >> "$OUT"
echo " Rapport klaar" >> "$OUT"
echo " Bestand: $OUT" >> "$OUT"
echo "============================================" >> "$OUT"


echo ""
echo "Klaar."
echo "Rapport opgeslagen als:"
echo "$OUT"
```

Op Android tablets of smartphones werkt het via ADB:
- Als je USB-foutopsporing hebt ingeschakeld:
- Installeer Android Platform Tools op je computer.
- Sluit de Android aan via USB.
- Open een terminal of opdrachtprompt.
- Voer uit:

```
adb devices
adb shell getprop > [hostname].txt
adb shell dumpsys >> [hostname].txt
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
