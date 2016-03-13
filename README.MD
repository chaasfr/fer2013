#TP final deeplearning

##How to run
Ouvrir main.ipynb puis renseigner les 2 variables caffe_root et TP_root:
-caffe_root est le dossier contenant l'installation de caffe
-TP_root est le dossier contenant ce git (la racine de model, pretrained et predictions)

##Descriptions des NN:
5 réseaux de neuronnes sont utilisés. Ils sont entrainnés sur 10 000 itérations, à l'exception de microsoft, qui est entrainé sur 4 000 itérations:
- Lenet correspond au réseau de neuronne de Lenet fourni comme exemple dans caffe
- monoconv est le réseau Lenet amputée de sa deuxieme couche de convolution
- tripleconv est le réseau Lenet amputée de la deuxieme couche innerprodut mais avec 3 couches de convolutions
- 3conv2ip est le réseau Lenet avec une convolution supplémentaire
- microsoft est le réseau décrit dans ce papier http://research.microsoft.com/pubs/258194/icmi2015_ChaZhang.pdf
- microsoftlr2 est le même réseau que microsoft mais entrainé avec un pas d'apprentissage de 2.

##Stratégie du code
Les réseaux de neuronnes sont entrainés individuellement sur Emotion_Public_Train. Ensuite, ils sont testés sur Emotion_Private_Test. Les prédictions qu'ils réalisent sont utilisés de deux manières:
- Soit par une stratégie par vote, où le vote de chaque réseau est pondéré par la précision qu'il a obtenu lors de son test individuel. La précision obtenue sur le private est alors de 58%.
- Soit, chaque prédictions des réseaux de neuronnes est réutilisé pour entrainer un SVM en One vs All. La précision est alors de 99% sur le Private Test, mais l'évaluation sur Public Test reste autour de 17%. Le SVM fait sans doute de l'overfeeting...

##Précisions obtenues:
###Précisions sur le private test
Précision de Chaque réseau individuellement:
- Lenet: 52.07%
- Monoconv: 47.97%
- Tripleconv: 47.02%
- 3conv2ip: 45.49%
- Microsoft: 46.8%
- Microsoftlr2: 45.97%

Meilleure précision obtenue avec la stratégie des votes pondérés: 58.00%

Précision du SVM OvR entrainé sur les prédictions des réseaux: 99.91%
Précision du SVM linéaire: 57.05%

###Précisions sur le Public test
Précision de chaque réseau individuellement:
- Lenet: 17.31%
- Monoconv: 17.89%
- Tripleconv: 18.03%
- 3conv2ip: 18.23%
- Microsoft: 17.85%
- Microsoftlr2: 16.45%

Précision obtenue en donnant les prédictions des réseaux au SVM entraîné sur le private test: 17.63%

##Analyse des résultats

###Commentaire sur le public test
- Sans raison apparente, les résultats sont beaucoup plus faibles sur le public test. La piste la plus plausible serait celle du dataset défaillant, car le réseau Lenet n'est qu'à 17.31% de
précision.

###Stratégie du SVM
- L'idée derrière le SVM est d'avoir chaque réseau spécialisé dans 2 ou 3 des 7 expressions faciales à reconnaître. Le SVM va apprendre, à partir des prédictions donnés par les réseaux sur le private test, quel réseau est "spécialisé" pour quel expression. Les SVM linéaire, One vs One et One vs Rest ont été testé, la fonction de décision la plus précise est OvR.
- On obtient ainsi une précision de 99% sur le private test. Cependant, le résultat final de 17.63% sur le public test est questionnable. 2 hypothèses peuvent être formulées:  
-- Le SVM a pu faire du surapprentissage sur le Private Test  
-- Le Public test est peut être défaillant


