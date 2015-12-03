#Protokoll Feder-Masse-Dämpfer-System

##Aufstellen der Übertragungsfunktion

G_SISO=tf(sys_SISO)

G_SISO =
 
                       276.1
  -----------------------------------------------
  s^4 + 13.5 s^3 + 303.5 s^2 + 2431 s + 3.304e-13

Beim Aufstellen der Übertragungsfunktion fällt auf, dass diese einen Pol bei 3.3e-13 hat, der eigentlich bei 0 sein müsste.
Dies ist auf Maschinenungenauigkeit zurückzuführen, denn die Standartlösung von Matlab mit dem Befehl eig ist numerisch sehr ungünstig. 
Besser wäre eine QR-Zerlegung?????. Daraus folgt dass für eine konstante Eingangsspannung das System für t->∞ 
