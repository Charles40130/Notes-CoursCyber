pointeur : 64 bits ( 8 octets)

décompiler : objdump -Mintel -d exe

Outils gef ( utile pour l’exploi binaire, reverse engineering)

désassembler un programme :

file exe

disas /r main

disas /r nomdelafonction

starti : permet de lancer la 1ère instruction du programme et met un breakpoint

hexdump byte 0x0000000000000002 --size 400

break main : met un breakpoint à

i b ; affiche les breakpoints dispos

conti : continue après le breakpoint

disas : affiche où on en est dans le programme

context : affiche là où on est

si : permet d’aller à la prochaine instruction

ni : un peu dans le même style à revoir

ip : instruction pointer , pointeur qui pointe vers une instruction dans le but d’indiquer au processeur quel instructions exécuter = boussole du cpu

evolution avec le temps : ip→ eip → rip

rip ( reextended instruction pointer)

mettre un breakpoint à une adresse précise : breakpoint *adresse

d 2 : delete le breakpoint 2

vmap : ~ls , view mémory map,

dereference —length 4 $sp : affiche la stack selon la taille des données précisés de $sp taille en nombre d’octet

hexdump byte $sp —size 32 : affiche la stack selon la taille des données précisés de $sp taille en nombre de bits

- Ecrire un script [compil.sh](http://compil.sh)
- Une fonction en assembleur avec prolog epilogue stockage de var locale sur la stack frame: exemple Fibo();