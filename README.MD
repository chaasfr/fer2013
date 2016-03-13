#TP final deeplearning

##How to run
Renseigner les 2 variables caffe_root et TP_root:
-caffe_root est le dossier contenant l'installation de caffe
-TP_root est le dossier contenant ce git (la racine de model, pretrained et predictions)

##Descriptions des NN:
5 réseaux de neuronnes sont utilisés:
- Lenet correspond au réseau de neuronne de Lenet fourni comme exemple dans caffe
- monoconv est le réseau Lenet amputée de sa deuxieme couche de convolution
- tripleconv est le réseau Lenet amputée de la deuxieme couche innerprodut mais avec 3 couches de convolutions
- 3conv2ip est le réseau Lenet avec une convolution supplémentaire
- microsoft est le réseau décrit dans ce papier http://research.microsoft.com/pubs/258194/icmi2015_ChaZhang.pdf
- microsoftlr2 est le même réseau que microsoft mais entrainé avec un pas d'apprentissage de 2.

##Stratégie du code
Les réseaux de neuronnes sont entrainés individuellement sur Emotion_Public_Train. Ensuite, ils sont testés sur Emotion_Private_Test. Les prédictions qu'ils réalisent sont utilisés de deux manières:
-Soit par une stratégie par vote, où le vote de chaque réseau est pondéré par la précision qu'il a obtenu lors de son test individuel. La précision obtenue sur le private est alors de 58%.
-Soit, chaque prédictions des réseaux de neuronnes est réutilisé pour entrainer un SVM en One vs All. La précision est alors de 99% sur le Private Test, mais l'évaluation sur Public Test reste autour de 17%. Le SVM fait sans doute de l'overfeeting...
