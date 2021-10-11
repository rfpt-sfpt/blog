---
layout: post
author: An VO QUANG
image: images/AnVoQuang_image.png
toc: true
comments: true
hide: false
search_exclude: true
categories: [Sentinel-2, Série temporelle, Deep learning, Classification supervisée, Dégradation forestière, Afrique de l'Ouest]
description: "Evaluation de l'apport potentiel des nouveaux capteurs satellitaires optiques et radars"
title: "Détection des forêts dégradées en Guinée à partir des images satellites Sentinel-2" 
---


{% include info.html text="Les travaux de recherche presentés en-dessous ont été menés au sein du [*Laboratoire Interdisciplinaire des Energies de Demain* (LIED)](https://u-paris.fr/sdv/laboratoire-interdisciplinaire-des-energies-de-demain/)" %}



![]({{ site.baseurl }}/images/AnVoQuang_image.png "Figure. Un faible effort de photo-interprétation pour la production de
cartes de forêts dégradées.")



# Contexte

 Les travaux de la thèse CIFRE ont été réalisés dans le cadre d\'un
 partenariat entre l\'Institut interdisciplinaire de recherche en
 énergie de Paris (LIED) et IGN FI, une société d\'ingénierie
 géographique (partenaire export de l\'IGN - Institut national de
 l'information géographique et forestière) qui réalise des projets sur
 tous les continents et dans tous les domaines d\'application de la
 géomatique, notamment l\'aménagement du territoire, l\'environnement,
 l\'agriculture, l\'administration foncière ou la gestion des risques.
 Plus spécifiquement, les travaux de thèse se sont intégrés au projet
 de Zonage Agro-Ecologique de Guinée (ZAEG) coordonné par IGN FI et
 financé par l\'Agence Française de Développement (AFD) pour le
 ministère de l'Agriculture de Guinée.

 Contrairement à la déforestation, la dégradation forestière implique
 un changement de la structure forestière sans modification de
 l\'utilisation du sol. Ce changement est subtil et moins visible que
 la déforestation. La dégradation des forêts est une préoccupation
 majeure car un potentiel de séquestration du carbone est perdu. Ce
 phénomène varie en fonction de l\'emplacement géographique, des
 facteurs anthropiques, du climat, des types de forêts impactées, donc
 il n\'existe pas de méthodologie de détection unique pour
 cartographier la dégradation des forêts à l\'échelle mondiale.

 En Guinée, le principal processus de dégradation est l\'exploitation
 forestière sélective dans la forêt de massif, en plus de la
 fragmentation de la forêt causée par le changement d\'utilisation des
 terres.

 L'objectif est d'optimiser les méthodes de photo-interprétation
 utilisées par IGN FI pour détecter les zones de forêt dégradée. Le
 suivi du couvert forestier à l\'aide des méthodes traditionnelles de
 télédétection nécessite un coût important en termes d\'expertise en
 photo-interprétation. Nous proposons une approche de suivi par une
 procédure de classification semi-automatisée avec un coût de
 photo-interprétation minimum en incluant le contexte pixellaire, en
 intégrant les données du capteur Sentinel-2, acquises de manière
 répétitive.

# Méthodologies

 Les travaux de thèse proposent une procédure de détection des forêts
 dégradées à l\'aide de l\'imagerie multispectrale Sentinel-2 en
 Guinée, en Afrique de l\'Ouest, notamment une série temporelle de 23
 images Sentinel-2 acquises de 2015 à 2020.

 Des indices radiométriques sont dérivés des réflectances Sentinel-2.
 Leurs séries temporelles montrent que ceux liés à l\'humidité (Canopy
 Water Content CWC, Moisture Stress Index MSI), associés à l\'indice de
 surface foliaire (LAI), sont fortement liés au degré de dégradation,
 car ils sont liés à la dessiccation du feuillage de la canopée.

 Des méthodologies de classification sont établies impliquant ces
 indices LAI, MSI et CWC. Deux types de classification sont comparés :
 une classification standard basée sur les pixels et une méthode de
 classification prenant en compte des informations contextuelles en
 incluant les valeurs de pixels voisins dans l\'algorithme. La dernière
 méthode montre de meilleurs scores de précision et les résultats
 cartographiques ont été validés à l\'aide d\'un ensemble
 d\'observations de terrain réalisées avec un forestier local. Mais la
 méthode contextuelle ne peut pas se passer d'une part de
 photo-interprétation. Cette part est réduite puisque l'interprétation
 visuelle de 5% de la surface de l'image suffit à étalonner la méthode
 semi-automatisée, ce qui correspond à une nette avancée par rapport
 aux méthodes existantes utilisées dans les projets internationaux de
 certification de l'état des forêts tropicales.

 L\'inclusion de pixels voisins dans le processus de classification
 conduit à une forte amélioration des résultats, ce qui peut refléter
 le fait que le contexte est le critère principal des méthodes basées
 sur la photo-interprétation pour détecter les forêts dégradées.

 Une autre méthode utilisant le contexte a donc été développée basée
 sur l'intelligence artificielle, afin de s'affranchir du besoin de
 photo-interprétation. Au vu des résultats obtenus avec la méthode
 contextuelle, il nous a paru évident d'essayer des méthodes de Deep
 Learning (DL). Comme pour le Random Forest, les classifications sont
 faites sur une seule photo avec le DL. En revanche dans le cas du DL,
 l\'entraînement se fait sur des séries d\'images (sur plusieurs
 endroits, à plusieurs époques). L\'entraînement se fait donc sur de
 plus grandes quantités de données et plus la quantité de données est
 élevée, plus les modèles sont capables de reconnaître une plus grande
 variabilité dans les paysages et les saisons. Notre modèle est un UNET
 adapté à une utilisation avec des images à 13 bandes. Ce réseau, étant
 convolutionnel de bout en bout, peut accepter en entrée des images de
 toutes tailles. Il repose sur la segmentation sémantique, qui associe
 une catégorie à chaque pixel d\'une image.

## Dataset
 La tâche la plus chronophage a été de constituer un dataset assez
 volumineux pour entraîner notre UNET adapté. Les tâches de DL, de par
 les architectures des réseaux et le nombre de paramètres à entraîner
 requièrent des quantités phénoménales de données pour atteindre les
 résultats souhaités. Le fait que la tâche soit de la segmentation
 n'arrange rien au problème. Finalement le dataset d\'entraînement
 contenait plus de 39000 km2 labellisés sur jusqu'à trois années sur
 certaines régions, afin de permettre au modèle de faire la différence
 entre les changements saisonniers et naturels.

## Evaluation
 Ce modèle a été testé entre autres sur Nimba, une zone montagneuse
 boisée à l'extrémité sud de la Guinée, à la frontière de la Côte
 d'Ivoire. Le modèle de Deep Learning parvient à inférer sur cette zone
 forestière sur laquelle il ne s'est pas entraîné, sur des images
 acquises en 2020 (mois de décembre). Afin d'évaluer la qualité et la
 performance du modèle, une carte de référence a permis de produire les
 matrices de confusion et métriques kappa. Les taux de réussite sont de
 95% en moyenne sur les trois zones testées en Guinée.

# Conclusion
 L'ensemble de ces recherches présente une base solide pour les
 opérateurs nationaux lors de l'étape de vectorisation et un grand
 soutien pour la procédure de photo-interprétation pour une production
 de cartes moins longue et coûteuse, offrant des bases importantes pour
 la délimitation des forêts dégradées dans des régions présentant des
 conditions similaires de forêts dégradées.

# Bibliographie

## Articles

-   Bruno Smets, Marcel Buchhorn, Gabriel Jaffrain, Marian-Daniel
    Iordache, Niels Souverijns, Adrien Moiret, ***An Vo Quang***, Myroslava
    Lesiv, Nandin-Erdene Tsendbaza (2020). Elastic mapping through the
    Copernicus Global Land Cover layers. IGARSS (submitted).

-   Jaffrain, G., Leroux, A., ***Vo Quang, A.***, Pinet, C., Dessagboli, B.,
    Gauthier, Y., & Moiret, A. (2021). Suivi de la dynamique de
    l'occupation du sol en République de Guinée par imagerie
    satellitaire Spot: Transfert technologique pour le développement
    d'outils performants d'aide à la décision. Revue Française De
    Photogrammétrie Et De Télédétection, 223, 59--80.
    https://doi.org/10.52638/rfpt.2021.563

-   Giraldo, C., Boutoute, M., Mayzaud, P., Tavernier, E., ***Vo Quang, A.***,
    & Koubbi, P. (2017). Lipid dynamics in early life stages of the
    icefish Chionodraco hamatus in the Dumont d'Urville Sea (East
    Antarctica). Polar Biology, 40(2), 313-320.

## Posters

-   (poster) ***Vo Quang A.***, Jaffrain G., Delbart N. The challenge of mapping forest
    cover changes: forest degradation detection by optical remote
    sensing time series analysis (2019) Geophysical Research Vol. 21,
    EGU2019, EGU General Assembly 2019

-   (poster) ***Vo Quang A.***, Giraldo C., Tavernier E., Koubbi P., Mayzaud P. Lipid
    dynamics in early life stages in the icefish Chionodraco sp. in East
    Antarctica (2014) Poster, SCAR Open Science Conference, New Zealand.

#  Contact

> avoquang@ignfi.fr

