# Exploring Github API

<br />

Usage demonstration of some base python functionalities to compute results on data from API.

<br />

### Computed results:
1. Total stargazers count per language
2. Average Stargazers-count per Language
3. Language-wise ratio of forks to watchers and individual averages of each
4. 'Repo Bigness' and its effect on %age contribution to total watchers of its respective language

<br />

### Key highlights:
1. Approach
2. Documentation
3. Attention to forward compatibility
4. (one of the) Simple solution to **_Windowing_** problem.


<br />

## [Solution IPython notebook can be found here](https://github.com/zeeshanhs/github-api-explore/blob/master/Github_API_explore.ipynb) 

<br />
<br />
---

<br />
<br />

## Total stargazers count per language

<br />

Basic summation of stargazers on each language. The solution makes use of:

- filter
- sorted

<br />
<br />

## 2. Average Stargazers-count per Language

A basic calculation of average stargazers-count per language and then display top 3 languages as result. Formula for calculation is given below:

> formula of average stargazers per language = (total stargazers for language)/(repo count of language)

<br />

### Key areas:

1. Kept "keys_of_interest" as set to use 'issubset' - optimization for removing invalids (for demonstration only)
2. Projected data from dict to tuple - remove unnecessary data
3. column_wise_sum(): removes hardcoding of which columns to sum. Also unifies logic in 1 place for exception handling. Supports any number of columns to find creative summations.

<br />
<br />

## 3. Language-wise ratio of forks to watchers and individual averages of each

Refer to the ratio of forks to watchers as 'Conversion'.

<br />

This is really do demonstrate the re-usability of the approach I used in earlier example. Only data projection step will differ and final formula since we are calculating on multiple columns but rest stays the same as it is forward compatible.

This is usually my preferred approach of coding. Keep things nicely atomic so changes are centralized. And keep Tier 1, 2, and 3 functions where T1 are atomic functions, T2 uses T1 functions, and T3 is mostly pipeline structured that group multiple operations together.

> column_wise_sum() function will really come in handy here as we will be calculating sum of 5 columns at once.


<br />
<br />

## 4. Display 'bigness' in descending order of the %age contribution to total watchers of its respective language

<br />

Computed an arbitrary column 'bigness' for demonstration; categorized repos to 'small', 'medium', and 'large' on the basis of their 'size' attribute. The idea was to compute two aggregates with different grain on data in efficient way, i.e., one grouping over 'bigness' and other to calculate total over 'language'.

Suggested methodology computes aggregates in O(N) on a generator function and then uses the dictionary representation for data that is easy to display output with.

<br />

Operation looks like:


```
e.g.,
Last 2 columns are computed

bigness      | watchers  | language  | language_watchers  | bigness_contribution
................................................................................
medium       | 200       | Python    | 500                | 40%
small        | 300       | Python    | 500                | 60%
medium       | 150       | Ruby      | 200                | 75%
large        | 50        | Ruby      | 200                | 25%


RESULT:
bigness    | watchers  | language  | language_watchers  | bigness_contribution
..............................................................................
medium     | 150       | Ruby      | 200                | 75%
small      | 300       | Python    | 500                | 60%
medium     | 200       | Python    | 500                | 40%
large      | 50        | Ruby      | 200                | 25%
```