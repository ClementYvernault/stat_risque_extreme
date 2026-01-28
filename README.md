# 📈 Analyse de la Value-at-Risk (VaR) : Renault - Crise 2008

Ce projet compare l'efficacité de la **Loi Normale** (standard) face à la **Loi Skew-Student** (avancée) pour modéliser le risque de marché lors d'une crise systémique.

---

## ⚖️ Stratégie de Comparaison (Ceteris Paribus)

Pour isoler l'impact de la distribution statistique, nous comparons les deux lois au sein de trois cadres méthodologiques identiques.

### 1. Approche Statique (Seuil Fixe)
* **Méthode :** Calcul de la VaR sur la base des paramètres $(\mu, \sigma)$ constants de la période d'apprentissage.
* **Comparaison :** VaR Normale vs VaR Skew-Student.
* **Objectif :** Isoler l'effet des **queues de distribution** (Fat Tails).
* **Observation :** Bien que la Skew-Student soit plus prudente, l'absence de mise à jour de la volatilité conduit les deux modèles à un taux d'échec élevé (>15%).

### 2. Approche Dynamique (EWMA)
* **Méthode :** Mise à jour quotidienne de la volatilité $\sigma_t$ via un facteur de lissage $\lambda$ (0.90, 0.95, 0.99).
* **Comparaison :** VaR EWMA Normale vs VaR EWMA Skew-Student.
* **Objectif :** Mesurer la **réactivité** du modèle face à l'explosion de la volatilité en 2008.
* **Observation :** C'est le modèle le plus performant. La Skew-Student affine les résultats de l'EWMA en capturant mieux les krachs extrêmes résiduels.

### 3. Approche Prospective (Diffusion Monte Carlo 10j)
* **Méthode :** Simulation de 10 000 trajectoires à 10 jours suivant le processus :
    $$dS_t = S_t (\mu dt + \sigma \epsilon_t \sqrt{dt})$$
* **Comparaison :** Chocs gaussiens $\epsilon \sim N(0,1)$ vs Chocs Skew-Student $\epsilon \sim ST(\nu, \gamma)$.
* **Objectif :** Évaluer le **Capital Réglementaire** (Bâle III).
* **Observation :** La loi Normale sous-estime drastiquement les pertes à 10 jours, contrairement à la Skew-Student qui génère des scénarios extrêmes réalistes.

---

## 📊 Tableau de Synthèse des Modèles

| Cadre | Loi Normale | Loi Skew-Student | Focus |
| :--- | :--- | :--- | :--- |
| **Statique** | Faible protection | Meilleure forme (Fat Tails) | Forme de la distribution |
| **Dynamique** | Réactif | **Optimal (Réactif + Précis)** | Gestion de crise |
| **Diffusion** | Optimiste (Risque caché) | Réaliste (Risque extrême) | Horizon temporel |

---

## 🚀 Conclusion
La performance d'un modèle de risque ne repose pas uniquement sur la loi choisie, mais sur sa capacité à être **dynamique**. Pour l'action Renault en 2008, le modèle **EWMA Skew-Student** s'impose comme la seule solution capable de satisfaire les exigences de backtesting réglementaire.
