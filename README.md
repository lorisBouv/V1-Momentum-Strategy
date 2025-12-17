# V1 Momentum Strategy

## Quantitative Long/Short Trading System

Momentum trading framework développé avec une approche quantitative systématique sur un univers d'actions sur plusieurs indices mondiaux.

---

##  Objectif du Projet

Système de trading quantitatif Long/Short Momentum sur un univers d'actions multi-indices avec **position sizing dynamique** et **risk management institutionnel**.

**Le but de ce projet est de le connecter à un portefeuille réel dans le futur.**

---

##  Approche

- **Méthodologie quantitative** : Signal generation basé sur momentum avec filtres statistiques avancés (Momentum, régression linéaire avec slope, coefficient de détermination, autocorrélation)

- **Backtesting rigoureux** : Walk-forward testing, validation In/Out-of-Sample avec plusieurs choix de split et d'indices

- **Risk management professionnel** : Position sizing adaptatif (KELLY), TP/SL, drawdown control

- **Architecture modulaire** : Code organisé, reproductible, scalable, utilisable en réel

---

##  Données Historiques

| Caractéristique | Détail |
|----------------|--------|
| **Source** | EOD Historical Data (EODHD.COM) |
| **Période** | 24 ans de données (2000-2024) |
| **Volume** | ~7+ millions de lignes de prix historiques |
| **Indices** | SP500, NASDAQ100, DOW30, CAC40, DAX40, FTSE100, HANGSENG |
| **Qualité** | Nettoyage automatique, standardisation, filtres de qualité |

---

##  Méthodologie Quantitative

### 1. Signal Generation

Architecture à **deux niveaux** :

#### Niveau 1 : Signaux Momentum

Détection de configurations directionnelles fortes basées sur performance passée.

- **Long signals** : Momentum haussier significatif
- **Short signals** : Momentum baissier significatif

#### Niveau 2 : Filtres Quantitatifs

Application de filtres statistiques pour améliorer la qualité des signaux :

- Coefficient de détermination pour valider la linéarité du trend avec autocorrélation
- Régression linéaire et slope pour confirmer la direction
- **Élimination systématique des faux signaux (~60% de filtrage)**

**Features statistiques :**

- Régression linéaire sur séries temporelles (log-prix)
- Mesures de qualité de trend
- Confirmations directionnelles multiples

---

### 2. Position Sizing

**Approche : Kelly Criterion avec paliers adaptatifs**

**Solution implémentée :**

- Système adaptatif selon le capital actuel et les résultats quantitatifs
- Transition progressive entre modes conservateur et agressif suivant les signaux et la force directionnelle

---

### 3. Risk Management

#### Exit Logic

- **Profit Targets** : Take profit (%)
- **Stop Loss** : Protection drawdown par trade (%)
- **Time-based Exit** : Max holding period pour éviter positions stagnantes

#### Portfolio Constraints

- **Position Limits** : Nombre maximum de positions simultanées
- **Concentration Limits** : Caps par position en % et en valeur absolue
- **Transaction Costs** : Intégrés dans le backtesting

#### Risk Metrics Monitored

- Drawdown en temps réel
- Exposition nette Long/Short
- Volatility tracking

---

##  Backtesting Framework

### Walk-Forward Testing

Le système permet de valider la robustesse de la stratégie sur plusieurs périodes sans overfitting.

**Approche :**

- **In-Sample** : Développement et optimisation sur période historique
- **Out-of-Sample** : Validation sur période non vue
- **Multiple Splits** : Tests possibles sur 7+ configurations temporelles différentes

**Exemple de Périodes testables :**

- Full period (2000-2024)
- Post-crisis splits (2008, 2020)
- Recent periods (2018-2024, 2022-2024)

**Objectif :** Vérifier que la stratégie performe de manière cohérente sur différents régimes de marché.

---

##  Performance Metrics

Le système calcule automatiquement un ensemble complet de métriques :

### Performance

- Total Return, Annual Return, CAGR
- Risk-Free Rate adjusted returns
- Heatmaps rendements

### Risk-Adjusted Returns

- **Sharpe Ratio** : Mesure ajustée entre le risque et la performance
- **Sortino Ratio** : Focus sur downside risk
- **Calmar Ratio** : Return vs maximum drawdown

### Risk Metrics

- Maximum Drawdown (% et valeur absolue)
- Volatility (annualisée)
- Downside deviation

### Trading Statistics

- Win Rate (global, par direction Long/Short)
- Profit Factor (global, par direction)
- Average Win/Loss ratio
- Skewness et Kurtosis
- Trade frequency & duration (min, moyenne, max)
- Alpha et benchmark par rapport au Buy and Hold

---

##  Architecture Technique

### Structure du Projet

```
V1/
│
├── README.md                       # Documentation
│
├── src/                            # Code source
│   ├── config.py                   # Configuration centralisée
│   ├── datasreader.py              # Data loading multi-indices
│   ├── signals.py                  # Signal generation (simple + advanced)
│   ├── backtest.py                 # Backtesting engine avec Kelly
│   ├── performance.py              # Metrics & visualizations
│   └── main.py                     # Point d'entrée orchestration
│
├── WORLD_DATA1/                    # Données historiques
│   ├── SP500/
│   ├── NASDAQ100/
│   ├── DOW30/
│   ├── CAC40/
│   ├── DAX40/
│   ├── FTSE100/
│   └── HANGSENG/
│
└── V1_RESULTS/                     # Outputs (auto-générés)
    ├── graphs/                     # Dashboards PNG
    ├── *_trades.csv                # Détail trades
    └── *_metrics.txt               # Performance metrics
```

### Development Approach

- AI-assisted development (Claude 4.5 pour architecture & optimisations)
- Continuous refactoring for clean code

---

##  Évolutions Futures

### Advanced Analytics

- Machine Learning integration
- Regime detection (trend vs mean-reversion adaptation)
- Portfolio optimization (multi-strategy allocation)

### Operational

- Live trading integration (API connectivity)
- Real-time monitoring dashboard (Streamlit/Dash)
- Automated reporting & alerts

---

##  Example Results Format

### Benchmark Comparison

```
================================================================================
                             BENCHMARK BUY & HOLD                             
================================================================================

🔍 Indice détecté: CAC40
📈 Benchmark chargé: XXXX jours de données

================================================================================
                           STRATÉGIE vs BUY & HOLD                            
================================================================================

┌─ RETURNS ────────────────────────────────────────────────────────────┐
│ Stratégie          :   XXX.XX% ( X% annuel)                          │
│ Buy & Hold         :    XX% ( X% annuel)                             │
│ ALPHA              :     X% ( X% annuel)                             │
└───────────────────────────────────────────────────────────────────────┘

┌─ CAPITAL ────────────────────────────────────────────────────────────┐
│ Stratégie          :   XXXXXXX €                                     │
│ Buy & Hold         :   XXXXXXX €                                     │
│ Outperformance     :     XXXX € (XXX.XX%)                            │
└───────────────────────────────────────────────────────────────────────┘

┌─ RISK METRICS ───────────────────────────────────────────────────────┐
│ Sharpe Stratégie   :      X                                          │
│ Sharpe Buy & Hold  :      X                                          │
│ Sharpe Ratio       :      X.XXx                                      │
│ Max DD Stratégie   :   -XX.XX%                                       │
│ Max DD Buy & Hold  :   -XX.XX%                                       │
└───────────────────────────────────────────────────────────────────────┘
```

### Performance Report

```
================================================================================
                       RAPPORT DE PERFORMANCE                        
================================================================================

┌─ PERFORMANCE ───────────────────────────────────────────────────────┐
│ Capital Initial        :   100,000 €                                │
│ Capital Final          :         X €                                │
│ Total Return           :         X%                                 │
│ Annual Return          :         X%                                 │
│ CAGR                   :         X%                                 │
│ Duration               :     X years                                │
└───────────────────────────────────────────────────────────────────────┘

┌─ TRADING STATS ─────────────────────────────────────────────────────┐
│ Total Trades           :         X                                  │
│ Long Trades            :         X ( X%)                            │
│ Short Trades           :         X ( X%)                            │
│ Win Rate               :      X%                                    │
│   • Long Win Rate      :      X%                                    │
│   • Short Win Rate     :      X%                                    │
│ Avg Win                :      X%                                    │
│ Avg Loss               :     -X%                                    │
│ Profit Factor          :      X                                     │
│   • Long Profit Factor :      X                                     │
│   • Short Profit Factor:      X                                     │
│ Avg Holding Days       :      X                                     │
│   • Long (min/avg/max) :  X / X / X days                            │
│   • Short (min/avg/max):  X / X / X days                            │
└───────────────────────────────────────────────────────────────────────┘

┌─ RISK METRICS ───────────────────────────────────────────────────────┐
│ Sharpe Ratio           :      X                                     │
│ Sortino Ratio          :      X                                     │
│ Calmar Ratio           :      X                                     │
│ Max Drawdown           :     -X%                                    │
│ Max Drawdown (€)       :     -X €                                   │
│ Volatility (annual)    :      X%                                    │
│ Skewness               :      X                                     │
│ Kurtosis (excess)      :      X                                     │
└───────────────────────────────────────────────────────────────────────┘

┌─ POSITION SIZING ────────────────────────────────────────────────────┐
│ Position moyenne       :      X € (X%)                              │
│ Position max           :      X €                                   │
│ Position min           :      X €                                   │
└───────────────────────────────────────────────────────────────────────┘
```

### Output Files

```
 Génération des graphiques...
    Dashboard sauvegardé: V1_RESULTS/graphs/Momentum_Advanced_Full_Dashboard.png
    Trades sauvegardés: V1_RESULTS/Momentum_Advanced_Full_trades.csv
    Métriques sauvegardées: V1_RESULTS/Momentum_Advanced_Full_metrics.txt

================================================================================
                             BACKTEST FULL TERMINÉ                             
================================================================================

 Résumé:
   • Stratégie: Momentum_Advanced_Full
   • Actions analysées: X
   • Trades: X
   • Win rate: X%
   • Sharpe Ratio: X
   • Max Drawdown: -X%
   • Annual Return: X%

 Fichiers sauvegardés dans: V1_RESULTS/
   • Momentum_Advanced_Full_Dashboard.png
   • Momentum_Advanced_Full_trades.csv
   • Momentum_Advanced_Full_metrics.txt

================================================================================
                            ANALYSE COMPLÈTE !                            
================================================================================
``'
