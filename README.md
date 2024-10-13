# TL;DR

_**IMPORTANT: The values provided in this spreadsheet are inferred, not provided by SFUSD or Stanford CEPA.**_

https://docs.google.com/spreadsheets/d/1IItX67J8q2NNqTC_N5C9TQ3S-CJtyc0sBRMlFn-Dlgo/edit?usp=sharing

# Background

On 2024-10-08, San Franicsco Unified School District (SFUSD) released a provisional list of schools they will recommend for closure in at the end of the 2024-2025 academic year.

Their list was generated by (a) filtering schools with enrollment lower than 260 and (b) picking the schools with the lowest "composite score".

The composite scores were shared with the community in a few ways:
* The final scores were shared alongside each school
* SFUSD linked to a [map application](https://pyj2z6-michael-chrzan.shinyapps.io/Map-App/) which listed _both_ the composite scores and the _rank_ of each of the component scores.

The composite score methodology paper was also shared by the district. That document is included in this repository.

# Problem Statement

The data was extrated from the map application into [a spreadsheet](https://docs.google.com/spreadsheets/d/1IItX67J8q2NNqTC_N5C9TQ3S-CJtyc0sBRMlFn-Dlgo/edit?usp=sharing), which was then used to run a convex optimization problem.

We know that the composite score for school _i_ (`c_values[i]`) is:

```python
c_values[i] = 0.5 x_equ[i] + 0.25 x_exc[i] + 0.25 * x_eff[i]
```

We can back out the values for `x_equ[i]` (equity score), `x_exc[i]` (excellence score), and `x_eff` (effective use of resources score) by finding the solution that minimizes

```python
0.5 * cp.sum_squares(0.5 * x_equ + 0.25 * x_exc + 0.25 * x_eff - c_values)
```

subject to a set of contraints that ensure that the ranks of the computed scores are consistent with the provided ranks.

# Using this repository

- `cvx.ipynb` has the core statement of the problem in a python jupyter notbebook
- `requirements.txt` defines the python package versions used to run this notebook
- `scores.txt` lists the raw scores as outputted from running `cvx.ipynb`






_______

<p xmlns:cc="http://creativecommons.org/ns#" xmlns:dct="http://purl.org/dc/terms/"><a property="dct:title" rel="cc:attributionURL" href="https://github.com/pariser/sfusd-composite-score">SFUSD Composite Score Reverse Engineering</a> by <a rel="cc:attributionURL dct:creator" property="cc:attributionName" href="https://github.com/pariser">Andrew Pariser</a> is licensed under <a href="https://creativecommons.org/licenses/by/4.0/?ref=chooser-v1" target="_blank" rel="license noopener noreferrer" style="display:inline-block;">CC BY 4.0<img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/cc.svg?ref=chooser-v1" alt=""><img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/by.svg?ref=chooser-v1" alt=""></a></p>
