# Documentation de KStars en français

Fichiers PO de la documentation de l'application KStars.

## Fichiers PO de la documentation

Les fichiers se trouvent sur le SVN de KDE :

https://websvn.kde.org/trunk/l10n-support/fr/summit/messages/documentation-kstars-docs-kde-org/

**Remarque** : il peut y avoir de nouveaux fichiers. Liste de tous les [*.pot](https://websvn.kde.org/trunk/l10n-support/templates/summit/docmessages/kstars/)

### Récupération initiale

```bash
svn checkout svn://anonsvn.kde.org/home/kde/trunk/l10n-support/fr/summit/messages/documentation-kstars-docs-kde-org/ ~/sources/kstars-l10n-svn
```

### Mise à jour

```bash
cd ~/sources/kstars-l10n-svn && svn update
```

`svn update` indique quels fichiers ont changé (`U fichier.po`). Copier ensuite les fichiers modifiés vers ce dépôt git et pousser.

## Ressources

- [Glossaire KDE en français](https://fr.l10n.kde.org/dict/)
- [Documentation KStars en anglais](https://docs.kde.org/trunk5/en/kstars/kstars/index.html)
- [Doc pology](https://community.kde.org/KDE_Localization/fr/pology)

## Traduction

### Extraction des chaînes non-traduites

```bash
msgattrib --untranslated source.po -o output.po
# Pour les fuzzy :
msgattrib --only-fuzzy source.po -o output.po
```

### Différence entre deux versions

```bash
diff -u <(msgfmt -o - v1.po | msgunfmt) <(msgfmt -o - v2.po | msgunfmt)
```

## Vérification des PO

```bash
# Pology (règles françaises)
python3 ~/sources/pology/scripts/posieve.py check-rules \
  -s rfile:~/sources/pology/lang/fr/rules/typography.rules \
  -s rfile:~/sources/pology/lang/fr/rules/common-mistakes.rules \
  -s rfile:~/sources/pology/lang/fr/rules/team-choices.rules \
  fichier.po

# i18nspector
i18nspector -l fr fichier.po
```

Comme ce sont des fichiers pris dans la branche trunk, il faut rajouter ce drapeau aux fichiers PO. Un script Python le fait :

```bash
python3 add_trunk.py fichier.po
```

## Génération des docbook

Ne pas oublier les images, sans quoi la compilation ne fonctionnera pas.

```bash
cd ~/sources/l10n-scripty
./update_xml ~/sources/kstars-documentation/fr kstars
```

Si tout se passe bien les `*.docbook` seront créés. Sinon, corriger les erreurs dans les `*.po` — sans quoi `index.docbook` ne sera pas créé.

Il faut aussi remplacer dans `index.docbook` :

```
<! ENTITY % fr "INCLUDE"
```

par :

```
<! ENTITY % French "INCLUDE"
```

## Génération des fichiers HTML

```bash
cd ~/sources/kstars-documentation/fr/docs/kstars/kstars
meinproc5 --check index.docbook
```

Corriger les erreurs en partant du haut des messages d'erreur.

Si les fichiers CSS ne sont pas trouvés lors de la visualisation dans un navigateur, exécuter :

```bash
./corriger_css.sh
```

## Génération du PDF

```bash
cd ~/sources/kstars-documentation/fr/docs/kstars/kstars
./buildpdf.sh index.docbook
```

Cela crée `kstars.pdf`.

## Traduction du site web

Le site se trouve [ici](https://kstars.kde.org/fr/).

Le PO est [ici](https://websvn.kde.org/trunk/l10n-support/fr/summit/messages/websites-kstars-kde-org/).

## Bonnes pratiques

- Pas de guillemets autour de `Ekos` et `INDI`
- L'apostrophe utilisée par l'équipe KDE-FR est U+0027 `'` (apostrophe simple droite)
- Points cardinaux : voir les [règles orthotypographiques](https://www.btb.termiumplus.gc.ca/redac-chap?lang=fra&lettr=chapsect3&info0=3.3.2)
- Règle pour `anti-` : voir le [Wiktionnaire](https://fr.wiktionary.org/wiki/anti-#fr)

## Glossaire

| Anglais | Français |
|---------|----------|
| Autofocus | Mise au point automatique |
| Capture | Acquisition |
| Focuser | Moteur de mise au point |
| Meridian flip | Retournement au méridien |
| Plate solver | Résolveur de plaque |
