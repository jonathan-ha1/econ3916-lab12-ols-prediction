# OLS as a Line of Best Fit: Estimation versus Prediction

## Objective
I used California Housing data to show that the same regression machinery can either estimate a coefficient or predict an outcome, and that each job needs its own scorecard.

## Methodology
- Fitted one line of median house value on median income three ways (the closed-form formula, statsmodels and scikit-learn) and checked that all three gave the same intercept and slope.
- Added 0, 1, 5, 20 and 100 columns of pure random noise beside median income, refitted each time, and compared R-squared with adjusted R-squared.
- Split the data into training and test rows before fitting anything, fitted an eight-feature model on the training rows only, and scored it on both sets.
- Used one specification, house value on median income and rooms per household, for two jobs: estimating the AveRooms coefficient with its 95% confidence interval, and predicting house values on test rows.
- Wrote a function, holdout_rmse(), that splits the data the same way every time, fits on the training rows and returns the test RMSE, then used it to score median income on its own.

## Key Findings
- **Three routes, one line:** all three methods gave a slope of 0.4179, so a 10,000 dollar rise in median income is associated with about a 41,790 dollar rise in median house value.
- **R-squared can't fall:** R-squared rose from 0.4734 with no noise to 0.4758 with 100 noise columns, even though the extra columns were random numbers. That's why I can't pick a model by in-sample R-squared.
- **Train vs. test:** the eight-feature model had a train RMSE of 0.7197 and a test RMSE of 0.7456. The two are close, so the model isn't badly overfitting, but a typical error of about 74,560 dollars is too large to price a house with.
- **Estimation vs. prediction:** the AveRooms coefficient was −0.0381, with a 95% CI of [−0.0430, −0.0333]. The interval excludes zero, so the estimate is precise, but a significant coefficient doesn't mean the model predicts well.
- **Median income alone:** holdout_rmse() gave a test RMSE of 0.8421 for median income on its own, about 9,650 dollars worse than all eight features. Because both models were scored on the same unseen test rows, this comparison is fair in a way in-sample R-squared isn't.
