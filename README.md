# Sanitised-Stat-Project

## Background
Whereas the thresholds in the standards are often defined as percentile-based bounds of a given asset fleet, there is sufficient data with online measurements to create bespoke thresholds per transformer. This increased specificity in the norm-generating population should therefore increase the relevancy in the generated thresholds. This project created an unsupervised conditional anomaly detection approach for power transformers. It was developed into a practical, deployable system that is both intuitive and interpretable to help with decision support regarding the condition monitoring of transformers using DGA data as a basis. A primary motivator for this is the lack of ground-truth validation data. Since the overall accuracy cannot be quantified, it is important that the engineer can easily validate the outputs by having a traceable and interpretable set of metrics. This system is designed as a decision-support tool helping guide engineers’ attention to areas most likely to contain problems.

I cannot share the code, but here is an overview comparable to what would be contained in a paper, i.e. description of functionality achieved. The analytics was done in R. I also developed a Python GUI Wrapper (not shown / included here) to allow for local batch processing via selection of a .cvs file. I also created a separate prototype that was deployed on a commercial platform within a company's sandboxed test environment.

*Please note*, the scope of the project was specifically to use *multi-variate normal distributions*. This greatly limits my options in delivering a technical solution.

## Annotated Example of Output
This is screenshot of an output generated within the GUI Wrapper (also within R IDE if ran there). Based on GGPlot2 Library.
[]

## Process Overview
This is a very high level overview of the flow of data from input to output.

## Functionality
A lot of software engineering went into making it flexible and extensible.
  *  Input variables can be grouped for multi-variate analysis
  *  Multiple groups can be specified (but there is not "cross-talk" between groups)
  *  Input variables can be defined as having one-way relationships (e.g. ambient weather affects gas levels unidirectionally)
  *  The detrending method can be specified
  *  The output metric method can be specified
  *  The output metric thresholds can be specified

[]

## Imputation
Imputation is needed to account for missing data. Various methods were implemented and could be selected at run-time via input arguments. We did not have case study data with real missing data, so it was difficult to validate performance. I ended up suggested to simply use the last known value as I did not feel comfortable suggesting alternatives sans validation.

[]

## Conditional Anomaly Detection
The concept was to adjust expectations based on environmental factors (conditions). This is an overview of the process. Again, because only normal multi-variate distributions were wanted, it required a means to deal with a higher propensity to flag outliers. The chosen approach was to consider to consecutive samples together. In hindsight, I regret this. It *significantly* complicated the implementation and feels ineligant. 

[]

## "Consolidation" Process
Once any missing data was imputed, and the relationships, etc., set up, the final stage was to calculate a metric to signify to what extent the current sample is expected, given the previous samples. This is an overview of that process. I may have made a mistake here, but cannot go back an check. I may have not accounted for the serial correlation inherent to looking at a rolling window. This would underestimate the expected variance and inflate the number of flagged samples.

[]
