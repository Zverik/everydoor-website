---
title: Every Door FAQ
---
# Foire Aux Questions

## Pourquoi la carte est si petite ?

Parce que ce n’est pas le but. La carte sert uniquement à vous positionner et afficher une liste des points d’intérêts à proximité triés en fonction de leur distance par rapport à votre position.

La carte est agrandie lors de l’édition de points d’intérêts éloignés de votre position.
Bien que l’application est avant tout destinée à vous aider à éditer ce que vous voyez de vous même.

## À quoi servent les marqueurs de validation ?

Ces marqueurs indiquent que les données ont été confirmées, ils ajoutent la clé `check_date` avec la date actuelle.

L’idée de ces marqueurs est que, dans l’exemple où vous auriez répertorié la moitié de la ville sur un premier trajet, qu’il vous soit possible de revenir sur ces points quelques mois plus tard et d’indiquer que rien n’a changé. Un marqueur et c’est validé.

Le marqueur reste affiché pour deux semaines, après quoi il sera de nouveau possible de les repointer.

## Pourquoi l’application est-elle en allemand ?

Si vous souhaitez de l’anglais, essayez d’aller dans les paramètres du téléphone, section « Système » et « Langues ». Ajoutez l’anglais à la liste et redémarrez l’application.

Un sélecteur de langue sera disponible dans une future version.

## Comment ajouter l’entrée d’un bâtiment ?

En haut à droite, un bouton permet de basculer entre différent modes d’édition : points d’intérêts, micromapping et entrées.

Dans le mode « Entrées », appuyez ou faites glisser l’icône de porte, dans le coin inférieur droit, vers la carte.

## Comment utiliser des lettres dans les numéros d’adresse ?

Si votre clavier numérique ne peut pas basculer vers un clavier complet, vérifiez les paramètres de l’application. Appuyez sur le bouton du coin supérieur gauche où vous trouverez le « clavier numérique étendu ».

## Quelle est la différence entre les étages « 3 » et « /3 » ?

Le premier correspond à l’attribut `addr:floor=3`, c’est l’étage comme indiqué lors de la navigation et généralement utilisé mais dépend de la localité, tandis que le second correspond à `level=3` selon le schéma `building:level` où le rez-de-chaussée est l’étage 0.

Ainsi, les valeurs suivantes indiqueront :

* `2` : `addr:floor=2` et `level=*` ;
* `4/` : `addr:floor=4`, sans `level` ;
* `/1` : `level=1`, sans `addr:floor` ;
* `1/0` : `addr:floor=1` et `level=0`.

## Questions relatives aux attributs

Pourquoi un élément est manquant dans l’éditeur ? Quand ces points blancs sont-ils affiché en mode Micromapping ? Comment sont triés les éléments ?

Les réponses à toutes ces questions sont dans [good\_tags.dart](https://github.com/Zverik/every_door/blob/main/lib/helpers/good_tags.dart) dont les thèmes principaux :

* L’ordre des attributs : list `kMainKeys` ;
* Les éléments sont téléchargés : function `isGoodTags` ;
* Ce qui est considéré comme un point d’intérêt : function `isAmenityTags` ;
* Les points qui sont aimantés : function `detectSnap` ;
* Quand un objet micro-cartographié est incomplet : function `needsMoreInfo`.

## Comment changer ou traduire les champs d’un préréglage ?

L’éditeur est basé sur les préréglages de l’éditeur iD. Pour les modifier, allez sur [ce dépot](https://github.com/openstreetmap/id-tagging-schema). Malheureusement, cela n’est pas maintenu pour le moment, nous avons besoin de volontaires et de convaincre les propriétaires.

La traduction passe par [Transifex](https://www.transifex.com/openstreetmap/id-editor/translate/#ru/presets/).

Pour traduire les valeurs, indiquez les options désirées au dépot comme dans [cet exemple](https://github.com/openstreetmap/id-tagging-schema/blob/main/data/fields/camera/type.json).
Une fois la source mise à jour sur Transifex, elle sera disponible pour traduction, [comme ici](https://www.transifex.com/openstreetmap/id-editor/translate/#ru/presets/101711314?q=key%3Apresets.fields.camera%2Ftype).
