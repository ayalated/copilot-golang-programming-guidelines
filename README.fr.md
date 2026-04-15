# Règles de codage et de conception Go (pour AI / Copilot)

> Tout code généré doit respecter les règles suivantes.

---

# 1. Principes de base

* Suivre la **philosophie Unix**
* Suivre les **idiomes Go**
* Priorité : **simplicité > explicite > composition**

---

# 2. Conception de style Unix (OBLIGATOIRE)

* Une fonction = une responsabilité
* Fonctions petites et focalisées
* Préférer la **composition** aux abstractions complexes
* Le code doit être **composable**

Recommandé :

```go
func Process(r io.Reader, w io.Writer) error
```

À éviter :

```go
func ProcessFile(path string) ([]byte, error)
```

---

# 3. Interfaces

* Interfaces **petites (1–2 méthodes)**
* Définies du côté **utilisation**
* Interfaces à une méthode : suffixe `-er`

Bon :

```go
type Store interface {
    Save(User) error
}
```

Mauvais :

```go
type Service interface {
    Create()
    Update()
    Delete()
    Send()
}
```

---

# 4. Gestion des erreurs

* Toujours gérer les erreurs explicitement
* Ne jamais ignorer les erreurs
* Ajouter du contexte (wrapping)

```go
if err != nil {
    return fmt.Errorf("create user: %w", err)
}
```

---

# 5. Fonctions

* Petites et à responsabilité unique
* Recommandé : ≤ 50 lignes
* Recommandé : ≤ 3 paramètres

---

# 6. Structs

* Éviter les **God structs**
* Utiliser la composition
* Éviter `interface{}`

---

# 7. Flux de données

* Préférer `io.Reader` / `io.Writer`
* Éviter les chemins codés en dur
* Éviter l’état global

---

# 8. Concurrence

* Ne pas lancer de goroutines sans contrôle
* Toutes les goroutines doivent être annulables
* Utiliser `context.Context`

```go
func Run(ctx context.Context)
```

---

# 9. Channels

* Par défaut : non bufferisé ou taille = 1
* Éviter les orchestrations complexes

---

# 10. Temps

* Utiliser `time.Time` / `time.Duration`
* Ne pas utiliser `int` pour le temps

---

# 11. init()

* Éviter `init()` sauf nécessité

---

# 12. main()

* Garder `main()` minimal
* Pas de logique métier

---

# 13. Logs

* Utiliser des logs structurés (ex: slog)
* Ne pas utiliser `fmt.Println`

---

# 14. JSON / API

* Structs explicites
* Éviter `map[string]interface{}`

---

# 15. Sécurité des données (pratique Uber)

* Ne pas exposer des données mutables internes
* Copier aux frontières si nécessaire

```go
return append([]T(nil), data...)
```

---

# 16. Règles générales

* Pas d’état global mutable
* Pas d’effets de bord implicites
* Préférer l’explicite
* Priorité à la lisibilité

---

# 17. Contraintes Copilot

Lors de la génération :

* Utiliser de petites interfaces
* Éviter les abstractions lourdes
* Éviter les couches inutiles
* Éviter le code inutilisé
* Éviter le sur-design
* Suivre les idiomes Go
* Favoriser la composition

---

# 18. Principe final

> Écrire un code composable, testable et compréhensible.
