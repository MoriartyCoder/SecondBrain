# NLP / SLU Mock Exam Bundle

## Modul: Spoken- and Natural Language Understanding
**Dauer:** 90 Minuten pro Examen
**Erlaubte Hilfsmittel:** Nicht-programmierbarer Taschenrechner

---

# Testexamen 1 (Fokus: Grundlagen & Vorverarbeitung)

### Aufgabe 1: Grundlagen (10 Punkte)
(a) Erklären Sie das **Zipf’sche Gesetz**. Skizzieren Sie den Zusammenhang zwischen Rang und Häufigkeit und geben Sie die mathematische Formel an.
(b) Was ist der Unterschied zwischen einem **Wort-Type** und einem **Wort-Token**? Erläutern Sie dies am Satz: *"To be or not to be."*
(c) Erklären Sie das Prinzip des **erweiterten Turing-Tests** mit drei Teilnehmern.

### Aufgabe 2: Text-Vorverarbeitung & Segmentierung (15 Punkte)
(a) Markieren Sie im folgenden Text alle Elemente, die für ein ASR-System (Spracherkennung) **normalisiert** werden müssen, und nennen Sie das Zielsatzformat:
> "Die NASA startete am 16. Juli 1969 die Mission Apollo 11. Die Kosten beliefen sich auf ca. 25,4 Mrd. USD."
(b) Erklären Sie den **MaxMatch-Algorithmus** zur Wortsegmentierung (z.B. im Chinesischen). Wo liegen die Grenzen dieses Verfahrens?
(c) Entwerfen Sie einen **Entscheidungsbaum**, der entscheidet, ob ein Punkt (.) ein Satzende markiert oder zu einer Abkürzung gehört.

### Aufgabe 3: N-Gramme (15 Punkte)
Gegeben ist der Trainings-Text: *"Ich esse Pizza. Ich esse Äpfel. Pizza ist gut."*
(a) Wie groß ist das Vokabular (Anzahl der Types)?
(b) Berechnen Sie die Wahrscheinlichkeit für den Test-Satz **"Ich esse Pizza"** unter Verwendung eines **Bigram-Modells** (Maximum Likelihood Estimation).
(c) Was passiert bei der Berechnung, wenn im Test-Satz das Wort "Nudeln" vorkommt? Nennen Sie zwei Verfahren, um dieses Problem zu lösen.

### Aufgabe 4: Chomsky-Hierarchie (10 Punkte)
(a) Nennen Sie die 4 Sprachtypen der Chomsky-Hierarchie und ordnen Sie ihnen die jeweiligen Automatentypen zu.
(b) Zeichnen Sie einen endlichen Automaten für die reguläre Sprache $L = \{1^n 2^m \mid n \ge 1, m \ge 2\}$.

### Aufgabe 5: Information Retrieval (15 Punkte)
(a) Erklären Sie den Aufbau eines **Bag-of-Words (BOW)** Vektors. Wie viele Vektoren müssen für ein Dokument erstellt werden?
(b) Was beschreibt das Maß **TF-IDF**? Erläutern Sie kurz die beiden Komponenten.
(c) Gegeben sind zwei Vektoren $A = [1, 0, 1]$ und $B = [0, 1, 1]$. Berechnen Sie die **Cosinus-Ähnlichkeit**.

### Aufgabe 6: RNNs & Deep Learning (15 Punkte)
(a) Skizzieren Sie ein **Recurrent Neural Network (RNN)** für die Aufgabe der **Intent Detection** (z.B. Erkennung von "Licht an" vs. "Licht aus").
(b) Was ist der Unterschied zwischen einem "folded" (gefalteten) und einem "unfolded" (entfalteten) RNN?
(c) Warum wird in der Praxis oft der **hierarchische Softmax** oder **Negative Sampling** beim Training von Embeddings verwendet?

### Aufgabe 7: ASR & TTS (10 Punkte)
(a) Nennen Sie die **9 Schritte des Feature-Front-Ends** zur Erstellung von MFCCs in der korrekten Reihenfolge.
(b) Nennen Sie die **3 Grundarten der Sprachsynthese (TTS)** und beschreiben Sie diese jeweils in einem Satz.

---

# Testexamen 2 (Fokus: Modellierung & Architektur)

### Aufgabe 1: Linguistik & Grundlagen (10 Punkte)
(a) Erklären Sie den Unterschied zwischen **Stemming** und **Lemmatisierung**. Geben Sie für beide ein Beispiel anhand des Wortes "gingen".
(b) Was macht ein **Part-of-Speech (POS) Tagger**? Geben Sie ein Beispiel für einen getaggten Satz.
(c) Zwei Sprachmodelle werden evaluiert. Modell A hat eine Perplexität von 85, Modell B eine von 140. Welches Modell bevorzugen Sie und warum?

### Aufgabe 2: N-Gramme & Wahrscheinlichkeiten (15 Punkte)
(a) Leiten Sie die allgemeine Formel eines Sprachmodells $P(w_1, w_2, ..., w_m)$ aus der Kettenregel der Wahrscheinlichkeit her.
(b) Erklären Sie die **Markov-Annahme** im Kontext von N-Grammen.
(c) Berechnen Sie die Perplexität für einen Test-Text, wenn das Sprachmodell für diesen Text eine Gesamtwahrscheinlichkeit von $0,008$ bei einer Textlänge von 3 Wörtern berechnet hat.

### Aufgabe 3: Chomsky-Hierarchie (10 Punkte)
(a) Welche Chomsky-Sprache kann Python-Quellcode am ehesten beschreiben? Begründen Sie Ihre Antwort.
(b) Geben Sie ein Beispiel für eine **kontextfreie Sprache**, die nicht regulär ist, und begründen Sie die Einordnung.

### Aufgabe 4: Word Embeddings (15 Punkte)
(a) Erklären Sie den Unterschied zwischen den Architekturen **cBOW** und **Skip-Gram** bei Word2Vec.
(b) Was ist eine **One-Hot-Kodierung** und welches Hauptproblem ergibt sich daraus bei großen Vokabularien?
(c) Wie kann man die Ähnlichkeit von zwei Word-Embeddings mathematisch bestimmen?

### Aufgabe 5: RNNs & Attention (20 Punkte)
(a) Geben Sie die mathematische Formel für den **Dot-Product Attention** Mechanismus an. Erklären Sie die Rollen der Vektoren **Q** (Query), **K** (Key) und **V** (Value).
(b) Was ist ein **RNN without delay**? Nennen Sie eine typische Anwendung für diesen Netzwerktyp.
(c) Zeichnen Sie die Architektur eines **Encoder-Decoder-Modells** für die maschinelle Übersetzung (NMT).

### Aufgabe 6: ASR & TTS (20 Punkte)
(a) Erklären Sie die **Bayes’sche Grundformel der Spracherkennung**: $\text{argmax}_w P(w|X)$. In welche zwei Teilprobleme wird diese zerlegt?
(b) Nennen Sie 3 Qualitätskriterien für ein TTS-System (Stichwort: **C.I.N.P.**).
(c) Warum ist die Text-Normalisierung für TTS kritischer als für einfaches Information Retrieval? Geben Sie Beispiele.

---

# Testexamen 3 (Fokus: Anwendung & IR)

### Aufgabe 1: Diverses (10 Punkte)
(a) Erklären Sie das Konzept der **Konstituentenanalyse** (Constituency Parsing).
(b) Was versteht man unter **Stopwords**? Wie kann man sie statistisch (ohne feste Liste) identifizieren?
(c) Erläutern Sie kurz die Funktionsweise einer **Finite State Machine (FSM)** zur Konvertierung von Groß- in Kleinschreibung.

### Aufgabe 2: IR & Vektoren (15 Punkte)
(a) Was bedeutet eine Cosinus-Ähnlichkeit von **-1**, **0** und **1** im Kontext von Dokumentenähnlichkeit?
(b) Wie verändert sich ein BOW-Vektor, wenn man von 1-Grammen zu 2-Grammen übergeht? Welche Auswirkungen hat dies auf die Vektordimension?
(c) Erklären Sie kurz das Prinzip des **PageRank** Algorithmus.

### Aufgabe 3: N-Gramme (15 Punkte)
(a) Gegeben: Train = *"Apfel rot. Apfel grün. Birne grün."*
Berechnen Sie die Bigram-Wahrscheinlichkeiten $P(\text{grün} \mid \text{Apfel})$ und $P(\text{rot} \mid \text{Apfel})$.
(b) Was ist **Back-off** im Zusammenhang mit Sprachmodellen?
(c) Wie unterscheidet sich die Berechnung eines Sprachmodells für ASR von der für eine Suchmaschine?

### Aufgabe 4: Chomsky & Automaten (10 Punkte)
(a) Erstellen Sie die Produktionsregeln für die Sprache $L = \{A^n B \mid n \ge 1\}$.
(b) Kann ein Sprachmodell als **Transducer (FST)** dargestellt werden? Begründen Sie kurz.

### Aufgabe 5: Attention & Transformers (20 Punkte)
(a) Erklären Sie den Unterschied zwischen **Self-Attention** und **Multi-Head Attention**.
(b) Was sind **Positional Encodings** und warum werden sie in Transformer-Modellen (im Gegensatz zu RNNs) benötigt?
(c) Beschreiben Sie kurz, was das **BERT** Modell auszeichnet (Stichwort: Bidirektionalität).

### Aufgabe 6: ASR & TTS (20 Punkte)
(a) Skizzieren Sie den Ablauf der **Feature-Extraktion** im ASR Front-End. Warum wird eine **Mel-Skala** verwendet?
(b) Was ist ein **HMM (Hidden Markov Model)** im Kontext der akustischen Modellierung?
(c) Nennen und erklären Sie 3 Schritte der **Textanalyse** innerhalb eines TTS-Systems.
(d) Was ist der **BLEU-Score** und wie wird er (grob) berechnet?