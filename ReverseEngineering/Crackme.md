
00:

mdp : mistigri

./00 mistigri → renvoi ok et retourne 0

01 :

Avec gef , run avec 2 argument pour étudier le comportement

$rdi = 0x0000000000402001 → 0x4b4f007465726365 ("ecret"?),  
$rsi = 0x00007fffffffe1b7 → 0x4c00610061003130 ("01"?),

02 :
Procédure utilisé:
	- `file 02 test`
	- `info function` : présence d'une fct main.
	- `disas /r main` : Lecture des instructions et détection d'un strcmp ( comparatif de 2 chaines de carac)
	- `break *adresse_instruction_strcmp` : Mise en place d'un break point à ladresse d'instruction du strcmp
	- `starti` : Lancement du programme afin et lecture via la stack à l'instruction strcmp , récupération du mdp !

shl :

03 :
Procédure utilisé :
	1 . Inspections des fcts via `info function`
	2.
	3.L'instruction `lea edx, [rax+0x1]` montrait qu'on rajouté 1 à chaque lettre puis on compararait le résultat avec la chaine secrète Wfsz0Fbtz
	4. Entrée + 1 = secret, il s'agit d'un chiffrement de Caesar tel que Wfsz0Fbtz - 1 = Very/Easy
	5. Mdp : Very/Easy