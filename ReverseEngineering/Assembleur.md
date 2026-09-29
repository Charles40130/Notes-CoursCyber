langage binaire le plus proche de la machine

code binaire = assembleur (différence de présentation seulement)

RAX (Accumulator Register) : utilisé principalement pour stocker la valeur de retour d'une fonction ou le résultat d'opérations artihmétiques
- contient le num de l'appel système (sous linux)

 RDI ( Destination Index) : utilisé comme index de destination pour les opérateurs sur les chaines de caractères. Contient le 1er paramètre d'une fonction (sous AMD 64)

RSI ( Source Index ) : Utilisé comme index source pour la lecture de données en mémoire. Contient le 2ème paramètre d'une fonction (x86-64)

Conventions d'appel et registres

- **Passage des arguments (fonctions) :** Les six premiers entiers ou pointeurs sont passés dans les registres `rdi`, `rsi`, `rdx`, `rcx`, `r8` et `r9`.

- **Arguments flottants :** Les huit premiers paramètres à virgule flottante utilisent les registres `xmm0` à `xmm7`.

- **Pile :** Les arguments supplémentaires sont placés sur la pile dans l'ordre inverse. La pile doit être alignée sur 16 octets avant un appel de fonction.

- **Valeur de retour :** Stockée dans `rax` (et `rdx` pour les grands types)[![x86 Assembly – GSN](https://gourisnair.wordpress.com/wp-content/uploads/2018/12/rax.png)




nasm -f elf64 addition3.asm -o addition3.o

