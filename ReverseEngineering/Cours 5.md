 
github corkami elf64 à avoir pour la dissection

gcc -fdump-tree-all-graph -fdump-ipa-all-graph -fdump-rtl-all-graph -c max.c -o max.o

dot -Tpdf max.c.*.cfg.dot -o max_cfg.pdf

dans gef:

info function

disas /r $rip,0x100 : démarre à ladresse

crackme5 : intruction overlapping

echo $'\xaa\xbb\x01' | hexdump -Cv



| **Paramètre de la fonction C** | **Registre assigné** |
| ------------------------------ | -------------------- |
| **1ᵉʳ argument**               | **`rdi`**            |
| 2ᵉ argument                    | `rsi`                |
| 3ᵉ argument                    | `rdx`                |
| 4ᵉ argument                    | `rcx`                |
| 5ᵉ argument                    | `r8`                 |
| 6ᵉ argument                    | `r9`                 |

droit en rw sur la stack

Se renseigner sur l'attack ret to func


commande gef :
checksec: