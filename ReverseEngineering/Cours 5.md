
github corkami elf64 à avoir pour la dissection

gcc -fdump-tree-all-graph -fdump-ipa-all-graph -fdump-rtl-all-graph -c max.c -o max.o

dot -Tpdf max.c.*.cfg.dot -o max_cfg.pdf

dans gef:

info function

disas /r $rip,0x100 : démarre à ladresse

crackme5 : intruction overlapping

echo $'\xaa\xbb\x01' | hexdump -Cv