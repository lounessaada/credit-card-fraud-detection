# 💳 Credit Card Fraud Detection (Machine Learning)

## 🎯 Objectif du projet
Ce projet vise à préparer un dataset propre, transformé et équilibré afin d’entraîner un modèle de Machine Learning capable de détecter des transactions bancaires frauduleuses.

Le contexte est celui de la fraude bancaire européenne, un phénomène rare mais coûteux, où :
- les fraudes non détectées (FN) entraînent des pertes financières importantes,
- les fausses alertes (FP) génèrent des coûts opérationnels élevés.

## 📊 Description du dataset
- Nombre de lignes : 284 807
- Nombre de variables : 31
- Variable cible : `Class`
  - 0 : transaction normale
  - 1 : transaction frauduleuse
- Variables :
  - V1 à V28 : variables issues d’une PCA
  - Amount : montant de la transaction
  - Time : temps en secondes

La fraude représente environ **0,17 %** des transactions, ce qui rend le dataset fortement déséquilibré.

## ⚠️ Problématique et métriques
L’accuracy n’est pas pertinente dans ce contexte.
Les métriques adaptées sont :
- **ROC-AUC**
- **PR-AUC (métrique prioritaire)**, centrée sur la classe rare (fraude)

## 🔍 Analyse exploratoire (EDA)
- `Amount` est asymétrique et contient des outliers.
- Les fraudes se concentrent souvent sur :
  - de très petits montants (transactions de test),
  - de très grands montants.
- Analyse par déciles de `Amount` : distribution en **forme de U** du risque de fraude.
- Extraction de l’heure depuis la variable `Time`.

## 🧪 Séparation Train / Test (anti data leakage)
- Split effectué avant toute transformation.
- Split stratifié pour conserver le même taux de fraude.
- Objectif : éviter toute fuite d’information.

## 🔧 Transformations appliquées
- `log1p(Amount)` pour réduire l’asymétrie
- `RobustScaler` pour gérer les outliers
- Transformation de Yeo-Johnson sur certaines variables V*

## 🧠 Feature engineering (logique métier)
- `Hour` : heure extraite depuis `Time`
- `is_very_small_amount` : Amount < Q10
- `is_very_large_amount` : Amount > Q90

Les seuils Q10 et Q90 sont calculés uniquement sur le jeu d’entraînement.

## ⚖️ Gestion du déséquilibre des classes
Méthodes testées :
- Baseline (sans rééchantillonnage)
- `class_weight='balanced'`
- Random UnderSampling
- SMOTE

## 📈 Résultats
La métrique principale retenue est la **PR-AUC**.

- Baseline : PR-AUC ≈ 0.74 (bonne précision, recall plus faible)
- SMOTE : PR-AUC ≈ 0.73 (recall élevé, nécessite ajustement du seuil)
- UnderSampling : PR-AUC ≈ 0.62 (performance dégradée)

## 🎚️ Ajustement du seuil
- Seuil par défaut (0.5) → trop de faux positifs
- Seuil optimisé : **0.95**
- Résultat : forte réduction des faux positifs tout en conservant un recall élevé (~0.89)

## ✅ Validation finale
- Dataset final prêt pour le Machine Learning
- 33 variables finales
- Train : 227 845 lignes
- Test : 56 962 lignes

## 🏁 Conclusion
Ce projet respecte les bonnes pratiques du Machine Learning :
- prévention du data leakage,
- transformations adaptées,
- métriques pertinentes pour un dataset déséquilibré.

Il montre que, dans un contexte de fraude bancaire, la **PR-AUC** et l’**ajustement du seuil** sont essentiels pour obtenir un modèle exploitable métier.

## 👥 Auteurs
- Yazid Aloui  
- Yanis Hanouti  
- Lounes Saada  
- Omar El Mejdoubi El Imam  

