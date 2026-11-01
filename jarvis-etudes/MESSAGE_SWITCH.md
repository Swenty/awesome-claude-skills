# Message de passation pour l'autre conversation Claude

Copie tout ce qui se trouve sous la ligne ci-dessous et colle-le dans la nouvelle conversation, avec ces deux pièces jointes :

1. le fichier `jarvis-etudes.html` (le tableau de bord) ;
2. ton dossier de cours (en .zip, ou les fichiers directement).

---

Bonjour Claude. Je te confie la suite d'un projet commencé dans une autre conversation.

## Contexte

J'ai un tableau de bord de suivi de mes études, dans le style JARVIS (`jarvis-etudes.html`, en pièce jointe). C'est une page HTML autonome qui contient un **bloc de données JSON** entre les balises `<script id="jarvis-data" type="application/json">` et `</script>`. Pour l'instant ce bloc contient des **données d'exemple** inventées.

Toute l'interface (briefing JARVIS, rappel Anki, progression, cours à refaire au propre, cours à réviser, échéances, calendrier, plans de révision) est **calculée automatiquement** à partir de ce bloc. Tu n'as donc **rien à coder** : ton travail est de remplir ce bloc avec mes vraies données, tirées de mon dossier de cours.

## Règle importante sur le dossier

Dans mon dossier de cours, il y a un sous-dossier qui contient les cours **d'un étudiant de l'année précédente** (son nom ressemble à « cours étudiant »). **Ignore-le complètement** : ce ne sont pas mes cours et ils ne doivent pas compter dans ma progression. Si tu as un doute sur un autre dossier (même genre de contenu, autre nom), demande-moi avant de le compter.

## Ce que tu dois faire

### Étape 1 : explorer le dossier

Parcours toute l'arborescence (sauf le dossier de l'étudiant de l'an dernier) et repère :

- **Les matières / UE** : en général un dossier par matière. Récupère le code de l'UE et le coefficient s'ils apparaissent (syllabus, maquette, MCC).
- **Les chapitres** : un fichier ou un groupe de fichiers par cours (CM, TD, TP). Regroupe le CM et le TD d'un même thème en un seul chapitre quand c'est clairement lié.
- **La date de chaque cours** : nom du fichier (« CM3 – 05/10 »), en-tête du document (« Cours du… »), sinon la date de création du fichier. Si tu ne trouves rien, mets `null` et ajoute-le à `meta.a_confirmer`.
- **L'état de mes notes**, pour chaque chapitre :
  - `vu` : il existe des notes de moi sur ce cours.
  - `propre` : il existe une version mise au propre (fichier nommé « fiche », « propre », « clean », « résumé », « synthèse », ou un document tapé et structuré). Des notes manuscrites scannées, des phrases incomplètes, des abréviations partout, des trous = pas propre.
  - `qualite_notes` : `"propre"`, `"brouillon"`, `"incomplet"` (il manque des parties du cours) ou `"manuscrit"` (complet mais à retaper).
  - `anki` : il existe un paquet Anki pour ce chapitre (fichiers `.apkg`, `.txt`/`.csv` d'export Anki, dossier « Anki »). Si tu ne peux pas le savoir, mets `false` et demande-moi.
  - `difficulte` : 1 (facile), 2 (moyen) ou 3 (dense ou difficile), selon la longueur et la densité du cours.
  - `note` : une phrase utile si tu remarques un problème concret (« il manque la fin du CM », « schéma illisible », « TD non corrigé »). Sinon `""`.
- **Les devoirs** (DM, comptes-rendus de TP, exposés, QCM en ligne) avec leur date limite : consignes, emplois du temps, captures Moodle, mails enregistrés.
- **Les examens** (partiels, contrôles continus, oraux, TP notés) avec date, heure, durée, format (QCM, rédactionnel, exercices, oral) et coefficient : documents de MCC (modalités de contrôle des connaissances), calendrier des examens, syllabus.

### Étape 2 : me poser les questions qui restent

Quand tu as fini l'exploration, pose-moi **en une seule fois** une courte liste des questions auxquelles le dossier ne répond pas. Par exemple :

- mon prénom (pour que JARVIS me salue) ;
- les dates d'examens et de devoirs introuvables ;
- quels chapitres ont déjà des cartes Anki, si tu n'as pas pu le voir ;
- quels chapitres j'ai déjà révisés, combien de fois et quand (approximativement) ;
- à quelle heure je veux le rappel Anki et combien de minutes par jour.

**N'invente jamais une date d'examen ou de devoir.** Si je ne sais pas, laisse la donnée de côté et ajoute une ligne dans `meta.a_confirmer`.

### Étape 3 : remplir le bloc JSON

Remplace **uniquement** le contenu entre `<script id="jarvis-data" type="application/json">` et `</script>`. Ne touche à rien d'autre dans le fichier (ni le CSS, ni le JavaScript).

Contraintes :

- JSON strict : pas de commentaires, pas de virgule finale, guillemets doubles.
- Dates au format `AAAA-MM-JJ`, heures au format `HH:MM`.
- Les `id` doivent être uniques, courts et sans espaces (`"bc1"`, `"biochimie"`, `"e2"`).
- Mets `"exemple": false` et `"maj"` à la date du jour.
- La chaîne `</script>` ne doit apparaître nulle part dans le JSON.
- Vérifie que le JSON se parse (par exemple avec `JSON.parse` ou `python -m json.tool`) avant de me rendre le fichier.

### Étape 4 : me rendre le résultat

1. Le fichier `jarvis-etudes.html` complet, mis à jour, prêt à ouvrir dans un navigateur (en fichier téléchargeable ou en artifact).
2. Un résumé court : ce que tu as trouvé (nombre de matières, de chapitres, d'échéances), ce qui reste à confirmer, et les 3 actions que tu me recommandes cette semaine.

Si tu ne peux pas produire le fichier complet, donne-moi seulement le JSON : je peux le coller dans le tableau de bord avec le bouton « Importer » (section Données).

## Schéma du bloc de données

```json
{
  "meta": {
    "exemple": false,
    "prenom": "Prénom",
    "formation": "Intitulé de la formation",
    "annee": "2026-2027",
    "semestre": "S3",
    "maj": "AAAA-MM-JJ",
    "source": "D'où viennent les données (ex. : dossier de cours du 1er nov.)",
    "a_confirmer": ["Questions ou dates encore inconnues"]
  },
  "parametres": {
    "anki_heure_rappel": "20:00",
    "anki_objectif_minutes": 30,
    "intervalles_revision_jours": [1, 3, 7, 14, 30, 60],
    "semaines_revision_par_defaut": 4
  },
  "anki_journal": ["AAAA-MM-JJ"],
  "matieres": [
    {
      "id": "bio-cell",
      "nom": "Biologie cellulaire",
      "code": "UE1",
      "coef": 3,
      "couleur": "#3fd8ff",
      "chapitres": [
        {
          "id": "bc1",
          "titre": "Membrane plasmique",
          "type": "CM",
          "date_cours": "AAAA-MM-JJ",
          "vu": true,
          "propre": false,
          "anki": false,
          "nb_revisions": 0,
          "derniere_revision": null,
          "difficulte": 2,
          "qualite_notes": "brouillon",
          "fichier": "Chemin/relatif/du/fichier.pdf",
          "note": ""
        }
      ]
    }
  ],
  "devoirs": [
    {
      "id": "d1",
      "titre": "Compte-rendu TP",
      "matiere": "bio-cell",
      "echeance": "AAAA-MM-JJ",
      "heure": "23:59",
      "type": "rendu",
      "fait": false,
      "details": "Consignes utiles en une phrase"
    }
  ],
  "examens": [
    {
      "id": "e1",
      "titre": "Partiel de biologie cellulaire",
      "matiere": "bio-cell",
      "date": "AAAA-MM-JJ",
      "heure": "09:00",
      "duree": "2h",
      "type": "partiel",
      "format": "QCM",
      "coef": 0.6,
      "chapitres": [],
      "debut_revision": null,
      "semaines_revision": null,
      "conseils": []
    }
  ]
}
```

### Détail des champs

| Champ | Rôle |
|---|---|
| `meta.prenom` | Utilisé dans le briefing (« Bonsoir, Prénom »). Vide = pas de prénom. |
| `meta.a_confirmer` | Liste affichée dans la section Données. Mets-y tout ce qui est incertain. |
| `parametres.intervalles_revision_jours` | Répétition espacée : après le cours, révision à J+1, puis J+3, J+7, J+14, J+30, J+60. Garde ces valeurs sauf demande contraire. |
| `anki_journal` | Jours où j'ai fait mes cartes Anki. Laisse `[]` au début. |
| `matieres[].couleur` | Couleur de la matière. Utilise des couleurs vives qui ressortent sur fond sombre : `#3fd8ff`, `#ffb648`, `#b892ff`, `#4cefa2`, `#ff7a9c`, `#7ab8ff`, `#ffe066`. |
| `chapitres[].vu` / `propre` / `anki` | Les trois étapes cliquables du tableau de bord. `propre` ou `anki` à `true` implique `vu` à `true`. |
| `chapitres[].nb_revisions` / `derniere_revision` | Nombre de révisions déjà faites et date de la dernière. Inconnu = `0` et `null`. |
| `chapitres[].qualite_notes` | `"propre"`, `"brouillon"`, `"incomplet"`, `"manuscrit"` ou `null` si pas encore vu. Plus c'est mauvais, plus le chapitre remonte dans « À refaire au propre ». |
| `chapitres[].note` | Affichée en italique sous la recommandation. Une phrase concrète. |
| `devoirs[].type` | Libre : `"DM"`, `"rendu"`, `"oral"`, `"QCM"`, `"projet"`… |
| `examens[].chapitres` | Liste des `id` de chapitres au programme. **Vide = tous les chapitres de la matière.** |
| `examens[].format` | Contient `QCM`, `rédactionnel`, `exercices` ou `oral` : le conseil de la phase « Entraînement » s'adapte. |
| `examens[].debut_revision` | Laisse `null` : le tableau calcule seul la date de début (2 à 6 semaines avant, selon le nombre et la difficulté des chapitres). Ne la fixe que si je te la donne. |
| `examens[].semaines_revision` | Pour forcer la durée des révisions en semaines. Sinon `null`. |
| `examens[].conseils` | Conseils spécifiques que tu trouves dans les documents (« calculatrice interdite », « annales disponibles sur Moodle », « le prof insiste sur les schémas »). |

## Ce que le tableau calcule tout seul (pour info)

- **Progression** : chaque chapitre compte 4 étapes (vu, propre, Anki, révisé au moins une fois).
- **À refaire au propre** : chapitres vus mais pas au propre, classés par ancienneté, qualité des notes et proximité de l'examen.
- **À réviser** : chapitres dont la prochaine révision espacée est dépassée, ou au programme d'un examen dans moins de 14 jours.
- **Plans de révision** : pour chaque examen, 4 phases datées (consolidation, mémorisation active, entraînement, dernière ligne droite) avec la méthode à suivre.
- **Rappel Anki** : bandeau après l'heure de rappel tant que la session du jour n'est pas validée.

## Mises à jour suivantes

Plus tard, je reviendrai avec :

- le texte copié via le bouton « Copier mes données » (il contient ma progression cochée dans le navigateur) ;
- éventuellement de nouveaux cours ou de nouvelles dates.

Dans ce cas, pars de ce JSON, ajoute ou corrige ce qui a changé sans perdre ma progression (`vu`, `propre`, `anki`, `nb_revisions`, `derniere_revision`, `fait`, `anki_journal`), mets `meta.maj` à jour, et rends-moi le fichier complet comme à l'étape 4.

Tu peux commencer par explorer le dossier.
