---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
---

{% if author.googlescholar %}
  You can also find my articles on <u><a href="{{author.googlescholar}}">my Google Scholar profile</a>.</u>
{% endif %}

{% include base_path %}

{% for post in site.publications reversed %}
  {% include archive-single.html %}
{% endfor %}

<!-- ## Research Publications

9. Sommerville, Parker J.W.; Li, Yilin; Don, Ban Xuan; Zhang, Yongcao; Onorato, Jonathan, W.; **Tatum, Wesley K.**; Balzer, Alex H.; Stingelin, Natalie; Patel, Shrayesh, N.; Nealey, Paul F.; Luscombe, Christine, K. Elucidating the Influence of Side-Chain Circular Distribution on the Crack Onset Strain and Hole Mobility of Near-Amorphous Indacenodithiophene Copolymers. _Marcromolecules_, 2020, 53 (17), 7511-7518.

8. **Tatum, Wesley K.**; Torrejon, Diego; O'Neil, Patrick; Onorato, Jonathan w.; Resing, Anton; Holliday, Sarah; Flagg, Lucas Q.; Ginger, David S.; Luscombe, Christine K. A Generalizable Framework for Algorithmic Interpretation of Thin Film Morphologies in Scanning Probe Images. _ACS J. Chem. Inf. and Model._, 2020, 60 (7), 33387-3397.

7. **Tatum, Wesley K.**; Resing, Anton B.; Flagg, Lucas Q.; Ginger, David S.; Luscombe, Christine K. Defect Tolerance of π-Conjugated polymers and their relevance to optoelectronic applications. _ACS Appl. Polym. Mat._, 2019, 1 (6), 1466-1475.

6. Keerthisinghe, Chanaka; Ahumada-Paras, Mareldi; Pozzo, Lilo D.; Kirschen, Daniel S.; Pontes, Hugo; **Tatum, Wesley K.**; Matos, Marvi A. PV-Battery Systems for Critical Loads During Emergencies. _IEEE Power and Energy_, 2019, 17, 1, 82-92.

5. Shi, Qinqin; **Tatum, Wesley K.**; Zhang, Junxiang; Scott, Colleen; Luscombe, Christine K; Marder, Seth R.; Blakely, Simon B. The Direct Arylation Polymerization (DArP) of Well‐Defined Alternating Copolymers Based On 5,6‐Dicyano[2,1,3]benzothiadiazole (DCBT). _Asian J. of Org. Chem._, 2018, 7 (7), 1419-1425.

4. **Tatum, Wesley K.**; Luscombe, Christine K. π-Conjugated polymer nanowires: advances and perspectives towards effective commercial implementation. _Polym. J._, 2018, 50 (8), 659-669.

3. Li, Yilin; **Tatum, Wesley K.**; Onorato, Jonathan W.; Zhang, Yongcao; Luscombe, Christine K. Low elastic modulus and high charge mobility of low-crystallinity indacenodithiophene-based semiconducting polymers for potential applications in stretchable electronics. _Macromolecules_, 2018, 51 (16), 6352-6358.

2. Xi, Yuyin; Li, David S.; Newbloom, Greg M.; **Tatum, Wesley K.**; O'Donnell, Matthew; Luscombe, Christine K.; Pozzo, Lilo D. Sonocrystallization of conjugated polymers with ultrasound fields. _Soft Matter_, 2018, 14 (24), 4963-4976.

1. Li, Yilin; **Tatum, Wesley K.**; Onorato, Jonathan W.; Barajas, Sierra D.; Yang, Yun Y.; Luscombe, Christine K. An indacenodithiophene-based semiconducting polymer with high ductility for stretchable organic electronics. _Polym. Chem._ 2017, 8 (34), 5185-5193.


## White Papers

Haydon, Ian; Herpoldt, Karla-Louise; Hosseinzadeh, Parisa; Kang, Christine; Kang, Lauren J.; Montoni, Nicholas P.; **Tatum, Wesley K.** Workforce diversity: Strategies for cultivating inclusion in research. _Inside eLife_, 2018, [Article link](https://elifesciences.org/inside-elife/8a24d01a/workforce-diversity-strategies-for-cultivating-inclusion-in-research)

## Dissertation

**Tatum, Wesley K.** Establishing Quantitative Relationships Between Composition, Morphology, and Performance in Polymer Electronics. 2020, [Link to Dissertation](https://digital.lib.washington.edu/researchworks/handle/1773/46501) -->