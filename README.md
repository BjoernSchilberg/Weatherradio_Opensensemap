# README

Das kleine Progrämmchen nimmt die Daten von einer HTTP-Schnittstelle aus und
schickt sie mithilfe von der HTTP-Method "POST" zu Opensensemap.



## Socat

socat TCP-LISTEN:3000,fork TCP:10.1.106.36:80
