




---


# Datenaugmentation für die Infrarot-Drohnendetektion: Sensorphysikalische Rauschmodellierung, Grenzfall-Transformationen und State-of-the-Art-Methoden

Die automatische Detektion unbemannter Luftfahrzeuge (Unmanned Aerial Vehicles, UAVs) im Infrarotspektrum (Infrared, IR) stellt ein kritisches Element moderner Überwachungs-, Aufklärungs- und Abwehrsysteme dar. Während Wärmebildkameras in den Bandbreiten des mittwelligen (MWIR, $3\text{--}5\,\mu\text{m}$) und langwelligen Infrarots (LWIR, $8\text{--}14\,\mu\text{m}$) den Einsatz bei Dunkelheit, Rauch oder ungünstigen Witterungsbedingungen ermöglichen, bringt die Modalität inhärente physikalische und systemtechnische Einschränkungen mit sich. Hierzu zählen ein geringer Kontrast zwischen Zielen und Hintergrund, eine ausgeprägte Texturlosigkeit sowie komplexe stochastische und deterministische Sensorrauschmuster.

Deep-Learning-basierte Objektkaskaden und Segmentierungsnetzwerke – wie YOLO-Varianten, Feature Pyramid Networks (FPN) oder Dense Nested Attention Networks (DNANet) – reagieren hochgradig sensibel auf die Qualität und Varianz der Trainingsdaten. Da annotierte thermische Datensätze für kleine fliegende Objekte (z. B. DAUB, HIT-UAV, IRSTD-1k) in der Praxis schwer zu beschaffen und oft domänenspezifisch eingeschränkt sind, ist die mathematisch und sensorphysikalisch fundierte Datenaugmentation von zentraler Bedeutung.

## Sensorphysikalische Rausch- und Artefaktmodellierung im Infrarotspektrum

Um neuronale Netze gegenüber realen Umgebungs- und Hardwarebedingungen zu robotisieren, reicht die Anwendung konventioneller, visueller Augmentationsmethoden wie zufällige Rotationen oder Spiegelungen nicht aus. Die Synthese von Trainingsdaten muss die elektro-optische Übertragungskette eines Infrarotsensors explizit nachbilden. Reale Infrarot-Focal-Plane-Arrays (FPAs), wie gekühlte Photodetektoren (z. B. InSb, MCT) oder ungekühlte Mikrobolometer, unterliegen physikalischen Rauschprozessen, die direkt in den Augmentationsprozess integriert werden müssen.

### Photometrische Grundlagen und Temporäres Rauschen (NETD)

Das temporäre Rauschen eines Infrarotsensors beschreibt stochastische Fluktuationen über die Zeit und bestimmt das fundamentale Detektionslimit eines thermischen Bildgebers. Standardisiert wird dieses Systemverhalten durch die temperaturäquivalente Rauschdifferenz (Noise Equivalent Temperature Difference, NETD) charakterisiert. Die NETD definiert denjenigen Temperaturunterschied $\Delta T$ des beobachteten Objekts im Vergleich zum Hintergrund, der am Sensorausgang ein Signal-Rausch-Verhältnis (Signal-to-Noise Ratio, SNR) von genau 1 erzeugt.

In praktischen Synthese-Pipelines setzt sich das temporäre Rauschen $n_t(i, j, t)$ an der Pixelposition $(i, j)$ zum Zeitpunkt $t$ aus mehreren physikalischen Komponenten zusammen:

$$\sigma_{\text{temp}}^2 = \sigma_{\text{shot}}^2 + \sigma_{\text{MUX}}^2 + \sigma_{\text{ADC}}^2$$

Hierbei repräsentiert $\sigma_{\text{shot}}$ das Photonen-Schrotrauschen (proportional zur Wurzel der einfallenden Photonenstromdichte), $\sigma_{\text{MUX}}$ das Ausleserauschen der Multiplexer-Schaltkreise und $\sigma_{\text{ADC}}$ das Quantisierungsrauschen des Analog-Digital-Wandlers.

Zur augmentativen Generierung von NETD-konformem temporärem Rauschen wird die Rauschstandardabweichung $\sigma_{\text{noise}}$ im Grauwertraum proportional zur Temperatursensitivität skaliert. Nimmt man eine Szenenkontrastannahme von $\Delta T$ (z. B. $5\,\text{K}$) an, errechnet sich der einzuspeisende Gauß'sche Rauschterm gemessen an der Intensitätsstandardabweichung der Pixel $\sigma_{\text{image}}$ wie folgt:

$$\sigma_{\text{noise}} = \frac{\text{NETD}}{\Delta T} \cdot \sigma_{\text{image}}$$

Das augmentierte Bild $I_{\text{aug}}(i, j)$ ergibt sich durch Überlagerung eines frequenz-unabhängigen Gauß'schen Rauschfeldes $n_t(i, j) \sim \mathcal{N}(0, \sigma_{\text{noise}}^2)$. Ein maßgeblicher Befund der Literatur liegt darin, dass NETD-bezogenes Rauschen vor geometrischen Resizing- und Interpolationsschritten auf das Rohbild angewendet werden muss, da räumliche Skalierungen die Rauschvarianz künstlich glätten und somit die Sensorcharakteristik verfälschen.

### Räumliches Nichtuniformitätsrauschen (Fixed Pattern Noise - FPN)

Im Gegensatz zum temporären Rauschen ist das Fixed Pattern Noise (FPN) eine räumlich korrelierte, zeitinvariante oder langsam driftende Inhomogenität, die durch herstellungsbedingte Toleranzen der einzelnen Detektorelemente im FPA entsteht. Die Inhomogenität des Sensorelements $(i, j)$ wird standardmäßig über ein lineares Signalmodell beschrieben:

$$y(i, j, t) = a(i, j) \cdot x(i, j, t) + b(i, j) + n_t(i, j, t)$$

Dabei stellt $x(i, j, t)$ die eintreffende Bestrahlungsstärke (Clean Image), $y(i, j, t)$ das beobachtete Sensorsignal, $a(i, j)$ den multiplikativen Verstärkungsfaktor (Gain Non-uniformity) und $b(i, j)$ den additiven Offset-Fehler (Offset Non-uniformity) dar. Trotz werksseitiger oder laufender Non-Uniformity Correction (NUC) verbleibt in der Praxis stets ein korreliertes Rest-FPN.

Zur Modellierung strukturierter FPN-Artefakte wird der additive Offset $b(i, j)$ in drei überlagerte Komponenten zerlegt:

$$b(i, j) = b_w(i, j) + b_r(j) + b_c(i)$$

Das unstrukturierte FPN $b_w(i, j) \sim \mathcal{N}(0, \sigma_{bu}^2)$ beschreibt räumlich weißes, von Pixel zu Pixel unkorreliertes Offset-Rauschen. Das Zeilenstreifenrauschen $b_r(j) \sim \mathcal{N}(0, \sigma_{br}^2)$ stellt ein zeilenweise konstantes Offset-Rauschen dar, welches durch Instabilitäten in den zeilenspezifischen Referenzspannungen bedingt ist. Das Spaltenstreifenrauschen $b_c(i) \sim \mathcal{N}(0, \sigma_{bc}^2)$ ist spaltenweise konstant und wird durch unterschiedliche Übertragungsfunktionen der Spalten-Auslesekanäle (Readout Integrated Circuit, ROIC) hervorgerufen.

Dieses ausgeprägte Spalten- und Zeilenstreifenmuster (Stripe Noise) führt bei Detektoren häufig zu Fehlalarmen entlang vertikaler oder horizontaler Strukturen. Durch gezielte synthetische Überlagerung von $b_c(i)$ und $b_r(j)$ mit variablen Standardabweichungen ($\sigma_{bc}, \sigma_{br} \in [2, 10]$ Grauwerten) lernt das neuronale Netz, linienförmige Strukturartefakte von echten, lokal kompakten Drohnen-Signaturen zu unterscheiden.

### Niederfrequente Inhomogenitäten und Common Mode Noise

Über hochfrequentes FPN hinaus treten in Wärmebildkameras niederfrequente räumliche Intensitätsgradienten auf, die als Common Mode Noise oder Low-Frequency Non-Uniformity bezeichnet werden. Ursache hierfür ist primär die thermische Eigenstrahlung der Optik und des Kameragehäuses (Thermal Self-Emission), die zu schattenartigen Verläufen oder Vignettierungseffekten auf dem Sensor führt.

Zusätzlich zeigt die Ausleseelektronik ein ausgeprägtes $1/f$-Rauschen (Flicker Noise) im Frequenzbereich. Die Leistungsspektraldichte (Power Spectral Density, PSD) des $1/f$-Rauschens wird im Frequenzraum wie folgt formuliert:

$$S_{1/f}(f_u, f_v) = \frac{K}{(f_0^2 + f_u^2 + f_v^2)^{\alpha/2}}$$

Wobei $K$ die Rauschintensität, $f_0$ einen Regularisierungsterm zur Vermeidung von Singularitäten bei der Frequenz Null und $\alpha \approx 1$ den exponentiellen Abfall beschreiben. In der Augmentationspipeline wird weißes Gauß'sches Rauschen generiert, über eine Fouriertransformation (FFT) mit der Frequenzfilterfunktion $H(f_u, f_v) = \sqrt{S_{1/f}(f_u, f_v)}$ gewichtet und mittels inverser Fouriertransformation (IFFT) zurück in den Bildraum transformiert. Das Ergebnis ist ein räumlich weich verlaufendes, niederfrequentes Störfeld, das als additives Common Mode Noise fungiert und den Detektor zwingt, Objekte auch unter stark schwankenden Hintergrund-Helligkeitsniveaus zu isolieren.

## Transformationen für Grenzfälle: Geringes SNR/SCR und Kleinstobjekt-Augmentation

Ein Hauptproblem bei der Infrarot-Drohnendetektion liegt in der extremen Ungleichheit zwischen der Target-Fläche und dem komplexen Hintergrund (Cloud-Clutter, Boden-Clutter). Fernbereichs-Drohnen zeichnen sich durch räumliche Abmessungen von teils nur $2 \times 2$ bis $8 \times 8$ Pixeln aus und besitzen kaum Textur- oder Forminformationen. Ihr Erscheinungsbild entspricht nahezu punktförmigen Gauß'schen Abstrahlungen mit extrem niedrigem Signal-Rausch-Verhältnis (SNR) sowie geringem Signal-to-Clutter Ratio (SCR).

### Skalierungsartefakte und Anti-Aliasing-Downscaling

Standardmäßige räumliche Skalierungsoperationen wie die bilineare oder bikubische Interpolation führen bei der Verkleinerung kleiner Infrarotobjekte zu destruktiver Pixelauflösung und unkontrollierbarem Feature Shift. Wird ein $6 \times 6$ Pixel großes Drohnensignal unbedacht auf 50 % herunterskaliert, gehen die Randpixel durch Mittelwertbildung im Hintergrund auf, wodurch die thermische Energie des Objekts rechnerisch vernichtet wird.

Um Kleinstobjekte verlustfrei zu augmentieren, greifen spezialisierte Pipelines auf ein zweistufiges Verfahren zurück. Zunächst wird das Intensitätsprofil eines thermischen Punktziels vor der Skalierung als zweidimensionale Gauß-Funktion modelliert:

$$I(x, y) = I_0 \cdot \exp\left( -\left( \frac{(x - x_0)^2}{2\sigma_x^2} + \frac{(y - y_0)^2}{2\sigma_y^2} \right) \right)$$

Anstelle konventioneller Max-Pooling- oder Bilinear-Methoden wird anschließend ein Anti-Aliasing-Tiefpassfilter (wie Area-Based Resampling oder Bilaterales Filtern) vorgeschaltet, bevor die Pixelreduktion erfolgt. Dadurch bleibt das mathematische Integral der abgestrahlten thermischen Energie (die kumulierte Intensität des Drohnenkerns) weitgehend erhalten, während die Objektfläche realistisch im Subpixelbereich verkleinert wird.

### Grauwert-Histogrammverschiebung (Histogram Shifting)

Die absolute Helligkeit im Infrarotbild spiegelt die gemessene Strahlungsintensität wider. Schwankungen der Umgebungstemperatur, jahreszeitliche Wechsel oder Änderungen der atmosphärischen Transmission verändern das globale Intensitätsniveau eines IR-Bildes. Das Histogram Shifting simuliert diese globalen thermischen Pegelverschiebungen:

$$I_{\text{shift}}(x, y) = \text{clamp}\left( I(x, y) + \Delta g, \, 0, \, 2^B - 1 \right)$$

Hierbei ist $\Delta g \sim \mathcal{U}(-g_{\text{max}}, g_{\text{max}})$ ein zufälliger Intensitäts-Offset und $B$ die Bit-Tiefe des Bildes ($B=8$ oder $B=14$). Das reine Verschieben des Gesamthistogramms verändert die thermischen Differenzen $\Delta I = I_{\text{target}} - I_{\text{background}}$ lokal nicht, verhindert aber, dass tiefe neuronale Netze feste absolute Schwellenwerte für die Aktivierung von Erkennungs-Head-Features erlernen.

### Stauchung von Grauwert-Histogrammen (Histogram Compression)

Die Grauwert-Histogrammstauchung (Dynamic Range Compression) ist eine der effektivsten Methoden zur Synthese von Extremfällen mit niedrigem SNR/SCR. Während eine Verschiebung das Mittelmaß verstellt, reduziert die Stauchung die Varianz der Grauwertverteilung und komprimiert den dynamischen Bereich. Mathematisch wird die Histogrammstauchung um den lokalen oder globalen Mittelwert $\mu$ der Bildintensität über einen Stauchungsfaktor $\gamma \in (0, 1)$ definiert:

$$I_{\text{comp}}(x, y) = \text{clamp}\left( \mu + \gamma \cdot \left( I(x, y) - \mu \right), \, 0, \, 2^B - 1 \right)$$

Die physikalischen und systemischen Implikationen dieser Transformation sind vielschichtig. An heißen Sommertagen oder bei langen Flugzeiten gleicht sich die Oberflächen- und Abgastemperatur der Drohnenelektrik bzw. der Rotoren an die Umgebungstemperatur an. Ein Wert von $\gamma \rightarrow 0{,}2$ senkt den thermischen Kontrast drastisch ab und erzwingt ein extrem niedriges SCR.

Darüber hinaus liefern Infrarotsensoren nativ oft 14-Bit-Rohdaten ($16.384$ Grauwertstufen), die über Automatic Gain Control (AGC) Algorithmen auf 8-Bit ($256$ Stufen) für Standard-Prozessoren abgebildet werden. Bei schlechter AGC-Einstellung kommt es zur Kammbildung (Comb-like Artifacts) im Histogramm. Die Kombination aus Histogrammstauchung und nachfolgender Quantisierung bildet genau diese Informationsverluste ab.

Das lokale Signal-to-Clutter Ratio (SCR) wird mathematisch wie folgt berechnet:

$$\text{SCR} = \frac{\vert{}\mu_T - \mu_B\vert{}}{\sigma_B}$$

Wobei $\mu_T$ der Mittelwert der Objektpixel, $\mu_B$ der Mittelwert der direkten Hintergrund-Umgebung (Target Bounding Box Margin) und $\sigma_B$ die Standardabweichung des Hintergrunds ist. Durch die Histogrammstauchung sinkt der Zähler $\vert{}\mu_T - \mu_B\vert{}$ linear mit $\gamma$, was das SCR präzise auf kritische Grenzbereiche ($\text{SCR} < 2{,}0$) absenkt, bei denen herkömmliche Detektoren eine hohe Missed-Detection-Rate aufweisen.

## State-of-the-Art Augmentationsmethoden aus der Literatur

In den letzten Jahren hat sich die Forschung von einfachen geometrischen und photometrischen Transformationen hin zu physikalisch geführten, kontextsensitiven und generativen Augmentationsmodellen entwickelt.

### Prior-Guided Data Augmentation (PGDA)

Ein Meilenstein für die Augmentation kleiner Infrarotziele ist die von Wang et al. (IEEE J-STARS 2022) eingeführte Prior-Guided Data Augmentation (PGDA). PGDA löst das Grundproblem, dass Standard-Copy-Paste-Methoden auf Infrarotbildern zu schwerwiegenden physikalischen Artefakten führen, etwa unnatürliche scharfe Kanten oder Drohnen, deren Intensität dunkler als der kalte Himmel ist.

Das PGDA-Verfahren baut auf zwei physikalischen Priorisierungen auf. Zum einen nutzt es den Maßstabsverteilungs-Prior (Scale Distribution Prior): PGDA analysiert die statistische Verteilung des Flächenverhältnisses $A$ (Area Ratio) aller Realdatenobjekte im Datensatz:

$$A = \frac{\text{Area}(\text{Target})}{\text{Area}(\text{Image})}$$

Instanzen werden beim Einfügen selektiv so skaliert, dass ihr Flächenverhältnis strikt innerhalb der realen Verteilungsgrenzen (z. B. $A \le 0{,}15\%$) verbleibt. zum anderen basiert es auf der SCR-Perzeption und 2D-Gauß-Einfügung (Gaussian Paste & SCR Perception): Wenn eine Zielinstanz ausgeschchnitten und an einer neuen Zielposition eingefügt wird, berechnet PGDA das SCR des lokalen Einfüge-Hintergrunds. Die Zielintensität wird dynamisch modifiziert, um eine physikalisch konsistente Infrarotabstrahlung zu garantieren. Zur Vermeidung harter Schnittkanten wird eine 2D-Gauß'sche Unschärfemaske $W_{\text{Gauss}}$ über die Objektmaske gelegt, wodurch der Objektrand fließend in die Pixelstruktur des neuen Hintergrunds gedämpft wird.

PGDA bewirkt eine vollständige Entkopplung der Ziel-Beschreibungseigenschaften (Target Description) von den reinen Lokalisierungsfähigkeiten (Localization) des Detektors, ohne zusätzliche Modellspeicherlast zu erzeugen.

### Thermisch-Kontextsensitive Copy-Paste-Verfahren und Target Adaptive Proliferation

Klassische Copy-Paste-Verfahren versagen im IR-Bereich oft, da Objekte ohne ihren thermischen Gradienten versetzt werden. Neuere Ansätze wie die Target Adaptive Proliferation (TAP) und Context-Aware Thermal Copy-Paste erweitern den Einfügeprozess.

Anstatt nur die binär maskierten Target-Pixel zu kopieren, wird beim Prinzip der lokalen Kontext-Kopplung (Local Context Retention) ein erweiterter lokaler Umgebungssaum (Local Background Ring) mitkopiert. Dadurch bleiben lokale Absorptions- und Streueffekte der Luftschicht um die Drohne erhalten. Um Grauwert-Diskrepanzen zwischen der Quelldomäne und der Zieldomäne auszugleichen, wird das Objekt über eine kumulative Verteilungsfunktion (CDF) mittels Histogramm-Anpassung via Look-Up Tables (LUT) an das Helligkeits- und Rauschniveau des Zielbildes angeglichen. Die Platzierung von augmentierten Flugobjekten erfolgt strikt unter Ausschaltung unmöglicher Regionen (z. B. Platzierung einer fliegenden Drohne im Wasser oder tief im Boden-Clutter), indem automatisierte Hintergrund-Segmentierungs-Priors genutzt werden.

### Generative und Diffusionsbasierte Augmentationsansätze

Für die hochgradig komplexe Synthese kompletter Szenarien gewinnen generative Modelle zunehmend an Bedeutung. Ansätze wie CycleGAN werden eingesetzt, um sichtbare RGB-Drohnenbilder in das Infrarotspektrum zu übersetzen (Image-to-Image Translation). Dadurch lassen sich riesige Mengen an synthetischen IR-Hintergründen erzeugen, in die anschließend thermische Zielsignaturen eingerechnet werden.

Neueste Veröffentlichungen nutzen label-erhaltende Diffusionsmodelle (z. B. DiffuseMix) sowie multimodale Language-Guided Networks (z. B. MIRSAM, LGNet, LangIR). Über Text-Prompts wie "dim small drone in heavy cloud clutter at night" werden semantisch konsistente Hintergrundvariationen synthetisiert, ohne die exakte räumliche Bounding-Box des Ziels zu zerstören.

Forschungsarbeiten von Liiv et al. demonstrieren darüber hinaus den Erfolg von "Synthetic-First"-Strategien. Hierbei werden physikalisch basierte 3D-CAD-Modelle von Drohnen in virtuellen Umgebungen mit variablen Flug-Poses, thermischen Emissionen und Hintergründen gerendert. Anschließend erfolgt die Anpassung an die Realdatendomäne über ein fokussiertes Fine-Tuning, wobei Metriken wie der Synthetic Dataset Quality Metric (SDQM) die Feature-Space-Distanz zur echten IR-Domäne minimieren.

## Vergleichende Synthese und Systematisches Pipeline-Design

Zur systematischen Übersicht ordnen die folgenden Tabellen die sensorphysikalischen Rauschmodelle sowie die State-of-the-Art Augmentationsverfahren ihren mathematischen und anwendungsbezogenen Eigenschaften zu.

### Systematischer Vergleich der Sensor-Rauschmodelle

|**Rausch- / Artefakt-Typ**|**Physikalische Ursache**|**Mathematische Formulierung / Modellierung**|**Implementierung in der Augmentations-Pipeline**|
|---|---|---|---|
|**Temporäres Rauschen (NETD)**|Photonen-Schrotrauschen, MUX- & ADC-Rauschen|$n_t \sim \mathcal{N}\left(0, \left(\frac{\text{NETD}}{\Delta T} \cdot \sigma_{\text{img}}\right)^2\right)$|Additives Gauß'sches Rauschfeld vor geometrischem Resizing injizieren.|
|**Unstrukturiertes FPN ($b_w$)**|Pixelspezifische Offset-Inhomogenitäten des FPA|$y = a \cdot x + b_w + n_t$, $b_w \sim \mathcal{N}(0, \sigma_{bu}^2)$|Pixelweises, zeitinvariantes Offset-Gitter auf das Rohbild addieren.|
|**Spaltenstreifenrauschen ($b_c$)**|Nichtuniformität der ROIC-Spalten-Auslesekanäle|$b_c(i) \sim \mathcal{N}(0, \sigma_{bc}^2)$, konstant für Spalte $i$|Erzeugung eines 1D-Rauschvektors, ausgedehnt über alle Zeilen der Bildmatrix.|
|**Common Mode Noise / $1/f$-Rauschen**|Thermische Gehäuse-Eigenstrahlung & Flicker-Noise|PSD: $S(f) = \frac{K}{(f_0^2 + f^2)^{\alpha/2}}$|Spektral gefiltertes Rauschen via 2D-FFT/IFFT zur Generierung weicher Gradienten.|

### Systematischer Vergleich der SOTA-Augmentationsmethoden

|**Methode / Technik**|**Kernmechanismus**|**Solvierte Problematik / Zielsetzung**|**Auswirkung auf IRSTD-Metriken**|
|---|---|---|---|
|**Prior-Guided Data Augmentation (PGDA)**|Scale-Distribution Prior, 2D-Gauß-Blending & dynamische SCR-Perzeption|Verhindert unphysikalische Schnittkanten und Intensitätsbrüche beim Copy-Paste|Signifikante Steigerung von mAP50 und IoU; Vermeidung von False Alarms|
|**Context-Aware Thermal Copy-Paste**|Kopieren inklusive Local Background Ring, Histogram Matching via LUT|Beseitigt thermische Domänen-Gaps zwischen Quell- und Zielhintergrund|Höhere Precision bei dichtem Clutter; verbesserte Generalisierung über Szenen|
|**Histogram Compression & Dynamic Range Reduction**|Mathematische Stauchung der Intensitätsvarianz $\gamma \cdot (I - \mu)$|Simuliert thermisches Gleichgewicht (Low SCR) & AGC-Quantisierungsfehler|Erhöht die Detektionsrate ($P_d$) bei extrem dimmen, weit entfernten Drohnen|
|**Synthetic-First & Domain Randomization**|3D-CAD-Rendering + SDQM Feature-Space Adaptation|Löst den Mangel an manuell annotierten Realdaten-Szenarien|Ermöglicht robustes Zero-Shot / Few-Shot Learning auf unbekannten IR-Sensoren|

### Architektur und Ausführungsreihenfolge einer optimalen Augmentationspipeline

Um das Zusammenspiel dieser Methoden gewinnbringend in der Praxis einzusetzen, muss die Ausführungsreihenfolge innerhalb der Daten-Pipeline physikalisch korrekt gestaltet werden. Eine Fehlplatzierung einzelner Schritte – etwa das Einfügen von Rauschen nach der geometrischen Skalierung oder das Hinzufügen von Zielobjekten nach der Sensorauslesesimulation – zerstört die physikalische Realitätstreue des synthetisierten Bildes.

Die optimale Verarbeitungsfolge gliedert sich in vier aufeinander aufbauende Phasen:

1. **Instanz-Augmentation (SOTA Layer):** Zunächst werden Objektinstanzen mittels PGDA oder Context-Aware Thermal Copy-Paste aus Quelldatensätzen extrahiert und in den Zielhintergrund eingebettet. Hierbei kommen 2D-Gauß-Blending und lokale SCR-Anpassungen zum Einsatz, um verfälschende Kantenartefakte zu eliminieren.
    
2. **Photometrische Kontrast-Transformationen:** Auf das zusammengesetzte Bild werden globale photometrische Operationen angewendet. Das Histogram Shifting simuliert wechselnde Umgebungstemperaturen, während die Histogrammstauchung (Histogram Compression) den Kontrast absenkt, um thermische Gleichgewichtszustände und niedrige SCR-Werte zu erzeugen.
    
3. **Sensorphysikalische Rauscheinspeisung:** Im nächsten Schritt werden die kameraspezifischen Störgrößen auf das kontrastmodifizierte Bild aufprägt. Dies umfasst die Einspeisung von niederfrequentem Common Mode Noise ($1/f$-Rauschen via FFT/IFFT), das Hinzufügen von strukturiertem Rest-FPN (Spalten- und Zeilenstreifen) sowie das Überlagern von temporärem NETD-Gaußrauschen.
    
4. **Geometrische Skalierung & Quantisierung:** Abschließend erfolgen die räumlichen Skalierungen mittels Anti-Aliasing Downscaling und Subpixel Resampling, um Kleinstobjekte ohne Signalverlust zu verkleinern. Den Abschluss bildet die AGC-Quantisierung von der native Sensordynamik (14 Bit) auf das finale Bildformat (8 Bit) inklusive Clipping.
    

Der entscheidende Vorteil dieser Reihenfolge liegt darin, dass synthetisch eingefügte Zielinstanzen zuerst photometrisch an die Szene angeglichen werden, bevor sie gemeinsam mit dem Hintergrund den identischen Sensorstörungen und geometrischen Skalierungsverlusten unterworfen werden. Dadurch entsteht ein physikalisch konsistentes Bildprodukt, das vom Detektor nicht aufgrund künstlicher Artefakte durchschaut werden kann.

## Fazit und Ausblick

Die effektive Datenaugmentation für die Infrarot-Drohnendetektion erfordert den Übergang von rein visuellen Operatoren hin zu sensorphysikalisch fundierten Syntheseverfahren. Das präzise Verständnis und die mathematische Modellierung von NETD-bedingtem temporärem Rauschen, strukturiertem Fixed Pattern Noise (insbesondere Spaltenstreifen) und niederfrequentem Common Mode Noise sind essenziell, um neuronale Netze gegenüber den Hardware-Unzulänglichkeiten realer FPAs zu härten.

Für die Bewältigung von Extremfällen – wie der Detektion winziger, thermisch schwacher Drohnen im Fernbereich – erweisen sich zielgerichtete Histogramm-Transformationen als unverzichtbar. Während das Histogram Shifting veränderliche Umwelt- und Umgebungstemperaturen simuliert, stellt die Histogrammstauchung (Dynamic Range Compression) ein mächtiges Werkzeug dar, um künstlich extrem niedrige SCR-Werte zu erzeugen. Dies zwingt das Modell, feine räumliche Gradienten und kontextuelle Muster anstelle einfacher Intensitätsschwellenwerte zu erlernen.

In der Literatur zeigen neuere Verfahren wie die Prior-Guided Data Augmentation (PGDA) und thermisch-kontextsensitive Copy-Paste-Ansätze, dass das Einfügen von Zielinstanzen strikt durch maßstabs- und strahlungsphysikalische Priors reguliert werden muss. Zukünftige Entwicklungen werden verstärkt auf die Kombination von physikalischen 3D-Simulationen (Synthetic-First) mit generativen Diffusionsmodellen setzen, um eine noch höhere Szenenvarianz ohne manuellen Annotationsaufwand zu erzielen. Durch den Aufbau modularer Augmentationspipelines, welche die physikalische Sensorübertragungskette exakt abbilden, lässt sich die Erkennungsleistung und Robustheit von Infrarot-Drohnendetektoren in operationalen Szenarien drastisch steigern.