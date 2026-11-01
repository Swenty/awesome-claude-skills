# Message de passation pour l'autre conversation Claude

Copie tout ce qui se trouve sous la ligne ci-dessous et colle-le dans la nouvelle conversation, avec ces deux pièces jointes :

1. le fichier `syllabus.html` (l'application) ;
2. ton dossier de cours (en .zip, ou les fichiers directement).

---

Bonjour Claude. Je te confie la suite d'un projet commencé dans une autre conversation.

## Contexte

J'ai une application de suivi de mes études, **Syllabus** (`syllabus.html`, en pièce jointe). C'est une page HTML autonome avec plusieurs onglets :

- Vue d'ensemble ;
- Examens ;
- Devoirs ;
- Cours ;
- Révisions ;
- Session ;
- Anki ;
- Calendrier.

Tout ce que l'application affiche est **calculé automatiquement** à partir d'un **bloc de données JSON**, placé entre `<script id="syllabus-data" type="application/json">` et `</script>`. Les calculs comprennent : la liste du jour, la mémoire estimée de chaque chapitre, la prévision de préparation aux examens, les plans de révision, la charge de travail, les sessions de travail et les notes nécessaires.

Pour l'instant, ce bloc contient des **données d'exemple** inventées. Tu n'as **rien à coder**. Ton travail est de remplir ce bloc avec mes vraies données, tirées de mon dossier de cours.

## Règle importante sur le dossier

Dans mon dossier de cours, il y a un sous-dossier avec les cours **d'un étudiant de l'année précédente**. Son nom ressemble à « cours étudiant ». **Ignore-le complètement** : ce ne sont pas mes cours et ils ne doivent pas compter dans ma progression.

Si un autre dossier te semble dans le même cas (même genre de contenu, autre nom), demande-moi avant de le compter.

## Ce que tu dois faire

### Étape 1 : explorer le dossier

Parcours toute l'arborescence, sauf le dossier de l'étudiant de l'an dernier, et repère les éléments suivants.

**Les matières (UE).** En général, il y a un dossier par matière. Récupère le code de l'UE et le coefficient s'ils apparaissent quelque part : syllabus, maquette ou MCC (modalités de contrôle des connaissances).

**Les chapitres.** Il y a en général un fichier ou un groupe de fichiers par cours (CM, TD, TP). Regroupe le CM et le TD d'un même thème en un seul chapitre quand le lien est évident.

**La date de chaque cours.** Cherche-la dans cet ordre :

1. le nom du fichier (« CM3 – 05/10 ») ;
2. l'en-tête du document (« Cours du… ») ;
3. la date de création du fichier.

Si tu ne trouves rien, mets `null` et ajoute une ligne à `meta.a_confirmer`.

**L'état de mes notes**, pour chaque chapitre :

- `vu` : il existe des notes de moi sur ce cours.
- `propre` : il existe une version mise au propre. Ce sont par exemple des fichiers nommés « fiche », « propre », « clean », « résumé » ou « synthèse », ou un document tapé et bien structuré. Ne compte pas comme propre : des notes manuscrites scannées, des phrases incomplètes, des abréviations partout ou des passages manquants.
- `qualite_notes` : une de ces valeurs :
  - `"propre"` ;
  - `"brouillon"` ;
  - `"incomplet"` : il manque des parties du cours ;
  - `"manuscrit"` : le cours est complet mais à retaper ;
  - `null` si le cours n'a pas encore eu lieu.
- `anki` : il existe un paquet Anki pour ce chapitre. Cherche des fichiers `.apkg`, des exports `.txt` ou `.csv`, ou un dossier « Anki ». Si tu ne peux pas le savoir, mets `false` et demande-moi.
- `difficulte` : 1 (facile), 2 (moyen) ou 3 (dense ou difficile), selon la longueur et la densité du cours.
- `note` : une phrase courte si tu remarques un problème concret, par exemple « il manque la fin du CM », « schéma illisible » ou « TD non corrigé ». Sinon, `""`.

**Les devoirs** (DM, comptes-rendus de TP, exposés, QCM en ligne), avec leur date limite. Cherche dans les consignes, les emplois du temps, les captures Moodle et les mails enregistrés.

**Les examens** (partiels, contrôles continus, oraux, TP notés). Pour chacun, relève la date, l'heure, la durée, le format (QCM, rédactionnel, exercices, oral) et le coefficient. Cherche dans les MCC, le calendrier des examens et le syllabus. Relève aussi mes **notes déjà obtenues** si elles sont dans le dossier.

### Étape 2 : me poser les questions qui restent

Une fois l'exploration finie, pose-moi **en une seule fois** une courte liste des questions auxquelles le dossier ne répond pas. Par exemple :

- mon prénom ;
- les dates d'examens et de devoirs que tu n'as pas trouvées ;
- quels chapitres ont déjà des cartes Anki, si tu n'as pas pu le voir ;
- quels chapitres j'ai déjà révisés, combien de fois et quand (même approximativement) ;
- mes notes déjà obtenues et ma moyenne visée ;
- l'heure de mon rappel Anki et le nombre de minutes par jour.

**N'invente jamais une date d'examen ou de devoir, ni une note.** Si je ne sais pas, laisse la donnée de côté et ajoute une ligne dans `meta.a_confirmer`.

### Étape 3 : remplir le bloc JSON

Remplace **uniquement** le contenu entre `<script id="syllabus-data" type="application/json">` et `</script>`. Ne touche à rien d'autre dans le fichier, ni au CSS ni au JavaScript.

Contraintes :

- JSON strict : pas de commentaires, pas de virgule finale, guillemets doubles.
- Dates au format `AAAA-MM-JJ`, heures au format `HH:MM`.
- Des `id` uniques, courts et sans espaces (`"bc1"`, `"biochimie"`, `"e2"`).
- `"exemple": false`, et `"maj"` à la date du jour.
- `anki_journal` et `journal` : vides (`[]`) au départ, sauf si je te donne un historique.
- La chaîne `</script>` ne doit apparaître nulle part dans le JSON.
- Vérifie que le JSON se parse (avec `JSON.parse` ou `python -m json.tool`) avant de me rendre le fichier.

### Étape 4 : me rendre le résultat

Rends-moi deux choses :

1. Le fichier `syllabus.html` complet et mis à jour, prêt à ouvrir dans un navigateur (en fichier téléchargeable ou en artifact).
2. Un résumé court :
   - ce que tu as trouvé (nombre de matières, de chapitres, de devoirs, d'examens) ;
   - ce qui reste à confirmer ;
   - les 3 actions que tu me recommandes cette semaine.

Si tu ne peux pas produire le fichier complet, donne-moi seulement le JSON. Je peux le coller dans l'application : Vue d'ensemble → Annexes → « Données et mise à jour avec Claude » → Importer.

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
    "source": "D'où viennent les données",
    "a_confirmer": ["Questions ou dates encore inconnues"]
  },
  "parametres": {
    "anki_heure_rappel": "20:00",
    "anki_objectif_minutes": 30,
    "seuil_memoire": 0.7,
    "objectif_moyenne": 12,
    "semaines_revision_par_defaut": 4
  },
  "anki_journal": [],
  "journal": [],
  "matieres": [
    {
      "id": "bio-cell",
      "nom": "Biologie cellulaire",
      "code": "UE1",
      "coef": 3,
      "couleur": "#4f7cf0",
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
      "type": "Rendu",
      "statut": "a_faire",
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
      "type": "Partiel",
      "format": "QCM",
      "coef": 1,
      "chapitres": [],
      "resultat": null,
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
| `meta.prenom` | Utilisé dans la salutation. Vide = pas de prénom. |
| `meta.a_confirmer` | Affiché dans la Vue d'ensemble. Mets-y tout ce qui est incertain. |
| `parametres.seuil_memoire` | Un chapitre passe « à réviser » quand sa mémoire estimée descend sous ce seuil (0,7 = 70 %). |
| `parametres.objectif_moyenne` | Moyenne visée, sur 20. Sert à calculer la note nécessaire aux prochains examens. |
| `matieres[].coef` | Coefficient de la matière dans la moyenne générale. |
| `matieres[].couleur` | Une couleur par matière, lisible en thème clair et sombre : `#4f7cf0`, `#e39a2d`, `#a06be0`, `#2aa889`, `#e0607e`, `#2ba3c4`, `#c9a227`, `#7a8a99`. |
| `chapitres[].vu` / `propre` / `anki` | Les trois étapes cliquables. `propre` ou `anki` à `true` implique `vu` à `true`. |
| `chapitres[].nb_revisions` / `derniere_revision` | Nombre de révisions déjà faites et date de la dernière. Si c'est inconnu : `0` et `null`. |
| `chapitres[].difficulte` | Modifie la vitesse d'oubli estimée, la durée des blocs de session et la longueur du plan de révision. |
| `chapitres[].qualite_notes` | Plus les notes sont mauvaises, plus le chapitre remonte dans « À remettre au propre ». |
| `devoirs[].statut` | `"a_faire"`, `"en_cours"` ou `"rendu"`. |
| `devoirs[].type` | Libre, avec une majuscule : `"DM"`, `"Rendu"`, `"Oral"`, `"QCM"`, `"Projet"`… |
| `examens[].coef` | Poids de l'épreuve **dans sa matière**. Par exemple CC 0,4 + partiel 0,6. |
| `examens[].chapitres` | Liste des `id` de chapitres au programme. **Une liste vide signifie tous les chapitres de la matière.** |
| `examens[].format` | Mets `QCM`, `rédactionnel`, `exercices` ou `oral` : les conseils de la phase « Entraînement » s'adaptent. |
| `examens[].resultat` | Note obtenue sur 20, ou `null`. |
| `examens[].debut_revision` / `semaines_revision` | Laisse `null` : l'application calcule seule quand commencer (2 à 6 semaines avant, selon le nombre et la difficulté des chapitres). Ne les remplis que si je te les donne. |
| `examens[].conseils` | Consignes trouvées dans les documents, par exemple « calculatrice interdite », « annales sur Moodle » ou « le prof insiste sur les schémas ». |
| `anki_journal` | Liste des jours où j'ai fait mes cartes Anki (`AAAA-MM-JJ`). |
| `journal` | Historique des actions, rempli par l'application (`{"d": "AAAA-MM-JJ", "type": "revision", "id": "bc1"}`). Garde-le tel quel lors des mises à jour. |

## Mises à jour suivantes

Plus tard, je reviendrai avec le texte copié par le bouton « Copier le prompt pour Claude » (il contient ma progression), et éventuellement de nouveaux cours ou de nouvelles dates.

Dans ce cas :

1. Pars de ce JSON.
2. Ajoute ou corrige ce qui a changé, sans perdre ma progression : `vu`, `propre`, `anki`, `nb_revisions`, `derniere_revision`, `statut`, `resultat`, `anki_journal`, `journal`.
3. Mets `meta.maj` à la date du jour.
4. Rends-moi le fichier complet, comme à l'étape 4.

Tu peux commencer par explorer le dossier.
