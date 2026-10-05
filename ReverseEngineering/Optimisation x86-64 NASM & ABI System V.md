
---
tags:
  - asm
  - x86-64
  - nasm
  - system-v
  - reverse-engineering
  - optimisation
date: 2026-10-01
---⚡ Optimisation x86-64 (NASM) & ABI System V

# ⚡ Optimisation x86-64 (NASM) & ABI System V

---

## 1. 🏛️ Fondamentaux & ABI System V (Linux / AMD64)

> [!tip] Ordre des registres d'arguments
> Les paramètres entiers et pointeurs sont passés dans l'ordre strict suivant :
> `rdi` $\to$ `rsi` $\to$ `rdx` $\to$ `rcx` $\to$ `r8` $\to$ `r9`

- **Valeur de retour** : Toujours dans `rax` (ou ses sous-registres `eax`, `ax`, `al` selon le type C).
- **`ret` vs `rax`** :
  - `ret` ne lit **jamais** `rax`. Il dépile uniquement l'adresse de retour (`[rsp]`) vers `rip`.
  - C'est la fonction appelante (ou le code compilé par GCC/Clang) qui va lire la valeur laissée dans `rax`.
- **Labels et Fallthrough** :
  - Un label n'est qu'un symbole d'adresse, pas une frontière d'exécution.
  - Sans instruction de rupture de flux (`jmp`, `ret`, saut conditionnel pris), le CPU exécute les instructions suivantes en séquence (*fallthrough*).
- **Pas d'opération mémoire à mémoire** :
  - `mov [rdi], [rsi]` est **invalide** en x86-64.
  - Obligation de transiter par un registre intermédiaire :
    ```nasm
    mov al, byte [rsi]
    mov byte [rdi], al
    ```

---

## 2. 🎛️ Instructions Matérielles Spécialisées (« String Instructions »)

Ces instructions exploitent les compteurs matériels et remplacent de volumineuses boucles d'itération par une poignée d'octets.

### A. `repne scasb` (Scan String Byte — ex: `strlen`)
Compare l'octet pointé par `[rdi]` avec `al`, puis incrémente `rdi` (si $DF=0$) et décrémente `rcx`. Le préfixe `repne` répète l'opération tant que `[rdi] != al` et `rcx != 0`.

```nasm
; --- Implémentation ultra-courte de strlen ---
xor eax, eax        ; al = 0 (recherche du terminateur nul '\0')
or  rcx, -1         ; rcx = -1 (0xFFFFFFFFFFFFFFFF, limite max)
repne scasb         ; Décrémente rcx jusqu'à trouver le '\0'
not rcx             ; Inverse tous les bits (rcx = distance parcourue + 1)
dec rcx             ; rcx = longueur exacte de la chaîne
````

### B. `rep stosb` (Store String Byte — ex: `memset`)

Écrit l'octet contenu dans `al` à l'adresse pointée par `[rdi]`, incrémente `rdi` et décrémente `rcx`. `rep` itère jusqu'à épuisement de `rcx`.

  

Extrait de code

```
; --- my_memset(void *rdi, int sil, size_t rdx) ---
mov rax, rdi        ; Sauvegarde de l'adresse de base pour le return
mov al, sil         ; Octet à dupliquer
mov rcx, rdx        ; Compteur n
rep stosb           ; Remplit [rdi] n fois
ret
```

### C. `cmovcc` (Conditional Move)

Déplace une valeur conditionnellement selon l'état des flags de `rflags`, **sans saut de branchement**.

  

- **Avantage** : Évite les pénalités de _branch misprediction_ et économise la déclaration de labels.
    
      
    
- **Exemple (`strrchr`)** :
    
      
    
    Extrait de code
    
    ```
    cmp byte [rdi], sil
    cmove rax, rdi      ; Mémorise l'adresse courante si le caractère correspond
    ```
    

## 3. 🔬 Miniaturisation Binaire (`size -A -d`)

Techniques d'optimisation en taille (golfing / gain d'octets) :

  

|**Intention**|**Écriture naïve**|**Écriture optimisée**|**Économie**|
|---|---|---|---|
|**Mettre à zéro**|`mov rax, 0` _(7 octets)_|`xor eax, eax` _(2 octets)_|**5 octets**|
|**Charger 1**|`mov rax, 1` _(7 octets)_|`push 1`<br><br>  <br>  <br><br>`pop rax` _(3 octets)_|**4 octets**|
|**Renvoyer -1**|`mov rax, -1` _(7 octets)_|`push -1`<br><br>  <br>  <br><br>`pop rax` _(3 octets)_|**4 octets**|
|**Tester si nul**|`cmp rax, 0` _(4 octets)_|`test rax, rax` _(3 octets)_|**1 octet**|
|**Case-insensitive**|Deux blocs de comparaison|Masque binaire via `or al, 0x20`|**$\approx 50\%$ du bloc**|

> [!note] Remarque sur l'écriture 32 bits
> 
> En x86-64, toute opération écrivant dans un registre 32 bits (ex: `eax`) remet automatiquement à zéro les 32 bits supérieurs du registre 64 bits correspondant (`rax`). C'est pourquoi `xor eax, eax` efface l'intégralité de `rax`.
> 
>   

## 4. 🛑 Syscalls & Gestion d'Errno

### Format du retour noyau

- Le numéro du syscall est spécifié dans `rax` (`0` = `sys_read`, `1` = `sys_write`, etc.).
    
      
    
- En cas d'échec, le kernel renvoie une valeur dans l'intervalle **$[-4095, -1]$** (soit $-errno$).
    
      
    
- La détection d'erreur se fait typiquement avec un test de signe :
    
      
    
    Extrait de code
    
    ```
    test rax, rax
    js   .error_handler   ; Branch si négatif (bit de signe actif)
    ```
    

### Règle d'alignement de pile (16 octets)

L'instruction `call` exige que `rsp` soit aligné sur un multiple de **16 octets** avant son exécution.

  

> [!warning] Le piège classique de `call __errno_location`
> 
>   
> 
> 1. À l'entrée d'une fonction, `call` a déjà empilé `rip` (8 octets), donc la pile est décalée : `rsp % 16 == 8`.
>     
>       
>     
> 2. Exécuter un `push rax` empile 8 octets supplémentaires, ce qui rétablit l'alignement sur **16 octets** (`rsp % 16 == 0`).
>     
>       
>     
> 3. Cela sauvegarde simultanément la valeur de l'erreur pendant que la libc utilise `rax` pour renvoyer le pointeur sur `errno`.
>     

Extrait de code:

```nasm
.error_handler:
    neg  rax                    ; rax = errno positif
    push rax                    ; 1) Aligne la pile sur 16 octets
                                ; 2) Sauvegarde la valeur d'errno
    call __errno_location wrt ..plt
    pop  rcx                    ; Récupère la valeur d'errno
    mov  [rax], ecx             ; *__errno_location() = errno
    mov  rax, -1                ; Code de retour d'échec C standard
    ret
```

