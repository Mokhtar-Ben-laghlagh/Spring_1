# Injection de Dépendances en Java — IoC

## 📁 Structure du projet

```
src/
├── dao/
│   ├── IDao.java           ← Interface DAO
│   ├── DaoImpl.java        ← Implémentation 1 (retourne 100.0)
│   └── DaoImpl2.java       ← Implémentation 2 (retourne 2005)
├── metier/
│   ├── IMetier.java        ← Interface Métier
│   ├── MetierImpl.java     ← Implémentation 1 (valeur × 2)
│   └── MetierImpl2.java    ← Implémentation 2 (valeur × 5)
└── presentation/
    └── Presentation2.java  ← Point d'entrée (injection par réflexion)

config.txt                  ← Fichier de configuration des classes
```

---

## ⚙️ Fichier `config.txt`

Le fichier de configuration indique quelles classes utiliser (DAO puis Métier, par paires) :


dao.DaoImpl
metier.MetierImpl
dao.DaoImpl2
metier.MetierImpl2



## 🚀 Lancer le projet

### Compiler
```bash
javac -d out src/dao/*.java src/metier/*.java src/presentation/*.java
```

### Exécuter
```bash
java -cp out presentation.Presentation2
```

> ⚠️ Le fichier `config.txt` doit être à la **racine du projet** (là où tu lances la commande).

---

## 📝 Résultats attendus

| DAO        | Valeur | Métier        | Calcul | Résultat    |
|------------|--------|---------------|--------|-------------|
| `DaoImpl`  | 100.0  | `MetierImpl`  | × 2    | **200.0**   |
| `DaoImpl2` | 2005.0 | `MetierImpl2` | × 5    | **10025.0** |

---

## 📚 Concepts clés

| Concept                      | Description                                                            |
|------------------------------|------------------------------------------------------------------------|
| **Interface**                | `IDao` et `IMetier` définissent les contrats                           |
| **Injection de dépendances** | `setDao()` injecte l'implémentation dans le métier                     |
| **Réflexion Java**           | `Class.forName()` charge les classes dynamiquement depuis `config.txt` |
| **IoC**                      | Le fichier de config contrôle quelles classes sont utilisées           |

---

## 🔄 Comment ça fonctionne


config.txt
    ↓
Class.forName("dao.DaoImpl")        → crée une instance de DaoImpl

## Resultat d'execution :

<img width="538" height="272" alt="Capture d&#39;écran 2026-03-09 140849" src="https://github.com/user-attachments/assets/98ab7c65-7fb8-4d6e-aef0-f6750af6c237" />


Class.forName("metier.MetierImpl")  → crée une instance de MetierImpl
setDao(dao)                         → injecte dao dans metier
metier.calcul()                     → dao.getValue() × 2 = 200.0
