# [REGEX](https://regexr.com/)

sont entourées de `/` et terminent par :

- `g` -> global (conserve l'index de la dernière correspondance, sans quoi toutes les recherches renvoient la même correspondance),
- `i` -> insensible à la casse,
- `m` -> multiligne (`^` et `$` s'appliquent à la ligne entière),
- `s` -> 'dotall' (le `.` correspondra à n'importe quel caractère y compris la nouvelle ligne),
- `u` -> unicode (),
- `y` -> sticky (),

```js
/           /g
```

## classes de caractères

- . -> tout caractère (chiffres, lettres, espace, caractères spéciaux)
- [abc] -> a OU b OU c
- [^abc] -> NI a, NI b, NI c
- [a-f] -> a OU b OU c OU d OU e OU f
- \w -> tout caractère de 'mot' (chiffres, lettres, PAS espace, PAS caractères spéciaux)
- \d -> tout caractère de 'nombre' (chiffres, PAS lettres, PAS espace, PAS caractères spéciaux)
- \s -> le caractère 'espace'
- \W -> qui n'est pas \w (PAS chiffres, PAS lettres, espace, caractères spéciaux)
- \D -> qui n'est pas \d (PAS chiffres, lettres, espace, caractères spéciaux)
- \S -> qui n'est pas \s (chiffres, lettres, PAS espace, caractères spéciaux)
- \t -> tabulation
- \n -> saut de ligne
- \r -> retour chariot

## tous les chiffres

```js
[0-9]
```

OU

```js
\d
```

## tous les chiffres sauf 3, 4, 5, 6 et 7

```js
[0-28-9]
```

## toutes les lettres minuscules

```js
[a-z]
```

## toutes les lettres majuscules

```js
[A-Z]
```

## toutes les lettres

```js
[a-zA-Z]
```

## contraire : commence par le symbole ^

ex: les chiffres 3, 4, 5, 6 et 7

```js
[^0-28-9]
```

## Ancres

- ^abc -> commence par `abc`
- abc$ -> fini par `abc`
- \b -> limite de mot (\btoto\b)
- \B -> limite de non mot (\B-\B)

## nombre d'occurence

- ? -> 0 ou 1 fois (notion d'optionnel) : colou`?`r -> `color`, `colour`
- ? -> rend le quantificateur précédent paresseux (le moins de caractère possible) : b\w+`?` -> b, `be`, `be`e, `be`er
- \* -> 0 ou n fois
- \+ -> plus d'1 fois
- {2} -> exactement `2` fois
- {2,} -> `2` fois ou plus
- {2,3} -> `2` à `3` fois
- | -> le OU : b(a`|`e`|`i)d -> `bad`, bod, `bed`

## caractères spéciaux : mettre un `\` devant

- \-
- /
- [
- ]
- \
- ^
- ?
- \*

## groupement

- (abc) -> entourent un groupement référencé par `#1` : (ba)+ -> `baba`lle, `ba`llon, `ba`o`ba`b
- (?:abc) -> entourent un groupement `NON` référencé par `#1`
- (?<nom>abc) -> entourent un groupement nommé `nom`
- \1 -> fait référence au groupement `#1` : (\w)a`\1` -> `bab`alle, bao`bab`   |   (\w+)o`\1` -> `baoba`b

## lookaround

- (?=abc) -> positive lookahead (matche un groupe contenant l'expression recherchée après sans l'inclure dans le résultat) : \d(?`=`px) -> 1pt `2`px 3em `4`px
- (?!abc) -> negative lookahead (matche un groupe `NE` contenant `PAS` l'expression recherchée après sans l'inclure dans le résultat) : \d(?`!`px) -> `1`pt 2px `3`em 4px
- (?<=abc) -> positive lookbehind (matche un groupe contenant l'expression recherchée avant sans l'inclure dans le résultat) : (?`<=`étape) -> étape`1` étape`2` etape3 étap4
- (?<!abc) -> negative lookbehind (matche un groupe `NE` contenant `PAS` l'expression recherchée avant sans l'inclure dans le résultat) : (?`<!`étape) -> étape1 étape2 etape`3` étap`4`

## substitution

- $& -> insert le résultat
- $1 -> insert le groupe
- $` -> insert ce qu'il y a avant le résultat
- $' -> insert ce qu'il y a après le résultat
- $$ -> insert le dollar

### [objet RegExp](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Objets_globaux/RegExp/n)

```js
var re = /(\w+)\s(\w+)/;
var str = 'Jean Biche';
str.replace(re, '$2, $1'); // "Biche, Jean"
RegExp.$1; // "Jean"
RegExp.$2; // "Biche"
```