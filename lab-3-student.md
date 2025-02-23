# Lab 3: Exploring Rodents with ggplot2
Michael Torrisi
2025-02-23

- [Part 1: Setup](#part-1-setup)
  - [GitHub Workflow](#github-workflow)
  - [Seeking Help](#seeking-help)
  - [Lab Instructions](#lab-instructions)
  - [Setup](#setup)
- [Part 2: Data Context](#part-2-data-context)
  - [Reading the Data into `R`](#reading-the-data-into-r)
- [Part 3: Exploratory Data Analysis with
  **`ggplot2`**](#part-3-exploratory-data-analysis-with-ggplot2)
  - [Scatterplot](#scatterplot)
  - [Boxplots](#boxplots)
- [Lab 3 Submission](#lab-3-submission)

# Part 1: Setup

## GitHub Workflow

Set up your GitHub workflow (either using the method of creating a
repository and using Version Control to set up your project or vice
versa using the `usethis` package commands we have learned).

Use appropriate naming conventions for your project (see Code Style
Guide), e.g. lab-3-ggplot2.

Your project folder should contain the following:

- `.Rproj`  
- `lab-3-student.qmd`  
- `data` folder
  - `surveys.csv`  
- rendered document (`.md`)

You will submit a link to your GitHub repository with all content.

## Seeking Help

Part of learning to program is learning from a variety of resources.
Thus, I expect you will use resources that you find on the internet.
There is, however, an important balance between copying someone else’s
code and *using their code to learn*. Therefore, if you use external
resources, I want to know about it.

- If you used Google, you are expected to “inform” me of any resources
  you used by **pasting the link to the resource in a code comment next
  to where you used that resource**.

- If you used ChatGPT, you are expected to “inform” me of the assistance
  you received by (1) indicating somewhere in the problem that you used
  ChatGPT (e.g., below the question prompt or as a code comment),
  and (2) downloading and attaching the `.txt` file containing your
  **entire** conversation with ChatGPT. ChatGPT can we used as a “search
  engine”, but you should not copy and paste prompts from the lab or the
  code into your lab.

Additionally, you are permitted and encouraged to work with your peers
as you complete lab assignments, but **you are expected to do your own
work**. Copying from each other is cheating, and letting people copy
from you is also cheating. Please don’t do either of those things.

## Lab Instructions

The questions in this lab are noted with numbers and boldface. Each
question will require you to produce code, whether it is one line or
multiple lines.

This document is quite plain, meaning it does not have any special
formatting. As part of your demonstration of creating professional
looking Quarto documents, **I would encourage you to spice your
documents up (e.g., declaring execution options, specifying how your
figures should be output, formatting your code output, etc.).**

## Setup

In the code chunk below, load in the packages necessary for your
analysis. You should only need the `tidyverse` package for this
analysis.

<details class="code-fold">
<summary>Code</summary>

``` r
library(tidyverse)
```

</details>

    ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
    ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
    ✔ purrr     1.0.2     
    ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ✖ dplyr::filter() masks stats::filter()
    ✖ dplyr::lag()    masks stats::lag()
    ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

# Part 2: Data Context

The Portal Project is a long-term ecological study being conducted near
Portal, AZ. Since 1977, the site has been used to study the interactions
among rodents, ants, and plants, as well as their respective responses
to climate. To study the interactions among organisms, researchers
experimentally manipulated access to 24 study plots. This study has
produced over 100 scientific papers and is one of the longest running
ecological studies in the U.S.

We will be investigating the animal species diversity and weights found
within plots at the Portal study site. The data are stored as a comma
separated value (CSV) file. Each row holds information for a single
animal, and the columns represent:

| Column            | Description                        |
|-------------------|------------------------------------|
| `record_id`       | Unique ID for the observation      |
| `month`           | month of observation               |
| `day`             | day of observation                 |
| `year`            | year of observation                |
| `plot_id`         | ID of a particular plot            |
| `species_id`      | 2-letter code                      |
| `sex`             | sex of animal (“M”, “F”)           |
| `hindfoot_length` | length of the hindfoot in mm       |
| `weight`          | weight of the animal in grams      |
| `genus`           | genus of animal                    |
| `species`         | species of animal                  |
| `taxon`           | e.g. Rodent, Reptile, Bird, Rabbit |
| `plot_type`       | type of plot                       |

## Reading the Data into `R`

We are going to use the `read_csv()` function to load in the
`surveys.csv` dataset (stored in the data folder). For simplicity, name
the data `surveys`. We will learn more about this function next week.

<details class="code-fold">
<summary>Code</summary>

``` r
surveys <- read_csv("data-raw/surveys.csv")
```

</details>

    Rows: 30463 Columns: 15
    ── Column specification ────────────────────────────────────────────────────────
    Delimiter: ","
    chr  (7): species_id, sex, day_of_week, plot_type, genus, species, taxa
    dbl  (7): record_id, month, day, year, plot_id, hindfoot_length, weight
    date (1): date

    ℹ Use `spec()` to retrieve the full column specification for this data.
    ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

<details class="code-fold">
<summary>Code</summary>

``` r
glimpse(surveys)
```

</details>

    Rows: 30,463
    Columns: 15
    $ record_id       <dbl> 63, 64, 65, 66, 67, 68, 69, 71, 74, 75, 78, 79, 81, 82…
    $ month           <dbl> 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, …
    $ day             <dbl> 19, 19, 19, 19, 19, 19, 19, 19, 19, 19, 19, 19, 19, 19…
    $ year            <dbl> 1977, 1977, 1977, 1977, 1977, 1977, 1977, 1977, 1977, …
    $ plot_id         <dbl> 3, 7, 4, 4, 7, 8, 2, 7, 8, 8, 1, 7, 4, 4, 6, 19, 23, 1…
    $ species_id      <chr> "DM", "DM", "DM", "DM", "DM", "DO", "PF", "DM", "PF", …
    $ sex             <chr> "M", "M", "F", "F", "M", "F", "M", "F", "M", "F", "M",…
    $ hindfoot_length <dbl> 35, 37, 34, 35, 35, 32, 15, 36, 12, 32, 16, 34, 14, 35…
    $ weight          <dbl> 40, 48, 29, 46, 36, 52, 8, 35, 7, 22, 9, 42, 8, 41, 37…
    $ date            <date> 1977-08-19, 1977-08-19, 1977-08-19, 1977-08-19, 1977-…
    $ day_of_week     <chr> "Fri", "Fri", "Fri", "Fri", "Fri", "Fri", "Fri", "Fri"…
    $ plot_type       <chr> "Long-term Krat Exclosure", "Rodent Exclosure", "Contr…
    $ genus           <chr> "Dipodomys", "Dipodomys", "Dipodomys", "Dipodomys", "D…
    $ species         <chr> "merriami", "merriami", "merriami", "merriami", "merri…
    $ taxa            <chr> "Rodent", "Rodent", "Rodent", "Rodent", "Rodent", "Rod…

**1. What are the dimensions (# of rows and columns) of these data?**

> There are 15 columns and 30,463 rows in this dataset.

**2. What are the data types of the variables in this dataset?**

> There are 7 character variables: species_id, sex, day_of_week,
> plot_type, genus, species, taxa. There are 7 double variables:
> record_id, month, day, year, plot_id, hindfoot_length, weight. There
> is 1 date variable.

# Part 3: Exploratory Data Analysis with **`ggplot2`**

`ggplot()` graphics are built step by step by adding new elements.
Adding layers in this fashion allows for extensive flexibility and
customization of plots.

To build a `ggplot()`, we will use the following basic template that can
be used for different types of plots:

<details class="code-fold">
<summary>Code</summary>

``` r
ggplot(data = <DATA>,
       mapping = aes(<VARIABLE MAPPINGS>)) +
  <GEOM_FUNCTION>()
```

</details>

Let’s get started!

## Scatterplot

**3. First, let’s create a scatterplot of the relationship between
`weight` (on the $x$-axis) and `hindfoot_length` (on the $y$-axis).**

<details class="code-fold">
<summary>Code</summary>

``` r
ggplot(data = surveys,
       mapping = aes(x = weight, y = hindfoot_length)) +
  geom_point() +
  theme_light()
```

</details>

![](lab-3-student_files/figure-commonmark/scatterplot-1.png)

We can see there are **a lot** of points plotted on top of each other.
Let’s try and modify this plot to extract more information from it.

**4. Let’s add transparency (`alpha`) to the points, to make the points
more transparent and (possibly) easier to see.**

<details class="code-fold">
<summary>Code</summary>

``` r
ggplot(data = surveys,
       mapping = aes(x = weight, y = hindfoot_length)) +
  geom_point(alpha = 0.25) +
  theme_light()
```

</details>

![](lab-3-student_files/figure-commonmark/scatterplot%20with%20alpha%20adjustment-1.png)

Despite our best efforts there is still a substantial amount of
overplotting occurring in our scatterplot. Let’s try splitting the
dataset into smaller subsets and see if that allows for us to see the
trends a bit better.

**6. Facet your scatterplot by `species`.**

<details class="code-fold">
<summary>Code</summary>

``` r
ggplot(data = surveys,
       mapping = aes(x = weight, y = hindfoot_length)) +
  geom_point(alpha = 0.25) +
  geom_jitter(alpha = 0.25) +
  facet_grid(.~species, scales = "free") +
  theme_light()
```

</details>

![](lab-3-student_files/figure-commonmark/scatterplot%20with%20alpha%20adjustment%20and%20facet-1.png)

**7. No plot is complete without axis labels and a title. Include reader
friendly labels and a title to your plot.**

<details class="code-fold">
<summary>Code</summary>

``` r
ggplot(data = surveys,
       mapping = aes(x = weight, 
                     y = hindfoot_length)) +
  geom_point(alpha = 0.25) +
  geom_jitter(alpha = 0.25) +
  facet_grid(.~species,
             scales = "free") +
  labs(x = "Weight (g)",
       y = "Hindfoot Length (cm)")
```

</details>

![](lab-3-student_files/figure-commonmark/scatterplot%20with%20alpha,%20facet,%20and%20labels-1.png)

<details class="code-fold">
<summary>Code</summary>

``` r
  theme_light()
```

</details>

    List of 136
     $ line                            :List of 6
      ..$ colour       : chr "black"
      ..$ linewidth    : num 0.5
      ..$ linetype     : num 1
      ..$ lineend      : chr "butt"
      ..$ arrow        : logi FALSE
      ..$ inherit.blank: logi TRUE
      ..- attr(*, "class")= chr [1:2] "element_line" "element"
     $ rect                            :List of 5
      ..$ fill         : chr "white"
      ..$ colour       : chr "black"
      ..$ linewidth    : num 0.5
      ..$ linetype     : num 1
      ..$ inherit.blank: logi TRUE
      ..- attr(*, "class")= chr [1:2] "element_rect" "element"
     $ text                            :List of 11
      ..$ family       : chr ""
      ..$ face         : chr "plain"
      ..$ colour       : chr "black"
      ..$ size         : num 11
      ..$ hjust        : num 0.5
      ..$ vjust        : num 0.5
      ..$ angle        : num 0
      ..$ lineheight   : num 0.9
      ..$ margin       : 'margin' num [1:4] 0points 0points 0points 0points
      .. ..- attr(*, "unit")= int 8
      ..$ debug        : logi FALSE
      ..$ inherit.blank: logi TRUE
      ..- attr(*, "class")= chr [1:2] "element_text" "element"
     $ title                           : NULL
     $ aspect.ratio                    : NULL
     $ axis.title                      : NULL
     $ axis.title.x                    :List of 11
      ..$ family       : NULL
      ..$ face         : NULL
      ..$ colour       : NULL
      ..$ size         : NULL
      ..$ hjust        : NULL
      ..$ vjust        : num 1
      ..$ angle        : NULL
      ..$ lineheight   : NULL
      ..$ margin       : 'margin' num [1:4] 2.75points 0points 0points 0points
      .. ..- attr(*, "unit")= int 8
      ..$ debug        : NULL
      ..$ inherit.blank: logi TRUE
      ..- attr(*, "class")= chr [1:2] "element_text" "element"
     $ axis.title.x.top                :List of 11
      ..$ family       : NULL
      ..$ face         : NULL
      ..$ colour       : NULL
      ..$ size         : NULL
      ..$ hjust        : NULL
      ..$ vjust        : num 0
      ..$ angle        : NULL
      ..$ lineheight   : NULL
      ..$ margin       : 'margin' num [1:4] 0points 0points 2.75points 0points
      .. ..- attr(*, "unit")= int 8
      ..$ debug        : NULL
      ..$ inherit.blank: logi TRUE
      ..- attr(*, "class")= chr [1:2] "element_text" "element"
     $ axis.title.x.bottom             : NULL
     $ axis.title.y                    :List of 11
      ..$ family       : NULL
      ..$ face         : NULL
      ..$ colour       : NULL
      ..$ size         : NULL
      ..$ hjust        : NULL
      ..$ vjust        : num 1
      ..$ angle        : num 90
      ..$ lineheight   : NULL
      ..$ margin       : 'margin' num [1:4] 0points 2.75points 0points 0points
      .. ..- attr(*, "unit")= int 8
      ..$ debug        : NULL
      ..$ inherit.blank: logi TRUE
      ..- attr(*, "class")= chr [1:2] "element_text" "element"
     $ axis.title.y.left               : NULL
     $ axis.title.y.right              :List of 11
      ..$ family       : NULL
      ..$ face         : NULL
      ..$ colour       : NULL
      ..$ size         : NULL
      ..$ hjust        : NULL
      ..$ vjust        : num 1
      ..$ angle        : num -90
      ..$ lineheight   : NULL
      ..$ margin       : 'margin' num [1:4] 0points 0points 0points 2.75points
      .. ..- attr(*, "unit")= int 8
      ..$ debug        : NULL
      ..$ inherit.blank: logi TRUE
      ..- attr(*, "class")= chr [1:2] "element_text" "element"
     $ axis.text                       :List of 11
      ..$ family       : NULL
      ..$ face         : NULL
      ..$ colour       : chr "grey30"
      ..$ size         : 'rel' num 0.8
      ..$ hjust        : NULL
      ..$ vjust        : NULL
      ..$ angle        : NULL
      ..$ lineheight   : NULL
      ..$ margin       : NULL
      ..$ debug        : NULL
      ..$ inherit.blank: logi TRUE
      ..- attr(*, "class")= chr [1:2] "element_text" "element"
     $ axis.text.x                     :List of 11
      ..$ family       : NULL
      ..$ face         : NULL
      ..$ colour       : NULL
      ..$ size         : NULL
      ..$ hjust        : NULL
      ..$ vjust        : num 1
      ..$ angle        : NULL
      ..$ lineheight   : NULL
      ..$ margin       : 'margin' num [1:4] 2.2points 0points 0points 0points
      .. ..- attr(*, "unit")= int 8
      ..$ debug        : NULL
      ..$ inherit.blank: logi TRUE
      ..- attr(*, "class")= chr [1:2] "element_text" "element"
     $ axis.text.x.top                 :List of 11
      ..$ family       : NULL
      ..$ face         : NULL
      ..$ colour       : NULL
      ..$ size         : NULL
      ..$ hjust        : NULL
      ..$ vjust        : num 0
      ..$ angle        : NULL
      ..$ lineheight   : NULL
      ..$ margin       : 'margin' num [1:4] 0points 0points 2.2points 0points
      .. ..- attr(*, "unit")= int 8
      ..$ debug        : NULL
      ..$ inherit.blank: logi TRUE
      ..- attr(*, "class")= chr [1:2] "element_text" "element"
     $ axis.text.x.bottom              : NULL
     $ axis.text.y                     :List of 11
      ..$ family       : NULL
      ..$ face         : NULL
      ..$ colour       : NULL
      ..$ size         : NULL
      ..$ hjust        : num 1
      ..$ vjust        : NULL
      ..$ angle        : NULL
      ..$ lineheight   : NULL
      ..$ margin       : 'margin' num [1:4] 0points 2.2points 0points 0points
      .. ..- attr(*, "unit")= int 8
      ..$ debug        : NULL
      ..$ inherit.blank: logi TRUE
      ..- attr(*, "class")= chr [1:2] "element_text" "element"
     $ axis.text.y.left                : NULL
     $ axis.text.y.right               :List of 11
      ..$ family       : NULL
      ..$ face         : NULL
      ..$ colour       : NULL
      ..$ size         : NULL
      ..$ hjust        : num 0
      ..$ vjust        : NULL
      ..$ angle        : NULL
      ..$ lineheight   : NULL
      ..$ margin       : 'margin' num [1:4] 0points 0points 0points 2.2points
      .. ..- attr(*, "unit")= int 8
      ..$ debug        : NULL
      ..$ inherit.blank: logi TRUE
      ..- attr(*, "class")= chr [1:2] "element_text" "element"
     $ axis.text.theta                 : NULL
     $ axis.text.r                     :List of 11
      ..$ family       : NULL
      ..$ face         : NULL
      ..$ colour       : NULL
      ..$ size         : NULL
      ..$ hjust        : num 0.5
      ..$ vjust        : NULL
      ..$ angle        : NULL
      ..$ lineheight   : NULL
      ..$ margin       : 'margin' num [1:4] 0points 2.2points 0points 2.2points
      .. ..- attr(*, "unit")= int 8
      ..$ debug        : NULL
      ..$ inherit.blank: logi TRUE
      ..- attr(*, "class")= chr [1:2] "element_text" "element"
     $ axis.ticks                      :List of 6
      ..$ colour       : chr "grey70"
      ..$ linewidth    : 'rel' num 0.5
      ..$ linetype     : NULL
      ..$ lineend      : NULL
      ..$ arrow        : logi FALSE
      ..$ inherit.blank: logi TRUE
      ..- attr(*, "class")= chr [1:2] "element_line" "element"
     $ axis.ticks.x                    : NULL
     $ axis.ticks.x.top                : NULL
     $ axis.ticks.x.bottom             : NULL
     $ axis.ticks.y                    : NULL
     $ axis.ticks.y.left               : NULL
     $ axis.ticks.y.right              : NULL
     $ axis.ticks.theta                : NULL
     $ axis.ticks.r                    : NULL
     $ axis.minor.ticks.x.top          : NULL
     $ axis.minor.ticks.x.bottom       : NULL
     $ axis.minor.ticks.y.left         : NULL
     $ axis.minor.ticks.y.right        : NULL
     $ axis.minor.ticks.theta          : NULL
     $ axis.minor.ticks.r              : NULL
     $ axis.ticks.length               : 'simpleUnit' num 2.75points
      ..- attr(*, "unit")= int 8
     $ axis.ticks.length.x             : NULL
     $ axis.ticks.length.x.top         : NULL
     $ axis.ticks.length.x.bottom      : NULL
     $ axis.ticks.length.y             : NULL
     $ axis.ticks.length.y.left        : NULL
     $ axis.ticks.length.y.right       : NULL
     $ axis.ticks.length.theta         : NULL
     $ axis.ticks.length.r             : NULL
     $ axis.minor.ticks.length         : 'rel' num 0.75
     $ axis.minor.ticks.length.x       : NULL
     $ axis.minor.ticks.length.x.top   : NULL
     $ axis.minor.ticks.length.x.bottom: NULL
     $ axis.minor.ticks.length.y       : NULL
     $ axis.minor.ticks.length.y.left  : NULL
     $ axis.minor.ticks.length.y.right : NULL
     $ axis.minor.ticks.length.theta   : NULL
     $ axis.minor.ticks.length.r       : NULL
     $ axis.line                       : list()
      ..- attr(*, "class")= chr [1:2] "element_blank" "element"
     $ axis.line.x                     : NULL
     $ axis.line.x.top                 : NULL
     $ axis.line.x.bottom              : NULL
     $ axis.line.y                     : NULL
     $ axis.line.y.left                : NULL
     $ axis.line.y.right               : NULL
     $ axis.line.theta                 : NULL
     $ axis.line.r                     : NULL
     $ legend.background               :List of 5
      ..$ fill         : NULL
      ..$ colour       : logi NA
      ..$ linewidth    : NULL
      ..$ linetype     : NULL
      ..$ inherit.blank: logi TRUE
      ..- attr(*, "class")= chr [1:2] "element_rect" "element"
     $ legend.margin                   : 'margin' num [1:4] 5.5points 5.5points 5.5points 5.5points
      ..- attr(*, "unit")= int 8
     $ legend.spacing                  : 'simpleUnit' num 11points
      ..- attr(*, "unit")= int 8
     $ legend.spacing.x                : NULL
     $ legend.spacing.y                : NULL
     $ legend.key                      : NULL
     $ legend.key.size                 : 'simpleUnit' num 1.2lines
      ..- attr(*, "unit")= int 3
     $ legend.key.height               : NULL
     $ legend.key.width                : NULL
     $ legend.key.spacing              : 'simpleUnit' num 5.5points
      ..- attr(*, "unit")= int 8
     $ legend.key.spacing.x            : NULL
     $ legend.key.spacing.y            : NULL
     $ legend.frame                    : NULL
     $ legend.ticks                    : NULL
     $ legend.ticks.length             : 'rel' num 0.2
     $ legend.axis.line                : NULL
     $ legend.text                     :List of 11
      ..$ family       : NULL
      ..$ face         : NULL
      ..$ colour       : NULL
      ..$ size         : 'rel' num 0.8
      ..$ hjust        : NULL
      ..$ vjust        : NULL
      ..$ angle        : NULL
      ..$ lineheight   : NULL
      ..$ margin       : NULL
      ..$ debug        : NULL
      ..$ inherit.blank: logi TRUE
      ..- attr(*, "class")= chr [1:2] "element_text" "element"
     $ legend.text.position            : NULL
     $ legend.title                    :List of 11
      ..$ family       : NULL
      ..$ face         : NULL
      ..$ colour       : NULL
      ..$ size         : NULL
      ..$ hjust        : num 0
      ..$ vjust        : NULL
      ..$ angle        : NULL
      ..$ lineheight   : NULL
      ..$ margin       : NULL
      ..$ debug        : NULL
      ..$ inherit.blank: logi TRUE
      ..- attr(*, "class")= chr [1:2] "element_text" "element"
     $ legend.title.position           : NULL
     $ legend.position                 : chr "right"
     $ legend.position.inside          : NULL
     $ legend.direction                : NULL
     $ legend.byrow                    : NULL
     $ legend.justification            : chr "center"
     $ legend.justification.top        : NULL
     $ legend.justification.bottom     : NULL
     $ legend.justification.left       : NULL
     $ legend.justification.right      : NULL
     $ legend.justification.inside     : NULL
     $ legend.location                 : NULL
     $ legend.box                      : NULL
     $ legend.box.just                 : NULL
     $ legend.box.margin               : 'margin' num [1:4] 0cm 0cm 0cm 0cm
      ..- attr(*, "unit")= int 1
     $ legend.box.background           : list()
      ..- attr(*, "class")= chr [1:2] "element_blank" "element"
     $ legend.box.spacing              : 'simpleUnit' num 11points
      ..- attr(*, "unit")= int 8
      [list output truncated]
     - attr(*, "class")= chr [1:2] "theme" "gg"
     - attr(*, "complete")= logi TRUE
     - attr(*, "validate")= logi TRUE

It takes a larger cognitive load to read text that is rotated. It is
common practice in many journals and media outlets to move the $y$-axis
label to the top of the graph under the title.

**8. Specify your $y$-axis label to be empty and move the $y$-axis label
into the subtitle.**

<details class="code-fold">
<summary>Code</summary>

``` r
ggplot(data = surveys,
       mapping = aes(x = weight, 
                     y = hindfoot_length)) +
  geom_point(alpha = 0.25) +
  geom_jitter(alpha = 0.25) +
  facet_grid(.~species,
             scales = "free") +
  labs(x = "Weight (g)",
       y = "",
       title = "Species Hindfoot Length per Weight" ,
       subtitle = "Hindfoot Length (mm)")
```

</details>

![](lab-3-student_files/figure-commonmark/scatterplot%20with%20alpha,%20facet,%20and%20journal-standard%20labels-1.png)

<details class="code-fold">
<summary>Code</summary>

``` r
  theme_light()
```

</details>

    List of 136
     $ line                            :List of 6
      ..$ colour       : chr "black"
      ..$ linewidth    : num 0.5
      ..$ linetype     : num 1
      ..$ lineend      : chr "butt"
      ..$ arrow        : logi FALSE
      ..$ inherit.blank: logi TRUE
      ..- attr(*, "class")= chr [1:2] "element_line" "element"
     $ rect                            :List of 5
      ..$ fill         : chr "white"
      ..$ colour       : chr "black"
      ..$ linewidth    : num 0.5
      ..$ linetype     : num 1
      ..$ inherit.blank: logi TRUE
      ..- attr(*, "class")= chr [1:2] "element_rect" "element"
     $ text                            :List of 11
      ..$ family       : chr ""
      ..$ face         : chr "plain"
      ..$ colour       : chr "black"
      ..$ size         : num 11
      ..$ hjust        : num 0.5
      ..$ vjust        : num 0.5
      ..$ angle        : num 0
      ..$ lineheight   : num 0.9
      ..$ margin       : 'margin' num [1:4] 0points 0points 0points 0points
      .. ..- attr(*, "unit")= int 8
      ..$ debug        : logi FALSE
      ..$ inherit.blank: logi TRUE
      ..- attr(*, "class")= chr [1:2] "element_text" "element"
     $ title                           : NULL
     $ aspect.ratio                    : NULL
     $ axis.title                      : NULL
     $ axis.title.x                    :List of 11
      ..$ family       : NULL
      ..$ face         : NULL
      ..$ colour       : NULL
      ..$ size         : NULL
      ..$ hjust        : NULL
      ..$ vjust        : num 1
      ..$ angle        : NULL
      ..$ lineheight   : NULL
      ..$ margin       : 'margin' num [1:4] 2.75points 0points 0points 0points
      .. ..- attr(*, "unit")= int 8
      ..$ debug        : NULL
      ..$ inherit.blank: logi TRUE
      ..- attr(*, "class")= chr [1:2] "element_text" "element"
     $ axis.title.x.top                :List of 11
      ..$ family       : NULL
      ..$ face         : NULL
      ..$ colour       : NULL
      ..$ size         : NULL
      ..$ hjust        : NULL
      ..$ vjust        : num 0
      ..$ angle        : NULL
      ..$ lineheight   : NULL
      ..$ margin       : 'margin' num [1:4] 0points 0points 2.75points 0points
      .. ..- attr(*, "unit")= int 8
      ..$ debug        : NULL
      ..$ inherit.blank: logi TRUE
      ..- attr(*, "class")= chr [1:2] "element_text" "element"
     $ axis.title.x.bottom             : NULL
     $ axis.title.y                    :List of 11
      ..$ family       : NULL
      ..$ face         : NULL
      ..$ colour       : NULL
      ..$ size         : NULL
      ..$ hjust        : NULL
      ..$ vjust        : num 1
      ..$ angle        : num 90
      ..$ lineheight   : NULL
      ..$ margin       : 'margin' num [1:4] 0points 2.75points 0points 0points
      .. ..- attr(*, "unit")= int 8
      ..$ debug        : NULL
      ..$ inherit.blank: logi TRUE
      ..- attr(*, "class")= chr [1:2] "element_text" "element"
     $ axis.title.y.left               : NULL
     $ axis.title.y.right              :List of 11
      ..$ family       : NULL
      ..$ face         : NULL
      ..$ colour       : NULL
      ..$ size         : NULL
      ..$ hjust        : NULL
      ..$ vjust        : num 1
      ..$ angle        : num -90
      ..$ lineheight   : NULL
      ..$ margin       : 'margin' num [1:4] 0points 0points 0points 2.75points
      .. ..- attr(*, "unit")= int 8
      ..$ debug        : NULL
      ..$ inherit.blank: logi TRUE
      ..- attr(*, "class")= chr [1:2] "element_text" "element"
     $ axis.text                       :List of 11
      ..$ family       : NULL
      ..$ face         : NULL
      ..$ colour       : chr "grey30"
      ..$ size         : 'rel' num 0.8
      ..$ hjust        : NULL
      ..$ vjust        : NULL
      ..$ angle        : NULL
      ..$ lineheight   : NULL
      ..$ margin       : NULL
      ..$ debug        : NULL
      ..$ inherit.blank: logi TRUE
      ..- attr(*, "class")= chr [1:2] "element_text" "element"
     $ axis.text.x                     :List of 11
      ..$ family       : NULL
      ..$ face         : NULL
      ..$ colour       : NULL
      ..$ size         : NULL
      ..$ hjust        : NULL
      ..$ vjust        : num 1
      ..$ angle        : NULL
      ..$ lineheight   : NULL
      ..$ margin       : 'margin' num [1:4] 2.2points 0points 0points 0points
      .. ..- attr(*, "unit")= int 8
      ..$ debug        : NULL
      ..$ inherit.blank: logi TRUE
      ..- attr(*, "class")= chr [1:2] "element_text" "element"
     $ axis.text.x.top                 :List of 11
      ..$ family       : NULL
      ..$ face         : NULL
      ..$ colour       : NULL
      ..$ size         : NULL
      ..$ hjust        : NULL
      ..$ vjust        : num 0
      ..$ angle        : NULL
      ..$ lineheight   : NULL
      ..$ margin       : 'margin' num [1:4] 0points 0points 2.2points 0points
      .. ..- attr(*, "unit")= int 8
      ..$ debug        : NULL
      ..$ inherit.blank: logi TRUE
      ..- attr(*, "class")= chr [1:2] "element_text" "element"
     $ axis.text.x.bottom              : NULL
     $ axis.text.y                     :List of 11
      ..$ family       : NULL
      ..$ face         : NULL
      ..$ colour       : NULL
      ..$ size         : NULL
      ..$ hjust        : num 1
      ..$ vjust        : NULL
      ..$ angle        : NULL
      ..$ lineheight   : NULL
      ..$ margin       : 'margin' num [1:4] 0points 2.2points 0points 0points
      .. ..- attr(*, "unit")= int 8
      ..$ debug        : NULL
      ..$ inherit.blank: logi TRUE
      ..- attr(*, "class")= chr [1:2] "element_text" "element"
     $ axis.text.y.left                : NULL
     $ axis.text.y.right               :List of 11
      ..$ family       : NULL
      ..$ face         : NULL
      ..$ colour       : NULL
      ..$ size         : NULL
      ..$ hjust        : num 0
      ..$ vjust        : NULL
      ..$ angle        : NULL
      ..$ lineheight   : NULL
      ..$ margin       : 'margin' num [1:4] 0points 0points 0points 2.2points
      .. ..- attr(*, "unit")= int 8
      ..$ debug        : NULL
      ..$ inherit.blank: logi TRUE
      ..- attr(*, "class")= chr [1:2] "element_text" "element"
     $ axis.text.theta                 : NULL
     $ axis.text.r                     :List of 11
      ..$ family       : NULL
      ..$ face         : NULL
      ..$ colour       : NULL
      ..$ size         : NULL
      ..$ hjust        : num 0.5
      ..$ vjust        : NULL
      ..$ angle        : NULL
      ..$ lineheight   : NULL
      ..$ margin       : 'margin' num [1:4] 0points 2.2points 0points 2.2points
      .. ..- attr(*, "unit")= int 8
      ..$ debug        : NULL
      ..$ inherit.blank: logi TRUE
      ..- attr(*, "class")= chr [1:2] "element_text" "element"
     $ axis.ticks                      :List of 6
      ..$ colour       : chr "grey70"
      ..$ linewidth    : 'rel' num 0.5
      ..$ linetype     : NULL
      ..$ lineend      : NULL
      ..$ arrow        : logi FALSE
      ..$ inherit.blank: logi TRUE
      ..- attr(*, "class")= chr [1:2] "element_line" "element"
     $ axis.ticks.x                    : NULL
     $ axis.ticks.x.top                : NULL
     $ axis.ticks.x.bottom             : NULL
     $ axis.ticks.y                    : NULL
     $ axis.ticks.y.left               : NULL
     $ axis.ticks.y.right              : NULL
     $ axis.ticks.theta                : NULL
     $ axis.ticks.r                    : NULL
     $ axis.minor.ticks.x.top          : NULL
     $ axis.minor.ticks.x.bottom       : NULL
     $ axis.minor.ticks.y.left         : NULL
     $ axis.minor.ticks.y.right        : NULL
     $ axis.minor.ticks.theta          : NULL
     $ axis.minor.ticks.r              : NULL
     $ axis.ticks.length               : 'simpleUnit' num 2.75points
      ..- attr(*, "unit")= int 8
     $ axis.ticks.length.x             : NULL
     $ axis.ticks.length.x.top         : NULL
     $ axis.ticks.length.x.bottom      : NULL
     $ axis.ticks.length.y             : NULL
     $ axis.ticks.length.y.left        : NULL
     $ axis.ticks.length.y.right       : NULL
     $ axis.ticks.length.theta         : NULL
     $ axis.ticks.length.r             : NULL
     $ axis.minor.ticks.length         : 'rel' num 0.75
     $ axis.minor.ticks.length.x       : NULL
     $ axis.minor.ticks.length.x.top   : NULL
     $ axis.minor.ticks.length.x.bottom: NULL
     $ axis.minor.ticks.length.y       : NULL
     $ axis.minor.ticks.length.y.left  : NULL
     $ axis.minor.ticks.length.y.right : NULL
     $ axis.minor.ticks.length.theta   : NULL
     $ axis.minor.ticks.length.r       : NULL
     $ axis.line                       : list()
      ..- attr(*, "class")= chr [1:2] "element_blank" "element"
     $ axis.line.x                     : NULL
     $ axis.line.x.top                 : NULL
     $ axis.line.x.bottom              : NULL
     $ axis.line.y                     : NULL
     $ axis.line.y.left                : NULL
     $ axis.line.y.right               : NULL
     $ axis.line.theta                 : NULL
     $ axis.line.r                     : NULL
     $ legend.background               :List of 5
      ..$ fill         : NULL
      ..$ colour       : logi NA
      ..$ linewidth    : NULL
      ..$ linetype     : NULL
      ..$ inherit.blank: logi TRUE
      ..- attr(*, "class")= chr [1:2] "element_rect" "element"
     $ legend.margin                   : 'margin' num [1:4] 5.5points 5.5points 5.5points 5.5points
      ..- attr(*, "unit")= int 8
     $ legend.spacing                  : 'simpleUnit' num 11points
      ..- attr(*, "unit")= int 8
     $ legend.spacing.x                : NULL
     $ legend.spacing.y                : NULL
     $ legend.key                      : NULL
     $ legend.key.size                 : 'simpleUnit' num 1.2lines
      ..- attr(*, "unit")= int 3
     $ legend.key.height               : NULL
     $ legend.key.width                : NULL
     $ legend.key.spacing              : 'simpleUnit' num 5.5points
      ..- attr(*, "unit")= int 8
     $ legend.key.spacing.x            : NULL
     $ legend.key.spacing.y            : NULL
     $ legend.frame                    : NULL
     $ legend.ticks                    : NULL
     $ legend.ticks.length             : 'rel' num 0.2
     $ legend.axis.line                : NULL
     $ legend.text                     :List of 11
      ..$ family       : NULL
      ..$ face         : NULL
      ..$ colour       : NULL
      ..$ size         : 'rel' num 0.8
      ..$ hjust        : NULL
      ..$ vjust        : NULL
      ..$ angle        : NULL
      ..$ lineheight   : NULL
      ..$ margin       : NULL
      ..$ debug        : NULL
      ..$ inherit.blank: logi TRUE
      ..- attr(*, "class")= chr [1:2] "element_text" "element"
     $ legend.text.position            : NULL
     $ legend.title                    :List of 11
      ..$ family       : NULL
      ..$ face         : NULL
      ..$ colour       : NULL
      ..$ size         : NULL
      ..$ hjust        : num 0
      ..$ vjust        : NULL
      ..$ angle        : NULL
      ..$ lineheight   : NULL
      ..$ margin       : NULL
      ..$ debug        : NULL
      ..$ inherit.blank: logi TRUE
      ..- attr(*, "class")= chr [1:2] "element_text" "element"
     $ legend.title.position           : NULL
     $ legend.position                 : chr "right"
     $ legend.position.inside          : NULL
     $ legend.direction                : NULL
     $ legend.byrow                    : NULL
     $ legend.justification            : chr "center"
     $ legend.justification.top        : NULL
     $ legend.justification.bottom     : NULL
     $ legend.justification.left       : NULL
     $ legend.justification.right      : NULL
     $ legend.justification.inside     : NULL
     $ legend.location                 : NULL
     $ legend.box                      : NULL
     $ legend.box.just                 : NULL
     $ legend.box.margin               : 'margin' num [1:4] 0cm 0cm 0cm 0cm
      ..- attr(*, "unit")= int 1
     $ legend.box.background           : list()
      ..- attr(*, "class")= chr [1:2] "element_blank" "element"
     $ legend.box.spacing              : 'simpleUnit' num 11points
      ..- attr(*, "unit")= int 8
      [list output truncated]
     - attr(*, "class")= chr [1:2] "theme" "gg"
     - attr(*, "complete")= logi TRUE
     - attr(*, "validate")= logi TRUE

## Boxplots

<details class="code-fold">
<summary>Code</summary>

``` r
ggplot(data = surveys, 
       mapping = aes(x = species, 
                     y = weight)) +
  geom_boxplot() + 
  theme(axis.text.x = element_text(angle = 25,
                                   vjust = 0.5,
                                   hjust = 0.3),
        legend.position = "none")
```

</details>

![](lab-3-student_files/figure-commonmark/boxplot-1.png)

**9. Create side-by-side boxplots to visualize the distribution of
weight within each species.**

A fundamental complaint of boxplots is that they do not plot the raw
data. However, with **ggplot** we can add the raw points on top of the
boxplots!

**10. Add another layer to your previous plot that plots each
observation using `geom_point()`.**

<details class="code-fold">
<summary>Code</summary>

``` r
ggplot(data = surveys, 
       mapping = aes(x = species, 
                     y = weight)) +
  geom_boxplot() +
  geom_point(alpha = 0.25,
             size = 0.25) +
  theme(axis.text.x = element_text(angle = 25,
                                   vjust = 0.5,
                                   hjust = 0.3),
        legend.position = "none")
```

</details>

![](lab-3-student_files/figure-commonmark/boxplot%20with%20scatter%20points-1.png)

Alright, this should look less than optimal. Your points should appear
rather stacked on top of each other. To make them less stacked, we need
to jitter them a bit, using `geom_jitter()`.

**11. Remove the previous layer and include a `geom_jitter()` layer
instead.**

<!-- Continue to modify the plot you made for question 9! -->
<details class="code-fold">
<summary>Code</summary>

``` r
ggplot(data = surveys, 
       mapping = aes(x = species, 
                     y = weight)) +
  geom_boxplot() +
  geom_point(position = position_jitter(),
             alpha = 0.25,
             size = 0.25) +
  theme(axis.text.x = element_text(angle = 25,
                                   vjust = 0.5,
                                   hjust = 0.3),
        legend.position = "none")
```

</details>

![](lab-3-student_files/figure-commonmark/boxplot%20with%20jittered%20scatter%20points-1.png)

That should look a bit better! But its really hard to see the points
when everything is black.

**12. Set the `color` aesthetic in `geom_jitter()` to change the color
of the points and add set the `alpha` aesthetic to add transparency.**
You are welcome to use whatever color you wish! Some of my favorites are
“springgreen4” and “steelblue4”. Check them out on [R
Charts](https://r-charts.com/colors/)

<details class="code-fold">
<summary>Code</summary>

``` r
ggplot(data = surveys, 
       mapping = aes(x = species, 
                     y = weight)) +
  geom_boxplot() +
  geom_point(position = position_jitter(),
             alpha = 0.25,
             size = 0.25,
             color = "steelblue4") +
  theme(axis.text.x = element_text(angle = 25,
                                   vjust = 0.5,
                                   hjust = 0.3),
        legend.position = "none")
```

</details>

![](lab-3-student_files/figure-commonmark/boxplot%20with%20jittered%20scatter%20points%20and%20color-1.png)

Great! Now that you can see the points, you should notice something odd:
there are two colors of points still being plotted. Some of the
observations are being plotted twice, once from `geom_boxplot()` as
outliers and again from `geom_jitter()`!

**13. Inspect the help file for `geom_boxplot()` and see how you can
remove the outliers from being plotted by `geom_boxplot()`. Make this
change in your code!**

<details class="code-fold">
<summary>Code</summary>

``` r
ggplot(data = surveys, 
       mapping = aes(x = species, 
                     y = weight)) +
  geom_boxplot(outliers = FALSE) +
  geom_point(position = position_jitter(),
             alpha = 0.25,
             size = 0.25,
             color = "steelblue4") +
  theme(axis.text.x = element_text(angle = 25,
                                   vjust = 0.5,
                                   hjust = 0.3),
        legend.position = "none")
```

</details>

![](lab-3-student_files/figure-commonmark/boxplot%20with%20jittered%20scatter%20points,%20color%20and%20removed%20outliers-1.png)

Some small changes can make **big** differences to plots. One of these
changes are better labels for a plot’s axes and legend.

**14. Modify the $x$-axis and $y$-axis labels to describe what is being
plotted. Be sure to include any necessary units! You might also be
getting overlap in the species names – use `theme(axis.text.x = ____)`
or `theme(axis.text.y = ____)` to turn the species axis labels 45
degrees.**

<details class="code-fold">
<summary>Code</summary>

``` r
ggplot(data = surveys, 
       mapping = aes(x = species, 
                     y = weight)) +
  geom_boxplot(outliers = FALSE) +
  geom_point(position = position_jitter(),
             alpha = 0.25,
             size = 0.25,
             color = "steelblue4") +
  theme(axis.text.x = element_text(angle = 25,
                                   vjust = 0.5,
                                   hjust = 0.3),
        legend.position = "none") +
  labs(x = "Species",
       y = "",
       title = "Weight Distribution by Species" ,
       subtitle = "Weight (g)")
```

</details>

![](lab-3-student_files/figure-commonmark/boxplot%20with%20jittered%20scatter%20points,%20color,%20removed%20outliers%20and%20labels-1.png)

Some people (and journals) prefer for boxplots to be stacked with a
specific orientation! Let’s practice changing the orientation of our
boxplots.

**15. Now copy-paste your boxplot code you’ve been adding to above. Flip
the orientation of your boxplots. If you created horizontally stacked
boxplots, your boxplots should now be stacked vertically. If you had
vertically stacked boxplots, you should now stack your boxplots
horizontally!**

<details class="code-fold">
<summary>Code</summary>

``` r
ggplot(data = surveys, 
       mapping = aes(x = species, 
                     y = weight)) +
  geom_boxplot(outliers = FALSE) +
  geom_point(position = position_jitter(),
             alpha = 0.25,
             size = 0.25,
             color = "steelblue4") +
  theme(legend.position = "none") +
  labs(x = "",
       y = "Weight (g)",
       title = "Weight Distribution by Species" ,
       subtitle = "Species") +
  coord_flip()
```

</details>

![](lab-3-student_files/figure-commonmark/rotated-boxplot-1.png)

Notice how vertically stacked boxplots make the species labels more
readable than horizontally stacked boxplots (even when the axis labels
are rotated). This is good practice!

# Lab 3 Submission

For Lab 3 you will submit the link to your GitHub repository. Your
rendered file is required to have the following specifications in the
YAML options (at the top of your document):

- have the plots embedded (`embed-resources: true`)
- include your source code (`code-tools: true`)
- include all your code and output (`echo: true`)

**If any of the options are not included, your Lab 3 or Challenge 3
assignment will receive an “Incomplete” and you will be required to
submit a revision.**

In addition, your document should not have any warnings or messages
output in your HTML document. **If your HTML contains warnings or
messages, you will receive an “Incomplete” for document formatting and
you will be required to submit a revision.**
