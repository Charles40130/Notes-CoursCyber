
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


Fox : prédicat obfusqué
On jump sur un registre qui a été calculé à partir ... , ghidra ne peut pas connaitre ça













06 : 

Via Ghidra: 
1. On cherche via les différentes fonctions à l'aide du SymbolTree si on répère une longue fonction en c qui ressemblerait à un main
2. Bingo ! Il semble qu'on ait trouvé une fct main , on peut renommer et éditer la signature de la fct( renommer types,vars , nom de la fct etc...) via click droit sur nom de fct = > edit function signature
3. OK => somme de atoi 2 args qui doivent être égal à 0x79e


07 : 
Via ghidra:
1. Répère du main
2. Fonctions en cascade => attention à pas se perdre
3. 0x2b doit être égale à atoi(argv[1])

08 :
$rsp : 0x0041414141414100

cmp    bh, BYTE PTR [rdi+rcx*1-0x4]

bh = 0x4100
rdi + rcx -0x4: 0x3368 + 0x16 -0x4

Valeur rentré :

|     | rdi +rcx-0x4                     | valeur [rdi+rcx-0x4] | bh   |     |
| --- | -------------------------------- | -------------------- | ---- | --- |
| T1  | 0x402014 + 0x16 - 0x4 = 0x402026 | 0x67 =               | 0x67 |     |
| T2  | 0x402014  +0x14-0x4 = 0x402024   | 0x57=                | 0x57 |     |
| T3  |                                  |                      |      |     |
| T4  |                                  |                      |      |     |
| T5  |                                  |                      |      |     |
>[!remarque] L'instruction  `loop` intègre des instructions cachés tel que :
>`dec $rcx ` pour décrémenter rcx de 1 et `test rcx, rcx` (si rcx égal à 0 sortie de la boucle)

Boucle qui comparait 1 caractère sur 2 de la chaîne "gW0-0zd43En10luDg3h" avec la chaîne passé en param.
=> Mdp : "g00d3n0ugh"

09:

char* chaine = malloc(11)

char[i] = '9' - char('0')
inc lVar2
arrêt à 10
argv[1]

Boucle qui créer une chaine de caractère '9876543210' et la compare avec notre argument passé en paramètre


10 :

