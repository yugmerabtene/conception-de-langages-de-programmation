# Cours — Conception de langage

## Introduction

La conception de langage consiste à créer un langage de programmation en définissant :

* sa syntaxe ;
* ses règles ;
* sa manière d’être exécuté ;
* ses types de données ;
* ses mots-clés ;
* ses structures de contrôle ;
* son modèle mémoire ;
* ses outils d’analyse.

Quand un développeur écrit un programme, la machine ne comprend pas directement le texte. Le code suit un chemin précis :

```txt
Code source
   ↓
Lexer
   ↓
Tokens
   ↓
Parser
   ↓
Arbre syntaxique
   ↓
Compilateur ou Interpréteur
   ↓
Exécution
```

Ce cours explique comment concevoir un langage en partant de cette chaîne.

---

# Chapitre 1 — Qu’est-ce qu’un langage de programmation ?

## 1.1 Définition

Un langage de programmation est un système de règles permettant d’écrire des instructions compréhensibles par un outil logiciel, qui va ensuite les transformer en actions exécutables.

Un langage n’est pas seulement une syntaxe. Il contient aussi :

* un vocabulaire ;
* une grammaire ;
* une sémantique ;
* un système de types ;
* une stratégie d’exécution.

## 1.2 Exemples réels

### Exemple en Java

```java
int age = 30;
System.out.println(age);
```

### Exemple en Python

```python
age = 30
print(age)
```

Ces deux langages permettent d’exprimer la même idée, mais avec des règles différentes.

## 1.3 Les grandes questions de la conception de langage

Quand on crée un langage, on doit se poser les questions suivantes :

### Syntaxe

Comment l’utilisateur écrit-il son code ?

### Sémantique

Que signifie réellement chaque instruction ?

### Types

Quelles valeurs sont autorisées ?

### Exécution

Le langage sera-t-il interprété ou compilé ?

### Lisibilité

Le langage doit-il être simple, strict, permissif, minimaliste ?

### Domaine

Le langage est-il généraliste ou spécialisé ?

---

# Chapitre 2 — Les grandes familles de langages

## 2.1 Langages généralistes

Ils servent à développer presque tout type d’application.

Exemples :

* C
* Java
* Python
* Rust
* Go

## 2.2 Langages spécialisés

Ils sont conçus pour un besoin précis.

Exemples :

* SQL pour les bases de données
* HTML pour la structure de page
* CSS pour la mise en forme
* Bash pour les scripts système

## 2.3 Langages impératifs

Le programme donne des ordres étape par étape.

Exemple :

```java
int x = 5;
x = 8;
```

## 2.4 Langages orientés objet

Le code est structuré autour d’objets, de classes, d’attributs et de méthodes.

Exemple :

```java
class Utilisateur {
    String nom;

    void afficherNom() {
        System.out.println(nom);
    }
}
```

## 2.5 Langages fonctionnels

Le langage favorise les fonctions pures et la transformation de données.

Exemples :

* Haskell
* OCaml
* F#

---

# Chapitre 3 — Le cycle de vie d’un programme

## 3.1 Vue d’ensemble

Le cœur de la conception de langage repose sur cette chaîne :

```txt
Code source
   ↓
Lexer
   ↓
Tokens
   ↓
Parser
   ↓
Arbre syntaxique
   ↓
Compilateur ou Interpréteur
   ↓
Exécution
```

## 3.2 Le code source

Le code source est le texte écrit par le développeur.

Exemple :

```txt
let age = 30;
print(age);
```

## 3.3 Le lexer

Le lexer lit le texte caractère par caractère et le transforme en unités appelées **tokens**.

## 3.4 Les tokens

Les tokens sont des éléments classés :

* mot-clé ;
* identifiant ;
* nombre ;
* opérateur ;
* séparateur ;
* parenthèse.

## 3.5 Le parser

Le parser vérifie que les tokens suivent les règles de grammaire du langage.

## 3.6 L’arbre syntaxique

Le parser construit souvent un arbre représentant la structure logique du code.

## 3.7 Compilateur ou interpréteur

Le programme est ensuite :

* soit exécuté par un interpréteur ;
* soit transformé par un compilateur.

## 3.8 L’exécution

Le programme produit enfin un résultat observable.

---

# Chapitre 4 — Concevoir la syntaxe d’un langage

## 4.1 Pourquoi la syntaxe est essentielle

La syntaxe est ce que l’utilisateur voit en premier. Un langage peut être puissant, mais s’il est illisible ou incohérent, il sera difficile à utiliser.

## 4.2 Exemples de styles syntaxiques

### Style proche du C

```txt
if (age > 18) {
    print("ok");
}
```

### Style proche de Python

```txt
if age > 18:
    print("ok")
```

### Style très minimaliste

```txt
if age > 18 then print "ok"
```

## 4.3 Questions de conception

Quand vous concevez la syntaxe, vous devez trancher :

* les blocs utilisent-ils des accolades ?
* les instructions se terminent-elles par `;` ?
* les types sont-ils obligatoires ?
* les noms de méthodes suivent-ils une convention particulière ?
* les mots-clés sont-ils en anglais ?

## 4.4 Bonnes pratiques

Une bonne syntaxe doit être :

* cohérente ;
* lisible ;
* prévisible ;
* régulière ;
* facile à parser.

---

# Chapitre 5 — L’analyse lexicale

## 5.1 Rôle du lexer

Le lexer transforme le code brut en tokens.

Exemple de code source :

```txt
let total = 42;
```

Sortie du lexer :

```txt
KEYWORD(let)
IDENTIFIER(total)
ASSIGN(=)
NUMBER(42)
SEMICOLON(;)
```

## 5.2 Ce que reconnaît un lexer

Un lexer reconnaît généralement :

* les mots-clés ;
* les identifiants ;
* les nombres ;
* les chaînes ;
* les opérateurs ;
* les symboles ;
* les commentaires ;
* les espaces à ignorer.

## 5.3 Exemple réel

Code :

```txt
print("Bonjour");
```

Tokens :

```txt
IDENTIFIER(print)
LPAREN
STRING("Bonjour")
RPAREN
SEMICOLON
```

## 5.4 Erreurs lexicales

Le lexer peut signaler des erreurs comme :

* caractère interdit ;
* chaîne non fermée ;
* nombre mal formé.

Exemple :

```txt
print("Bonjour);
```

Erreur possible :

* chaîne non terminée.

## 5.5 Exemple simple de pseudo-code de lexer

```java
while (position < source.length()) {
    char courant = source.charAt(position);

    if (Character.isWhitespace(courant)) {
        position++;
    } else if (Character.isDigit(courant)) {
        lireNombre();
    } else if (Character.isLetter(courant)) {
        lireIdentifiantOuMotCle();
    } else if (courant == '=') {
        ajouterToken("ASSIGN", "=");
        position++;
    } else if (courant == ';') {
        ajouterToken("SEMICOLON", ";");
        position++;
    } else {
        erreur("Caractère invalide : " + courant);
    }
}
```

### Explication

Ce code illustre le principe :

* lire un caractère ;
* décider de sa catégorie ;
* produire un token ;
* avancer dans la source.

---

# Chapitre 6 — Les tokens

## 6.1 Définition

Un token est une unité reconnue par le lexer.

## 6.2 Catégories fréquentes

### Mots-clés

Exemples :

* `if`
* `else`
* `while`
* `let`
* `return`

### Identifiants

Exemples :

* `age`
* `total`
* `calculerMontant`

### Littéraux

Exemples :

* `42`
* `"Bonjour"`
* `true`

### Opérateurs

Exemples :

* `=`
* `>`
* `<`
* `*`
* `/`

### Délimiteurs

Exemples :

* `(`
* `)`
* `{`
* `}`
* `;`

## 6.3 Exemple complet

Code :

```txt
if (age > 18) { print("ok"); }
```

Tokens :

```txt
KEYWORD(if)
LPAREN
IDENTIFIER(age)
GREATER_THAN
NUMBER(18)
RPAREN
LBRACE
IDENTIFIER(print)
LPAREN
STRING("ok")
RPAREN
SEMICOLON
RBRACE
```

---

# Chapitre 7 — L’analyse syntaxique

## 7.1 Rôle du parser

Le parser reçoit les tokens et vérifie qu’ils respectent les règles du langage.

## 7.2 Ce que fait réellement le parser

Le parser ne se contente pas de lire une liste de tokens. Il détermine la structure de l’instruction.

Exemple :

```txt
let total = 42;
```

Il comprend qu’il s’agit :

* d’une déclaration ;
* d’une affectation ;
* avec un identifiant ;
* et une valeur numérique.

## 7.3 Exemple d’erreur syntaxique

Code :

```txt
let = total 42;
```

Le lexer peut produire des tokens valides, mais leur ordre est incorrect. Le parser rejettera l’instruction.

## 7.4 Grammaire simplifiée

Exemple de grammaire :

```txt
declaration   → "let" IDENTIFIER "=" expression ";"
expression    → NUMBER | IDENTIFIER | STRING
```

Cette règle signifie :

* une déclaration commence par `let` ;
* suivie d’un identifiant ;
* puis `=` ;
* puis d’une expression ;
* puis `;`.

## 7.5 Exemple réel

Code :

```txt
let nom = "Yug";
```

Le parser valide cette structure si votre grammaire autorise :

* mot-clé ;
* identifiant ;
* affectation ;
* chaîne ;
* point-virgule.

---

# Chapitre 8 — L’arbre syntaxique abstrait

## 8.1 Définition

L’arbre syntaxique abstrait, souvent appelé AST, représente la structure logique du code.

## 8.2 Exemple simple

Code :

```txt
let age = 30;
```

AST possible :

```txt
Declaration
├── nom : age
└── valeur : NumberLiteral(30)
```

## 8.3 Exemple avec condition

Code :

```txt
if (age > 18) {
    print("majeur");
}
```

AST simplifié :

```txt
IfStatement
├── condition
│   └── GreaterThan(age, 18)
└── body
    └── Print("majeur")
```

## 8.4 Pourquoi l’AST est central

L’AST sert à :

* valider la structure ;
* vérifier les types ;
* interpréter le code ;
* générer du code machine ;
* optimiser le programme.

---

# Chapitre 9 — La sémantique

## 9.1 Différence entre syntaxe et sémantique

### Syntaxe

Répond à la question :
est-ce bien écrit ?

### Sémantique

Répond à la question :
est-ce que cela a un sens ?

## 9.2 Exemple

Code :

```txt
let age = "bonjour";
age = true;
```

La syntaxe peut être correcte, mais si le langage impose un type strict, la seconde affectation peut être interdite.

## 9.3 Vérifications sémantiques classiques

* variable déclarée avant usage ;
* type compatible ;
* méthode existante ;
* nombre d’arguments correct ;
* retour présent dans une méthode qui en exige un.

## 9.4 Exemple réel

Code :

```txt
let nom = "Yug";
print(age);
```

La syntaxe peut être correcte. Mais si `age` n’a jamais été déclaré, c’est une erreur sémantique.

---

# Chapitre 10 — Le système de types

## 10.1 Pourquoi il faut des types

Les types permettent de contrôler la nature des données manipulées.

Exemples :

* entier ;
* chaîne ;
* booléen ;
* nombre décimal ;
* tableau.

## 10.2 Typage statique

Le type est connu avant l’exécution.

Exemple :

```java
int age = 30;
```

## 10.3 Typage dynamique

Le type est géré pendant l’exécution.

Exemple :

```python
age = 30
```

## 10.4 Typage fort et typage faible

### Typage fort

Le langage impose davantage de cohérence.

### Typage faible

Le langage autorise plus de conversions implicites.

## 10.5 Question de conception

Votre langage doit-il être :

* strict ;
* souple ;
* hybride ;
* pédagogique ;
* orienté performance ?

---

# Chapitre 11 — Interpréteur ou compilateur

## 11.1 L’interpréteur

L’interpréteur lit l’AST et exécute directement les instructions.

Exemple :

```txt
let age = 30;
print(age);
```

L’interpréteur :

* crée une variable `age` ;
* stocke `30` ;
* appelle `print`.

## 11.2 Le compilateur

Le compilateur transforme le programme en une autre forme :

* bytecode ;
* code machine ;
* langage intermédiaire.

## 11.3 Comparaison pédagogique

### Interpréteur

Avantages :

* plus simple à prototyper ;
* pratique pour créer un langage rapidement ;
* utile pour le débogage.

### Compilateur

Avantages :

* meilleures performances ;
* meilleure optimisation ;
* déploiement plus proche du système.

## 11.4 Bon choix pour un premier langage

Pour un premier projet de conception de langage, il est souvent plus réaliste de commencer par un **interpréteur**.

---

# Chapitre 12 — Le runtime

## 12.1 Définition

Le runtime est l’environnement d’exécution du langage.

Il gère souvent :

* la mémoire ;
* les variables ;
* les appels de méthodes ;
* les objets ;
* les erreurs ;
* les conversions ;
* la pile d’exécution.

## 12.2 Exemple simple

Quand vous exécutez :

```txt
let age = 30;
print(age);
```

Le runtime doit :

* créer l’environnement courant ;
* enregistrer la variable `age` ;
* stocker la valeur `30` ;
* récupérer `age` pour l’affichage.

## 12.3 Éléments du runtime

* environnement global ;
* table des symboles ;
* pile d’appels ;
* gestion des objets ;
* exceptions.

---

# Chapitre 13 — Concevoir un mini langage

Nous allons imaginer un petit langage nommé **Nova**.

## 13.1 Objectif

Nova doit permettre :

* de déclarer des variables ;
* d’afficher des valeurs ;
* de faire des conditions ;
* de définir des méthodes simples.

## 13.2 Exemples de code Nova

```txt
let nom = "Yug";
let age = 30;

if (age > 18) {
    print("majeur");
}
```

## 13.3 Mots-clés

Mots-clés choisis :

* `let`
* `if`
* `else`
* `while`
* `fn`
* `return`
* `print`

## 13.4 Types de base

* `int`
* `string`
* `bool`

## 13.5 Structures autorisées

* déclaration ;
* affectation ;
* appel de méthode ;
* condition ;
* boucle ;
* retour.

---

# Chapitre 14 — Définir la grammaire de Nova

## 14.1 Règles de base

```txt
programme       → instruction*
instruction     → declaration
                | printStatement
                | ifStatement

declaration     → "let" IDENTIFIER "=" expression ";"
printStatement  → "print" "(" expression ")" ";"
ifStatement     → "if" "(" expression ")" bloc

bloc            → "{" instruction* "}"
expression      → NUMBER
                | STRING
                | IDENTIFIER
```

## 14.2 Lecture pédagogique

Cette grammaire dit :

* un programme contient plusieurs instructions ;
* une instruction peut être une déclaration, un affichage ou une condition ;
* une déclaration suit une structure fixe ;
* un bloc contient plusieurs instructions.

---

# Chapitre 15 — Exemple complet du trajet du code

Prenons ce programme Nova :

```txt
let age = 30;
print(age);
```

## 15.1 Code source

```txt
let age = 30;
print(age);
```

## 15.2 Lexer

Sortie :

```txt
KEYWORD(let)
IDENTIFIER(age)
ASSIGN(=)
NUMBER(30)
SEMICOLON
IDENTIFIER(print)
LPAREN
IDENTIFIER(age)
RPAREN
SEMICOLON
```

## 15.3 Parser

Le parser reconnaît :

* une déclaration ;
* un appel à `print`.

## 15.4 AST

```txt
Program
├── LetDeclaration(name=age, value=30)
└── PrintStatement(value=Identifier(age))
```

## 15.5 Interprétation

L’interpréteur :

1. crée `age` ;
2. assigne `30` ;
3. lit `age` ;
4. affiche `30`.

## 15.6 Exécution

Résultat :

```txt
30
```

---

# Chapitre 16 — Représentation objet de l’AST

Si vous développez l’outil en Java, vous pouvez modéliser l’AST avec des classes.

## 16.1 Exemple simple en Java

```java
abstract class Node {
}

abstract class Statement extends Node {
}

abstract class Expression extends Node {
}

class LetDeclaration extends Statement {
    String name;
    Expression value;

    LetDeclaration(String name, Expression value) {
        this.name = name;
        this.value = value;
    }
}

class PrintStatement extends Statement {
    Expression value;

    PrintStatement(Expression value) {
        this.value = value;
    }
}

class NumberLiteral extends Expression {
    int value;

    NumberLiteral(int value) {
        this.value = value;
    }
}

class Identifier extends Expression {
    String name;

    Identifier(String name) {
        this.name = name;
    }
}
```

### Explication

Ici :

* `Node` est la base ;
* `Statement` représente une instruction ;
* `Expression` représente une valeur ou une expression ;
* `LetDeclaration` stocke un nom et une expression ;
* `PrintStatement` stocke une expression à afficher.

---

# Chapitre 17 — Interpréter l’AST

## 17.1 Principe

L’interpréteur parcourt l’AST et exécute les nœuds.

## 17.2 Exemple conceptuel

```java
Map<String, Object> variables = new HashMap<>();

void execute(Statement statement) {
    if (statement instanceof LetDeclaration decl) {
        Object value = eval(decl.value);
        variables.put(decl.name, value);
    } else if (statement instanceof PrintStatement print) {
        Object value = eval(print.value);
        System.out.println(value);
    }
}

Object eval(Expression expression) {
    if (expression instanceof NumberLiteral number) {
        return number.value;
    } else if (expression instanceof Identifier id) {
        return variables.get(id.name);
    }
    throw new RuntimeException("Expression inconnue");
}
```

### Explication

Ce code montre un mini interpréteur :

* `variables` stocke l’état du programme ;
* `execute` traite les instructions ;
* `eval` calcule une expression.

---

# Chapitre 18 — Ajouter des conditions

## 18.1 Nouveau code Nova

```txt
let age = 30;

if (age > 18) {
    print("majeur");
}
```

## 18.2 Nouvelles règles

Le langage doit maintenant gérer :

* les comparaisons ;
* les conditions ;
* les blocs.

## 18.3 Extension de la grammaire

```txt
ifStatement   → "if" "(" condition ")" bloc
condition     → expression ">" expression
```

## 18.4 AST possible

```txt
IfStatement
├── condition : GreaterThan(Identifier(age), NumberLiteral(18))
└── body : Block(PrintStatement("majeur"))
```

## 18.5 Sémantique

L’interpréteur :

* évalue la condition ;
* si elle est vraie, il exécute le bloc ;
* sinon, il l’ignore.

---

# Chapitre 19 — Ajouter des méthodes

## 19.1 Exemple de syntaxe

```txt
fn saluer(nom) {
    print(nom);
}
```

## 19.2 Problèmes à résoudre

Pour ajouter les méthodes, il faut gérer :

* la déclaration de méthode ;
* les paramètres ;
* l’appel ;
* la portée locale ;
* le retour.

## 19.3 Exemple de grammaire

```txt
functionDecl   → "fn" IDENTIFIER "(" parameters ")" bloc
parameters     → IDENTIFIER ("," IDENTIFIER)* | ε
callExpression → IDENTIFIER "(" arguments ")"
arguments      → expression ("," expression)* | ε
```

## 19.4 Impact sur le runtime

Le runtime doit maintenant gérer :

* la pile d’appels ;
* les variables locales ;
* les paramètres ;
* le retour de méthode.

---

# Chapitre 20 — Gestion des erreurs

## 20.1 Trois grandes familles d’erreurs

### Erreurs lexicales

Caractère non reconnu.

### Erreurs syntaxiques

Structure invalide.

### Erreurs sémantiques

Code bien écrit mais incorrect dans son sens.

## 20.2 Exemples

### Erreur lexicale

```txt
let nom = @;
```

### Erreur syntaxique

```txt
let = nom 42;
```

### Erreur sémantique

```txt
print(age);
```

si `age` n’existe pas.

## 20.3 Bonnes pratiques de conception

Un bon langage doit produire des messages d’erreur :

* précis ;
* compréhensibles ;
* localisés ;
* utiles pour corriger.

---

# Chapitre 21 — Compilation vers une représentation intermédiaire

## 21.1 Pourquoi une représentation intermédiaire

Au lieu d’exécuter directement l’AST, on peut le convertir vers une forme intermédiaire, par exemple :

* bytecode ;
* instructions internes ;
* arbre optimisé.

## 21.2 Exemple simple

Code source :

```txt
let age = 30;
print(age);
```

Instructions intermédiaires :

```txt
LOAD_CONST 30
STORE age
LOAD_VAR age
PRINT
```

## 21.3 Intérêt

Cette approche permet :

* d’optimiser ;
* de séparer analyse et exécution ;
* de créer une machine virtuelle.

---

# Chapitre 22 — Choix d’architecture pour créer un langage

## 22.1 Architecture simple

Pour un premier projet :

* lexer ;
* parser ;
* AST ;
* interpréteur ;
* environnement.

## 22.2 Architecture plus avancée

Pour un projet plus ambitieux :

* lexer ;
* parser ;
* AST ;
* analyse sémantique ;
* IR ;
* optimiseur ;
* génération de code ;
* runtime ;
* VM ou backend natif.

## 22.3 Choix réaliste pour un étudiant

Pour un premier langage :

1. mini syntaxe ;
2. petit lexer ;
3. parser récursif descendant ;
4. AST simple ;
5. interpréteur minimal ;
6. quelques types ;
7. gestion d’erreurs propre.

---

# Chapitre 23 — Méthodologie pour concevoir un langage

## Étape 1 — Définir l’objectif

Exemples :

* langage pédagogique ;
* langage de script ;
* langage système ;
* DSL métier.

## Étape 2 — Définir les fonctionnalités minimales

Exemples :

* variables ;
* conditions ;
* boucles ;
* méthodes ;
* types.

## Étape 3 — Définir la syntaxe

Choisir :

* mots-clés ;
* ponctuation ;
* blocs ;
* appels ;
* déclarations.

## Étape 4 — Définir la grammaire

Écrire des règles formelles.

## Étape 5 — Développer le lexer

Transformer le texte en tokens.

## Étape 6 — Développer le parser

Construire l’AST.

## Étape 7 — Ajouter les vérifications sémantiques

Variables, types, portée, cohérence.

## Étape 8 — Développer l’exécution

Interpréteur ou compilateur.

## Étape 9 — Ajouter les erreurs

Messages lisibles et précis.

## Étape 10 — Tester avec de vrais programmes

Cas simples, erreurs, cas limites.

---

# Chapitre 24 — Exemple de mini progression pédagogique de projet

## Niveau 1

Le langage supporte :

* nombres ;
* chaînes ;
* déclarations ;
* affichage.

## Niveau 2

Le langage supporte :

* comparaisons ;
* conditions ;
* blocs.

## Niveau 3

Le langage supporte :

* boucles ;
* méthodes ;
* retour.

## Niveau 4

Le langage supporte :

* typage ;
* portée ;
* analyse sémantique.

## Niveau 5

Le langage supporte :

* bytecode ;
* VM ;
* optimisations simples.

---

# Chapitre 25 — Ce qu’il faut retenir

## 25.1 Les concepts fondamentaux

La conception de langage repose sur plusieurs piliers :

* syntaxe ;
* grammaire ;
* lexer ;
* tokens ;
* parser ;
* AST ;
* sémantique ;
* types ;
* runtime ;
* exécution.

## 25.2 Le trajet complet du code

```txt
Code source
   ↓
Lexer
   ↓
Tokens
   ↓
Parser
   ↓
Arbre syntaxique
   ↓
Compilateur ou Interpréteur
   ↓
Exécution
```

## 25.3 La logique pédagogique

* le développeur écrit du texte ;
* le lexer le découpe ;
* le parser lui donne une structure ;
* l’AST le représente proprement ;
* l’interpréteur ou le compilateur le transforme en action ;
* le runtime gère l’exécution réelle.

---

# Conclusion

Concevoir un langage, ce n’est pas seulement inventer une syntaxe élégante. C’est construire un système complet qui doit :

* être cohérent ;
* être compréhensible ;
* être analysable ;
* être exécutable ;
* produire des erreurs utiles ;
* rester maintenable.

Le chemin naturel pour apprendre est le suivant :

1. comprendre le rôle du lexer ;
2. comprendre le rôle du parser ;
3. construire un AST ;
4. écrire un mini interpréteur ;
5. ajouter les types ;
6. ajouter l’analyse sémantique ;
7. évoluer ensuite vers la compilation.

---

# Proposition de plan de cours court

## Chapitre 1 — Introduction à la conception de langage

* Définition
* Objectifs
* Types de langages

## Chapitre 2 — Le trajet du code

* Code source
* Lexer
* Tokens
* Parser
* AST
* Exécution

## Chapitre 3 — Conception de la syntaxe

* Lisibilité
* Cohérence
* Grammaire

## Chapitre 4 — Analyse lexicale

* Rôle du lexer
* Catégories de tokens
* Erreurs lexicales

## Chapitre 5 — Analyse syntaxique

* Rôle du parser
* Grammaire
* Construction de l’AST

## Chapitre 6 — Sémantique et types

* Vérifications
* Portée
* Système de types

## Chapitre 7 — Interprétation et compilation

* Interpréteur
* Compilateur
* Runtime

## Chapitre 8 — Projet de mini langage

* Variables
* Affichage
* Conditions
* Méthodes

---

# Exercices proposés

## Exercice 1

À partir du code suivant, écrire la liste des tokens :

```txt
let ville = "Paris";
print(ville);
```

## Exercice 2

Dire si les codes suivants contiennent une erreur lexicale, syntaxique ou sémantique.

## Exercice 3

Dessiner un AST simple pour :

```txt
let age = 30;
```

## Exercice 4

Proposer trois mots-clés pour un mini langage et expliquer leur rôle.

## Exercice 5

Concevoir la grammaire minimale d’un langage qui permet :

* déclaration ;
* affichage ;
* condition.

