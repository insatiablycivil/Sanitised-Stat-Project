# Sanitised-Stat-Project

## Background
This project was intended to create data-driven limits for anomaly detection of gas levels in Transformers.
*Please note*, the scope of the project was specifically to use *multi-variate normal distributions*. This greatly limits my options in delivering a technical solution.
I cannot share the code, but here is an overview comparable to what would be contained in a paper, i.e. description of functionality achieved. The analytics was done in R. I also developed a Python GUI Wrapper (not shown / included here) to allow for local batch processing via selection of a .cvs file. I also created a separate prototype that was deployed on a commercial platform within a company's sandboxed test environment.

Unfortunately, you can't see much of the work which was more software engineering related. The code allows for almost every aspect to be adjusted via input arguments at run-time. From the number of variables, the relationships between them, the algorithms used for detrending, the metrics and limits used for flagging, etc. This module was built to not crash; it can handle different variables asynchronously going offline and coming back, etc.

## Motivation
Whereas the thresholds in the standards are often defined as percentile-based bounds of a given asset fleet, there is sufficient data with online measurements to create bespoke thresholds per transformer. This increased specificity in the norm-generating population should therefore increase the relevancy in the generated thresholds. This project created an unsupervised conditional anomaly detection approach for power transformers. It was developed into a practical, deployable system that is both intuitive and interpretable to help with decision support regarding the condition monitoring of transformers using DGA data as a basis. A primary motivator for this is the lack of ground-truth validation data. Since the overall accuracy cannot be quantified, it is important that the engineer can easily validate the outputs by having a traceable and interpretable set of metrics. This system is designed as a decision-support tool helping guide engineers’ attention to areas most likely to contain problems.

## Annotated Example of Output
This is screenshot of an output generated within the GUI Wrapper (also within R IDE if ran there). Based on GGPlot2 Library. The GUI Wrapper also supported Plotly for interactive plots if desired.

![alt text](/CAD_Demo_Output.png "Annotated Screenshot of Output")

## Process Overview
This is a very high level overview of the flow of data from input to output.

![alt text](/CAD_Overview.png "Flowchart of Overall Process")

## Functionality
A lot of software engineering went into making it flexible and extensible.
  *  Input variables can be grouped for multi-variate analysis
  *  Multiple groups can be specified (but there is no "cross-talk" between groups)
  *  Input variables can be defined as having one-way relationships (e.g. ambient weather affects gas levels unidirectionally)
  *  The detrending method can be specified
  *  The output metric method can be specified
  *  The output metric thresholds can be specified

![alt text](/CAD_Functionality.png "Flowchart of Functionality Options")

## Imputation
Imputation is needed to account for missing data. Various methods were implemented and could be selected at run-time via input arguments. We did not have case study data with real missing data, so it was difficult to validate performance. I ended up suggested to simply use the last known value as I did not feel comfortable suggesting alternatives sans validation.

![alt text](/CAD_Imputation.png "Flowchart of Imputation Functionality")

## Conditional Anomaly Detection
The concept was to adjust expectations based on environmental factors (conditions). This is an overview of the process.

![alt text](/CAD_CAD.png "Flowchart of Conditional Anomaly Detection Functionality")

## Dual-Sample Approach
Again, because only normal multi-variate distributions were wanted, it required a means to deal with a higher propensity to flag outliers. The chosen approach was to consider two consecutive samples together. In hindsight, I regret this. It *significantly* complicated the implementation and feels inelegant.

![alt text](/CAD_Dual_Sample.png "Flowchart of Dual Sample Approach Used for Anomaly Detection")

## "Consolidation" Process
Once any missing data was imputed, and the relationships, etc., set up, the final stage was to calculate a metric to signify to what extent the current sample is expected, given the previous samples. This is an overview of that process. I may have made a mistake here, but cannot go back an check. I may have not accounted for the serial correlation inherent to looking at a rolling window. This would underestimate the expected variance and inflate the number of flagged samples.

![alt text](/CAD_Consolidation.png "Flowchart of Consolidation Process to Generate Output Metric")
