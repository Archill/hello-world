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
also einen festen Wert(276.1/3.304e-14) hat. Dies wiederspricht jedoch der Physik, in der bei konstanter Spannung sich der Wagen immer weiter ins Unendliche bewegen würde. 
Deshalb wenden wir in Matlab minreal auf die Übertragungsfunktion an, welches dann dem realen System entspricht. 
Wenn wir dieses zurück in ein Zustandsraummodell zurück führen ist das System nur noch ähnlich, aber nicht mehr dasselbe, weil Information verloren geht.??????Erklärung????? 
Dieses neue System ist nicht mehr physikalisch zu interpretieren, was es schwierig macht Nebenbedingungen zu definieren.

>> G_SISO_min=minreal(G_SISO)

G_SISO_min =
 
G_SISO_min =

               276.1
-----------------------------------
s^4 + 13.5 s^3 + 303.5 s^2 + 2431 s

##Identifizierung der Parameter c und d
