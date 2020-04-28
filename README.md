## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/sophieyount/Yount_capstone/edit/master/README.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/sophieyount/Yount_capstone/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.

### Sophie Yount Capstone 2020
04/27/2020
```{r}
library(tidyverse)
```
1) Provide a brief background and significance about a specific research problem that interests you. It could be project you’re involved with now, or a rotation project, or something you’d like to work on. The reader will need to understand enough background to make sense of the experiment you propose below. Keep it brief. In one short paragraph.

**Chronic cocaine use has been shown to decrease the ability to update action outcome contingencies, and subsequently increase deferral to habitual behavior. Action-outcome contingency can be measured by comparing the response rate at the reinforced versus the degraded nose poke during a contingency degradation behavioral test. Higher response ratio, defined by the ratio between the reinforced nose pokes:degraded nose pokes, indicates goal directed mice, while lower response ratios indicate habitual behavior. A mouse strain was selectively bred to be predisposed to habitual behavior. This selectively bred mouse strain showed increased levels of the melanocortin 4 receptor (MC4R) in the dorsal medium striatum (DMS). Subsequent studies using a Cre-lox system revealed that knocking down MC4R expression selectively in the DMS of MC4Rflox/flox mice decreased the rate of responding at the degraded nose poke during behavioral testing, and higher R:D ratio compared to control mice. Therefore, decreased expression of MC4R in the DMS aids goal-directed behavior.**

2) Briefly state something that is unknown about this system that can be discovered through, and leads to, an experiment.  For example, "It is not known whether....."

**It is not known whether reduced expression of MC4R in other areas of the striatum, such as the ventral striatum (VS), increase goal-directed behavior in the same manner.**

3) Make an “if” “then” prediction that is related to item #2. It should be of the general form, “if X is true, then Y should happen”.

**If MC4R expression is knocked down in the ventral striatum, then it will alter the rate of responding on the degraded nose poke compared to the reinforced nose poke.**

4) What dependent variable will be observed to test this prediction in item #3? What predictor variable will be used to manipulate the system experimentally? Define the inherent properties of these variables (eg, are they sorted, ordered or measured).

**The dependent variable will be R:D ratio, calculated by the ratio between the number of nose pokes/minute on each nose poke aperture. Alhtough the number of nose pokes is a discrete variable, when we trasnform the nose pokes into a ratio this becomes a continuous measurment. The independent variable is the viral vector injected into the VS. The viral vector is a discrete variable that will be at two levels, empty-GFP and Cre-GFP.**

5) Write a statistical hypothesis.  There should be a null and alternate. These should be explicitly consistent with the prediction in item #3 and the response variable in #4. In other words, make sure the statistical hypotheses that you write here serves as a test of the prediction made in item #3.

**Where µ represents the average R:D ratio of the populations corresponding to the sampled groups at each time point, the null and alternative hypothesis are H0: μempty= μCre and H1: μempty ≠ μCre respectively.** 

6) What is the statistical test you would use to test the hypothesis in item #5? Briefly defend what makes this appropriate for the hypothesis and the experimental variables. If there are alternatives, why is this approach chosen instead? Points will not be awarded if the justification involves something like "because everybody does it this way".

**A two-sided unpaired t-test will be used for comparing group means. We want to compare the means of the R:D ratio in each group. These groups, eGFP (control) and Cre (VS MC4R knockdown) are randomly allocated treatments with no intrinsic link. We chose a two sided paired t-test because we want to see if knocking down MC4R in the VS has any effect on goal directed behavior, either increase or decrease. We wouldn't choose a one-sided t-test because we really aren't sure if this manipulation will elicit similar or opposing trends compared to previous data.**

7) List the procedures and decision rules you have for executing and interpreting the experiment. These procedures range from selection of experimental units, to randomization to primary endpoint to threshold decisions. Define (and defend) what you believe will be the independent replicate.

**We will be using the response rate at each nose poke as defined by the number of pokes on the recess over the entire 15-minute test (pokes/min). The response ratio for each mouse is calculated by the response rate at the reinforced aperture: the response rate at the degraded aperture. Because this is a ratio, it does not have units. The mean for each treatment group is defined as the average R:D ratio of that group. Each mouse subject is an independent experimental unit. The sample size will be based upon a power of 90%, which means that the tolerance level for type2 error will be 10%. The decision threshold for type1 error will be 5%. Thus, the null hypothesis will be rejected at a p-value of less than 0.05. I want a strong type one error to protect myself against falsely rejecting the null hypothesis.  I also want relatively strong protection against falsely claiming the null is true, so the threshold for type 2 error is 10%.**

8) Produce a graph of a simulation for the expected results. Create a dataMaker-like function in R to create and plot the data. Label and scale any axis. The graph should illustrate the magnitude of the expected response, or the level of response that you expect to see and would be minimally scientifically relevant. Be sure to illustrate any variation that is expected.

**This is one result we might expect**

```{r}
#Cre viral vector selectively knocksdown MC4R in the VS. We expect these mice to be very goal directed.
Cre <- rnorm(5,1.80, 0.25)  

#eGFP viral vector is just a vechicle control. We expect these mice to be goal directed, but not stellar. 
eGFP <- rnorm(5,1.10,0.25)

data <- tibble(Cre, eGFP)%>% gather(viral_vector, response_rate_ratio)

ggplot(data, aes(x=viral_vector, y=response_rate_ratio)) +
  geom_jitter(size=5, alpha=0.8, color = "#002878") + 
  geom_boxplot(alpha=0.5, color = "#d28e00") +
  scale_x_discrete(labels=c("Cre Knockdown", "eGFP Control")) +
  xlab("Viral Vector") +
  ylab("R:D Response Ratio")
```
9) Write and perform a Monte Carlo analysis to calculate a sample size necessary to test the hypothesis. This Monte Carlo must test the primary endpoint.
**Start by making some groups so this is easier to work with. Estimate the mean and standard deviation for R:D ratio.**
```{r}
#Sampled population parameters 

#sample A is the control (eGFP viral vector)
meanA <- 1.10
sdA <- 0.25
nA <- 5

#sample B is the knockdown (Cre viral vector)
meanB <-1.8
sdB <- 0.25
nB <- 5

```
**Lets go ahead and set up t-test function arguments as initializers. Also, declare how many simulations we want and an empty vector to fill with generated p-values.**
```{r}
#t-test function arguments 
alt <- "two.sided"
pairing <- FALSE
var <- TRUE 
alpha <- 0.05


#Simulations, the more the better accuracy 
nSims <- 1000 

# nice vector

p <- c()

```

**Lets run it**
```{r}
# Monte Carlo 

for(i in 1:nSims){ #for each simulated experiment
  x<-rnorm(n = nA, mean = meanA, sd = sdA) #produce n simulated participants
  #with mean and SD
  y<-rnorm(n = nB, mean = meanB, sd = sdB) #produce n simulated participants
  #with mean and SD
  z<-t.test(x,y, 
            alternative = alt, 
            paired = pairing, 
            var.equal = var, 
            conf.level = 1-alpha) #perform the t-test
  
  p[i]<-z$p.value #get the p-value and store it
}

```



**Whats power (number of significant results/total number of simulations)?**

```{r}
hits <- length(which(p<alpha)); paste("hits=", hits)
power <- hits/nSims; paste("power=",power)
```



What does this mean? 

**The objective of our t-test is to determine if the mean R:D ratio of our knockdown group differs from the mean R:D ratio of the control. In order to confidently determine differences between these two groups we should be fine using five mice per group as determined by the Monte Carlo simulation above. This is telling us that 96.5% of random simulations from this t-test set up had p-values less than 0.05. Thats pretty good reassurance.** 
