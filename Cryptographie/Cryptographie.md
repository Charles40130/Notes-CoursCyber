
>[!important] Raisons d'utiliser la cryptographie
> - **Confidentialité** : Garantie le fait que les données ne peuvent être lue que par une personne autorisée
> - **Integrité** : On garantie que les données du message n'ont pas été altéré par une personne non autorisée
> -  **Authenticité** : L'information est attribuée à son auteur légitime
> -  **Non-répudiation** : L'information ne peut faire l'objet d'un déni de la part de son auteur

>[!danger] Principe de Kerchhoffs
>La sécurité d'un système cryptographique ne doit reposer que sur le secret de la **clé**, et non sur le secret de l'algorithme (qui doit être considéré comme public).
### Attaques :

>[!important] Modèles d'attaquants
>- **Ciphertext-Only Attack (COA)** : L’attaquant connait seulement un
ou plusieurs chiffrés qu’il souhaite d´ecrypter.
>- **Known Plaintext Attack (KPA)** : L’attaquant connait plusieurs
couples clair/chiffré (qu’il n’a pas choisi lui-même).
>- **Chosen Plaintext Attack (CPA)** : L’attaquant possède une machine
chiffrante. Il peut donc chiffrer tous les messages qu’il souhaite.
>- **Chosen Ciphertext Attack (CCA)** : L’attaquant possèede une
machine d´echiffrante. Il peut donc choisir des chiffr´es puis obtenir
leurs messages clairs correspondants.

**Analyse fréquentielle**: Consiste à analyser la fréquence d'apparition de chaque lettre afin de venir le comparer avec les fréquences usuelles d'un texte dans une langue précise.

>[!note] Définition (bits de sécurité)
Lorsque que l’on dit qu’un cryptosystème a x bits de sécurité, cela signifie
qu’un attaquant à besoin d’effectuer au moins O(2x ) opérations pour le
casser.

>[!note] Théeorème (bits de sécurité et taille de clé)
Si la clé secrète que l’on veut retrouver s’écrit sur n bits, alors le
cryptosystème a **au plus** n bits de sécurité

## Chiffrement Basique

**Chiffrement par permutation** : Consiste à réordonner ou réarranger les lettres du message en clair selon une règle précise , sans la nature des lettres elle même
>[!example] Exemple : 
>- Chiffrement de Scytale

**Chiffrement par substitution** : Consiste à remplacer chaque lettre du texte clair par ne autre  selon une table ou une règle de correspondance, tout en conservant l'ordre original des positions.
>[!example] Example :
>- Chiffre de César
>- Chiffre de Vigenère


# Chiffrement asymétrique




Chiffrement asymétrique : fct à sens unique à trappe
- k<sub>pub</sub> : clé de chiffrement
- k<sub>priv</sub> : clé de déchiffrement
- Déchiffrement sans la clé **k<sub>priv</sub>** en temps exponentiel ( ~ NP -complet)
- Déchiffrement avec la clé k<sub>priv</sub> temps polynomial (k<sub>priv</sub> = trappe)


>[!note] Théorème de Bezout
> $$au + bv = pgcd(a,b)$$


>[!note] Théorème : Petit théroème de Fermat
>Si p est u un nombre premier et a $\in \mathbb{Z}$
>$$ a^p \equiv b \pmod{n}$$
>



# Chiffrement Symétrique

#### One-Time-Pad (OTP)
$$
\begin{aligned}
M &:= \{0,1\}^n
&&\longrightarrow \text{chaîne binaire de longueur } n \\[4pt]
K &:= \{0,1\}^n
&&\longrightarrow \text{la clé est aussi longue que le message à chiffrer} \\[4pt]
K &\text{ est distribuée uniformément sur } \{0,1\}^n \\[4pt]
\operatorname{Enc}(k,m) &:= k \oplus m
&&\text{où } \oplus \text{ est le XOR bit à bit} \\[4pt]
\operatorname{Dec}(k,m) &:= \operatorname{Enc}(k,m)
\end{aligned}
$$
- clé ne peut être utilisé qu'une fois
- clé incompressible car tiré uniformément

6+
### Chiffrements à flots
- très rapides
- on chiffre à la volée sans attendre d'avoir lu => bien pour les apps en tant réel
>[!note] Période maximale d'un LFSR à n bits : 
>$$2^n -1$$
De plus, si le polynôme de rétroaction est de degré n et irréductible, alors
le LFSR atteint la période maximale 2n − 1 pour tout registre initial non
nul.

Attaque: LFSR pas

>[!danger] Attaque : Algorithme Berlekamp-Massey
>Peut retrouver le polynôme de rétroaction en connaissant seulement 2n bits de la suite chiffrante
=> Conséquences : Attaques dans les modèles KPA, CPA, CCA 

A5/1 = Chiffrement standard du GSM , combine 3 LFSRs

### Chiffrement à blocs

>[!note] Théorie de Shannon
>Un bon algorithme de chiffrement doit satisfaire 2 propriétés:
> 1. **Diffusion** : Des changements minimes dans les données en entrée se traduisent par des changements importants dans les données en sortie
> 2. **Confusion** : Mesure la complexité de l'interdépendance entre la clé, le clair et le chiffré . Plus compléxité grande , meilleur est l'algo

**DES** :
- blocs de 64 bits
- clé maître rop courte : 64 bits sur le papier mais 56 effectifs car 8 bits de parité
- cassable en 24h

**AES** :
- taille blocs fixe : 128 bits
- taille de clé : 128 bits (10 tours) , 192 ( 12 tours), 256 (14 tours)
- 4 opérations sur une matrice 4*4 octets 
	- AddRoundKey
	- SubBytes (Confusion)
	- ShiftRows (Diffusion)
	- MixColumns (Diffusion)
- 

>[!note] **Mode opératoire de chiffrement par bloc** : 
>définissent la manière dont un algorithme de chiffrement par blocs (comme AES ou DES) traite un message plus long que sa taille de bloc standard (128 bits pour AES, 64 bits pour DES).
- ECE : Electronic Codeook , bloc (dé)chiffré **indépendamment** des autres blocs
	- Avantages : (dé)chiffrement parallélisable
	- Incovénient : 2 blocs en clair identiques sont chiffrés de manière identiques => pas iuf
- CBC : Cipher Block Chaining 
	- On effectue un XOR entre le bloc en clair Bi et le bloc ciffré précédent Ci-1
- CFB : Cipher Feedback
- OFB : Output Feedback
- CTR : Counter

# Fonction de hachage

>[!definition] Fonction de hachage
>Une fonctione de hachage permet à partir d'une suite de bits de renvoyer une suite de bits pseudo-aléatoire de taille fixe.

>[!danger] Collision
>Une fonction de hachage ne peut pas être injective : il existe x1 != x2 tel que H(x1) = H(x2)
>Résistance aux collisions : propriété d'une fct de hachage



Questions DS:
Soit un LFSR dont le polynôme de rétroaction est $P(X) = 1 + X^2 + X^4$ avec le registre initialisé à 1000. Quelle est sa période ?

Quelle est la différence entre une fonction de hachage et un MAC ?
Parmi les affirmations suivantes, laquelle ou lesquelles sont vraies ?

- On peut utiliser une fonction de hash pour authentifier un message
    
- On peut utiliser une fonction de hash pour chiffrer un message
    
- Une attaque par brute force sur la pré-image et la seconde pré-image sont de même complexité
    
- Aucune de ces affirmations n'est correcte
Soit un LFSR dont le polynôme de rétroaction est $P(X) = 1 + X^2 + X^4$ avec le registre initialisé à 1100. On souhaite l'utiliser dans un chiffrement de OTP pour chiffrer la séquence 0x5A (écriture hexadécimale).

1. Quel est le résultat du chiffré ?
    
2. Est-ce que c'est un chiffrement sûr (OTP + ce LFSR) ? Justifier.
Lequel des modes opératoires suivants n'offre pas de garantie de confidentialité ?
ECB
CTR
OFB
CBC
Aucune des ces réponses

Donner 3 exemples d'utilisations différentes d'empreinte.

Un MAC permet d'assurer :
la confidentialité
l'intégralité
la non répudiation
aucunes de ces réponses

Un attaquant intercepte un document numérique (par exemple un contrat de location) que vous avez signé. Il souhaite fabriquer un faux contrat qui générerait exactement la même empreinte de hachage que l'original, afin que la signature reste valide. Quelle propriété précise de la fonction de hachage cryptographique l'attaquant tente-t-il de briser (Résistance à la pré-image, à la seconde pré-image, ou aux collisions) ? Justifiez votre réponse

Il tente de briser la résistance à la seconde pré-image. Il connait déjà le message origina x1 et son haché , il cherche un autre message x2 != x1 tel que H(x1) = H(x2)

Pourquoi une fonction de hachage seule ne suffit-elle pas à prouver l'origine d'un message ?
Un hash garantit l'intégrité (le fichier n'a pas été modifié), mais n'importe qui peut recalculer un hash. Un MAC garantit l'intégrité ET l'authenticité de la source, car il nécessite une **clé secrète** partagée entre l'émetteur et le récepteur pour être calculé et vérifié.



Alice souhaite envoyer un message confidentiel à Bob et Bob veut être absolument certain que ce message provient bien d'Alice (non-répudiation). Expliquez les étapes de chiffrement et de signature qu'Alice doit effectuer en utilisant les clés asymétriques appropriées (précisez à chaque fois si elle utilise sa clé publique/privée ou celle de Bob).

Pour la **non-répudiation (signature)** : Alice hache son message et chiffre ce haché avec sa propre **clé privée** ($k\_priv\_Alice$). Pour la **confidentialité (chiffrement)** : Alice chiffre le message (et sa signature) avec la **clé publique** de Bob ($k\_pub\_Bob$). Seul Bob pourra déchiffrer avec sa clé privée, et il pourra vérifier la signature avec la clé publique d'Alice.