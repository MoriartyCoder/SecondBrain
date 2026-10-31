# Methods

## Sure selection
Checkbox for implemented
 - [ ] Flip (horizontal/ vertical)
 - [ ] Rotation (-30° - 30°)
 - [ ] Shear
 - [ ] Histogram Equalization / CLAHE
 - [ ] Gaussian Blur
 - [ ] Brightness & Contrast (Jitter)
 - [ ] Mosaic (Not so)
 
---
# Paper
* 
* 


---
## Geometric transfomations
The Power of Augmentations in IR Object Detection
Towards Visible and Thermal Drone
Monitoring with Convolutional Neural
Networks

## Illumination variation
Towards Visible and Thermal Drone
Monitoring with Convolutional Neural
Networks
To simulate drones in the shadows, we generate regular shadow maps by using random lines and irregular shadow maps via Perlin noise [28]. In the extreme lighting environments, we observe that drones tend to be in monochrome (i.e. the gray-scale) so that we change drone images to gray level ones.

## Image quality
Towards Visible and Thermal Drone
Monitoring with Convolutional Neural
Networks


## Artificial Fog?


## Artificial Bird/Plane/...
As a version of Copy-Paste-Aug (see below)


# AI Overview
| **Methode**                                   | **AI-Beschreibung**                                               | **Für IR geeignet?** | **Für IR-Drohnen geeignet?** | **Besonderheiten & Nutzen für deine Bachelorarbeit**                                                                                                                                                                  |
| --------------------------------------------- | ----------------------------------------------------------------- | -------------------- | ---------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Horizontaler / Vertikaler Flip**            | Spiegelung des Bildes an der X- oder Y-Achse.                     | **Ja** ✅             | **Ja** ✅                     | Drohnen können aus jeder Richtung fliegen. Ändert nichts am thermischen Profil.                                                                                                                                       |
| **Rotation & Shear**                          | Drehen des Bildes um bestimmte Winkel und Verzerren.              | **Ja** ✅             | **Ja** ✅                     | Simuliert unterschiedliche Aufnahmewinkel der Drohnenkamera.                                                                                                                                                          |
| **Scaling & Random Crop**                     | Zoomen und Ausschnitte erstellen (Tiling-Verwandt).               | **Ja** ✅             | **Ja** ✅                     | **Extrem wichtig** für kleine Objekte, um dem Modell verschiedene Drohnengrößen zu zeigen.                                                                                                                            |
| **Perspective Transform**                     | Verzerren der Perspektive, um 3D-Kippen zu simulieren.            | **Ja** ✅             | **Ja** ✅                     | Simuliert Schrägansichten der Drohne im Flug.                                                                                                                                                                         |
| **Brightness & Contrast (Jitter)**            | Zufälliges Ändern von Helligkeit und Kontrast.                    | **Ja** ✅             | **Ja** ✅                     | Simuliert unterschiedliche Umgebungstemperaturen, Tageszeiten oder Sensor-Gain-Einstellungen der Wärmebildkamera.                                                                                                     |
| **Histogram Equalization / CLAHE**            | Verbesserung des lokalen Kontrasts. XXX                           | **Ja** ✅             | **Ja** ✅                     | **Sehr gut für IR:** Hebt den thermischen Kontrast zwischen der (heißen) Drohne und dem (kalten) Himmel/Hintergrund hervor. ==🔴**AI-Recomendation**==                                                                |
| **Thermal Inversion (Hot-White / Hot-Black)** | Invertieren der Graustufen (Weiß = Heiß wird zu Schwarz = Heiß).  | **Ja** ✅             | **Ja** ✅                     | **Exklusiv für IR:** Thermografiekameras haben oft Modi ("White-Hot" vs. "Black-Hot"). Das Modell lernt, unabhängig vom Farbschema zu detektieren. ==🔴**AI-Recomendation**==                                         |
| **Gaussian / Sensor Noise**                   | Hinzufügen von Rauschen.                                          | **Ja** ✅             | **Ja** ✅                     | Simuliert das typische Rauschen von ungekühlten Mikrobolometer-Sensoren (Wärmebild-Rauschen). ==🔴**AI-Recomendation**==                                                                                              |
| **Gaussian / Motion Blur**                    | Unscharf machen (Weichzeichnen oder Bewegungsunschärfe).<br>XXXX  | **Ja** ✅             | **Ja** ✅                     | Simuliert atmosphärische Turbulenzen (Flimmern in der Luft) oder schnelle Drohnenbewegungen/Kameraschwenks.                                                                                                           |
| **Color Jitter / Hue / Saturation**           | Farbton-, Sättigungs- und Farbänderungen.<br>XXXXX                | **Nein** ❌           | **Nein** ❌                   | **Ungeeignet:** IR-Daten sind einkanalig (Grayscale / Thermalfarben) – echte Farbverschiebungen verfälschen die physikalischen Temperaturdaten.                                                                       |
| **Mosaic Augmentation**                       | Vier Bilder zu einem neuen Bild zusammenfügen (bekannt aus YOLO). | **Ja** ✅             | **Ja** ✅                     | Sehr effektiv, um verschiedene Kontexte (Himmel, Bäume, Gebäude) im Hintergrund zu mischen. ==🔴**AI-Recomendation**==                                                                                                |
| **MixUp / CutMix**                            | Überblenden von zwei Bildern oder Ausschneiden von Bereichen.     | **Bedingt** ⚠️       | **Bedingt** ⚠️               | Bei thermischen Daten vorsichtig sein: Das Mischen von Temperaturwerten kann physikalisch unsinnige Signaturen erzeugen, wird aber in der Praxis oft für Regularisierung genutzt. ==🔴**AI-Recomendation: Cut-Mix**== |
| **Copy-Paste Augmentation**                   | Ausschneiden einer Drohne aus Bild A und Einfügen in Bild B.      | **Ja** ✅             | **Ja** ✅                     | **Hervorragend für kleine Objekte:** Da Drohnen im IR-Datensatz oft selten sind, kannst du bestehende Drohnen-Heatmaps künstlich auf neue Hintergründe klonen (wichtig auf thermische Konsistenz achten!).            |
