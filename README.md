# Portefeuille actions simulé (2 M€)

Projet académique ESME (parcours finance, 3e année), octobre à décembre 2025 : gestion d'un portefeuille fictif de 2 M€ sur une trentaine d'actions américaines et européennes, avec analyse fondamentale, ordres stop-loss / take-profit automatisés et rémunération du cash.

Projet initial (Excel) réalisé dans le cadre d'un travail de groupe à l'ESME. Version Python, vérification et analyse : Amaya Garo.

## Ce que fait la version Python

1. **`portefeuille_simule.ipynb`** reproduit les calculs du classeur à quelques euros près, corrige deux erreurs du modèle Excel (taux de change de fin de semaine, prix d'exécution des ordres) et calcule performance et risque sur données hebdomadaires.
2. **`backtest_journalier.ipynb`** passe aux cours journaliers (yfinance) : contrôle des tickers, vrai jour de déclenchement des ordres à partir des plus hauts et plus bas (gaps compris), volatilité, VaR historique et ratio de Sharpe.

## Résultats (semaines 1 à 7, du 1er octobre au 11 décembre 2025)

| Scénario | Rendement |
|---|---|
| Modèle hebdomadaire, règles SL/TP du projet | +1,47 % |
| **Modèle journalier, règles SL/TP du projet** | **−1,05 %** |
| Mêmes positions, sans aucun ordre SL/TP | +1,88 % |
| CAC Next 20 (colonne « CAC » du classeur) | −0,37 % |
| CAC 40 (Yahoo) | +1,49 % |
| S&P 500 (Yahoo, en USD) | +2,83 % |

**Conclusion principale.** Sur cours hebdomadaires, les règles stop-loss / take-profit semblaient neutres. Sur cours journaliers, elles déclenchent 17 ordres au lieu de 6, surtout des stop-loss pendant le repli de mi-novembre suivi d'un rebond : elles coûtent environ 3 points face aux mêmes positions sans ordres. Le modèle hebdomadaire ne voyait que le cours de clôture de la semaine et masquait ces ventes. Dans la grille de sensibilité, presque aucun couple de seuils ne bat l'absence d'ordres, et les stop-loss serrés sont les plus pénalisés.

Risque mesuré au jour le jour (modèle journalier, règles du projet) : volatilité annualisée 8,4 %, drawdown maximal −4,3 %, VaR historique 95 % à un jour de 0,90 % (environ 18 k€ sur 2 M€). La faible volatilité s'explique par une part de cash importante (40 à 100 % investi selon les semaines).

Sur 7 semaines, aucun de ces chiffres n'est statistiquement significatif : l'intérêt du projet est la méthode et la mise en évidence du biais.

## Structure

```
portefeuille_simule.ipynb   notebook 1 : reproduction, corrections, données hebdomadaires
backtest_journalier.ipynb   notebook 2 : cours journaliers (nécessite Internet)
data/semaines.csv           dates, taux EUR/USD, seuils SL/TP, capitaux du classeur
data/positions.csv          positions hebdomadaires et ordres exécutés
data/indices.csv            Indice ESME, CAC Next 20, S&P 500
```

La colonne « CAC » du classeur correspond au CAC Next 20 (environ 11 700 points à l'époque), et non au CAC 40; le notebook 2 compare au CAC 40 téléchargé sur Yahoo.

## Lancer les notebooks

- **Google Colab** : ouvrir le notebook, créer un dossier `data` dans le panneau Fichiers et y déposer les trois CSV.
- **En local** : `pip install pandas numpy matplotlib yfinance jupyter`, puis `jupyter notebook`.

## Limites et pistes

Ordre des cours dans la journée inconnu (si les deux seuils sont touchés le même jour, le stop-loss est retenu), pas de frais de transaction ni de dividendes, une seule période de 7 semaines. Pistes : frais de courtage, test hors échantillon sur une autre période, comparaison à un portefeuille équipondéré.
