# Mixed effects models

Notes from [a kaggle kernel](https://www.kaggle.com/ojwatson/mixed-models)

---

## Question being asked:

> What effect does age have on people leaving our website?

## Data we have

| age | county | location | bounce_time |
| --- | --- | --- | --- |
| 16 | devon | a | 165.54 |
| 34 | devon | a | 167.55 |

* Age: age of visitor (years)
* County: County in London
* Location: Anonymized location within county
* bounce_time: how long session was in seconds

---

# Model 1:

$$ y = b_0 + b_1 x $$
where $x$ is age.

- $b_1$ is the effect we are looking for....
- but county is a confounding factor
- residuals show pattern

---

# Model 2:

$$ y = b_{0c} + b_{1c} x$$

For each county $c$, we have the effect of age encoded in $b_{1c}$. Lose a lot of information for county's where we don't have much data.

---

# Model 3:

Treat age as a *fixed effect* (the thing we want to model). If we reran the experiment with different subjects, we would expect the components associated with age to stay the same.

Treat county and location as *random effects*. They are confounding factors that we don't really care about but need to control for.

We have
`\begin{align}
y &= b_{c0} + b_{c1} x\\
b_{c0} &\sim N(\mu_{b_0}, \sigma_{b_0}^2)\\
b_{c1} &\sim N(\mu_{b_1}, \sigma_{b_1}^2)
\end{align}`

---

# Running the model

```
import statsmodels.api as sm
import statsmodels.formula.api as smf

# construct our model, with our county now shown as a group
md = smf.mixedlm("bounce_time ~ age_scaled", data, groups=data["county"])
mdf = md.fit()
print(mdf.summary())
```

---

# Product problem

```
rating ~ store
```

i.e. looking for the store effect on ratings (where store is a categorical variable)
