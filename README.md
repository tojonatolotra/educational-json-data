# AngC — Content Delivery Store (JSON)

Ce dépôt sert de stockage statique et de CDN pour les leçons d'anglais générées au format JSON par le projet **AngC**. Ces leçons sont structurées par niveau de difficulté et ordonnées de manière séquentielle pour être facilement consommées par l'application mobile.

> [!NOTE]
> Pour des raisons de protection des droits d'auteur, les noms des fichiers et les métadonnées publiques ne contiennent pas les titres originaux des vidéos. Ils utilisent un index séquentiel suivi d'un identifiant anonymisé (hash/UUID).

---

## 📂 Structure du dépôt

Les leçons d'anglais sont placées dans le dossier `english/` puis classées par niveau (`beginner`, `intermediate`, `advanced`). 

Les fichiers JSON sont nommés sous la forme `xxx_hash.json` pour garantir un ordre de progression chronologique sans exposer publiquement de termes sous droits d'auteur :

```
.
└── english/                   # 🇬🇧 Leçons d'Anglais (Projet AngC)
    ├── beginner/              # Niveau Débutant
    │   ├── 001_1c11ddf4.json
    │   └── 002_b4c23034.json
    ├── intermediate/          # Niveau Intermédiaire
    │   └── ...
    └── advanced/              # Niveau Avancé
        └── ...
```

---

## 📝 Liste des Leçons Disponibles

### 🟢 Beginner (Débutant)
| Fichier | Titre de la leçon (générique) | Source Vidéo (anonyme) |
|---|---|---|
| [`english/beginner/001_1c11ddf4.json`](./english/beginner/001_1c11ddf4.json) | Lesson 001 | Dessin animé |
| [`english/beginner/002_b4c23034.json`](./english/beginner/002_b4c23034.json) | Lesson 002 | Dessin animé |

### 🟡 Intermediate (Intermédiaire)
*(À venir)*

### 🔴 Advanced (Avancé)
*(À venir)*

---

## 🛠️ Structure d'un fichier JSON (Schema)

Chaque fichier JSON respecte le format standardisé suivant :

```json
{
  "video_id": "uuid-de-la-video",
  "lesson": {
    "id": "uuid-de-la-lecon",
    "title": "Titre de la leçon",
    "summary": "Résumé de la vidéo",
    "grammar_concepts": [
      {
        "name": "Nom du concept grammatical",
        "explanation": "Explication claire",
        "examples_from_transcript": [
          "Exemples extraits du dialogue de la vidéo"
        ]
      }
    ],
    "vocabulary": [
      {
        "word": "Mot",
        "definition": "Définition",
        "example_sentence": "Phrase d'exemple"
      }
    ]
  },
  "exercises": [
    {
      "id": "uuid-de-l-exercice",
      "exercise_type": "multiple_choice | fill_in_the_blank",
      "question": "Intitulé de la question",
      "explanation": "Explication de la réponse",
      "options": [
        {
          "text": "Option",
          "is_correct": true
        }
      ]
    }
  ]
}
```

---

## 📲 Utilisation dans l'Application Mobile

Pour récupérer une leçon, l'application mobile construit l'URL brute (Raw) sur GitHub en utilisant le niveau et le nom de fichier anonymisé.

### URL Raw Générique
```
https://raw.githubusercontent.com/<UTILISATEUR>/<DEPOT>/main/english/<NIVEAU>/<NUMERO>_<HASH>.json
```

### Exemple de fonction dynamique (React Native / JS)
```javascript
async function loadLesson(level, lessonFileName) {
  // level = 'beginner', 'intermediate', 'advanced'
  // lessonFileName = '001_1c11ddf4'
  
  const baseUrl = "https://raw.githubusercontent.com/VOTRE_PSEUDO/VOTRE_DEPOT/main/english";
  const url = `${baseUrl}/${level}/${lessonFileName}.json`;
  
  try {
    const response = await fetch(url);
    if (!response.ok) throw new Error(`Erreur réseau (${response.status})`);
    return await response.json();
  } catch (error) {
    console.error(`Erreur de chargement [${level}][${lessonFileName}] :`, error);
  }
}
```

---

## ➕ Comment ajouter une nouvelle leçon ?

1. Assurez-vous que la leçon est générée par le backend de **AngC**.
2. Nommez le fichier JSON sous la forme `xxx_hash.json` (ex: `003_e7f2b8a1.json`), où `hash` est l'identifiant court du fichier JSON généré ou l'ID de la leçon.
3. Déposez-le dans le dossier de son niveau (ex: `english/beginner/`).
4. Ajoutez la ligne correspondante au tableau des leçons de ce README (en restant générique sur le titre pour la sécurité des droits d'auteur).
5. Poussez les modifications sur votre dépôt GitHub.
