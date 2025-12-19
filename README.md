# Fourth Down Probability Estimation

This repository contains code and analysis for modeling the probability of successfully converting a fourth-down attempt in American football using play-by-play data. The goal is to provide probabilistic guidance that could be integrated into a larger decision-support system for fourth-down strategy.

## Motivation

American football is a team sport played between two teams of eleven players on a rectangular field with an end zone at each end. The game is divided into four quarters, and the objective is to advance the ball into the opponent’s end zone to score points via a touchdown or field goal. Play is organized into discrete segments called “downs,” with each team given four attempts to advance the ball at least ten yards. Success awards a new set of downs, while failure results in turnover of possession. Each play involves strategy, with the offense attempting to gain yards and the defense trying to prevent progress.

A particularly high-stakes situation occurs on **fourth down**, where the offensive team must decide between punting, attempting a field goal, or “going for it.” Going for it is a risky gamble: success grants a new set of downs and maintains the chance to score a touchdown, while failure gives the opposing team possession with favorable field position, potentially leading to a quick score. Field position, game context, and time remaining all influence this decision. In recent years, NFL teams have become more aggressive and are increasingly willing to take risks on fourth down, reflecting a shift toward data-driven strategies.

Historically, these decisions were made largely by gut instinct. Today, many football organizations employ data science teams and sophisticated win probability models to guide in-game decisions. The goal of this project is to develop a **probabilistic model that estimates the likelihood of successfully converting a fourth down when going for it**. While the model does not make final decisions, it provides **valuable probability estimates that can serve as a component of a larger decision-support system**, helping coaches evaluate risk and reward in real time. 

Because this model is intended as one piece of a broader system, its moderate predictive scores do not diminish its usefulness. Even a moderately accurate probability estimate can meaningfully inform fourth-down decisions when combined with additional contextual factors such as score differential, field position, and remaining time.

## Project Overview

The project focuses on **estimating the probability of a successful conversion** using key game-state features such as:

- `togo` — yards to the next first down or end zone  
- `yardline_100` — distance from opponent's end zone  
- `play_type` — run or pass (including scrambles and sacks)  
- `posteam` / `defteam` — offensive and defensive teams (target encoded)

The predicted probabilities can later be combined with additional contextual factors to support fourth-down decisions.

## Data

The project uses the [nflfastR dataset](https://github.com/mrcaseb/nflfastR) accessed via the Python `nflreadpy` package. Plays from the 2019–2024 seasons were used for training, and the 2025 season was reserved for testing to provide realistic temporal generalization.

## Models and Results

Three types of models were explored:

| Model                  | Accuracy | F1 Score | ROC-AUC | Brier Score |
|------------------------|---------|----------|---------|------------|
| Logistic Regression    | 0.6471  | 0.7037   | 0.6865  | 0.2184     |
| Random Forest          | 0.6446  | 0.6934   | 0.6903  | 0.2194     |
| Neural Network (PyTorch)| ~0.647 | ~0.704   | 0.6866  | 0.2191     |

> **Note:** Neither the Random Forest nor the neural network improved meaningfully over the logistic regression baseline, suggesting that the predictive signal in the available features is largely linear.

## Key Insights

- Logistic regression provides a **strong and interpretable baseline** for estimating fourth-down conversion probabilities.  
- Complex models like Random Forests and neural networks did not yield meaningful improvements with the current feature set.  
- The project demonstrates that probability-based modeling of fourth-down conversions is feasible and provides a foundation for further improvements.  
- Because the model is intended as a **component of a larger decision-support system**, even moderately accurate probability estimates are valuable for informing fourth-down decisions.  
- Future enhancements could include additional contextual features such as score differential, game quarter, weather conditions, and dynamic team statistics, which may enable more accurate probability estimates.
