# pf1-xml

Export XML brut du wiki francophone des règles de **Pathfinder JdR — première
édition**.

Ce dépôt contient une copie structurée du namespace `Pathfinder-RPG` du wiki
Pathfinder-FR, consacré aux règles, aux options de personnages, aux créatures,
aux objets et aux autres ressources de jeu de la première édition de
Pathfinder JdR.

Le wiki d'origine peut être consulté à l'adresse :

<https://www.pathfinder-fr.org/Wiki/Pathfinder-RPG.MainPage.ashx>

Une page historique décrit les différents formats d'export proposés par
Pathfinder-FR et leur objectif :

<https://www.pathfinder-fr.org/Wiki/Db.MainPage.ashx>

Les fichiers XML constituent une source de données intermédiaire destinée à
être réutilisée par des applications de recherche, de conversion ou de rendu.
Ils conservent à la fois le contenu exploitable et les métadonnées utiles à
ces traitements.

## Contenu actuel

L'export courant contient :

- le namespace `Pathfinder-RPG` uniquement ;
- 11 578 pages XML ;
- environ 234 Mo de données ;
- un fichier XML par page, sous `Pathfinder-RPG\`.

Exemple :

```text
Pathfinder-RPG\
├── Aasimar.xml
├── Aasimar-(Race).xml
├── Abadar-(Technique).xml
└── ...
```

Les noms de fichiers sont dérivés du titre des pages : les apostrophes sont
supprimées, les espaces sont remplacés par des tirets et la première lettre de
chaque mot est mise en majuscule. Les fichiers sont organisés sous le
namespace `Pathfinder-RPG`. Le nom complet original reste disponible dans
l'élément `fullName`.

## Format d'une page

Chaque fichier utilise une structure XML commune définie par le processus
d'export :

```xml
<?xml version="1.0"?>
<wikiPage>
  <title>...</title>
  <categories>
    <category>...</category>
  </categories>
  <lastModified>...</lastModified>
  <version>...</version>
  <inLinks>
    <link>...</link>
  </inLinks>
  <outLinks>
    <link>...</link>
  </outLinks>
  <body><![CDATA[...]]></body>
  <raw><![CDATA[...]]></raw>
  <fullName>Pathfinder-RPG.Exemple</fullName>
</wikiPage>
```

Les champs principaux sont :

- `title` : titre affiché de la page ;
- `categories` : catégories ScrewTurn associées ;
- `lastModified` : date de dernière modification ;
- `version` : dernière version connue de la page ;
- `inLinks` et `outLinks` : relations entre pages ;
- `body` : contenu formaté par ScrewTurn, encodé dans le template ;
- `raw` : contenu brut ScrewTurn ;
- `fullName` : nom complet incluant le namespace.

Le champ `raw` est généralement le point de départ le plus adapté pour une
conversion vers un autre format. Le champ `body` peut être utilisé lorsqu'un
traitement souhaite réutiliser le contenu déjà interprété ou formaté par le
wiki. Dans cet export, `body` correspond au contenu HTML produit pour
l'exportation ; son rendu peut différer légèrement de celui du site en ligne,
notamment pour certains snippets et pour la normalisation des liens. Les liens
entrants et sortants permettent de reconstruire une partie du graphe de
navigation entre les pages.

## Utilisation comme source de données

Le dépôt est destiné à être consommé par un programme, et non à être parcouru
ou modifié manuellement. Un traitement peut notamment :

- parcourir les fichiers XML du dossier `Pathfinder-RPG` ;
- sélectionner les pages par titre, catégorie ou namespace ;
- convertir le contenu `raw` vers Markdown, HTML ou un autre format ;
- exploiter `inLinks` et `outLinks` pour préserver les relations entre pages ;
- utiliser `lastModified` et `version` pour suivre les évolutions du corpus.

Les données correspondent à un export à une date donnée. Elles ne constituent
donc pas une synchronisation en temps réel du wiki en ligne ; pour vérifier le
contenu le plus récent, consulter le wiki d'origine.

## Position dans les formats Pathfinder-FR

Cet export XML est un format brut : il contient les pages complètes et leurs
métadonnées, sans tenter d'extraire uniquement les sorts, dons, monstres ou
autres catégories de règles. Il est donc adapté aux traitements qui ont besoin
du corpus complet ou de la syntaxe wiki originale.

Il se distingue d'un export normalisé, qui peut fournir des données structurées
dans des formats comme XML, CSV ou JSON pour des types de contenu particuliers.
Pour une analyse générale du wiki, le format présent dans ce dépôt constitue
la source la plus riche ; pour exploiter directement des données métier
normalisées, un export spécialisé peut être plus approprié.

## Versionnement et fins de ligne

Le fichier `.gitattributes` impose des fins de ligne `LF` pour les XML afin
d'éviter les différences artificielles entre Windows et les autres systèmes :

```gitattributes
* text=auto
*.xml text eol=lf
```

Après une nouvelle génération, les fichiers peuvent être normalisés avant
commit avec :

```powershell
git add --renormalize .
```

Les journaux et fichiers temporaires ne font pas partie du contenu exporté.

## Nature du dépôt

Ce dépôt contient des données générées et ne doit pas être édité manuellement.
Les modifications de contenu doivent être effectuées dans la source du wiki,
puis répercutées par une nouvelle génération.

## Licence et attribution

Les contenus du wiki restent soumis à leur licence d'origine, notamment
l'**Open Game License (OGL)**, ainsi qu'aux conditions de contribution de
Pathfinder-FR. Toute réutilisation doit également respecter les conditions
d'attribution indiquées par Pathfinder-FR et les ayants droit concernés.
