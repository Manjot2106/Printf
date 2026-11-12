# ft_printf

*Projet réalisé dans le cadre du cursus 42 par msingh.*

Une réimplémentation de `printf` de la libc, livrée sous forme de bibliothèque statique `libftprintf.a`.

```c
int	ft_printf(const char *format, ...);
```

La fonction renvoie le nombre de caractères écrits, comme `printf`.

## Conversions gérées

| Conversion | Affiche |
|---|---|
| `%c` | un caractère |
| `%s` | une chaîne (`(null)` si le pointeur est `NULL`) |
| `%p` | une adresse en hexadécimal préfixée par `0x` (`(nil)` pour `NULL`) |
| `%d` / `%i` | un entier signé en base 10 |
| `%u` | un entier non signé en base 10 |
| `%x` / `%X` | un entier non signé en hexadécimal minuscule / majuscule |
| `%%` | le caractère `%` |

## Compilation

```bash
make          # produit libftprintf.a
make clean
make fclean
make re
```

```bash
cc main.c -L. -lftprintf
```

## Fonctionnement

- **`ft_printf`** (`ft_printf.c`) parcourt la chaîne de format. Les caractères normaux sont écrits tels quels. À chaque `%`, le caractère suivant est envoyé à `ft_checkformat`, qui lit l'argument suivant avec `va_arg` et appelle l'afficheur correspondant. Chaque afficheur renvoie le nombre de caractères écrits, et ces valeurs sont additionnées.
- **`ft_printf_char.c`** : `ft_putchar`, `ft_putstr`, `ft_strlen`.
- **`ft_printf_nbr.c`** : `ft_putnbr` (gère `INT_MIN`), `ft_unsignednbr`, `ft_puthex`. L'affichage est récursif : on affiche d'abord `n / base`, puis le chiffre `n % base`.
- **`ft_putptr.c`** : `ft_print_ptr` convertit l'adresse en `unsigned long` et l'affiche en hexadécimal après `0x`.

Un `%` isolé en fin de chaîne est affiché tel quel, sans lire au-delà de la fin du format.

## Tests

Sortie et valeur de retour comparées à `printf` de la libc : `INT_MIN`, `INT_MAX`, `UINT_MAX`, `0`, chaîne vide, `NULL` pour `%s` et `%p`, `ULONG_MAX` pour `%p`, caractère `\0` avec `%c`, `%%` multiples. Tous les résultats sont identiques et valgrind ne signale aucune erreur.
