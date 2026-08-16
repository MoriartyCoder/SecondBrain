# Synthese von Super-Resolution, Denoising und Temporal Enhancement in der 4D-fMRI: Ein Unified Spatio-Temporal Contrastive Network Ansatz

## Physikalische Grundlagen der fMRI und die Problemstellung der Datenqualität

Die funktionelle Magnetresonanztomographie (fMRI) hat sich als unverzichtbares Instrument in der kognitiven Neurowissenschaft und der klinischen Diagnostik etabliert. Ihr primärer Nutzen liegt in der Fähigkeit, neuronale Aktivitäten indirekt über hämodynamische Veränderungen abzubilden, ein Phänomen, das als Blood Oxygen Level Dependent (BOLD) Effekt bekannt ist. Dieser Effekt resultiert aus den unterschiedlichen magnetischen Suszeptibilitäten von oxygeniertem und desoxygeniertem Hämoglobin, wobei aktive Hirnareale eine lokale Zunahme des Sauerstoffgehalts im Blut aufweisen, was die transversale Relaxationszeit $T_2^*$ verlängert und somit zu einer Signalsteigerung in $T_2^*$-gewichteten Sequenzen führt.

Die Qualität dieser Aufnahmen ist jedoch durch fundamentale physikalische Gesetze des MRT-Scanners limitiert. Ein zentrales Konzept ist dabei der k-Raum, der die Rohdaten im Fourier-Raum repräsentiert. Der Scanner erfasst die Signale in Form von harmonischen Komponenten, wobei das Zentrum des k-Raums die Informationen über den Gesamtkontrast und die groben Strukturen enthält, während die Peripherie die Hochfrequenzanteile für Kanten und feine anatomische Details beherbergt. Jede Erhöhung der räumlichen Auflösung erfordert die Abtastung eines größeren Bereichs des k-Raums, was zwangsläufig die Akquisitionszeit verlängert oder das Signal-Rausch-Verhältnis (SNR) verringert.

In der Praxis führt dies zu einem "Triple-Constraint-Problem":

1. **Räumliche Auflösung:** Kleinere Voxel (z. B. $1 \, \text{mm}^3$) bieten mehr anatomische Details, führen aber zu einem drastischen Abfall des SNR, da weniger Protonen pro Volumeneinheit zum Signal beitragen.
    
2. **Zeitliche Auflösung:** Eine schnellere Abtastung (kurze Repetitionszeit, TR) ist notwendig, um die Dynamik der hämodynamischen Antwortfunktion (HRF) präzise zu erfassen, reduziert jedoch das zeitliche Signal-Rausch-Verhältnis (tSNR), da weniger Zeit für die Signalakkumulation zur Verfügung steht.
    
3. **Rauschen:** fMRI-Daten sind inhärent verrauscht. Die Rauschquellen reichen von thermischem Rauschen der Elektronik über physiologische Schwankungen (Herzschlag, Atmung) bis hin zu Bewegungsartefakten der Probanden.
    

Die Aufgabe besteht darin, eine Netzwerk-Architektur zu entwickeln, die diese drei Dimensionen – Raum, Zeit und Rauschunterdrückung – simultan optimiert. Ein rein sequentieller Ansatz, bei dem zuerst entrauscht und dann die Auflösung erhöht wird, ist oft suboptimal, da Fehlinterpretationen im Entrauschungsschritt (z. B. das Glätten feiner Details als Rauschen) die anschließende Super-Resolution negativ beeinflussen können.

|**Parameter**|**Einfluss auf die Datenqualität**|**Trade-off**|
|---|---|---|
|Voxelgröße|Bestimmt die räumliche Trennschärfe.|Kleinere Voxel $\rightarrow$ niedrigeres SNR.|
|Repetitionszeit (TR)|Bestimmt die zeitliche Abtastrate.|Kürzere TR $\rightarrow$ niedrigeres tSNR.|
|k-Raum-Abtastung|Bestimmt den Informationsgehalt im Fourier-Raum.|Mehr Daten $\rightarrow$ längere Scanzeit.|
|BOLD-Magnitude|Signaländerung von ca. 1–5 % bei 3T.|Oft kleiner als die Rauschamplitude.|

## Analyse der Rauschquellen und ihre Trennung durch kontrastives Lernen

Um ein effektives Denoising durchzuführen, muss die Architektur die Struktur des fMRI-Rauschens verstehen. Rauschen ist in der fMRI nicht einfach additives weißes Gauß-Rauschen (AWGN). Es ist räumlich und zeitlich korreliert. Während thermisches Rauschen eher gleichmäßig verteilt ist, konzentriert sich physiologisches Rauschen oft in der Nähe von Blutgefäßen und den Liquorräumen (Cerebrospinalflüssigkeit, CSF).

Ein innovativer Ansatz zur Bewältigung dieses Problems ist das Konzept des kontrastiven Lernens, wie es in der DeepCor-Architektur implementiert ist. DeepCor nutzt die Tatsache aus, dass fMRI-Daten in Regionen von Interesse (Regions of Interest, ROIs – primär Graue Substanz) und Regionen ohne Interesse (Regions of No Interest, RONIs – Weiße Substanz und CSF) unterteilt werden können. RONIs enthalten per Definition keine aufgabenbezogenen BOLD-Signale, spiegeln aber die systemischen Rauschkomponenten (Scanner-Drift, Bewegung, Physiologie) wider, die auch in den ROIs vorhanden sind.

DeepCor implementiert dies durch einen Contrastive Variational Autoencoder (CVAE). Dieser verwendet zwei getrennte Encoder-Pfade:

- Ein **Signal-Encoder** extrahiert Merkmale, die spezifisch für das neuronale Signal in der Grauen Substanz sind.
    
- Ein **Rausch-Encoder** lernt die Repräsentation der unerwünschten Fluktuationen, die sowohl in ROIs als auch in RONIs auftreten.
    

Der entscheidende Vorteil dieses Ansatzes ist die Entflechtung (Disentanglement) im latenten Raum. Während des Trainings werden RONI-Daten nur durch den Rausch-Encoder geleitet, während ROI-Daten beide Pfade durchlaufen. Zur Inferenz wird der latente Raum des Rausch-Encoders auf Null gesetzt, sodass der Decoder nur das gereinigte Signal rekonstruiert. Für eine integrierte Architektur bedeutet dies, dass der Encoder-Teil des Netzwerks zwingend diese kontrastive Logik beibehalten sollte, um eine physikalisch und physiologisch begründete Trennung von Signal und Rauschen zu gewährleisten.

## Räumliche Super-Resolution in der MRT: Von SRCNN zu modernen Ansätzen

Der ursprüngliche Vorschlag des Anwenders sah SRCNN als Decoder-Komponente vor. SRCNN (Super-Resolution Convolutional Neural Network) war bahnbrechend, da es erstmals zeigte, dass eine einfache Sequenz von drei Faltungsschichten (Patch-Extraktion, nicht-lineares Mapping, Rekonstruktion) die klassische Bikubische Interpolation übertreffen kann. In der medizinischen Bildgebung, insbesondere bei der fMRI, stößt ein Standard-SRCNN jedoch auf drei wesentliche Probleme:

Erstens ist fMRI-Datenmaterial inhärent dreidimensional (Volumina) bzw. vierdimensional (Zeitreihen). Ein 2D-SRCNN müsste Slices einzeln verarbeiten, wodurch wertvolle Informationen über die Kontinuität anatomischer Strukturen entlang der z-Achse verloren gehen. Modernere Architekturen wie 3D-UNet oder mDCSRN (Multi-level Densely Connected Super-Resolution Network) nutzen 3D-Faltungen, um die volumetrische Natur der Daten direkt zu modellieren.

Zweitens neigen Netzwerke, die nur auf dem mittleren quadratischen Fehler (MSE) optimiert werden, dazu, Bilder zu glätten. Dies führt zu einem Verlust an Texturschärfe, was in der Neurobildgebung fatal sein kann, wenn feine Aktivierungsmuster (z. B. in den kortikalen Schichten) detektiert werden sollen. Hier haben sich GAN-basierte Ansätze (Generative Adversarial Networks) oder Modelle mit Perceptual Loss als überlegen erwiesen, da sie realistischere Hochfrequenzdetails generieren.

Drittens erfordert fMRI eine Erhöhung der Auflösung ohne Artefakte zu induzieren, die als falsche neuronale Aktivität missinterpretiert werden könnten. Der Einsatz von Pixel-Shuffle-Layern (Sub-Pixel Convolution) anstelle von Transponierten Konvolutionen (Deconvolution) wird empfohlen, um Checkerboard-Artefakte zu minimieren, die oft bei Upsampling-Prozessen auftreten.

| **Architektur** | **Dimension** | **Kernkonzept**              | **Eignung für fMRI SR**              |
| --------------- | ------------- | ---------------------------- | ------------------------------------ |
| SRCNN           | 2D            | Drei einfache Conv-Schichten | Gering (verliert 3D-Kontext).        |
| EDSR            | 2D            | Tiefe Residual-Netze ohne BN | Mittel (effizient, aber 2D).         |
| 3D-UNet         | 3D            | Encoder-Decoder mit Skips    | Hoch (bewahrt anatomische Struktur). |
| mDCSRN          | 3D            | Dichte Verbindungen, 3D      | Sehr hoch (optimiert für MR-Fluids). |
| nESRGAN         | 2D/3D         | Noise-based GAN              | Hoch (für visuelle Authentizität).   |

## Die zeitliche Dimension: Temporal Enhancement und Scaffolding

Das dritte Ziel, das "Temporal Enhancement", ist in der fMRI-Forschung vielschichtig. Es kann die Erhöhung des tSNR durch bessere Zeitreihenmodellierung bedeuten oder die tatsächliche Erhöhung der Bildwiederholfrequenz (zeitliche Super-Resolution). Die Modellierung der zeitlichen Dynamik ist entscheidend, da das BOLD-Signal eine langsame, aber charakteristische Form hat (die HRF erreicht ihren Peak erst nach ca. 5–6 Sekunden).

Ein vielversprechender theoretischer Rahmen ist die "Cross-Modal Temporal Scaffolding Theory". Sie postuliert, dass strukturierte zeitliche Kontexte – wie sie beispielsweise durch rhythmische Reize oder bekannte Sequenzen entstehen – die neuronale Verarbeitung stabilisieren und die Vorhersagbarkeit erhöhen. In einer KI-Architektur kann dieses "Scaffolding" durch die Integration von Transformer-Modulen realisiert werden. Transformers nutzen Self-Attention-Mechanismen, um weitreichende Abhängigkeiten in einer Sequenz zu erfassen. Im Gegensatz zu RNNs oder LSTMs können sie die gesamte Zeitreihe simultan betrachten und so globale Trends von kurzfristigem Rauschen trennen.

Für das Temporal Enhancement bedeutet dies:

1. **Denoising über die Zeit:** Identifikation von periodischen Rauschsignalen (Atmung, Herzschlag) durch Frequenzanalyse im latenten Raum.
    
2. **Hämodynamische Rekonstruktion:** Das Netzwerk lernt, wie sich das Signal zwischen zwei Zeitpunkten physikalisch korrekt verändern muss. Dies erlaubt eine Interpolation zu Zeitpunkten, an denen kein Scan stattgefunden hat (zeitliche SR), ähnlich wie bei 4DFlowNet-Anwendungen.
    
3. **tSNR-Maximierung:** Durch die Modellierung der zeitlichen Kohärenz wird die Varianz des Signals in Ruhephasen minimiert, was die Detektion von echten Signaländerungen während Aufgaben erleichtert.
    

## Proponierte Architektur: Das Spatio-Temporal Contrastive U-Net (ST-CUNet)

Auf Basis der Analyse wird hier eine Architektur vorgeschlagen, die die kontrastive Logik von DeepCor, die räumliche Rekonstruktionskraft eines 3D-U-Nets und die zeitliche Intelligenz von Transformers kombiniert. Diese Architektur bezeichnen wir als **ST-CUNet**.

### Der Encoder: 4D Spatio-Temporal Feature Extraction

Anstatt Voxel einzeln zu betrachten, verarbeitet der Encoder 4D-Patches (z. B. $32 \times 32 \times 32$ Voxel über 16 Zeitpunkte).

- **Duales Encoding:** Analog zu DeepCor v2 werden ROI-Patches (Graue Substanz) durch zwei parallele 3D-Faltungspfade geleitet: einen Signal-Pfad und einen Rausch-Pfad.
    
- **Coordinate Attention:** Die Voxel-Koordinaten werden als zusätzliche Kanäle integriert. Dies gibt dem Netzwerk eine implizite Information über die anatomische Position (z. B. Nähe zu den Ventrikeln oder dem Kortex), was die Rauschmodellierung verfeinert.
    
- **Residual Blocks:** Anstelle einfacher Conv-Layer werden Residual-Einheiten verwendet, um das Verschwinden von Gradienten in tiefen Strukturen zu verhindern und die Konvergenz zu beschleunigen.
    

### Das Bottleneck: Temporal Transformer & Latent Disentanglement

Im Zentrum des Netzwerks erfolgt die zeitliche Verarbeitung.

- **Temporal Attention:** Die extrahierten räumlichen Merkmale werden als Tokens in einen Transformer-Block gespeist. Dieser wendet Attention über die zeitliche Dimension an, um die BOLD-Dynamik zu isolieren.
    
- **Contrastive Loss im Bottleneck:** Hier wird eine Verlustfunktion angewandt, die sicherstellt, dass die Repräsentationen von ROI und RONI im Rausch-Pfad ähnlich sind, während der Signal-Pfad nur Informationen enthält, die in ROIs (Graue Substanz) einzigartig sind.
    

### Der Decoder: Multi-Task Upsampling und Rekonstruktion

Der Decoder nutzt Skip-Connections, um hochaufgelöste anatomische Details aus dem Encoder-Pfad direkt in die Rekonstruktion einzubeziehen.

- **Pixel-Shuffle SR:** Am Ende des Decoders befinden sich Pixel-Shuffle-Schichten, die die räumliche Auflösung um den Faktor 2 oder 4 erhöhen.
    
- **Joint Output:** Das Netzwerk liefert drei Ausgaben:
    
    1. Ein entrauschtes 4D-Volumen in Originalauflösung (Denoising-Fokus).
        
    2. Ein hochaufgelöstes 4D-Volumen (Super-Resolution-Fokus).
        
    3. Interpolierte Volumen zwischen den TR-Zeitpunkten (Temporal Enhancement).
        

### Mathematische Formulierung der Loss-Funktion

Das Training erfolgt über eine zusammengesetzte Loss-Funktion $L$, die alle drei Aufgaben gewichtet:

$$L = \lambda_1 L_{rec} + \lambda_2 L_{KL} + \lambda_3 L_{adv} + \lambda_4 L_{phys}$$

- **$L_{rec}$ (Reconstruction):** MSE oder Structural Similarity Index (SSIM) zwischen Rekonstruktion und Ground-Truth.
    
- **$L_{KL}$ (Kullback-Leibler):** Reguliert den latenten Raum des CVAE zur Erzeugung einer kontinuierlichen Signalverteilung.
    
- **$L_{adv}$ (Adversarial):** Ein Diskriminator bewertet die Realitätstreue des hochaufgelösten Volumens, um Unschärfe zu verhindern.
    
- **$L_{phys}$ (Physiological/Physics):** Eine Regularisierung, die sicherstellt, dass die zeitliche Ableitung des Signals (BOLD-Rate) innerhalb physiologisch plausibler Grenzen liegt oder physikalische Gesetze (wie die Massenerhaltung im Blutfluss bei 4D Flow) einhält.
    

## Mathematische Modellierung des Degradationsprozesses

Die Super-Resolution und das Denoising können als inverses Problem betrachtet werden. Wir definieren das ideale, hochaufgelöste und rauschfreie fMRI-Volumen als $x \in \mathbb{R}^{H \times W \times D \times T}$. Die beobachteten, degradiererten Daten $y$ entstehen durch:

$$y = \mathcal{D}(\mathcal{B}(\mathcal{T}(x))) + \eta$$

Wobei:

- $\mathcal{T}$ eine geometrische Transformation (z. B. Kopfbewegung) darstellt.
    
- $\mathcal{B}$ ein Unschärfe-Operator (Blurring) ist, der die Punktspreizfunktion (PSF) des MRT-Systems modelliert.
    
- $\mathcal{D}$ ein Downsampling-Operator ist, der die Reduktion der Auflösung im k-Raum beschreibt.
    
- $\eta$ das komplexe fMRI-Rauschen (thermisch + physiologisch) ist.
    

Das ST-CUNet zielt darauf ab, die posteriore Verteilung $p(x|y)$ zu maximieren. Im Gegensatz zu klassischen Methoden, die handvergebene Priors (wie Total Variation) nutzen, lernt das Netzwerk einen tiefen generativen Prior durch den CVAE-Teil der Architektur. Ein besonders kritischer Aspekt bei der Rekonstruktion ist die "Data Consistency". In modernen Ansätzen wie "Deep Unfolding" wird der Rekonstruktionsschritt iterativ durchgeführt, wobei nach jedem Denoising-Schritt eine Rückprojektion in den k-Raum erfolgt, um sicherzustellen, dass die generierten Daten mit den tatsächlich gemessenen Rohdaten konsistent bleiben.

## Datenstrategien: Umgang mit fehlenden Ground-Truth-Daten

Eine der größten Herausforderungen beim Training von Super-Resolution-Modellen für die fMRI ist das Fehlen von echten "Ground Truth"-Daten. Man kann nicht gleichzeitig ein hochaufgelöstes und ein schnelles, rauscharmes Volumen scannen. Daher müssen drei alternative Datenstrategien verfolgt werden:

### 1. Synthetische Daten aus hochaufgelösten anatomischen Scans

Man kann hochaufgelöste anatomische $T_1$-gewichtete Bilder (z. B. aus dem "Visible Human Project" oder dem HCP) als strukturelles Template nutzen. Durch die Simulation von BOLD-Effekten (Faltung von künstlichen Aktivierungsparadigmen mit einer HRF) und das Hinzufügen von realistischem Rauschen aus RONI-Messungen können Paare von $\{x, y\}$ generiert werden.

### 2. Cross-Modal Transfer (3T zu 7T)

Ein leistungsfähiger Weg ist die Nutzung von fMRI-Daten, die bei unterschiedlichen Feldstärken akquiriert wurden. 7-Tesla-Scanner bieten eine deutlich höhere native Auflösung und ein besseres SNR als klinische 3-Tesla-Scanner. Wenn Probanden in beiden Scannern mit dem gleichen Paradigma gemessen werden, können die 7T-Daten als Ground-Truth für die Verbesserung der 3T-Daten dienen.

### 3. Selbstüberwachtes Lernen (Self-Supervised Learning)

Architekturen wie "SDnDTI" oder spezielle CVAEs können so trainiert werden, dass sie Rauschen ohne saubere Referenzbilder unterdrücken, indem sie die statistische Unabhängigkeit von Rauschen in zeitlich benachbarten Volumen ausnutzen. Dies ist besonders wertvoll für klinische Datensätze, bei denen keine High-End-Scans verfügbar sind.

## Implementierung und Ressourcenoptimierung

Die Verarbeitung von 4D-fMRI-Daten ist extrem rechenintensiv. Die bereitgestellte Hardware mit einer 47 GB GPU bietet jedoch hervorragende Voraussetzungen. Dennoch muss die Implementierung speichereffizient gestaltet werden:

1. **Patch-based Processing:** Anstatt das gesamte Gehirn in das Netzwerk zu laden, werden zufällige spatiotemporale Sub-Blöcke (Cubes) trainiert. Dies erlaubt größere Batch-Größen und eine bessere Generalisierung.
    
2. **Mixed Precision Training:** Die Verwendung von FP16-Operationen halbiert den Speicherbedarf der Aktivierungen und beschleunigt das Training auf modernen GPUs signifikant.
    
3. **Ensemble Averaging:** Wie bei DeepCor v2 vorgeschlagen, erhöht das Mitteln der Vorhersagen von 20 unabhängig trainierten Modellen die Robustheit und reduziert die Varianz der Rekonstruktion.
    
4. **Transfer Learning:** Das Netzwerk sollte zuerst auf großen, öffentlich zugänglichen Datensätzen wie dem Human Connectome Project (HCP) vortrainiert werden, bevor es auf die spezifischen Projektdaten feinjustiert wird.
    

|**Ressource**|**Kapazität**|**Optimierungsstrategie**|
|---|---|---|
|GPU RAM|47 GB|Patching, Mixed Precision, Gradient Checkpointing.|
|System RAM|24 GB|Effizientes Data Loading (NumPy/Nifti-Streaming).|
|CPU|4 Kerne|Parallelisierung der Vorverarbeitung (Preprocessing).|
|Festplatte|150 GB|Komprimierte Speicherung von Zwischenergebnissen (Nii.gz).|

## Validierung: Mehr als nur Pixel-Metriken

In der medizinischen Bildgebung sind Standard-Metriken wie PSNR (Peak Signal-to-Noise Ratio) oder MSE oft unzureichend, da sie die klinische oder wissenschaftliche Relevanz nicht widerspiegeln. Eine Steigerung des PSNR durch Glättung kann beispielsweise die Detektierbarkeit kleiner Hirnläsionen oder funktionaler Hubs verschlechtern.

Die Validierung des ST-CUNet muss daher auf drei Ebenen erfolgen:

### Ebene 1: Bildqualität (Strukturell)

Messung von PSNR, SSIM und dem Dice-Koeffizienten für Gewebesegmentierungen. Ein erfolgreiches Modell sollte die Dice-Überlappung von Grauer und Weißer Substanz im Vergleich zu hochaufgelösten Referenzen erhöhen.

### Ebene 2: Signalstabilität (Zeitlich)

Berechnung des tSNR. Ein Anstieg des tSNR in der Grauen Substanz ohne Verlust an räumlicher Schärfe ist der primäre Indikator für ein gelungenes Temporal Enhancement. Zudem sollte die zeitliche Autokorrelation der Zeitreihen mit den theoretischen HRF-Modellen abgeglichen werden.

### Ebene 3: Funktionale Validität (Neurowissenschaftlich)

Dies ist der entscheidende Test. fMRI-Daten dienen dazu, neuronale Aktivierungskarten zu erstellen.

- **Kategorie-Selektivität:** Erhöht sich die statistische Signifikanz ($t$-Werte) in spezialisierten Arealen wie dem Fusiform Face Area (FFA) bei Gesichtsreizen oder dem Parahippocampal Place Area (PPA) bei Hausreizen?. DeepCor konnte hier Steigerungen von über 200 % erzielen.
    
- **Inter-Subject Similarity:** Führt das Denoising zu einer höheren Übereinstimmung der funktionellen Konnektivitätsmatrizen zwischen verschiedenen Probanden? Rauschen ist individuell, Signal ist (in gewissen Grenzen) überindividuell konsistent.
    
- **Falsch-Positiv-Rate:** Werden künstliche Aktivierungen in der Weißen Substanz oder den Ventrikeln (RONIs) nachweislich reduziert?.
    

## Expertenbeurteilung des Integrationsansatzes

Der Vorschlag des Anwenders, DeepCor (CVAE) und SRCNN (Conv-Mapping) in einem U-Net-Rückgrat zu vereinen, ist architektonisch fundiert, wenn man die zeitliche Dimension durch Transformers oder 3D-Faltungen ergänzt.

Ein kritischer Punkt ist die Platzierung der Skip-Connections. In einem reinen Super-Resolution-Szenario transportieren Skips hochfrequente Rauschanteile direkt vom Encoder zum Decoder. Da hier jedoch Denoising eine Kernaufgabe ist, müssen die Skip-Connections **hinter** dem Rausch-Entfernungs-Schritt im Bottleneck ansetzen oder gefiltert werden. Eine "Gated Skip Connection", die nur Merkmale weiterleitet, die nicht mit dem RONI-Rauschraum korrelieren, wäre eine innovative Lösung.

Zudem zeigt die Analyse von STNet , dass die Unterteilung der Zeitreihe in überlappende Schiebefenster (Sliding Windows) die Stabilität der Merkmalsextraktion massiv verbessert. Das ST-CUNet sollte daher nicht die gesamte Sequenz auf einmal verarbeiten, sondern in temporalen Fenstern operieren, um die Dynamik lokaler Hirnareale ("Hubs") besser zu erfassen.

## Fazit und Handlungsempfehlungen

Die Entwicklung einer Netzwerk-Architektur für die kombinierte Aufgabe von Super-Resolution, Denoising und Temporal Enhancement in der fMRI ist ein hochkomplexes Unterfangen, das weit über einfache Bildverarbeitung hinausgeht. Die hier vorgestellte **ST-CUNet** Architektur bietet einen fundierten Rahmen, um diese Herausforderungen zu meistern.

Für die praktische Umsetzung im Rahmen des KI-Projekts an der TH Ingolstadt werden folgende Schritte empfohlen:

1. **Architektur-Setup:** Implementieren Sie ein 3D-U-Net als Basis. Integrieren Sie die duale Encoder-Logik von DeepCor, um Signal- und Rauschrepräsentationen kontrastiv zu trennen.
    
2. **Zeitliche Modellierung:** Setzen Sie im Bottleneck einen Transformer-Block ein, um zeitliche Abhängigkeiten über mindestens 16–32 Frames zu modellieren. Dies adressiert das "Temporal Enhancement" direkt.
    
3. **Upsampling-Strategie:** Ersetzen Sie Deconvolutionen durch Pixel-Shuffle-Layer kombiniert mit Residual-Learning, um feine Strukturen artefaktfrei zu super-resolven.
    
4. **Loss-Funktions-Design:** Nutzen Sie eine Multi-Task-Loss-Funktion. Ein rein MSE-basierter Ansatz wird zu uncharfen Ergebnissen führen. Integrieren Sie einen Adversarial Loss (GAN) und einen KL-Divergenz-Term für den CVAE-Teil.
    
5. **Validierungsprotokoll:** Nutzen Sie die bereitgestellte VM für eine umfassende GLM-Analyse Ihrer prozessierten Daten. Der Erfolg Ihres Netzwerks misst sich nicht an hübschen Bildern, sondern an der statistischen Stärke der resultierenden Hirnaktivierungskarten (z. B. FFA/PPA Kontraste).
    

Durch die Synthese von kontrastivem generativen Lernen (DeepCor), tiefer räumlicher Rekonstruktion (U-Net/SR) und globaler zeitlicher Aufmerksamkeit (Transformer) kann eine Architektur geschaffen werden, die die Grenzen der physikalischen Akquisition überwindet und neue Einblicke in die Dynamik des menschlichen Gehirns ermöglicht.