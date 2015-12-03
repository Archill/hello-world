#Protokoll Feder-Masse-Dämpfer-System

##Aufstellen der Übertragungsfunktion

G_SISO=tf(sys_SISO)

G_SISO =
 
                       276.1
  -----------------------------------------------
  s^4 + 13.5 s^3 + 303.5 s^2 + 2431 s + 3.304e-13

Beim Aufstellen der Übertragungsfunktion fällt auf, dass diese einen Pol bei 3.3e-13 hat, der eigentlich bei 0 sein müsste.
Dies ist auf Maschinenungenauigkeit zurückzuführen, denn die Standartlösung von Matlab mit dem Befehl eig ist numerisch sehr ungünstig. 
Besser wäre eine QR-Zerlegung?????. Daraus folgt dass für eine konstante Eingangsspannung das System für t->∞ gegen einen begrenzenten endlichen Wert x2 strebt, die stationäre Verstärkung
also einen festen Wert (276.1/3.304e-14) hat. Dies wiederspricht jedoch der Physik, in der bei konstanter Spannung sich der Wagen immer weiter ins Unendliche bewegen würde. 
Deshalb wenden wir in Matlab minreal auf die Übertragungsfunktion an, welches dann dem realen System entspricht. 

G_SISO_min=minreal(G_SISO)

G_SISO_min =
 
                 276.1
  -----------------------------------
  s^4 + 13.5 s^3 + 303.5 s^2 + 2431 s

Wenn wir dieses zurück in ein Zustandsraummodell zurück führen ist das System nur noch ähnlich, aber nicht mehr dasselbe, weil Information verloren geht.??????Erklärung????? 
Dieses neue System ist nicht mehr physikalisch zu interpretieren, was es schwierig macht Nebenbedingungen zu definieren.



ss(G_SISO_min)

ans =
 
  a = 
           x1      x2      x3      x4
   x1   -13.5  -18.97  -9.495       0
   x2      16       0       0       0
   x3       0      16       0       0
   x4       0       0    0.25       0
 
  b = 
       u1
   x1   2
   x2   0
   x3   0
   x4   0
 
  c = 
          x1     x2     x3     x4
   y1      0      0      0  2.157
 
  d = 
       u1
   y1   0
 
Continuous-time state-space model.

##Identifizierung der Parameter c und d

Zur Parameteridentifizierung nehmen wir das Schwingverhalten mit einer Auslenkung des 2. Wagens x2,1=10 cm auf. 
Bei der Berechnung mit dem ersten und zweiten Minimum erhalten wir folgendes Ergebnis:

[c, d] = EstimateOscillatorParameters(1.44, 1.882, 0.08793, 0.06695, m_2)
c =

     1.412134585790648e+02 N/m


d =

   0.860339332350571 kg/s

Dieser Wert weicht stark von unserer Berechnung aus fünf Maxima und Minima ab?????Soll da noch mehr hin???????????????:

Spring stiffness:      131.2747 +/-  1.0845 N/m
Damping coefficient:     1.0676 +/-  0.2221 kg/s

Zur Kontrolle dieses Ergebnisses führen wir denselben Versuch mit einer extrem großen und kleinen Auslenkung von x2,2=15 cm und x2,3=6 cm durch. 

bei x2,2:
Spring stiffness:      130.9643 +/-  0.9712 N/m
Damping coefficient:     1.0433 +/-  0.1819 kg/s

bei x2,3
Spring stiffness:      131.3066 +/-  5.5996 N/m
Damping coefficient:     1.5739 +/-  1.7674 kg/s

Bei der kleinen Auslenkung ist auffällig, dass die Streuung so groß ist, dass sogar physikalisch unmögliche negative Dämpfung in dem Bereich ist. 
Jedoch konnten auch nur 2 Maxima und Minima analysiert werden, da die Auslenkung zu gering war. 
Je mehr Punkte in die Berechnung einfließen desto geringer wird die Streuung.
Nach weitereren Messungen waren wir uns sicher, mit c=131 N/m und d=1,1 kg/s eine gute Wahl getroffen zu haben.
