# Bash Scripting Cheat Sheet für Anfänger
## 1. Grundlagen und Best Practices
### A. Skript-Struktur

|   |   |   |
|---|---|---|
|**Konzept**|**Syntax**|**Zweck**|
|**Shebang (Anfang)**|`#!/usr/bin/env bash`|Definiert, dass das Skript mit Bash ausgeführt werden soll. Immer die erste Zeile.|
|**Kommentar**|`# Das ist ein Kommentar`|Erklärt, was das Skript tut.|
|**Ausführbarkeit**|`chmod +x skriptname.sh`|Macht das Skript ausführbar.|

### B. Der "Strict Mode" (Wichtig!)

Der Strict Mode hilft, häufige Fehler zu vermeiden. **Fügen Sie dies immer am Anfang Ihres Skripts ein.**

```
set -euo pipefail
IFS=$'\n\t'
```

|   |   |
|---|---|
|**Option**|**Erklärung**|
|**`-e`**|Exit sofort, wenn ein Befehl fehlschlägt.|
|**`-u`**|Behandelt die Verwendung von nicht gesetzten Variablen als Fehler.|
|**`-o pipefail`**|Der Rückgabewert einer Pipeline ist der Wert des letzten Befehls, der fehlschlägt (nicht der letzte in der Pipe).|
|**`IFS=$'\n\t'`**|Setzt den Internal Field Separator (IFS) nur auf Zeilenumbruch und Tab. Verhindert Probleme mit Leerzeichen in Dateinamen.|

## 2. Variablen und Quoting

### A. Variable setzen & verwenden

```
DATEI_NAME="bericht.txt"
BENUTZER=$(whoami) # Befehlsausgabe in Variable speichern
```

### B. Quoting (Anführungszeichen)

Quoting ist entscheidend, um Leerzeichen oder Sonderzeichen in Variablen korrekt zu handhaben.

|   |   |   |
|---|---|---|
|**Typ**|**Syntax**|**Effekt**|
|**Doppelte**|`"Hallo $NAME"`|**Variablen werden erweitert.** Bester Standard.|
|**Einfache**|`'Hallo $NAME'`|**Variablen werden NICHT erweitert.** Wörtliche Ausgabe.|
|**Ungequotet**|`echo $VAR`|**Gefährlich.** Kann zu Problemen mit Leerzeichen führen. Vermeiden Sie dies.|

### C. Variablen-Erweiterung (Parameter Expansion)

|   |   |   |   |
|---|---|---|---|
|**Syntax**|**Beispiel (bei PFAD="/home/user/log.txt")**|**Ergebnis**|**Zweck**|
|**Substring**|`${PFAD:11:3}`|`use`|Startet bei Index 11, nimmt 3 Zeichen.|
|**Länge**|`${#PFAD}`|`19`|Gibt die Länge des Strings zurück.|
|**Suffix entfernen**|`${PFAD%.txt}`|`/home/user/log`|Entfernt das kürzeste Suffix.|
|**Dateiname**|`${PFAD##*/}`|`log.txt`|Entfernt den längsten Präfix bis zum letzten `/`.|
|**Standardwert**|`${USER:-Gast}`|`$USER` oder **"Gast"**|Wenn `$USER` nicht gesetzt, verwende "Gast".|
|**Fehler bei Null**|`${PFAD:?Muss gesetzt sein}`|Fehler, falls `PFAD` nicht gesetzt ist.||

## 3. Bedingungen (Conditionals)

Verwenden Sie `[[ ... ]]` für Bedingungen.

### A. Datei-Tests

|   |   |
|---|---|
|**Bedingung**|**Beschreibung**|
|`[[ -e $DATEI ]]`|Datei oder Verzeichnis existiert.|
|`[[ -f $DATEI ]]`|Ist eine reguläre Datei.|
|`[[ -d $PFAD ]]`|Ist ein Verzeichnis.|
|`[[ -x $BEFEHL ]]`|Ist ausführbar.|
|`[[ $D1 -nt $D2 ]]`|Datei D1 ist neuer als D2.|

### B. String-Tests

|   |   |
|---|---|
|**Bedingung**|**Beschreibung**|
|`[[ -z $STR ]]`|String ist leer (Zero length).|
|`[[ -n $STR ]]`|String ist nicht leer (Non-zero length).|
|`[[ $S1 == $S2 ]]`|String S1 ist gleich S2.|
|`[[ $S1 != $S2 ]]`|String S1 ist ungleich S2.|
|`[[ $S1 =~ $REGEXP ]]`|String S1 passt zum regulären Ausdruck.|

### C. Numerische Tests

Für Zahlenvergleiche verwenden Sie die `-` Operatoren.

|   |   |
|---|---|
|**Operator**|**Beschreibung**|
|`-eq`|Gleich (_equal_)|
|`-ne`|Ungleich (_not equal_)|
|`-lt`|Kleiner als (_less than_)|
|`-gt`|Größer als (_greater than_)|

```
if [[ -f "config.yaml" ]]; then
  echo "Konfigurationsdatei gefunden."
elif [[ $ZAHL -lt 10 ]]; then
  echo "Zahl ist kleiner als 10."
fi
```

### D. Logische Operatoren

|                |                                                                  |
| -------------- | ---------------------------------------------------------------- |
| **Operator**   | **Beschreibung**                                                 |
| `&&`           | UND (Logische Verknüpfung in `[[...]]`)                          |
| `!`            | NICHT                                                            |
| `cmd1 && cmd2` | Führe `cmd2` nur aus, wenn `cmd1` erfolgreich war (Exit Code 0). |

## 4. Funktionen und Argumente

### A. Funktionen definieren und verwenden

Funktionen helfen, den Code zu strukturieren und wiederzuverwenden.

```
# Definition
protokoll_ausgeben() {
  local LOG_LEVEL=$1
  local NACHRICHT=$2

  # Lokale Variablen verwenden 'local', um globale Kollisionen zu vermeiden.
  echo "[$(date +%H:%M:%S)] [$LOG_LEVEL] $NACHRICHT"
  return 0 # 0 für Erfolg, >0 für Fehler
}

# Aufruf
protokoll_ausgeben "INFO" "Skript gestartet."
```

### B. Skript-Argumente

Auf Argumente, die dem Skript übergeben werden, greifen Sie über spezielle Variablen zu.

|   |   |
|---|---|
|**Variable**|**Beschreibung**|
|**`$0`**|Name des Skripts selbst.|
|**`$1`, `$2`, ...**|Das 1., 2., ... Argument.|
|**`$@`**|Alle Argumente, einzeln gequotet (Beste Wahl für Iteration).|
|**`$*`**|Alle Argumente als ein einziger String.|
|**`$#`**|Anzahl der Argumente.|
|**`$?`**|Exit-Code des zuletzt ausgeführten Befehls.|

## 5. Schleifen und Iteration

### A. For-Schleifen

```
# 1. Standard-Iteration über Liste/Ergebnisse
for datei in *.txt; do
  echo "Bearbeite: $datei"
done

# 2. C-Style-Schleife (für numerische Zähler)
for ((i=1; i<=5; i++)); do
  echo "Zähler: $i"
done

# 3. Iteration über Zahlenbereiche
for i in {10..15}; do
  echo "Zahl: $i" # 10, 11, 12, 13, 14, 15
done
```

### B. While-Schleifen

Am häufigsten zum zeilenweisen Lesen von Dateien oder Eingaben verwendet.

```
# Zeilenweise aus einer Datei lesen
while IFS= read -r ZEILE; do
  echo "Gelesene Zeile: $ZEILE"
done < input.txt

# Endlosschleife (mit Break)
while true; do
  read -p "Weitermachen? (j/n): " ANTWORT
  if [[ $ANTWORT == "n" ]]; then
    break # Schleife verlassen
  fi
done
```

## 6. Arrays (Listen)

Arrays speichern eine Liste von Werten.

|   |   |
|---|---|
|**Operation**|**Syntax**|
|**Definieren**|`Obst=('Apfel' 'Banane' 'Orange')`|
|**Aufruf (1. Element)**|`echo "${Obst[0]}"`|
|**Alle Elemente**|`echo "${Obst[@]}"`|
|**Anzahl**|`echo "${#Obst[@]}"`|
|**Hinzufügen**|`Obst+=('Mango')`|
|**Löschen**|`unset Obst[1]`|

## 7. Ein- und Ausgabe (I/O)

### A. Numerische Berechnung

Verwenden Sie `(( ... ))` für einfache Integer-Arithmetik.

```
ZAHL_A=10
ZAHL_B=5
ERGEBNIS=$((ZAHL_A * ZAHL_B + 2))
echo "Ergebnis: $ERGEBNIS" # Ausgabe: 52
```

### B. Benutzereingabe

|   |   |   |
|---|---|---|
|**Befehl**|**Beispiel**|**Zweck**|
|**Einfache Eingabe**|`read -p "Ihren Namen: " NAME`|Fragt den Benutzer nach Eingabe, speichert in `$NAME`.|
|**Wörtliche Eingabe**|`read -r ZEILE`|Verhindert, dass Backslashes interpretiert werden (wichtig!).|

### C. Redirection & Spezial-I/O

|                          |                                                                          |
| ------------------------ | ------------------------------------------------------------------------ |
| **Syntax**               | **Beschreibung**                                                         |
| `cmd > file`             | **stdout** in Datei schreiben (überschreiben).                           |
| `cmd >> file`            | **stdout** an Datei anhängen.                                            |
| `cmd 2> error.log`       | **stderr** in Datei schreiben.                                           |
| `cmd &> log.txt`         | **stdout und stderr** in Datei schreiben.                                |
| `cmd <<< "Input"`        | **Herestring**: Übergibt einen String als Eingabe an `cmd`.              |
| `cat << ENDE` ... `ENDE` | **Heredoc**: Übergibt einen mehrzeiligen Textblock als Eingabe an `cat`. |