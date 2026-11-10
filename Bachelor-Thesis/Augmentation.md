 
---
# Paper
* 
* 
---
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

# List
## IR-Kamera
* NETD
* Fixed Pattern Noise
* Common Mode Noise
* Blurring with PSF / Tiefenschärfe
* Motion Blur
* Salt-and-Pepper-noise
* Stuck pixels
## Corner-case
* Downscaling
* Shift of grey-value histogram?
## State-of-the-art
* a

## Explaination
### Template
#### What
#### Why
#### How
#### Paper

#### Code

### NETD
Noise Equivalent Temperature Difference
#### What
NETD is a measure of sensitivity for a thermal camera model. It describes the smallest thermal difference the camera can differentiate from its own internal noise. The smaller the value, the better the thermal resolution and the clearer the subtle temperature details in the resulting image.
#### Why
Images taken from a singular camera model can not represent all possible models. By adding NETD-noise cameras with a worse NETD value can be simulated.

Thermal cameras have two categories: cooled and uncooled. The latter type has often a higher NETD value -> more noise.

#### How

$$I_{\text{augmented}}(x,y) = \text{Clip}(\alpha \cdot I(x,y) + \mathcal{N}(0, \sigma^2), 0, 255)$$
* $\alpha \in [0.7, 1.0]$: Scaling
* Low NETD: $\sigma \in$ [1,3] High NETD: $\sigma \in$ [8, 20] -> Höheres NETD = größeres $\sigma$
#### Paper
#### Code

### Fixed pattern noise
#### What
FPN is the result of production variances of the single pixels. Its time independent. Taking two images taken in the absolutely same environment results in the same image. 
#### Why
Especially in dark images where bigger section are near black FPN noise becomes prominent. Since thermal image can have those, e.g. dark sky, and small UAVs are possible targets this becomes relevant as an issue that needs to be solved.
#### How
$I_{\text{noisy}}(i,j)=\left[I_{\text{clean}}(i,j)\cdot (1+\text{PRNU}(i,j))\right]+\text{DCNU}(i,j)+N_{\text{random}}(i,j)$

- $I_{\text{clean}}(i, j)$:** The true incoming irradiance/signal.
- $PRNU(i, j)$: Multiplicative noise component, often modeled as a zero-mean Gaussian field with small standard deviation ($\sigma_{\text{PRNU}} \approx 0.01 - 0.02$).
- $DCNU(i, j)$: Additive dark current/offset component, often modeled as a spatial Gaussian or low-frequency spatial map.
- $N_{\text{random}}(i, j)$: Time-varying temporal noise (shot noise, read noise).
#### Paper
Infrared Image Deconvolution Considering Fixed Pattern Noise
READ: https://www.teledynevisionsolutions.com/de-de/learn/learning-center/imaging-fundamentals/pattern-noise-dsnu-and-prnu/
#### Code
See apply_fixed_pattern_noise

### Common Mode Noise
#### What
Its the result of external or/and systematic electronic disturbances. The result are stripes either vertical or horizontal in the image.
#### Why

#### How
$$
\begin{aligned}
y_i = x_i + s_i \\
s_i \sim N (0, \sigma^2) \\
\sigma \sim U(0, \beta)
\end{aligned}
$$
#### Paper
Deep Unfolding for Iterative Stripe Noise Removal
A Novel Stripe Noise Removal Model for Infrared Images?
### Salt-and-Pepper-noise
#### What
#### Why
#### How
$$f(x) = \begin{cases}
s_{min} & \text{with probability p} \\
s_{max} & \text{with probability q} \\
u(x) & \text{with probability 1 − p − q}
\end{cases}
$$

[smin, smax] is the range of u(x). Without
loss of generality, we assume that the intensity value of the image is in the range of [0, 1], thus smin = 0 and smax = 1; 
#### Paper
Salt and pepper noise removal based on an approximation
of $l_0$ norm
### Stuck pixels

### CLAHE
AHE (adaptive histogram equalization) is a method to improve contrast in an image. But one of its downsides is that it has a tendency to overamplify noise in relatively homogeneous regions of an image. CLAHE can prevent that by limiting the amplification.

Steps: 
1. Divide image into tiles
2. Calculate histogram for each tile
3. Bins of the histogram over the clip limit get clipped and uniformly added to the other bins of the histogram
4. Histogram equalization
5. Interpolation between the equalized tiles

Improved and adaptive method of AHE (adaptive histogram equalization). AHE can overbrighten bright areas and overdarken dark areas when the lighting is uneven. This is an issue for UAV detection because these can have small objects that need to be detected.

### Motion blur
Motion blur parameters estimation of uncooled infrared images
$$
\begin{aligned}
g = h \otimes f + n \\
h = \frac{1}{\tau_{im}} * e^{- \frac{x}{\tau_{im}}} \text{, for } 0 \leq x \text{ otherwise 0}
\end{aligned}
$$
### Blurring with PSF
Polyblur: Removing mild blur by polynomial reblurring -> PARAMETRIC MILD BLUR MODEL AND ESTIMATION

$$
\begin{aligned}
\tilde{x} = x \cos\theta + y \sin\theta \\
\tilde{y} = -x \sin\theta + y \cos\theta \\

k(x, y) = \frac{1}{Z} \exp\left( - \left( \frac{\tilde{x}^2}{2\sigma_1^2} + \frac{\tilde{y}^2}{2\sigma_2^2} \right) \right)
\end{aligned}
$$
  Wobei $Z$ wieder die Normierungskonstante ist, sodass die Summe aller Kernel-Werte $1.0$ ergibt.
  
  
  ---
# IDK
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

# Basic formulars
## Histogram equalizaton
1. Create the bins/# of pixels with intensity x
2. 
## 