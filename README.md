# Portefeuille actions simulé (2 M€)

Projet académique ESME (parcours finance, 3e année), octobre à décembre 2025 : gestion d'un portefeuille fictif de 2 M€ sur une trentaine d'actions américaines et européennes, avec analyse fondamentale, ordres stop-loss / take-profit automatisés et rémunération du cash.

Le projet a d'abord été construit sous Excel. Ce dépôt en propose une version Python qui :

1. reproduit les calculs du classeur à quelques euros près
2. corrige deux limites du modèle Excel (taux de change de fin de semaine, prix d'exécution des ordres)
3. calcule les indicateurs de performance et de risque face aux indices
4. teste la sensibilité des seuils stop-loss / take-profit, et montre le biais d'un backtest sur données hebdomadaires

## Résultats (semaines 1 à 7)

| | Rendement | Volatilité hebdo | Drawdown max |
|---|---|---|---|
| Portefeuille (méthode corrigée) | +1,47 % | 1,30 % | −2,24 % |
| Portefeuille sans ordres SL/TP | +1,88 % | 1,40 % | −2,25 % |
| Indice ESME (référence du cours) | −0,35 % | 1,06 % | −1,73 % |
| CAC 40 | −0,37 % | 1,12 % | −1,74 % |
| S&P 500 (en USD) | +2,66 % | 2,14 % | −3,52 % |

Avec la méthode corrigée, le portefeuille fait mieux que l'Indice ESME 5 semaines sur 7. Sur 7 semaines, ces chiffres ne sont pas statistiquement significatifs : l'intérêt du projet est la méthode.

## Structure

```
portefeuille_simule.ipynb   notebook principal (exécuté, avec graphiques)
data/semaines.csv           dates, taux EUR/USD, seuils SL/TP, capitaux du classeur
data/positions.csv          positions hebdomadaires et ordres exécutés
data/indices.csv            Indice ESME, CAC 40, S&P 500
```

## Lancer le notebook

- **Google Colab** : ouvrir le notebook, créer un dossier `data` dans le panneau Fichiers et y déposer les trois CSV.
- **En local** : `pip install pandas numpy matplotlib jupyter`, puis `jupyter notebook portefeuille_simule.ipynb`.

## Limites

Données hebdomadaires (la règle SL/TP ne voit que le cours de fin de semaine), pas de frais de transaction ni de dividendes, portefeuille simulé. Prochaine étape : backtest sur cours journaliers.
