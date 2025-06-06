# 📘 Regex Cheat Sheet

## Inhaltsverzeichnis

1. [Einführung](#1-einführung)
2. [Zeichenklassen](#2-zeichenklassen)
3. [Quantifizierer](#3-quantifizierer)
4. [Metazeichen](#4-metazeichen)
5. [Metasequenzen](#5-metasequenzen)
6. [Anker](#6-anker)
7. [Ersetzungen](#7-ersetzungen)
8. [Gruppenkonstrukte](#8-gruppenkonstrukte)
9. [Assertions](#9-assertions)

## 1. Einführung

Diese Cheat-Sheet bietet eine schnelle Referenz für reguläre Ausdrücke (Regex), einschließlich Symbolen, Bereichen, Gruppierungen, Assertions und einigen Beispielmustern.

## 2. Zeichenklassen

| Muster        | Beschreibung                           |
| ------------- | -------------------------------------- |
| `[abc]`       | Ein Zeichen: a, b oder c               |
| `[^abc]`      | Ein Zeichen außer: a, b oder c         |
| `[a-z]`       | Ein Zeichen im Bereich: a bis z        |
| `[^a-z]`      | Ein Zeichen nicht im Bereich: a bis z  |
| `[0-9]`       | Eine Ziffer im Bereich: 0 bis 9        |
| `[a-zA-Z]`    | Ein Buchstabe: a bis z oder A bis Z    |
| `[a-zA-Z0-9]` | Ein Zeichen: a bis z, A bis Z oder 0-9 |

## 3. Quantifizierer

| Muster   | Beschreibung                      |
| -------- | --------------------------------- |
| `a?`     | Null oder einmal a                |
| `a*`     | Null oder mehrmals a              |
| `a+`     | Einmal oder mehrmals a            |
| `[0-9]+` | Eine oder mehr Ziffern            |
| `a{3}`   | Genau 3-mal a                     |
| `a{3,}`  | Mindestens 3-mal a                |
| `a{3,6}` | Zwischen 3- und 6-mal a           |
| `a*`     | Gieriger Quantifizierer           |
| `a*?`    | Fauler Quantifizierer             |
| `a*+`    | Besitzergreifender Quantifizierer |

## 4. Metazeichen

| Zeichen | Beschreibung                                     |                                              |
| ------- | ------------------------------------------------ | -------------------------------------------- |
| `^`     | Beginn einer Zeichenkette                        |                                              |
| `{`     | Beginn eines Quantifizierers                     |                                              |
| `+`     | Ein oder mehr des vorherigen Elements            |                                              |
| `<`     | Kein Standard-Regex-Metazeichen (häufig in HTML) |                                              |
| `[`     | Beginn einer Zeichenklasse                       |                                              |
| `*`     | Null oder mehr des vorherigen Elements           |                                              |
| `)`     | Ende einer Erfassungsgruppe                      |                                              |
| `>`     | Kein Standard-Regex-Metazeichen (häufig in HTML) |                                              |
| `.`     | Beliebiges Zeichen außer Zeilenumbruch           |                                              |
| `(`     | Beginn einer Erfassungsgruppe                    |                                              |
| \`      | \`                                               | Logisches ODER innerhalb eines Regex-Musters |
| `$`     | Ende einer Zeichenkette                          |                                              |
| `\`     | Escape-Zeichen für Metazeichen                   |                                              |
| `?`     | Null oder einmal des vorherigen Elements         |                                              |

_Hinweis: Diese Sonderzeichen mit `\` escapen._

## 5. Metasequenzen

| Muster     | Beschreibung                                             |
| ---------- | -------------------------------------------------------- |
| `.`        | Beliebiges einzelnes Zeichen                             |
| `\s`       | Beliebiges Leerzeichen                                   |
| `\S`       | Beliebiges Nicht-Leerzeichen                             |
| `\d`       | Beliebige Ziffer, entspricht `[0-9]`                     |
| `\D`       | Beliebiges Nicht-Ziffer, entspricht `[^0-9]`             |
| `\w`       | Beliebiges Wortzeichen                                   |
| `\W`       | Beliebiges Nicht-Wortzeichen                             |
| `\X`       | Beliebige Unicode-Sequenz, einschließlich Zeilenumbrüche |
| `\C`       | Ein Datenbyte                                            |
| `\R`       | Unicode-Zeilenumbrüche                                   |
| `\v`       | Vertikales Leerzeichen                                   |
| `\V`       | Negation von `\v`                                        |
| `\h`       | Horizontales Leerzeichen                                 |
| `\H`       | Negation von `\h`                                        |
| `\K`       | Rücksetzen des Matches                                   |
| `\n`       | Zeilenumbruch                                            |
| `\p{...}`  | Unicode-Eigenschaft oder Skriptkategorie                 |
| `\P{...}`  | Negation von `\p{...}`                                   |
| `\Q...\E`  | Literale Zeichenfolge                                    |
| `\k<name>` | Benannte Rückreferenz                                    |
| `\g<n>`    | Rückverweis auf die n-te Gruppe                          |
| `\xYY`     | Hexadezimalzeichen YY                                    |
| `\x{YYYY}` | Hexadezimalzeichen YYYY                                  |
| `\ddd`     | Oktalzeichen ddd                                         |
| `\cY`      | Steuerzeichen Y                                          |
| `[\b]`     | Rückschrittzeichen                                       |

## 6. Anker

| Muster | Beschreibung                    |
| ------ | ------------------------------- |
| `\G`   | Beginn des Matches              |
| `^`    | Beginn der Zeichenkette         |
| `$`    | Ende der Zeichenkette           |
| `\A`   | Beginn der Zeichenkette         |
| `\Z`   | Ende der Zeichenkette           |
| `\z`   | Absolutes Ende der Zeichenkette |
| `\b`   | Wortgrenze                      |
| `\B`   | Keine Wortgrenze                |

## 7. Ersetzungen

| Muster     | Beschreibung                       |
| ---------- | ---------------------------------- |
| `\0`       | Gesamter Match-Inhalt              |
| `\1`, `$1` | Inhalt der ersten Erfassungsgruppe |
| `${foo}`   | Inhalt der benannten Gruppe `foo`  |
| `\x20`     | Hexadezimalwert                    |
| `\t`       | Tabulator                          |
| `\r`       | Wagenrücklauf                      |
| `\n`       | Zeilenumbruch                      |
| `\f`       | Seitenvorschub                     |
| `\U`       | Großschreibung                     |
| `\L`       | Kleinschreibung                    |
| `\E`       | Beendet die Groß-/Kleinschreibung  |

## 8. Gruppenkonstrukte

| Muster         | Beschreibung                            |                          |
| -------------- | --------------------------------------- | ------------------------ |
| `(…)`          | Erfasst alles Eingeschlossene           |                          |
| \`(a           | b)\`                                    | Entweder a oder b        |
| `(?:…)`        | Nicht-erfassende Gruppe                 |                          |
| `(?>…)`        | Atomare Gruppe (nicht-erfassend)        |                          |
| \`(?           | …)\`                                    | Dupliziert Gruppenummern |
| `(?#…)`        | Kommentar                               |                          |
| `(?<name>…)`   | Benannte Erfassungsgruppe               |                          |
| `(?P<name>…)`  | Benannte Erfassungsgruppe (Python-Stil) |                          |
| `(?imsxXU)`    | Inline-Modifikatoren                    |                          |
| `(?(DEFINE)…)` | Vordefinierte Muster                    |                          |

## 9. Assertions

| Muster      | Beschreibung |                                         |
| ----------- | ------------ | --------------------------------------- |
| \`(?(1)yes  | no)\`        | Bedingte Anweisung                      |
| \`(?(R)yes  | no)\`        | Rekursive bedingte Anweisung            |
| \`(?(R#)yes | no)\`        | Rekursive bedingte Anweisung mit Nummer |
