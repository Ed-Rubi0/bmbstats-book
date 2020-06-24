---
output: html_document
editor_options: 
  chunk_output_type: console
---

# Causal inference

Does playing basketball makes one taller? This is a an example of a causal question. Wrestling with the concept of causality, as a philosophical construct is outside the scope of this book (and the author too), but I will define it using the *counterfactual theory* or *potential outcomes* perspective [@hernanSecondChanceGet2019; @kleinbergWhyGuideFinding2015; @pearlBookWhyNew2018; @angristMasteringMetricsPath2015; @gelmanCausalityStatisticalLearning2011] that define causes in terms of how things would have been different had the cause not occurred, as well as from *causality-as-intervention* perspective [@gelmanCausalityStatisticalLearning2011], which necessitates clearly defined interventions [@hernanCWordScientificEuphemisms2018; @hernanDoesObesityShorten2008; @hernanDoesWaterKill2016]. In other words, would someone be shorter if basketball was never trained?

There are two broad classes of inferential questions that focus on *what if* and *why*: *forward causal inference* ("What might happen if we do *X*?") and *reverse causal inference* ("What causes *Y*? Why?") [@gelmanCausalityStatisticalLearning2011]. Forward causation is more clearly defined problem, where the goal is to quantify the causal effect of treatment. Questions of forward causation are most directly studied using *randomization* [@gelmanCausalityStatisticalLearning2011] and are answered from the above mentioned causality-as-intervention and counterfactual perspectives. Reverse causation is more complex and it is more related to *explaining* the causal chains using the *system-variable* approach.  Article by @gelmanCausalityStatisticalLearning2011 provides great overview of the most common causal perspectives, out of which I will mostly focus on forward causation.    

## Necessary versus sufficient causality

Furthermore, we also need to distinguish between four kinds of causation [@pearlBookWhyNew2018; @kleinbergWhyGuideFinding2015]: *necessary causation*, *sufficient causation* and neither or both. For example, if someone says that A causes B, then:

- If A is *necessary* for B, it means that if A never happened (counterfactual reasoning), then B will never happen. Or, in other words, B can never happen without A. But sufficient causality also means that A can happen without B happening. 
- If A is *sufficient* for B, it means that if you have A, you will *always* have B. In other words, B always follows A. However, sometimes B can happen without A
- If A is *neither sufficient nor necessary* for B, then sometimes when A happens B will happen. B can also happen without A.
- If A is both necessary and sufficient for B, then B will always happen after A, and B will never happen without A. 

Table \@ref(tab:four-causes-table) contains summary of the above necessary and sufficient causality. In all four types of causation, the concept of counterfactual reasoning is invoked. 

(ref:four-causes-table-caption) **Four kinds of causation**

<table>
<caption>(\#tab:four-causes-table)(ref:four-causes-table-caption)</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> Cause </th>
   <th style="text-align:left;"> Necessary </th>
   <th style="text-align:left;"> Sufficient </th>
   <th style="text-align:left;"> Neither </th>
   <th style="text-align:left;"> Both </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> A happens </td>
   <td style="text-align:left;"> B might happen </td>
   <td style="text-align:left;"> B always happen </td>
   <td style="text-align:left;"> B might happen </td>
   <td style="text-align:left;"> B always happen </td>
  </tr>
  <tr>
   <td style="text-align:left;"> A doesn't happen </td>
   <td style="text-align:left;"> B never happens </td>
   <td style="text-align:left;"> B might happen </td>
   <td style="text-align:left;"> B might happen </td>
   <td style="text-align:left;"> B never happens </td>
  </tr>
</tbody>
</table>

Although the causal inference is a broad area of research, philosophical discussion and conflicts, there are a few key concepts that need to be introduced to get the big picture and understand the basics behind the aims of causal inference. Let’s start with an example involving the aforementioned question whether playing basketball makes one taller.

## Observational data

In order to answer this question, we have collected height data (expressed in cm) for the total of N=30 athletes, of which N=15 play basketball, and N=15 don’t play basketball (Table \@ref(tab:basketball-data)). Playing basketball can be considered *intervention* or *treatment*, in which causal effect we are interested in. Basketball players are considered *intervention group* or *treatment group* and those without the treatment are considered *comparison group* or *control group*

(ref:basketball-data-caption) **Height in the treatment and control groups**

<table class="table" style="font-size: 10px; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">(\#tab:basketball-data)(ref:basketball-data-caption)</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> Athlete </th>
   <th style="text-align:left;"> Treatment </th>
   <th style="text-align:right;"> Height (cm) </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Athlete 27 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 214 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 01 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 214 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 25 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 211 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 19 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 210 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 03 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 207 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 21 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 200 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 23 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 199 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 15 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 198 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 17 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 193 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 07 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 192 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 29 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 192 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 13 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 191 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 05 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 191 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 11 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 184 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 09 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 180 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 02 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 189 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 28 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 183 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 06 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 181 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 14 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 180 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 04 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 179 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 12 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 176 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 18 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 176 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 08 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 173 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 26 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 173 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 24 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 170 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 30 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 168 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 20 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 168 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 16 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 165 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 10 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 165 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 22 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 163 </td>
  </tr>
</tbody>
</table>

Using descriptive estimators introduced in the [Description] section, one can quickly calculate the group `mean` and `SD` as well as their difference (Table \@ref(tab:descriptive-group-analysis)). But does mean difference between basketball and control represent *average causal effect* (ACE)[^ACE_ATE]? No, unfortunately not!

[^ACE_ATE]: Another term used is *average treatment effect* (ATE)

(ref:descriptive-group-analysis-caption) **Descriptive analysis of the groups**

<table>
<caption>(\#tab:descriptive-group-analysis)(ref:descriptive-group-analysis-caption)</caption>
 <thead>
  <tr>
   <th style="text-align:left;">  </th>
   <th style="text-align:right;"> Mean (cm) </th>
   <th style="text-align:right;"> SD (cm) </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 198.59 </td>
   <td style="text-align:right;"> 10.86 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 174.04 </td>
   <td style="text-align:right;"> 7.54 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Difference </td>
   <td style="text-align:right;"> 24.55 </td>
   <td style="text-align:right;"> 13.22 </td>
  </tr>
</tbody>
</table>

## Potential outcomes or counterfactuals

To explain why this is the case, we need to imagine *alternate counterfactual reality*. What is needed are two potential outcomes: $Height_{0}$, which represents height of the person if one doesn't train basketball, and $Height_{1}$ which represents height of the person if basketball is being played (Table \@ref(tab:basketball-counterfactuals)). As can be guessed, the Basketball group has known $Height_{1}$, but unknown $Height_{0}$ and *vice versa* for the Control group.

(ref:basketball-counterfactuals-caption) **Counterfactuals of potential outcomes that are unknown**

<table class="table" style="font-size: 10px; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">(\#tab:basketball-counterfactuals)(ref:basketball-counterfactuals-caption)</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> Athlete </th>
   <th style="text-align:left;"> Treatment </th>
   <th style="text-align:right;"> Height_0 (cm) </th>
   <th style="text-align:right;"> Height_1 (cm) </th>
   <th style="text-align:right;"> Height (cm) </th>
   <th style="text-align:left;"> Causal Effect (cm) </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Athlete 27 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> ??? </td>
   <td style="text-align:right;"> 214 </td>
   <td style="text-align:right;"> 214 </td>
   <td style="text-align:left;"> ??? </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 01 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> ??? </td>
   <td style="text-align:right;"> 214 </td>
   <td style="text-align:right;"> 214 </td>
   <td style="text-align:left;"> ??? </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 25 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> ??? </td>
   <td style="text-align:right;"> 211 </td>
   <td style="text-align:right;"> 211 </td>
   <td style="text-align:left;"> ??? </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 19 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> ??? </td>
   <td style="text-align:right;"> 210 </td>
   <td style="text-align:right;"> 210 </td>
   <td style="text-align:left;"> ??? </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 03 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> ??? </td>
   <td style="text-align:right;"> 207 </td>
   <td style="text-align:right;"> 207 </td>
   <td style="text-align:left;"> ??? </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 21 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> ??? </td>
   <td style="text-align:right;"> 200 </td>
   <td style="text-align:right;"> 200 </td>
   <td style="text-align:left;"> ??? </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 23 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> ??? </td>
   <td style="text-align:right;"> 199 </td>
   <td style="text-align:right;"> 199 </td>
   <td style="text-align:left;"> ??? </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 15 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> ??? </td>
   <td style="text-align:right;"> 198 </td>
   <td style="text-align:right;"> 198 </td>
   <td style="text-align:left;"> ??? </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 17 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> ??? </td>
   <td style="text-align:right;"> 193 </td>
   <td style="text-align:right;"> 193 </td>
   <td style="text-align:left;"> ??? </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 07 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> ??? </td>
   <td style="text-align:right;"> 192 </td>
   <td style="text-align:right;"> 192 </td>
   <td style="text-align:left;"> ??? </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 29 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> ??? </td>
   <td style="text-align:right;"> 192 </td>
   <td style="text-align:right;"> 192 </td>
   <td style="text-align:left;"> ??? </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 13 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> ??? </td>
   <td style="text-align:right;"> 191 </td>
   <td style="text-align:right;"> 191 </td>
   <td style="text-align:left;"> ??? </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 05 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> ??? </td>
   <td style="text-align:right;"> 191 </td>
   <td style="text-align:right;"> 191 </td>
   <td style="text-align:left;"> ??? </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 11 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> ??? </td>
   <td style="text-align:right;"> 184 </td>
   <td style="text-align:right;"> 184 </td>
   <td style="text-align:left;"> ??? </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 09 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> ??? </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:left;"> ??? </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 02 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 189 </td>
   <td style="text-align:right;"> ??? </td>
   <td style="text-align:right;"> 189 </td>
   <td style="text-align:left;"> ??? </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 28 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 183 </td>
   <td style="text-align:right;"> ??? </td>
   <td style="text-align:right;"> 183 </td>
   <td style="text-align:left;"> ??? </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 06 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 181 </td>
   <td style="text-align:right;"> ??? </td>
   <td style="text-align:right;"> 181 </td>
   <td style="text-align:left;"> ??? </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 14 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> ??? </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:left;"> ??? </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 04 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 179 </td>
   <td style="text-align:right;"> ??? </td>
   <td style="text-align:right;"> 179 </td>
   <td style="text-align:left;"> ??? </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 12 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 176 </td>
   <td style="text-align:right;"> ??? </td>
   <td style="text-align:right;"> 176 </td>
   <td style="text-align:left;"> ??? </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 18 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 176 </td>
   <td style="text-align:right;"> ??? </td>
   <td style="text-align:right;"> 176 </td>
   <td style="text-align:left;"> ??? </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 08 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 173 </td>
   <td style="text-align:right;"> ??? </td>
   <td style="text-align:right;"> 173 </td>
   <td style="text-align:left;"> ??? </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 26 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 173 </td>
   <td style="text-align:right;"> ??? </td>
   <td style="text-align:right;"> 173 </td>
   <td style="text-align:left;"> ??? </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 24 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 170 </td>
   <td style="text-align:right;"> ??? </td>
   <td style="text-align:right;"> 170 </td>
   <td style="text-align:left;"> ??? </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 30 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 168 </td>
   <td style="text-align:right;"> ??? </td>
   <td style="text-align:right;"> 168 </td>
   <td style="text-align:left;"> ??? </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 20 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 168 </td>
   <td style="text-align:right;"> ??? </td>
   <td style="text-align:right;"> 168 </td>
   <td style="text-align:left;"> ??? </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 16 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 165 </td>
   <td style="text-align:right;"> ??? </td>
   <td style="text-align:right;"> 165 </td>
   <td style="text-align:left;"> ??? </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 10 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 165 </td>
   <td style="text-align:right;"> ??? </td>
   <td style="text-align:right;"> 165 </td>
   <td style="text-align:left;"> ??? </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 22 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 163 </td>
   <td style="text-align:right;"> ??? </td>
   <td style="text-align:right;"> 163 </td>
   <td style="text-align:left;"> ??? </td>
  </tr>
</tbody>
</table>

Unfortunately, these potential outcomes are unknown, and thus individual causal effects are unknown as well. We just do not know what might have happened to individual outcomes in counterfactual world (i.e. alternate reality). A good control group serves as a *proxy* to reveal what might have happened *on average* to the treated group in the counterfactual world where they are not treated. Since the basketball data is simulated, the exact DGP is known (the *true* systematic or main causal effect of playing basketball on height is exactly zero), which again demonstrates the use of simulations as a great learning tool, in this case understanding the underlying causal mechanisms (Table \@ref(tab:table-counterfactuals-simulated)). Individual causal effect in this case is the difference between two potential outcomes: $Height_{1}$ and $Height_{0}$. 

(ref:table-counterfactuals-simulated-caption) **Simulated causal effects and known counterfactuals**

<table class="table" style="font-size: 10px; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">(\#tab:table-counterfactuals-simulated)(ref:table-counterfactuals-simulated-caption)</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> Athlete </th>
   <th style="text-align:left;"> Treatment </th>
   <th style="text-align:right;"> Height_0 (cm) </th>
   <th style="text-align:right;"> Height_1 (cm) </th>
   <th style="text-align:right;"> Height (cm) </th>
   <th style="text-align:right;"> Causal Effect (cm) </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Athlete 27 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 214 </td>
   <td style="text-align:right;"> 214 </td>
   <td style="text-align:right;"> 214 </td>
   <td style="text-align:right;"> 0.12 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 01 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 214 </td>
   <td style="text-align:right;"> 214 </td>
   <td style="text-align:right;"> 214 </td>
   <td style="text-align:right;"> 0.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 25 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 212 </td>
   <td style="text-align:right;"> 211 </td>
   <td style="text-align:right;"> 211 </td>
   <td style="text-align:right;"> -1.10 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 19 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 210 </td>
   <td style="text-align:right;"> 210 </td>
   <td style="text-align:right;"> 210 </td>
   <td style="text-align:right;"> 0.90 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 03 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 208 </td>
   <td style="text-align:right;"> 207 </td>
   <td style="text-align:right;"> 207 </td>
   <td style="text-align:right;"> -0.07 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 21 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 200 </td>
   <td style="text-align:right;"> 200 </td>
   <td style="text-align:right;"> 200 </td>
   <td style="text-align:right;"> 0.20 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 23 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 198 </td>
   <td style="text-align:right;"> 199 </td>
   <td style="text-align:right;"> 199 </td>
   <td style="text-align:right;"> 0.57 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 15 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 198 </td>
   <td style="text-align:right;"> 198 </td>
   <td style="text-align:right;"> 198 </td>
   <td style="text-align:right;"> 0.18 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 17 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 193 </td>
   <td style="text-align:right;"> 193 </td>
   <td style="text-align:right;"> 193 </td>
   <td style="text-align:right;"> 0.44 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 07 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 193 </td>
   <td style="text-align:right;"> 192 </td>
   <td style="text-align:right;"> 192 </td>
   <td style="text-align:right;"> -0.09 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 29 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 193 </td>
   <td style="text-align:right;"> 192 </td>
   <td style="text-align:right;"> 192 </td>
   <td style="text-align:right;"> -0.40 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 13 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 192 </td>
   <td style="text-align:right;"> 191 </td>
   <td style="text-align:right;"> 191 </td>
   <td style="text-align:right;"> -0.36 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 05 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 191 </td>
   <td style="text-align:right;"> 191 </td>
   <td style="text-align:right;"> 191 </td>
   <td style="text-align:right;"> -0.15 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 11 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 183 </td>
   <td style="text-align:right;"> 184 </td>
   <td style="text-align:right;"> 184 </td>
   <td style="text-align:right;"> 0.46 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 09 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 179 </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> 0.72 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 02 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 189 </td>
   <td style="text-align:right;"> 189 </td>
   <td style="text-align:right;"> 189 </td>
   <td style="text-align:right;"> 0.06 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 28 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 183 </td>
   <td style="text-align:right;"> 184 </td>
   <td style="text-align:right;"> 183 </td>
   <td style="text-align:right;"> 0.41 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 06 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 181 </td>
   <td style="text-align:right;"> 181 </td>
   <td style="text-align:right;"> 181 </td>
   <td style="text-align:right;"> 0.42 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 14 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> -0.66 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 04 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 179 </td>
   <td style="text-align:right;"> 179 </td>
   <td style="text-align:right;"> 179 </td>
   <td style="text-align:right;"> 0.10 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 12 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 176 </td>
   <td style="text-align:right;"> 176 </td>
   <td style="text-align:right;"> 176 </td>
   <td style="text-align:right;"> -0.39 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 18 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 176 </td>
   <td style="text-align:right;"> 175 </td>
   <td style="text-align:right;"> 176 </td>
   <td style="text-align:right;"> -0.31 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 08 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 173 </td>
   <td style="text-align:right;"> 173 </td>
   <td style="text-align:right;"> 173 </td>
   <td style="text-align:right;"> -0.55 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 26 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 173 </td>
   <td style="text-align:right;"> 174 </td>
   <td style="text-align:right;"> 173 </td>
   <td style="text-align:right;"> 0.77 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 24 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 170 </td>
   <td style="text-align:right;"> 170 </td>
   <td style="text-align:right;"> 170 </td>
   <td style="text-align:right;"> 0.02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 30 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 168 </td>
   <td style="text-align:right;"> 168 </td>
   <td style="text-align:right;"> 168 </td>
   <td style="text-align:right;"> 0.27 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 20 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 168 </td>
   <td style="text-align:right;"> 168 </td>
   <td style="text-align:right;"> 168 </td>
   <td style="text-align:right;"> -0.03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 16 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 165 </td>
   <td style="text-align:right;"> 165 </td>
   <td style="text-align:right;"> 165 </td>
   <td style="text-align:right;"> -0.01 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 10 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 165 </td>
   <td style="text-align:right;"> 164 </td>
   <td style="text-align:right;"> 165 </td>
   <td style="text-align:right;"> -0.81 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 22 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 163 </td>
   <td style="text-align:right;"> 163 </td>
   <td style="text-align:right;"> 163 </td>
   <td style="text-align:right;"> 0.00 </td>
  </tr>
</tbody>
</table>

From Table \@ref(tab:table-counterfactuals-simulated), we can state that the mean difference between the groups consists of two components: *average causal effect* and the *selection bias* [@angristMasteringMetricsPath2015] (Equation \@ref(eq:mean-causal-effect)).

$$
\begin{equation}
  \begin{split}
    mean_{difference} &= Average \; causal\; effect + Selection\; bias \\
    Average \; causal\; effect &= \frac{1}{N_{Basketball}}\Sigma_{i=1}^{n}(Height_{1i} - Height_{0i}) \\
    Selection\; bias &= \frac{1}{N_{Basketball}}\Sigma_{i=1}^{n}Height_{0i} - \frac{1}{N_{Control}}\Sigma_{i=1}^{n}Height_{0i}
  \end{split}
  (\#eq:mean-causal-effect)
\end{equation}
$$



The mean group difference we have observed (24.55cm) is due to average causal effect (0.1cm) and selection bias (24.46cm). In other words, observed mean group difference can be explained solely by selection bias. Since we know the DGP behind the basketball data, we know that there is no systematic causal effect of playing basketball on height.  

On top of the selection bias involved in the example above, other *confounders* might be involved, such as age, sex, race, experience and others, some of which can be measured and some might be unknown. These are also referred to as the *third variable* which confounds the causal relationship between treatment and the outcome. In this example, all subjects from the Basketball group might be older males, whereas all the subjects from the Control group might be be younger females, and this can explain the group differences, rather than causal effect of playing basketball. 

## *Ceteris paribus* and the biases

It is important to understand that, in order to have causal interpretation, comparisons need to be made under *ceteris paribus* conditions [@angristMasteringMetricsPath2015], which is Latin for *other things equal*. In the basketball example above, we cannot make causal claim that playing basketball makes one taller, since comparison between the groups is not done in the *ceteris paribus* conditions due to the selection bias involved. We also know this since we know the DGP behind the observed data. 

Causal inference thus aims to achieve *ceteris paribus* conditions needed to make causal interpretations by careful considerations of the known and unknown biases involved [@angristMasteringMetricsPath2015; @hernanCausalDiagramsDraw2017; @hernanCausalInference2019; @hernanDoesWaterKill2016; @hernanSecondChanceGet2019; @ledererControlConfoundingReporting2019; @rohrerThinkingClearlyCorrelations2018; @shrierReducingBiasDirected2008].

According to Hernan *et al.* [@hernanCausalDiagramsDraw2017; @hernanCausalInference2019], there are three types of biases involved in causal inference: *confounding*, *selection bias* and *measurement bias*. 

Confounding is the bias that arises when treatment and outcome share causes. This is because treatment was not randomly assigned [@hernanCausalDiagramsDraw2017; @hernanCausalInference2019]. For example, athletes that are naturally taller might be choosing to play basketball due to success and enjoyment over their shorter peers. On the other hand, it might be some hidden confounder that motivates *to-be-tall* athletes to choose basketball. Known and measured confounders from the observational studies can be taken into account to create *ceteris paribus* conditions when estimating causal effects [@angristMasteringMetricsPath2015; @hernanCausalDiagramsDraw2017; @hernanCausalInference2019; @ledererControlConfoundingReporting2019; @rohrerThinkingClearlyCorrelations2018; @shrierReducingBiasDirected2008]. 

### Randomization

The first line of defence against confounding and selection bias is to randomly assign athletes to treatment, otherwise known as *randomized trial* or *randomized experiment*. Random assignment makes comparison between groups *ceteris paribus* providing the sample is large enough to ensure that differences in the individual characteristics such as age, sex, experience and other potential confounders are *washed out* [@angristMasteringMetricsPath2015]. In other words, random assignment works not by eliminating individual differences but rather by ensuring that the mix of the individuals being compared is the same, including the ways we cannot easily measure or observe [@angristMasteringMetricsPath2015]. 

In case the individuals from the basketball example were randomly assigned, given the known causal DGP, then the mean difference between the groups would be more indicative of the causal effect of playing basketball on height (Table \@ref(tab:randomized-basketball-data)). 

(ref:randomized-basketball-data-caption) **Randomized participants**

<table class="table" style="font-size: 10px; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">(\#tab:randomized-basketball-data)(ref:randomized-basketball-data-caption)</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> Athlete </th>
   <th style="text-align:left;"> Treatment </th>
   <th style="text-align:right;"> Height (cm) </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Athlete 01 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 214 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 25 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 211 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 19 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 210 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 03 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 207 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 23 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 199 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 15 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 198 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 13 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 191 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 02 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 189 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 28 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 184 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 14 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 180 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 04 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 179 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 12 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 176 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 26 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 174 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 24 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 170 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 16 </td>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 165 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 27 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 214 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 21 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 200 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 29 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 193 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 07 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 193 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 17 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 193 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 05 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 191 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 11 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 183 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 06 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 181 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 09 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 179 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 18 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 176 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 08 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 173 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 30 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 168 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 20 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 168 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 10 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 165 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 22 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 163 </td>
  </tr>
</tbody>
</table>

If we calculate the mean differences in this randomly assigned basketball treatment (Table \@ref(tab:basketball-randomized-summary)), we can quickly notice that random assignment washed out selection bias involved with the observational study, and that the mean difference is closer to the known systematic (or average or *expected*) causal effect. The difference between estimated systematic causal effect using mean group difference from the randomized trial and the true causal effect is due to the *sampling error* which will be explained in the [Statistical inference] section. 

(ref:basketball-randomized-summary-caption) **Descriptive summary of randomized participants**

<table>
<caption>(\#tab:basketball-randomized-summary)(ref:basketball-randomized-summary-caption)</caption>
 <thead>
  <tr>
   <th style="text-align:left;">  </th>
   <th style="text-align:right;"> Mean (cm) </th>
   <th style="text-align:right;"> SD (cm) </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Basketball </td>
   <td style="text-align:right;"> 189.91 </td>
   <td style="text-align:right;"> 16.15 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 182.65 </td>
   <td style="text-align:right;"> 14.44 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Difference </td>
   <td style="text-align:right;"> 7.25 </td>
   <td style="text-align:right;"> 21.66 </td>
  </tr>
</tbody>
</table>

Apart from creating *ceteris paribus* conditions, randomization generates a good control group that serves as a *proxy* to reveal what might have happened to the treated group in the counterfactual world where they are not treated, since $Height_0$ is not known for the basketball group. Creating those conditions with randomized trial demands careful considerations and *balance checking* since biases can *crawl* inside the causal interpretation. The logic of randomized trial is simple, yet the logistics can be quite complex. For example, a sample of sufficient size might not be practically feasible, and imbalances in the known confounders can be still found in the groups, thus demanding further control and adjustment in the analysis (e.g. using ANCOVA instead of ANOVA, adjusting for confounders in the linear regression by introducing them as interactions) in order to create *ceteris paribus* conditions needed to evaluate causal claims. Belief effect can sneak in, for example, if the treatment group *knows* they are being treated, or if researchers motivate treatment groups harder, since they expect and hope for better outcomes.  For this reason, *blinding* both the subjects and researches can be considered, as well as providing *placebo* treatment to the Control group. In sport science research blinding and providing placebo can be problematic. For example, if our intervention is a novel training method or a technology, both researchers and subjects will expect better outcomes which can bias causal interpretations. 

## Subject matter knowledge

One of the main problems with randomized trials is that it cannot be done in most real life settings, either due to the ethical or practical reasons. For example, if studying effects of smoking on baby mortality and birth defects, which parent would accept being in the treatment group. Or if studying effects of resistance training on injury risk in football players, which professional organization would allow random assignment to the treatment that is lesser than the known best practices and can predispose athletes to the injuries or sub-par preparation?

For this reason, reliance on observation studies is the best we can do. However, in order to create *ceteris paribus* conditions necessary to minimize bias in the causal interpretations, expert subject-matter knowledge is needed, not only to describe the causal structure of the system under study, but also to specify the causal questions and identify relevant data sources [@hernanSecondChanceGet2019]. Imagine asking the following causal question: "Does training load lead to overuse injuries in professional sports". It takes expert subject matter knowledge to specify the treatment construct (i.e. "training load"), to figure out how should be measured, as well as to quantify the measurement error which can induce *measurement bias*, to state over which time period the treatment is done, as well as to specify the outcome construct (i.e. "overuse-injuries"), and to define the variables and constructs that confound and define the causal network underlying such a question. This subject matter is fallible of course, and the constructs, variables and the causal network can be represented with pluralistic models that represents "Small World" maps of the complex "Large World", in which we are hoping to deploy the findings (please refer to the [Introduction] for more information about this concept). Drawing assumptions that underly causal structure using *direct acyclical graphs* (DAGs) [@hernanCausalDiagramsDraw2017; @hernanCausalInference2019; @pearlBookWhyNew2018; @rohrerThinkingClearlyCorrelations2018; @saddikiPrimerCausalityData2018; @shrierReducingBiasDirected2008; @textorRobustCausalInference2017] represents a step forward in acknowledging the issues above, by providing transparency of the assumptions involved and bridging the subjective - objective dichotomy.

## Example of randomized control trial

Let's consider the following example. We are interested in estimating causal effect of the plyometric training on the vertical jump height. To estimate causal effect, *randomized control trial* (RCT) is utilized. RCT utilizes two groups: Treatment (N=15) and Control (N=15), measured two times: Pre-test and Post-test. Treatment group received plyometric training over the course of three months, while Control group continued with *normal* training. The results of RCT study can be found in the Table \@ref(tab:rct-vj-data). To estimate practical significance of the treatment effect, SESOI of ±2.5cm is selected to indicate minimal change of the practical value. It is important to have "well defined interventions" [@hernanCWordScientificEuphemisms2018; @hernanDoesObesityShorten2008; @hernanDoesWaterKill2016], thus the question that should be answered is as follows: "Does plyometric training added to normal training improves vertical jump height over period of three months?"

(ref:rct-vj-data-caption) **Randomized control trial data**


<table class="table" style="font-size: 10px; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">(\#tab:rct-vj-data)(ref:rct-vj-data-caption)</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> Athlete </th>
   <th style="text-align:left;"> Group </th>
   <th style="text-align:right;"> Pre-test (cm) </th>
   <th style="text-align:right;"> Post-test (cm) </th>
   <th style="text-align:right;"> Change (cm) </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Athlete 01 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 37.98 </td>
   <td style="text-align:right;"> 52.86 </td>
   <td style="text-align:right;"> 14.93 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 27 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 44.79 </td>
   <td style="text-align:right;"> 58.50 </td>
   <td style="text-align:right;"> 13.43 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 19 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 46.77 </td>
   <td style="text-align:right;"> 59.76 </td>
   <td style="text-align:right;"> 12.99 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 25 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 38.90 </td>
   <td style="text-align:right;"> 49.58 </td>
   <td style="text-align:right;"> 10.81 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 03 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 41.29 </td>
   <td style="text-align:right;"> 51.41 </td>
   <td style="text-align:right;"> 10.34 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 23 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 48.41 </td>
   <td style="text-align:right;"> 57.57 </td>
   <td style="text-align:right;"> 8.58 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 17 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 44.81 </td>
   <td style="text-align:right;"> 51.41 </td>
   <td style="text-align:right;"> 7.85 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 21 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 37.14 </td>
   <td style="text-align:right;"> 44.95 </td>
   <td style="text-align:right;"> 7.37 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 15 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 46.69 </td>
   <td style="text-align:right;"> 52.73 </td>
   <td style="text-align:right;"> 6.14 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 29 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 42.77 </td>
   <td style="text-align:right;"> 47.38 </td>
   <td style="text-align:right;"> 5.02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 13 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 49.66 </td>
   <td style="text-align:right;"> 54.11 </td>
   <td style="text-align:right;"> 4.46 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 05 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 37.92 </td>
   <td style="text-align:right;"> 41.63 </td>
   <td style="text-align:right;"> 3.78 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 07 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 41.03 </td>
   <td style="text-align:right;"> 45.41 </td>
   <td style="text-align:right;"> 3.42 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 11 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 45.27 </td>
   <td style="text-align:right;"> 46.72 </td>
   <td style="text-align:right;"> 1.82 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 12 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 42.56 </td>
   <td style="text-align:right;"> 44.29 </td>
   <td style="text-align:right;"> 1.01 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 28 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 47.06 </td>
   <td style="text-align:right;"> 47.98 </td>
   <td style="text-align:right;"> 0.55 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 04 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 44.53 </td>
   <td style="text-align:right;"> 45.13 </td>
   <td style="text-align:right;"> 0.12 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 02 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 49.63 </td>
   <td style="text-align:right;"> 48.86 </td>
   <td style="text-align:right;"> -0.01 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 08 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 41.11 </td>
   <td style="text-align:right;"> 42.13 </td>
   <td style="text-align:right;"> -0.45 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 26 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 42.31 </td>
   <td style="text-align:right;"> 41.61 </td>
   <td style="text-align:right;"> -0.51 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 06 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 45.96 </td>
   <td style="text-align:right;"> 45.70 </td>
   <td style="text-align:right;"> -0.52 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 14 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 44.51 </td>
   <td style="text-align:right;"> 42.89 </td>
   <td style="text-align:right;"> -0.63 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 18 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 42.57 </td>
   <td style="text-align:right;"> 42.15 </td>
   <td style="text-align:right;"> -0.74 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 16 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 37.63 </td>
   <td style="text-align:right;"> 37.83 </td>
   <td style="text-align:right;"> -0.75 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 22 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 36.52 </td>
   <td style="text-align:right;"> 34.83 </td>
   <td style="text-align:right;"> -0.97 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 24 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 40.15 </td>
   <td style="text-align:right;"> 39.88 </td>
   <td style="text-align:right;"> -1.03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 30 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 39.34 </td>
   <td style="text-align:right;"> 38.34 </td>
   <td style="text-align:right;"> -1.21 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 09 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 47.61 </td>
   <td style="text-align:right;"> 45.62 </td>
   <td style="text-align:right;"> -1.57 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 20 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 38.94 </td>
   <td style="text-align:right;"> 36.97 </td>
   <td style="text-align:right;"> -1.72 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 10 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 36.77 </td>
   <td style="text-align:right;"> 34.15 </td>
   <td style="text-align:right;"> -2.26 </td>
  </tr>
</tbody>
</table>

Descriptive summary statistics for Treatment and Control groups are enlisted in the Table \@ref(tab:rct-summary), and visually depicted in the Figure \@ref(fig:rct-groups). 

(ref:rct-summary-caption) **RCT summary using mean ± SD**

<table>
<caption>(\#tab:rct-summary)(ref:rct-summary-caption)</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> Group </th>
   <th style="text-align:left;"> Pre-test (cm) </th>
   <th style="text-align:left;"> Post-test (cm) </th>
   <th style="text-align:left;"> Change (cm) </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:left;"> 43.4 ± 4.15 </td>
   <td style="text-align:left;"> 50.64 ± 5.4 </td>
   <td style="text-align:left;"> 7.29 ± 4.65 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:left;"> 41.97 ± 3.87 </td>
   <td style="text-align:left;"> 41.52 ± 4.49 </td>
   <td style="text-align:left;"> -0.61 ± 0.83 </td>
  </tr>
</tbody>
</table>

<div class="figure" style="text-align: center">
<img src="04-Causal-inference_files/figure-html/rct-groups-1.png" alt="(ref:rct-groups-caption)" width="90%" />
<p class="caption">(\#fig:rct-groups)(ref:rct-groups-caption)</p>
</div>

(ref:rct-groups-caption) **Visual analysis of RCT using Treatment and Control groups. A and B. **Raincloud plot of the Pre-test and Post-test scores for Treatment and Control groups. Blue color indicates Control group and orange color indicates Treatment group.  **C and D.** Raincloud plot of the change scores for the Treatment and Control groups. SESOI is indicated with a grey band

Further analysis might involve separate dependent groups analysis for both Treatment and Control (Table \@ref(tab:rct-change)), or in other words, the analysis of the change scores. To estimate `Cohen's d`, `pooled SD` of the Pre-test scores in both Treatment and Control is utilized. (see Equation \@ref(eq:cohen-diff-equation)). 

(ref:rct-change-caption) **Descriptive analysis of the change scores for Treatment and Control groups independently**

<table>
<caption>(\#tab:rct-change)(ref:rct-change-caption)</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> Estimator </th>
   <th style="text-align:right;"> Control </th>
   <th style="text-align:right;"> Treatment </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Mean change (cm) </td>
   <td style="text-align:right;"> -0.61 </td>
   <td style="text-align:right;"> 7.29 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SDchange (cm) </td>
   <td style="text-align:right;"> 0.83 </td>
   <td style="text-align:right;"> 4.65 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SDpre-test pooled (cm) </td>
   <td style="text-align:right;"> 4.01 </td>
   <td style="text-align:right;"> 4.01 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cohen's d </td>
   <td style="text-align:right;"> -0.15 </td>
   <td style="text-align:right;"> 1.82 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SESOI lower (cm) </td>
   <td style="text-align:right;"> -2.50 </td>
   <td style="text-align:right;"> -2.50 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SESOI upper (cm) </td>
   <td style="text-align:right;"> 2.50 </td>
   <td style="text-align:right;"> 2.50 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Change to SESOI </td>
   <td style="text-align:right;"> -0.12 </td>
   <td style="text-align:right;"> 1.46 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SDchange to SESOI </td>
   <td style="text-align:right;"> 0.17 </td>
   <td style="text-align:right;"> 0.93 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> pLower </td>
   <td style="text-align:right;"> 0.06 </td>
   <td style="text-align:right;"> 0.03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> pEquivalent </td>
   <td style="text-align:right;"> 0.93 </td>
   <td style="text-align:right;"> 0.14 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> pHigher </td>
   <td style="text-align:right;"> 0.01 </td>
   <td style="text-align:right;"> 0.83 </td>
  </tr>
</tbody>
</table>

Figure \@ref(fig:rct-paired-change) depicts same information as Figure \@ref(fig:rct-groups) but organized differently and conveying different comparison.  

<div class="figure" style="text-align: center">
<img src="04-Causal-inference_files/figure-html/rct-paired-change-1.png" alt="(ref:rct-paired-change-caption)" width="90%" />
<p class="caption">(\#fig:rct-paired-change)(ref:rct-paired-change-caption)</p>
</div>
(ref:rct-paired-change-caption) **Visual analysis of RCT using Treatment and Control groups. A and B. **Scatter plot of Pre-test and Post-test scores for Treatment and Control groups. Green line indicates change higher than SESOI upper, grey line indicates change within SESOI band, and red line indicates negative change lower than SESOI lower. **C. ** Distribution of the change scores for Treatment (orange) and Control (blue) groups. Grey rectangle indicates SESOI band.  

But we are not that interested in independent analysis of Treatment and Control groups, but rather on their differences and understanding of the causal effect of the treatment (i.e. understanding and estimating parameters of the underlying DGP). As stated, treatment effect consists of two components: systematic component or main effect (i.e. expected or average causal effect), and stochastic component or random effect (i.e. that varies between individuals) (see Figure \@ref(fig:te-and-nte-diagram)). As already explained, Control group serves as a proxy to what might have happened to the Treatment group in the counterfactual world, and thus allows for casual interpretation of the treatment effect. There are two effects at play with this RCT design: *treatment effect* and *non-treatment effect*. The latter captures all effects not directly controlled by a treatment, but assumes it affects both groups equally (Figure \@ref(fig:te-and-nte-diagram)). For example, if we are treating kids for longer period of time, non-treatment effect might be related to the growth and associated effects. Another non-treatment effect is *measurement error* (discussed in more details in [Measurement Error] section). 

<div class="figure" style="text-align: center">
<img src="figures/treatment-and-non-treatment-effects.png" alt="(ref:te-and-nte-diagram-caption)" width="100%" />
<p class="caption">(\#fig:te-and-nte-diagram)(ref:te-and-nte-diagram-caption)</p>
</div>
(ref:te-and-nte-diagram-caption) **Treatment and Non-treatment effects of intervention.** Both treatment and non-treatment effects consists of two components: systematic and random. Treatment group experiences both treatment and non-treatment effects, while Control group experiences only non-treatment effects. 

The following equation captures the essence of estimating Treatment effects from Pre-test and Post-test scores in the Treatment and Control groups (Equation \@ref(eq:te-and-nte-equation)):

$$
\begin{equation}
  \begin{split}
    Treatment_{post} &= Treatment_{pre} + Treatment \; Effect + NonTreatment \; Effect \\
    Control_{post} &= Control_{pre} + NonTreatment \; Effect \\
\\
    NonTreatment \; Effect &= Control_{post} - Control_{pre} \\
    Treatment \; Effect &= Treatment_{post} - Treatment_{pre} - NonTreatment \; Effect \\
\\
    Treatment \; Effect &= (Treatment_{post} - Treatment_{pre}) - (Control_{post} - Control_{pre}) \\
    Treatment \; Effect &= Treatment_{change} - Control_{change}
  \end{split}
  (\#eq:te-and-nte-equation)
\end{equation}
$$

From the Equation \@ref(eq:te-and-nte-equation), the differences between the changes in Treatment and Control groups can be interpreted as the estimate of the causal effect of the treatment. More precisely, average causal effect or expected causal effect represent systematic treatment effect. This is estimated using difference between `mean` Treatment change and `mean` Control change.

Table \@ref(tab:rct-te-estimates) contains descriptive statistics of the change score differences. Panel C in the Figure \@ref(fig:rct-paired-change) depicts distribution of the change scores and reflect the calculus in the Table \@ref(tab:rct-te-estimates) graphically. 

(ref:rct-te-estimates-caption) **Descriptive statistics of the change score differences**

<table>
<caption>(\#tab:rct-te-estimates)(ref:rct-te-estimates-caption)</caption>
 <thead>
  <tr>
   <th style="text-align:right;"> Mean difference (cm) </th>
   <th style="text-align:right;"> Cohen's d </th>
   <th style="text-align:right;"> Difference to SESOI </th>
   <th style="text-align:right;"> pLower diff </th>
   <th style="text-align:right;"> pEquivalent diff </th>
   <th style="text-align:right;"> pHigher diff </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 7.9 </td>
   <td style="text-align:right;"> 9.56 </td>
   <td style="text-align:right;"> 1.58 </td>
   <td style="text-align:right;"> -0.03 </td>
   <td style="text-align:right;"> -0.79 </td>
   <td style="text-align:right;"> 0.82 </td>
  </tr>
</tbody>
</table>

`Cohen's d` in the Table \@ref(tab:rct-te-estimates) is calculated by using the Equation \@ref(eq:te-nte-cohen) and it estimates standardized difference between change scores in Treatment and the Control groups.

$$
\begin{equation}
Cohen's\;d = \frac{mean_{treatment\; group \; change} - mean_{control\; group \;change}}{SD_{control\; group \; change}}
(\#eq:te-nte-cohen)
\end{equation}
$$

Besides estimating systematic component of the treatment (i.e. the difference between the mean change in Treatment and Control groups), we might be interested in estimating random component and proportions of lower, equivalent and higher effects compared to SESOI (`pLower`, `pEquivalent`, and `pHigher`). Unfortunately, differences in `pLower`, `pEquivalent`, and `pHigher` from Table \@ref(tab:rct-te-estimates) don't answer this question, but rather the expected difference in proportions compared to Control (e.g. the expected improvement of 0.82 in observing proportion of higher change outcomes compared to Control).

Since the changes in Treatment group are due both to the treatment and non-treatment effects (equation 29), the average treatment effect (systematic component) represents the difference between the `mean` changes in Treatment and Control groups (Table \@ref(tab:rct-te-estimates)). In the same manner, the `variance` of the change scores in the Treatment group are due to the random component of the treatment and non-treatment effects. Assuming normal (Gaussian) distribution of the random components, the *SD of the treatment effects* ($SD_{TE}$)[^SD_IR_comment] is estimated using the following Equation \@ref(eq:sd-te-equation).   

$$
\begin{equation}
  \begin{split}
    \epsilon_{treatment \;group \;change} &= \epsilon_{treatment \; effect} + \epsilon_{nontreatment \; effect} \\
    \epsilon_{control \;group \;change} &= \epsilon_{nontreatment \; effect} \\
    \epsilon_{treatment \; effect} &= \epsilon_{treatment \;group \;change} - \epsilon_{control \;group \;change} \\
    \\
    \epsilon_{treatment \; effect}  &\sim \mathcal{N}(0,\,SD_{TE}) \\
    \epsilon_{nontreatment \; effect}  &\sim \mathcal{N}(0,\,SD_{NTE}) \\
    \epsilon_{treatment \;group \;change} &\sim \mathcal{N}(0,\,SD_{treatment \;group \;change}) \\
    \epsilon_{control \;group \;change} &\sim \mathcal{N}(0,\,SD_{control \;group \;change}) \\
    \\
    SD_{TE} &= \sqrt{SD_{treatment \;group \;change}^2 - SD_{control \;group \;change}^2}
  \end{split}
  (\#eq:sd-te-equation)
\end{equation}
$$

[^SD_IR_comment]: Also referred to as $SD_{IR}$ or standard deviation of the intervention responses [@hopkinsIndividualResponsesMade2015; @swintonStatisticalFrameworkInterpret2018]. $SD_{IR}$ or $SD_{TE}$ represent estimate of treatment effect heterogeneity, also referred to as *variable treatment effect* (VTE)

This neat mathematical solution is due to assumption of Gaussian error, assumption that random treatment and non-treatment effects are equal across subjects (see [Ergodicity] section for more details about this assumption), and the use of squared errors. This is one beneficial property of using squared errors that I alluded to in the section [Cross-Validation] section. 

Thus, the estimated parameters of the causal treatment effects in the underlying DGP are are summarized with the following Equation \@ref(eq:treatment-effects-estimates). This treatment effect is graphically depicted in the Figure \@ref(fig:te-effects). 

$$
\begin{equation}
  \begin{split}
    Treatment \; effect &\sim \mathcal{N}(Mean_{TE},\,SD_{TE}) \\
    \\
    Mean_{TE} &= Mean_{treatment \;group \;change} - Mean_{control \;group \;change} \\ 
    \\
    SD_{TE} &= \sqrt{SD_{treatment \;group \;change}^2 - SD_{control \; group \; change}^2}
  \end{split}
  (\#eq:treatment-effects-estimates)
\end{equation}
$$


<div class="figure" style="text-align: center">
<img src="04-Causal-inference_files/figure-html/te-effects-1.png" alt="(ref:te-effects-caption)" width="90%" />
<p class="caption">(\#fig:te-effects)(ref:te-effects-caption)</p>
</div>

(ref:te-effects-caption) **Graphical representation of the causal Treatment effect.** Green area indicates proportion of higher than SESOI treatment effects, red indicates proportion of negative and lower than SESOI treatment effects, and grey indicates treatment effects that are within SESOI. `Mean` of treatment effect distribution represents average (or expected) causal effect or systematic treatment effect. `SD` of treatment effect distribution represents random systematic effect or $SD_{TE}$

Using SESOI, one can also estimate the proportion of lower, equivalent and higher changes (responses) caused by treatment. The estimates of the causal treatment effects, with accompanying proportions of responses are enlisted in the Table \@ref(tab:te-effects-estimates).

(ref:te-effects-estimates-caption) **Estimates of the causal treatment effects**

<table>
<caption>(\#tab:te-effects-estimates)(ref:te-effects-estimates-caption)</caption>
 <thead>
  <tr>
   <th style="text-align:right;"> Average causal effect (cm) </th>
   <th style="text-align:right;"> Random effect (cm) </th>
   <th style="text-align:left;"> SESOI (cm) </th>
   <th style="text-align:right;"> Average causal effect to SESOI </th>
   <th style="text-align:right;"> SESOI to random effect </th>
   <th style="text-align:right;"> pLower </th>
   <th style="text-align:right;"> pEquivalent </th>
   <th style="text-align:right;"> pHigher </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 7.9 </td>
   <td style="text-align:right;"> 4.57 </td>
   <td style="text-align:left;"> ±2.5 </td>
   <td style="text-align:right;"> 1.58 </td>
   <td style="text-align:right;"> 1.09 </td>
   <td style="text-align:right;"> 0.01 </td>
   <td style="text-align:right;"> 0.11 </td>
   <td style="text-align:right;"> 0.88 </td>
  </tr>
</tbody>
</table>

Therefore, we can conclude that plyometric training over three months period, on top of the normal training, cause improvements in vertical jump height (in the sample collected; *generalizations* beyond sample are discussed in the [Statistical inference] section). The expected improvement (i.e. average causal effect or systematic effect) is equal to 7.9cm, with 1, 11, and 88% of athletes having lower, trivial and higher improvements. 

## Prediction as a complement to causal inference

In the previous section, RCT is analyzed using *analysis of changes*. In this section, I will utilize linear regression model to analyze RCT data. There are multiple ways this could be done [@jDifferentWaysEstimate2018] and deeper analysis is beyond the scope of this book (see also [Frank Harrell](https://www.fharrell.com/post/errmed/#change) post on the use of change scores). The aim of this section is to provide an introduction to *model-based* and *prediction-based* RCT analysis, as well as to demonstrate potential uses of PDP+ICE plots as tools for counterfactual analysis. 

Analysis in the previous section can be represented using simple linear regression model (Equation \@ref(eq:simple-rct-change-regression)). 

$$
\begin{equation}
  \widehat{Change} = \hat{\beta_0} + \hat{\beta_1}Group
  (\#eq:simple-rct-change-regression)
\end{equation}
$$

According to [Frank Harrell](https://www.fharrell.com/post/errmed/#change), the use of change scores in the RCT analysis is problematic. Although this model definition (Equation \@ref(eq:simple-rct-change-regression)) will give us exactly the same results as obtained in the previous section, the use of change scores should be avoided. Thus, look at this example as *training vehicle*. After this initial discussion, valid model representation will be used. 

Since Group column is a string, how is Group column represented in the model? Group column needs to be  *dummy-coded*, using 0 for Control and 1 for Treatment (see Table \@ref(tab:dummy-coded)). 

(ref:dummy-coded-caption) **Dummy coding of the Group column to be used in linear regression model**

<table class="table" style="font-size: 10px; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">(\#tab:dummy-coded)(ref:dummy-coded-caption)</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> Athlete </th>
   <th style="text-align:right;"> groupTreatment </th>
   <th style="text-align:right;"> Pre-test (cm) </th>
   <th style="text-align:right;"> Post-test (cm) </th>
   <th style="text-align:right;"> Change (cm) </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Athlete 12 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 42.56 </td>
   <td style="text-align:right;"> 44.29 </td>
   <td style="text-align:right;"> 1.01 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 28 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 47.06 </td>
   <td style="text-align:right;"> 47.98 </td>
   <td style="text-align:right;"> 0.55 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 04 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 44.53 </td>
   <td style="text-align:right;"> 45.13 </td>
   <td style="text-align:right;"> 0.12 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 02 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 49.63 </td>
   <td style="text-align:right;"> 48.86 </td>
   <td style="text-align:right;"> -0.01 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 08 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 41.11 </td>
   <td style="text-align:right;"> 42.13 </td>
   <td style="text-align:right;"> -0.45 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 26 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 42.31 </td>
   <td style="text-align:right;"> 41.61 </td>
   <td style="text-align:right;"> -0.51 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 06 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 45.96 </td>
   <td style="text-align:right;"> 45.70 </td>
   <td style="text-align:right;"> -0.52 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 14 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 44.51 </td>
   <td style="text-align:right;"> 42.89 </td>
   <td style="text-align:right;"> -0.63 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 18 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 42.57 </td>
   <td style="text-align:right;"> 42.15 </td>
   <td style="text-align:right;"> -0.74 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 16 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 37.63 </td>
   <td style="text-align:right;"> 37.83 </td>
   <td style="text-align:right;"> -0.75 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 22 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 36.52 </td>
   <td style="text-align:right;"> 34.83 </td>
   <td style="text-align:right;"> -0.97 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 24 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 40.15 </td>
   <td style="text-align:right;"> 39.88 </td>
   <td style="text-align:right;"> -1.03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 30 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 39.34 </td>
   <td style="text-align:right;"> 38.34 </td>
   <td style="text-align:right;"> -1.21 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 20 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 38.94 </td>
   <td style="text-align:right;"> 36.97 </td>
   <td style="text-align:right;"> -1.72 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 10 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 36.77 </td>
   <td style="text-align:right;"> 34.15 </td>
   <td style="text-align:right;"> -2.26 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 01 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 37.98 </td>
   <td style="text-align:right;"> 52.86 </td>
   <td style="text-align:right;"> 14.93 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 27 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 44.79 </td>
   <td style="text-align:right;"> 58.50 </td>
   <td style="text-align:right;"> 13.43 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 19 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 46.77 </td>
   <td style="text-align:right;"> 59.76 </td>
   <td style="text-align:right;"> 12.99 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 25 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 38.90 </td>
   <td style="text-align:right;"> 49.58 </td>
   <td style="text-align:right;"> 10.81 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 03 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 41.29 </td>
   <td style="text-align:right;"> 51.41 </td>
   <td style="text-align:right;"> 10.34 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 23 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 48.41 </td>
   <td style="text-align:right;"> 57.57 </td>
   <td style="text-align:right;"> 8.58 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 17 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 44.81 </td>
   <td style="text-align:right;"> 51.41 </td>
   <td style="text-align:right;"> 7.85 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 21 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 37.14 </td>
   <td style="text-align:right;"> 44.95 </td>
   <td style="text-align:right;"> 7.37 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 15 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 46.69 </td>
   <td style="text-align:right;"> 52.73 </td>
   <td style="text-align:right;"> 6.14 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 29 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 42.77 </td>
   <td style="text-align:right;"> 47.38 </td>
   <td style="text-align:right;"> 5.02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 13 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 49.66 </td>
   <td style="text-align:right;"> 54.11 </td>
   <td style="text-align:right;"> 4.46 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 05 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 37.92 </td>
   <td style="text-align:right;"> 41.63 </td>
   <td style="text-align:right;"> 3.78 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 07 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 41.03 </td>
   <td style="text-align:right;"> 45.41 </td>
   <td style="text-align:right;"> 3.42 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 11 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 45.27 </td>
   <td style="text-align:right;"> 46.72 </td>
   <td style="text-align:right;"> 1.82 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 09 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 47.61 </td>
   <td style="text-align:right;"> 45.62 </td>
   <td style="text-align:right;"> -1.57 </td>
  </tr>
</tbody>
</table>

Estimated parameters for this linear model are enlisted in the Table \@ref(tab:simple-rct-model-coef)

(ref:simple-rct-model-coef-caption) **Estimated linear regression parameters for the simple RCT model**

<table>
<caption>(\#tab:simple-rct-model-coef)(ref:simple-rct-model-coef-caption)</caption>
 <thead>
  <tr>
   <th style="text-align:right;"> Intercept </th>
   <th style="text-align:right;"> groupTreatment </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> -0.61 </td>
   <td style="text-align:right;"> 7.9 </td>
  </tr>
</tbody>
</table>

Intercept in the Table \@ref(tab:simple-rct-model-coef) represents the `mean` Change in the Control group, while $\hat{\beta_1}$ (or slope, or `GroupTreatment` parameter) represents estimated average treatment effect (ATE) or average causal effect, since it represents the difference in the Change means between groups. For a reference please refer to Tables \@ref(tab:rct-change) and \@ref(tab:te-effects-estimates). 

Figure \@ref(fig:simple-rct-model) depicts this model graphically. 

<div class="figure" style="text-align: center">
<img src="04-Causal-inference_files/figure-html/simple-rct-model-1.png" alt="(ref:simple-rct-model-caption)" width="90%" />
<p class="caption">(\#fig:simple-rct-model)(ref:simple-rct-model-caption)</p>
</div>

(ref:simple-rct-model-caption) **Graphical representation of the simple linear regression model for the vertical jump RCT data**

Model residuals are depicted on Figure \@ref(fig:simple-rct-model-ba). Please note the *clusters* of the data-points which indicate groups (they are color-coded). 

<div class="figure" style="text-align: center">
<img src="04-Causal-inference_files/figure-html/simple-rct-model-ba-1.png" alt="(ref:simple-rct-model-ba-caption)" width="90%" />
<p class="caption">(\#fig:simple-rct-model-ba)(ref:simple-rct-model-ba-caption)</p>
</div>

(ref:simple-rct-model-ba-caption) **Model residuals using simple linear regression RCT model. **Grey band represents SESOI of ±2.5cm. Residuals are color coded; blue are Control group and orange are Treatment group. 

`SD` of the residuals for the Control group is equal to 0.83cm and for Treatment group is equal to 4.65cm. Please compare these estimates and estimated parameters from the Table \@ref(tab:simple-rct-model-coef) with Table \@ref(tab:rct-change) and Table \@ref(tab:te-effects-estimates). These estimates are identical since the model utilized (Equation \@ref(eq:simple-rct-change-regression)) is mathematically equivalent to the analysis done in the [Example of randomized control trial] section.

As alluded in the introduction of this section, RCT analysis using change scores should be avoided. Valid way to analyze the RCT in this case is to use Post-test as the outcome, and Pre-test and Group as predictors. This can be easily understood graphically (Figure \@ref(fig:ancova-rct-model)). On Figure \@ref(fig:ancova-rct-model) each group (i.e. Control and Treatment) is modeled separately. 

<div class="figure" style="text-align: center">
<img src="04-Causal-inference_files/figure-html/ancova-rct-model-1.png" alt="(ref:ancova-rct-model-caption)" width="90%" />
<p class="caption">(\#fig:ancova-rct-model)(ref:ancova-rct-model-caption)</p>
</div>
(ref:ancova-rct-model-caption) **Graphical representation of the valid way to analyze RCT data.** Dashed line represent *identity line*, where Post-test is equal to Pre-test (i.e., *the no effect* line). The effect of treatment represents vertical distance between the Control and Treatment lines. This is easily grasped since the lines are almost perfectly parallel. If the lines are not parallel, that would imply there is *interaction* between Group and Pre-test (i.e. individuals with higher Pre-test scores shows higher or lower change). 

Figure \@ref(fig:ancova-rct-model) also represents *ANCOVA* (analysis of co-variance) design. Equation \@ref(eq:ancova-rct-change-regression) represent model definition where effects of Group (i.e., Treatment) are estimated by controlling the effects of the Pre-test. 

$$
\begin{equation}
  \widehat{Post} = \hat{\beta_0} + \hat{\beta_1}Group + \hat{\beta_2}Pre
  (\#eq:ancova-rct-change-regression)
\end{equation}
$$

Estimated parameters for this linear model are enlisted in the Table \@ref(tab:ancova-rct-model-coef). Please note the similarity with the Table \@ref(tab:simple-rct-model-coef). 

(ref:ancova-rct-model-coef-caption) **Estimated linear regression parameters for the ANCOVA RCT model (see Equation \@ref(eq:ancova-rct-change-regression))**

<table>
<caption>(\#tab:ancova-rct-model-coef)(ref:ancova-rct-model-coef-caption)</caption>
 <thead>
  <tr>
   <th style="text-align:right;"> Intercept </th>
   <th style="text-align:right;"> groupTreatment </th>
   <th style="text-align:right;"> Pre-test </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 3.92 </td>
   <td style="text-align:right;"> 7.85 </td>
   <td style="text-align:right;"> 0.9 </td>
  </tr>
</tbody>
</table>

Model residuals are depicted on Figure \@ref(fig:ancova-rct-model-ba). Please note the *clusters* of the data-points which indicate groups. 

<div class="figure" style="text-align: center">
<img src="04-Causal-inference_files/figure-html/ancova-rct-model-ba-1.png" alt="(ref:ancova-rct-model-ba-caption)" width="90%" />
<p class="caption">(\#fig:ancova-rct-model-ba)(ref:ancova-rct-model-ba-caption)</p>
</div>

(ref:ancova-rct-model-ba-caption) **Model residuals using ANCOVA RCT model. **Grey band represents SESOI of ±2.5cm. Residuals are color coded; blue are Control group and orange are Treatment group. 

`SD` of the residuals for the Control group is equal to 1.43cm and for Treatment group is equal to 4.64cm. Please note the similarities with simple RCT model (i.e. using Change score as outcome and Group as predictor). 

As explained in the [Prediction] section, this model can be cross-validated. Predictive performance metrics using 10 repeats of 3 folds cross-validation are enlisted in the Table \@ref(tab:ancova-rct-model-perf-metrics). Since our RCT data has two groups (i.e. Control and Treatment), cross-validation needs to be *stratified*. This makes sure that each group has their own cross-validation folds, and that testing data size for each group is proportional to the group size. This avoids scenarios where most training or testing data comes from a single group (which is more probable if one group is larger).   

(ref:ancova-rct-model-perf-metrics-caption) **Cross-validated predictive performance metrics for the ANCOVA RCT model**

<table>
<caption>(\#tab:ancova-rct-model-perf-metrics)(ref:ancova-rct-model-perf-metrics-caption)</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> metric </th>
   <th style="text-align:right;"> training </th>
   <th style="text-align:right;"> training.pooled </th>
   <th style="text-align:right;"> testing.pooled </th>
   <th style="text-align:right;"> mean </th>
   <th style="text-align:right;"> SD </th>
   <th style="text-align:right;"> min </th>
   <th style="text-align:right;"> max </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> MBE </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> -0.04 </td>
   <td style="text-align:right;"> -0.04 </td>
   <td style="text-align:right;"> 1.55 </td>
   <td style="text-align:right;"> -3.22 </td>
   <td style="text-align:right;"> 3.18 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MAE </td>
   <td style="text-align:right;"> 2.41 </td>
   <td style="text-align:right;"> 2.36 </td>
   <td style="text-align:right;"> 2.76 </td>
   <td style="text-align:right;"> 2.76 </td>
   <td style="text-align:right;"> 0.67 </td>
   <td style="text-align:right;"> 1.38 </td>
   <td style="text-align:right;"> 3.80 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> RMSE </td>
   <td style="text-align:right;"> 3.31 </td>
   <td style="text-align:right;"> 3.22 </td>
   <td style="text-align:right;"> 3.78 </td>
   <td style="text-align:right;"> 3.66 </td>
   <td style="text-align:right;"> 0.97 </td>
   <td style="text-align:right;"> 1.72 </td>
   <td style="text-align:right;"> 5.15 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PPER </td>
   <td style="text-align:right;"> 0.54 </td>
   <td style="text-align:right;"> 0.56 </td>
   <td style="text-align:right;"> 0.49 </td>
   <td style="text-align:right;"> 0.49 </td>
   <td style="text-align:right;"> 0.13 </td>
   <td style="text-align:right;"> 0.33 </td>
   <td style="text-align:right;"> 0.80 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SESOI to RMSE </td>
   <td style="text-align:right;"> 1.51 </td>
   <td style="text-align:right;"> 1.55 </td>
   <td style="text-align:right;"> 1.32 </td>
   <td style="text-align:right;"> 1.49 </td>
   <td style="text-align:right;"> 0.52 </td>
   <td style="text-align:right;"> 0.97 </td>
   <td style="text-align:right;"> 2.91 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> R-squared </td>
   <td style="text-align:right;"> 0.75 </td>
   <td style="text-align:right;"> 0.76 </td>
   <td style="text-align:right;"> 0.67 </td>
   <td style="text-align:right;"> 0.65 </td>
   <td style="text-align:right;"> 0.29 </td>
   <td style="text-align:right;"> -0.53 </td>
   <td style="text-align:right;"> 0.95 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MinErr </td>
   <td style="text-align:right;"> -7.08 </td>
   <td style="text-align:right;"> -8.66 </td>
   <td style="text-align:right;"> -9.42 </td>
   <td style="text-align:right;"> -6.08 </td>
   <td style="text-align:right;"> 2.47 </td>
   <td style="text-align:right;"> -9.42 </td>
   <td style="text-align:right;"> -1.84 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MaxErr </td>
   <td style="text-align:right;"> 8.80 </td>
   <td style="text-align:right;"> 8.96 </td>
   <td style="text-align:right;"> 10.99 </td>
   <td style="text-align:right;"> 5.68 </td>
   <td style="text-align:right;"> 3.23 </td>
   <td style="text-align:right;"> 1.34 </td>
   <td style="text-align:right;"> 10.99 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MaxAbsErr </td>
   <td style="text-align:right;"> 8.80 </td>
   <td style="text-align:right;"> 8.96 </td>
   <td style="text-align:right;"> 10.99 </td>
   <td style="text-align:right;"> 7.74 </td>
   <td style="text-align:right;"> 2.34 </td>
   <td style="text-align:right;"> 2.96 </td>
   <td style="text-align:right;"> 10.99 </td>
  </tr>
</tbody>
</table>

From the Table \@ref(tab:ancova-rct-model-perf-metrics) we can conclude, that although we have *explained* the causal (or treatment) effects, predicting individual Post-test is not practically meaningful since the prediction error is too large (compared to SESOI). The take-home message is that high *explanatory power* of the model doesn't doesn't automatically yield high predictive power [@shmueliExplainPredict2010]. The selection of statistical analysis is thus related to the question asked. In my opinion, it would be insightful to complement causal estimates with prediction estimates. With the example above, we can predict the direction of the effect (using expected systematic change of 7.9cm and proportions of 1, 11, and 88% for lower, trivial and higher change magnitudes), but we are unable to predict individual Post-test (or changes scores) within acceptable practical precision (using SESOI as an anchor). In other words, we know that the effect will be 88% beneficial (i.e. higher than SESOI), but we are not able to predict individual responses.

For the sake of completeness, Table \@ref(tab:simple-rct-model-perf-metrics) contains performance metrics for the simple RCT model. 

(ref:simple-rct-model-perf-metrics-caption) **Cross-validated predictive performance metrics for the simple RCT model**

<table>
<caption>(\#tab:simple-rct-model-perf-metrics)(ref:simple-rct-model-perf-metrics-caption)</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> metric </th>
   <th style="text-align:right;"> training </th>
   <th style="text-align:right;"> training.pooled </th>
   <th style="text-align:right;"> testing.pooled </th>
   <th style="text-align:right;"> mean </th>
   <th style="text-align:right;"> SD </th>
   <th style="text-align:right;"> min </th>
   <th style="text-align:right;"> max </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> MBE </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 1.24 </td>
   <td style="text-align:right;"> -2.81 </td>
   <td style="text-align:right;"> 1.72 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MAE </td>
   <td style="text-align:right;"> 2.16 </td>
   <td style="text-align:right;"> 2.14 </td>
   <td style="text-align:right;"> 2.33 </td>
   <td style="text-align:right;"> 2.33 </td>
   <td style="text-align:right;"> 0.61 </td>
   <td style="text-align:right;"> 1.10 </td>
   <td style="text-align:right;"> 3.53 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> RMSE </td>
   <td style="text-align:right;"> 3.22 </td>
   <td style="text-align:right;"> 3.18 </td>
   <td style="text-align:right;"> 3.46 </td>
   <td style="text-align:right;"> 3.36 </td>
   <td style="text-align:right;"> 0.84 </td>
   <td style="text-align:right;"> 1.60 </td>
   <td style="text-align:right;"> 4.69 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PPER </td>
   <td style="text-align:right;"> 0.55 </td>
   <td style="text-align:right;"> 0.57 </td>
   <td style="text-align:right;"> 0.53 </td>
   <td style="text-align:right;"> 0.52 </td>
   <td style="text-align:right;"> 0.12 </td>
   <td style="text-align:right;"> 0.37 </td>
   <td style="text-align:right;"> 0.84 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SESOI to RMSE </td>
   <td style="text-align:right;"> 1.55 </td>
   <td style="text-align:right;"> 1.57 </td>
   <td style="text-align:right;"> 1.45 </td>
   <td style="text-align:right;"> 1.61 </td>
   <td style="text-align:right;"> 0.51 </td>
   <td style="text-align:right;"> 1.07 </td>
   <td style="text-align:right;"> 3.12 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> R-squared </td>
   <td style="text-align:right;"> 0.60 </td>
   <td style="text-align:right;"> 0.61 </td>
   <td style="text-align:right;"> 0.54 </td>
   <td style="text-align:right;"> 0.59 </td>
   <td style="text-align:right;"> 0.15 </td>
   <td style="text-align:right;"> 0.25 </td>
   <td style="text-align:right;"> 0.88 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MinErr </td>
   <td style="text-align:right;"> -7.64 </td>
   <td style="text-align:right;"> -8.63 </td>
   <td style="text-align:right;"> -9.27 </td>
   <td style="text-align:right;"> -5.67 </td>
   <td style="text-align:right;"> 2.52 </td>
   <td style="text-align:right;"> -9.27 </td>
   <td style="text-align:right;"> -0.40 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MaxErr </td>
   <td style="text-align:right;"> 8.86 </td>
   <td style="text-align:right;"> 9.74 </td>
   <td style="text-align:right;"> 9.89 </td>
   <td style="text-align:right;"> 5.79 </td>
   <td style="text-align:right;"> 2.84 </td>
   <td style="text-align:right;"> 1.88 </td>
   <td style="text-align:right;"> 9.89 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MaxAbsErr </td>
   <td style="text-align:right;"> 8.86 </td>
   <td style="text-align:right;"> 9.74 </td>
   <td style="text-align:right;"> 9.89 </td>
   <td style="text-align:right;"> 7.54 </td>
   <td style="text-align:right;"> 1.94 </td>
   <td style="text-align:right;"> 3.85 </td>
   <td style="text-align:right;"> 9.89 </td>
  </tr>
</tbody>
</table>

### Analysis of the individual residuals: responders vs non-responders

One particular use of the predictive analysis is in the identification of responders and non-responders to the treatment [@heckstedenIndividualResponseExercise2015; @heckstedenRepeatedTestingAssessment2018; @hopkinsIndividualResponsesMade2015; @swintonStatisticalFrameworkInterpret2018]. Common approach used in sport science [@hopkinsHowInterpretChanges2004], that I will name *observed outcome approach* (further discussed in [Measurement error] chapter and second part of this book), uses known SESOI and *measurement error* to estimate probability of lower, equivalent, and higher changes. In RCT, random non-treatment effect can be assumed to be due to measurement error. Figure \@ref(fig:rct-responders) depicts individual *adjusted change* (by deducting mean Control group change from observed change) with error bars representing *smallest detectable change* (`SDC`). `SDC` is calculated by multiplying Control group change `SD` by 1.96 (to get upper and lower change levels containing 95% of change distribution). This thus represent our *uncertainty* in true treatment effect (using Control group as source of information about random effects). 

<div class="figure" style="text-align: center">
<img src="04-Causal-inference_files/figure-html/rct-responders-1.png" alt="(ref:rct-responders-cation)" width="90%" />
<p class="caption">(\#fig:rct-responders)(ref:rct-responders-cation)</p>
</div>

(ref:rct-responders-cation) **Responses analysis for the Treatment group.** Change scores are adjusted by deducting Control group mean change. Error bars represent *smallest detectable change* (SDC) that is calculated using `SD` of the Control group change scores multiplied by 1.96 (to get 95% levels containing 95% of the change distribution). 

Using this approach, we can classify athletes with high probability of higher change score as responders, those with high probability of equivalent change score as non-responders, and finally those with high probability of lower change score as negative-responders. This approach is useful in figuring out who responded positively or negatively to a particular treatment, but it doesn't take into account information that might help explain the response (for example someone missing treatment session or having lower or higher treatment dose; see [Direct and indirect effect, covariates and then some] section). The topic is further discussed in [Measurement error] chapter and second part of this book. 

Another approach, that I have termed *residuals approach* or *model-based approach* can be used to help identifying *outliers* to intervention. To explain this approach, let's plot athletes' residuals ($\hat{y_i} - y_i$) against observed Post-test ($y_i$) (Figure \@ref(fig:ancova-rct-subj-resid)) using ANCOVA RCT model.

<div class="figure" style="text-align: center">
<img src="04-Causal-inference_files/figure-html/ancova-rct-subj-resid-1.png" alt="(ref:ancova-rct-subj-resid-cation)" width="90%" />
<p class="caption">(\#fig:ancova-rct-subj-resid)(ref:ancova-rct-subj-resid-cation)</p>
</div>

(ref:ancova-rct-subj-resid-cation) **Observed Post-test in vertical jump for each athlete in the study.** Black dot indicate observed Post-test value; vertical line indicate model prediction; colored bar indicated the residual magnitude compared to defined SESOI (±2.5cm in this example): grey for equivalent magnitude, green for lower magnitude, and red for higher magnitude; horizontal error bar represents cross-validated `RMSE` (see Table \@ref(tab:ancova-rct-model-perf-metrics), `RMSE` metric, column *testing.pooled*) and is used to indicate model predictive performance and uncertainty around model prediction graphically 

If we visualize simple model of RCT, using Change score as outcome and Group as predictor (see Figure \@ref(fig:simple-rct-model)), the predictions for athletes in each group are identical (i.e. the average change). This is depicted in Figure (Figure \@ref(fig:simple-rct-subj-resid)).

<div class="figure" style="text-align: center">
<img src="04-Causal-inference_files/figure-html/simple-rct-subj-resid-1.png" alt="(ref:simple-rct-subj-resid-cation)" width="90%" />
<p class="caption">(\#fig:simple-rct-subj-resid)(ref:simple-rct-subj-resid-cation)</p>
</div>

(ref:simple-rct-subj-resid-cation) **Observed Change in vertical jump for each athlete in the study.** Black dot indicate observed Change value; vertical line indicate model prediction; colored bar indicated the residual magnitude compared to defined SESOI (±2.5cm in this example): grey for equivalent magnitude, green for lower magnitude, and red for higher magnitude; horizontal error bar represents cross-validated `RMSE` (see Table \@ref(tab:simple-rct-model-perf-metrics), `RMSE` metric, column *testing.pooled*) and is used to indicate model predictive performance and uncertainty around model prediction graphically 

More complex models (e.g. ANCOVA RCT model in Figure \@ref(fig:ancova-rct-subj-resid)), like the one utilized in [Direct and indirect effect, covariates and then some] section will have different predictions for each athlete. 

Residuals approach uses observed scores and model predictions to indicate individuals who differ more or less than predicted by the model. If this difference between observed and predicted scores (or residual) is bigger than SESOI, this individual is *flagged*. But before jumping to conclusions, I need to remind you that the predictive performance of this simple model is pretty bad (see Table \@ref(tab:simple-rct-model-perf-metrics)). Thus, this type of analysis and visualization should be interpreted *given* the model performance (which is indicated by horizontal line on the Figures \@ref(fig:ancova-rct-subj-resid) and \@ref(fig:simple-rct-subj-resid) which indicates cross-validated pooled testing `RMSE`; see  Table \@ref(tab:ancova-rct-model-perf-metrics)). I will utilize this method with a better model in the [Direct and indirect effect, covariates and then some] section that has much lower cross-validated `RMSE`. What is important to remember with this analysis is that athletes who showed lower or higher observation compared to what was predicted by the model are flagged with red or green color. As opposed to the observed outcome approach, a model-based prediction approach uses *ceteris paribus* in estimating responders vs. non-responders, or at least providing residuals for such a decision. For example, everything else being equal, based on predictor variables, the expected observation is higher or lower than the model prediction. This indicates that there might be something not-identified by predictive algorithm and thus needs to be flagged for a further analysis. But more about this in the [Direct and indirect effect, covariates and then some]. 

Besides analyzing residuals in the training data-set, we can also check how model predicts for each individual within cross-validation using Bias-Variance decomposition (see [Bias-Variance decomposition and trade-off] section). Figure \@ref(fig:ancova-rct-subj-bias-var) depicts prediction error (decomposed to Bias and Variance) for each athlete using ANCOVA RCT model. 

<div class="figure" style="text-align: center">
<img src="04-Causal-inference_files/figure-html/ancova-rct-subj-bias-var-1.png" alt="(ref:ancova-rct-subj-bias-var-caption)" width="90%" />
<p class="caption">(\#fig:ancova-rct-subj-bias-var)(ref:ancova-rct-subj-bias-var-caption)</p>
</div>

(ref:ancova-rct-subj-bias-var-caption) **Bias-variance across 10 times repeated 3-fold cross-validation for each athlete. ** This analysis can also be utilize to flag certain observations (i.e. athlete in this case) that are troublesome for the predictive model

These graphs will make much more sense when more predictive model is applied in the [Direct and indirect effect, covariates and then some] section. 

### Counterfactual analysis and Individual Treatment Effects

As explained, causal inference and explanatory modeling aim to understand, or at least to quantify the causal (or treatment) effects. This is done by elaborate and transparent control of the confounders and study designs. Predictive modelling on the other hand is interested in providing valid predictions on new or unseen data without assuming underlying DGP by treating it as a black-box. In certain scenarios, when confounders are controlled, predictive modelling can be interpreted causally [@zhaoCausalInterpretationsBlackBox2019]. With observational studies, there is always a risk of not controlling all important confounders, but transparency of the causal model is there to be falsified and discussed openly [@gelmanSubjectiveObjectiveStatistics2017; @hernanCausalKnowledgePrerequisite2002; @hernanCWordScientificEuphemisms2018; @hernanDoesObesityShorten2008; @hernanDoesWaterKill2016; @hernanSecondChanceGet2019]. Even with the RCTs, there might be uncertainties in applying the findings from the experiments to  realistic settings [@gelmanCausalityStatisticalLearning2011; @heckmanRejoinderResponseSobel2005]. 

PDP and ICE plots are *model-agnostic* tools for interpreting black-box models that can be used in causal interpretations only after the effort to define the causal structure with domain-specific expertise is taken into account. This approach can be used to estimate, based on the predictive model, counterfactual change and Post-test scores when athletes do not receive treatment (for the Treatment group), or when athletes receive treatment (for the Control group). This way potential outcomes are predicted. 

To explain how this is done, let's again consider the Table \@ref(tab:dummy-coded). Changing the Group column (in this case `groupTreatment`) for every observation (athletes in this case) while keeping all other variables (i.e. columns) the same, we can get a glimpse into causal effects of the treatment (assuming the model and the data are valid for such an inference). The prediction model utilized is ANCOVA RCT model. 

In the Table \@ref(tab:ancova-rct-counterfactual-group), columns `Post-test_0 (cm)` and `Post-test_1 (cm)` indicate these counterfactual changes for which we are interested how the model predicts. These predictions are in the columns `Post-test_0 (cm)` and `Post-test_1 (cm)`. 

(ref:ancova-rct-counterfactual-group-caption) **Counterfactual table used to check how the model predicts when Group changes**

<table class="table" style="font-size: 10px; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">(\#tab:ancova-rct-counterfactual-group)(ref:ancova-rct-counterfactual-group-caption)</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> Athlete </th>
   <th style="text-align:right;"> groupTreatment </th>
   <th style="text-align:right;"> Pre-test (cm) </th>
   <th style="text-align:right;"> Post-test (cm) </th>
   <th style="text-align:right;"> Change (cm) </th>
   <th style="text-align:right;"> groupTreatment_0 </th>
   <th style="text-align:right;"> Post-test_0 (cm) </th>
   <th style="text-align:right;"> groupTreatment_1 </th>
   <th style="text-align:right;"> Post-test_1 (cm) </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Athlete 12 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 42.56 </td>
   <td style="text-align:right;"> 44.29 </td>
   <td style="text-align:right;"> 1.01 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 42.04 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 49.89 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 28 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 47.06 </td>
   <td style="text-align:right;"> 47.98 </td>
   <td style="text-align:right;"> 0.55 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 46.07 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 53.92 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 04 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 44.53 </td>
   <td style="text-align:right;"> 45.13 </td>
   <td style="text-align:right;"> 0.12 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 43.80 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 51.65 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 02 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 49.63 </td>
   <td style="text-align:right;"> 48.86 </td>
   <td style="text-align:right;"> -0.01 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 48.37 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 56.22 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 08 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 41.11 </td>
   <td style="text-align:right;"> 42.13 </td>
   <td style="text-align:right;"> -0.45 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 40.74 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 48.59 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 26 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 42.31 </td>
   <td style="text-align:right;"> 41.61 </td>
   <td style="text-align:right;"> -0.51 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 41.82 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 49.67 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 06 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 45.96 </td>
   <td style="text-align:right;"> 45.70 </td>
   <td style="text-align:right;"> -0.52 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 45.09 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 52.93 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 14 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 44.51 </td>
   <td style="text-align:right;"> 42.89 </td>
   <td style="text-align:right;"> -0.63 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 43.79 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 51.64 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 18 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 42.57 </td>
   <td style="text-align:right;"> 42.15 </td>
   <td style="text-align:right;"> -0.74 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 42.06 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 49.90 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 16 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 37.63 </td>
   <td style="text-align:right;"> 37.83 </td>
   <td style="text-align:right;"> -0.75 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 37.63 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 45.47 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 22 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 36.52 </td>
   <td style="text-align:right;"> 34.83 </td>
   <td style="text-align:right;"> -0.97 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 36.63 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 44.48 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 24 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 40.15 </td>
   <td style="text-align:right;"> 39.88 </td>
   <td style="text-align:right;"> -1.03 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 39.88 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 47.73 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 30 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 39.34 </td>
   <td style="text-align:right;"> 38.34 </td>
   <td style="text-align:right;"> -1.21 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 39.16 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 47.01 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 20 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 38.94 </td>
   <td style="text-align:right;"> 36.97 </td>
   <td style="text-align:right;"> -1.72 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 38.80 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 46.65 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 10 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 36.77 </td>
   <td style="text-align:right;"> 34.15 </td>
   <td style="text-align:right;"> -2.26 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 36.85 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 44.70 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 01 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 37.98 </td>
   <td style="text-align:right;"> 52.86 </td>
   <td style="text-align:right;"> 14.93 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 37.94 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 45.79 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 27 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 44.79 </td>
   <td style="text-align:right;"> 58.50 </td>
   <td style="text-align:right;"> 13.43 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 44.04 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 51.88 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 19 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 46.77 </td>
   <td style="text-align:right;"> 59.76 </td>
   <td style="text-align:right;"> 12.99 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 45.81 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 53.66 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 25 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 38.90 </td>
   <td style="text-align:right;"> 49.58 </td>
   <td style="text-align:right;"> 10.81 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 38.76 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 46.61 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 03 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 41.29 </td>
   <td style="text-align:right;"> 51.41 </td>
   <td style="text-align:right;"> 10.34 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 40.90 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 48.75 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 23 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 48.41 </td>
   <td style="text-align:right;"> 57.57 </td>
   <td style="text-align:right;"> 8.58 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 47.28 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 55.13 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 17 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 44.81 </td>
   <td style="text-align:right;"> 51.41 </td>
   <td style="text-align:right;"> 7.85 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 44.06 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 51.91 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 21 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 37.14 </td>
   <td style="text-align:right;"> 44.95 </td>
   <td style="text-align:right;"> 7.37 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 37.19 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 45.04 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 15 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 46.69 </td>
   <td style="text-align:right;"> 52.73 </td>
   <td style="text-align:right;"> 6.14 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 45.75 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 53.59 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 29 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 42.77 </td>
   <td style="text-align:right;"> 47.38 </td>
   <td style="text-align:right;"> 5.02 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 42.23 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 50.08 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 13 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 49.66 </td>
   <td style="text-align:right;"> 54.11 </td>
   <td style="text-align:right;"> 4.46 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 48.40 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 56.25 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 05 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 37.92 </td>
   <td style="text-align:right;"> 41.63 </td>
   <td style="text-align:right;"> 3.78 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 37.89 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 45.74 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 07 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 41.03 </td>
   <td style="text-align:right;"> 45.41 </td>
   <td style="text-align:right;"> 3.42 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 40.67 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 48.52 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 11 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 45.27 </td>
   <td style="text-align:right;"> 46.72 </td>
   <td style="text-align:right;"> 1.82 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 44.47 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 52.31 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 09 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 47.61 </td>
   <td style="text-align:right;"> 45.62 </td>
   <td style="text-align:right;"> -1.57 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 46.57 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 54.42 </td>
  </tr>
</tbody>
</table>

If we depict these changes in the Group for every athlete, we will get the ICE graph. Average of these predictions gives us the PDP graph. Figure \@ref(fig:ancova-rct-pdp-ice) depicts PDP and ICE for the Group variable. We will get back to this graph in the [Direct and indirect effect, covariates and then some] section. 

<div class="figure" style="text-align: center">
<img src="04-Causal-inference_files/figure-html/ancova-rct-pdp-ice-1.png" alt="(ref:simple-rct-pdp-ice-caption)" width="90%" />
<p class="caption">(\#fig:ancova-rct-pdp-ice)(ref:simple-rct-pdp-ice-caption)</p>
</div>

(ref:ancova-rct-pdp-ice-caption) **PDP and ICE plot for Group variable using ANCOVA RCT model**

Besides PDP and ICE plot, we can also create a counterfactual plot for each athlete. For example, for the athletes in the Treatment group, we are interested how would the predicted Post-test *change* (given model used) if they are in the Control group and *vice versa* for the athletes from the Control group. This is done by *flipping* "Treatment" and "Control" in the Group column and predicting Post-test using the trained model. Figure \@ref(fig:ancova-rct-counterfactual-effects) depicts this visually for each athlete. Arrows represents predicted Change when *flipping* the Group variable. 

<div class="figure" style="text-align: center">
<img src="04-Causal-inference_files/figure-html/ancova-rct-counterfactual-effects-1.png" alt="(ref:ancova-rct-counterfactual-effects-caption)" width="90%" />
<p class="caption">(\#fig:ancova-rct-counterfactual-effects)(ref:ancova-rct-counterfactual-effects-caption)</p>
</div>

(ref:ancova-rct-counterfactual-effects-caption) **Individual counterfactual prediction when the Group changes.** This way we can estimate model counterfactual predictions when the treatment changes (i.e. Controls receive treatment, and Treatment doesn't receive treatment). Arrows thus represent model predicted *Individual Treatment Effects* (pITE). Arrows are color coded based on the magnitude of the effect using defined SESOI (±2.5cm in this example): grey for equivalent magnitude, green for lower magnitude, and red for higher magnitude. Vertical line indicates observed Post-test scores. 

This analysis allows us to estimate counterfactual *individual causal (treatment) effects* (ITE) predicted by the model. These are indicated with the arrows on the Figure \@ref(fig:ancova-rct-counterfactual-effects). Mathematically, arrow width is calculates using the Equation \@ref(eq:ite-equation).

$$
\widehat{ITE_i} = \widehat{y_{i}^{Group=Treatment}} - \widehat{y_{i}^{Group=Control}} 
 (\#eq:ite-equation)
$$

Since ANCOVA RCT model is used, which predicts average Group effect for every participant, estimated counterfactual ITEs are all the same and are equal to 7.85cm. Table \@ref(tab:ancova-rct-pITE) contains all individual model predictions using ANCOVA RCT model. 

(ref:ancova-rct-pITE-caption) **Individual model predictions using ANCOVA RCT model**

<table class="table" style="font-size: 10px; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">(\#tab:ancova-rct-pITE)(ref:ancova-rct-pITE-caption)</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> subject </th>
   <th style="text-align:left;"> group </th>
   <th style="text-align:right;"> observed </th>
   <th style="text-align:right;"> predicted </th>
   <th style="text-align:right;"> residual </th>
   <th style="text-align:left;"> magnitude </th>
   <th style="text-align:right;"> counterfactual </th>
   <th style="text-align:right;"> pITE </th>
   <th style="text-align:left;"> pITE_magnitude </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Athlete 12 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 44.29 </td>
   <td style="text-align:right;"> 42.04 </td>
   <td style="text-align:right;"> -2.25 </td>
   <td style="text-align:left;"> Equivalent </td>
   <td style="text-align:right;"> 49.89 </td>
   <td style="text-align:right;"> 7.85 </td>
   <td style="text-align:left;"> Higher </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 28 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 47.98 </td>
   <td style="text-align:right;"> 46.07 </td>
   <td style="text-align:right;"> -1.91 </td>
   <td style="text-align:left;"> Equivalent </td>
   <td style="text-align:right;"> 53.92 </td>
   <td style="text-align:right;"> 7.85 </td>
   <td style="text-align:left;"> Higher </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 04 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 45.13 </td>
   <td style="text-align:right;"> 43.80 </td>
   <td style="text-align:right;"> -1.33 </td>
   <td style="text-align:left;"> Equivalent </td>
   <td style="text-align:right;"> 51.65 </td>
   <td style="text-align:right;"> 7.85 </td>
   <td style="text-align:left;"> Higher </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 02 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 48.86 </td>
   <td style="text-align:right;"> 48.37 </td>
   <td style="text-align:right;"> -0.48 </td>
   <td style="text-align:left;"> Equivalent </td>
   <td style="text-align:right;"> 56.22 </td>
   <td style="text-align:right;"> 7.85 </td>
   <td style="text-align:left;"> Higher </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 08 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 42.13 </td>
   <td style="text-align:right;"> 40.74 </td>
   <td style="text-align:right;"> -1.39 </td>
   <td style="text-align:left;"> Equivalent </td>
   <td style="text-align:right;"> 48.59 </td>
   <td style="text-align:right;"> 7.85 </td>
   <td style="text-align:left;"> Higher </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 26 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 41.61 </td>
   <td style="text-align:right;"> 41.82 </td>
   <td style="text-align:right;"> 0.21 </td>
   <td style="text-align:left;"> Equivalent </td>
   <td style="text-align:right;"> 49.67 </td>
   <td style="text-align:right;"> 7.85 </td>
   <td style="text-align:left;"> Higher </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 06 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 45.70 </td>
   <td style="text-align:right;"> 45.09 </td>
   <td style="text-align:right;"> -0.61 </td>
   <td style="text-align:left;"> Equivalent </td>
   <td style="text-align:right;"> 52.93 </td>
   <td style="text-align:right;"> 7.85 </td>
   <td style="text-align:left;"> Higher </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 14 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 42.89 </td>
   <td style="text-align:right;"> 43.79 </td>
   <td style="text-align:right;"> 0.90 </td>
   <td style="text-align:left;"> Equivalent </td>
   <td style="text-align:right;"> 51.64 </td>
   <td style="text-align:right;"> 7.85 </td>
   <td style="text-align:left;"> Higher </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 18 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 42.15 </td>
   <td style="text-align:right;"> 42.06 </td>
   <td style="text-align:right;"> -0.10 </td>
   <td style="text-align:left;"> Equivalent </td>
   <td style="text-align:right;"> 49.90 </td>
   <td style="text-align:right;"> 7.85 </td>
   <td style="text-align:left;"> Higher </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 16 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 37.83 </td>
   <td style="text-align:right;"> 37.63 </td>
   <td style="text-align:right;"> -0.21 </td>
   <td style="text-align:left;"> Equivalent </td>
   <td style="text-align:right;"> 45.47 </td>
   <td style="text-align:right;"> 7.85 </td>
   <td style="text-align:left;"> Higher </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 22 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 34.83 </td>
   <td style="text-align:right;"> 36.63 </td>
   <td style="text-align:right;"> 1.80 </td>
   <td style="text-align:left;"> Equivalent </td>
   <td style="text-align:right;"> 44.48 </td>
   <td style="text-align:right;"> 7.85 </td>
   <td style="text-align:left;"> Higher </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 24 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 39.88 </td>
   <td style="text-align:right;"> 39.88 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:left;"> Equivalent </td>
   <td style="text-align:right;"> 47.73 </td>
   <td style="text-align:right;"> 7.85 </td>
   <td style="text-align:left;"> Higher </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 30 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 38.34 </td>
   <td style="text-align:right;"> 39.16 </td>
   <td style="text-align:right;"> 0.82 </td>
   <td style="text-align:left;"> Equivalent </td>
   <td style="text-align:right;"> 47.01 </td>
   <td style="text-align:right;"> 7.85 </td>
   <td style="text-align:left;"> Higher </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 20 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 36.97 </td>
   <td style="text-align:right;"> 38.80 </td>
   <td style="text-align:right;"> 1.84 </td>
   <td style="text-align:left;"> Equivalent </td>
   <td style="text-align:right;"> 46.65 </td>
   <td style="text-align:right;"> 7.85 </td>
   <td style="text-align:left;"> Higher </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 10 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 34.15 </td>
   <td style="text-align:right;"> 36.85 </td>
   <td style="text-align:right;"> 2.70 </td>
   <td style="text-align:left;"> Higher </td>
   <td style="text-align:right;"> 44.70 </td>
   <td style="text-align:right;"> 7.85 </td>
   <td style="text-align:left;"> Higher </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 01 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 52.86 </td>
   <td style="text-align:right;"> 45.79 </td>
   <td style="text-align:right;"> -7.08 </td>
   <td style="text-align:left;"> Lower </td>
   <td style="text-align:right;"> 37.94 </td>
   <td style="text-align:right;"> -7.85 </td>
   <td style="text-align:left;"> Lower </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 27 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 58.50 </td>
   <td style="text-align:right;"> 51.88 </td>
   <td style="text-align:right;"> -6.62 </td>
   <td style="text-align:left;"> Lower </td>
   <td style="text-align:right;"> 44.04 </td>
   <td style="text-align:right;"> -7.85 </td>
   <td style="text-align:left;"> Lower </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 19 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 59.76 </td>
   <td style="text-align:right;"> 53.66 </td>
   <td style="text-align:right;"> -6.10 </td>
   <td style="text-align:left;"> Lower </td>
   <td style="text-align:right;"> 45.81 </td>
   <td style="text-align:right;"> -7.85 </td>
   <td style="text-align:left;"> Lower </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 25 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 49.58 </td>
   <td style="text-align:right;"> 46.61 </td>
   <td style="text-align:right;"> -2.98 </td>
   <td style="text-align:left;"> Lower </td>
   <td style="text-align:right;"> 38.76 </td>
   <td style="text-align:right;"> -7.85 </td>
   <td style="text-align:left;"> Lower </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 03 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 51.41 </td>
   <td style="text-align:right;"> 48.75 </td>
   <td style="text-align:right;"> -2.66 </td>
   <td style="text-align:left;"> Lower </td>
   <td style="text-align:right;"> 40.90 </td>
   <td style="text-align:right;"> -7.85 </td>
   <td style="text-align:left;"> Lower </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 23 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 57.57 </td>
   <td style="text-align:right;"> 55.13 </td>
   <td style="text-align:right;"> -2.44 </td>
   <td style="text-align:left;"> Equivalent </td>
   <td style="text-align:right;"> 47.28 </td>
   <td style="text-align:right;"> -7.85 </td>
   <td style="text-align:left;"> Lower </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 17 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 51.41 </td>
   <td style="text-align:right;"> 51.91 </td>
   <td style="text-align:right;"> 0.49 </td>
   <td style="text-align:left;"> Equivalent </td>
   <td style="text-align:right;"> 44.06 </td>
   <td style="text-align:right;"> -7.85 </td>
   <td style="text-align:left;"> Lower </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 21 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 44.95 </td>
   <td style="text-align:right;"> 45.04 </td>
   <td style="text-align:right;"> 0.09 </td>
   <td style="text-align:left;"> Equivalent </td>
   <td style="text-align:right;"> 37.19 </td>
   <td style="text-align:right;"> -7.85 </td>
   <td style="text-align:left;"> Lower </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 15 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 52.73 </td>
   <td style="text-align:right;"> 53.59 </td>
   <td style="text-align:right;"> 0.86 </td>
   <td style="text-align:left;"> Equivalent </td>
   <td style="text-align:right;"> 45.75 </td>
   <td style="text-align:right;"> -7.85 </td>
   <td style="text-align:left;"> Lower </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 29 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 47.38 </td>
   <td style="text-align:right;"> 50.08 </td>
   <td style="text-align:right;"> 2.69 </td>
   <td style="text-align:left;"> Higher </td>
   <td style="text-align:right;"> 42.23 </td>
   <td style="text-align:right;"> -7.85 </td>
   <td style="text-align:left;"> Lower </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 13 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 54.11 </td>
   <td style="text-align:right;"> 56.25 </td>
   <td style="text-align:right;"> 2.14 </td>
   <td style="text-align:left;"> Equivalent </td>
   <td style="text-align:right;"> 48.40 </td>
   <td style="text-align:right;"> -7.85 </td>
   <td style="text-align:left;"> Lower </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 05 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 41.63 </td>
   <td style="text-align:right;"> 45.74 </td>
   <td style="text-align:right;"> 4.11 </td>
   <td style="text-align:left;"> Higher </td>
   <td style="text-align:right;"> 37.89 </td>
   <td style="text-align:right;"> -7.85 </td>
   <td style="text-align:left;"> Lower </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 07 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 45.41 </td>
   <td style="text-align:right;"> 48.52 </td>
   <td style="text-align:right;"> 3.11 </td>
   <td style="text-align:left;"> Higher </td>
   <td style="text-align:right;"> 40.67 </td>
   <td style="text-align:right;"> -7.85 </td>
   <td style="text-align:left;"> Lower </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 11 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 46.72 </td>
   <td style="text-align:right;"> 52.31 </td>
   <td style="text-align:right;"> 5.59 </td>
   <td style="text-align:left;"> Higher </td>
   <td style="text-align:right;"> 44.47 </td>
   <td style="text-align:right;"> -7.85 </td>
   <td style="text-align:left;"> Lower </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 09 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 45.62 </td>
   <td style="text-align:right;"> 54.42 </td>
   <td style="text-align:right;"> 8.80 </td>
   <td style="text-align:left;"> Higher </td>
   <td style="text-align:right;"> 46.57 </td>
   <td style="text-align:right;"> -7.85 </td>
   <td style="text-align:left;"> Lower </td>
  </tr>
</tbody>
</table>

PDP and  ICE plots, as well as individual treatment effects plots (and estimates) can be very valuable tool in visualizing the causal effects, which are appropriate in this case since we are analyzing RCT data. But we need to be very wary when using them with the observational data and giving them causal interpretation. 

### Direct and indirect effect, covariates and then some

In the previous RCT example, we have assumed *binary* treatment (either plyometric training is done or not), whereas in real life there can be nuances in the treatment, particularly in volume of jumps performed, making the treatment continuous rather than binary variable. This way, we are interested in the effects of number of jumps on the changes in vertical jump height. 

There could also be *hidden variables* involved that *moderate* and *mediate* the effects of the treatment[^hidden_variables]. For example, the higher someone jumps in the Pre-test, the lower the change in the Post-test (i.e. it is harder to improve vertical jump height). Or, the stronger someone is in the Pre-test (measured using relative back squat 1RM) the more potentiated the effects of the plyometrics are. All these are needed expert subject-matter knowledge, required to understand the underlying DGP (and thus to avoid introducing bias in causal analyses; see @lubkeWhyWeShould2020). With such causal structure, we do not have *direct treatment effect* (plyometric --> change in vertical jump) only anymore, but moderated and mediated, or *indirect effects* estimated using the *interactions* in the regression models.

[^hidden_variables]: Using the randomization in the RCT it is assumed that these hidden variables are equally distributed, and that there is no *selection bias* involved. 

To explain these concepts, let's assume that that besides Pre-test and Post-test scores in our RCT study, we have also measured Back squat relative 1RMs since we believed that strength of the individual will moderate the effects of the plyometric treatment. This data is enlisted in the Table \@ref(tab:rct-data-with-squat). Squat 1RM in this case represent characteristic of the subject, or a *covariate*. Additional covariates (not considered here) might include gender, experience, height, weight and so forth. 

<table class="table" style="font-size: 10px; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">(\#tab:rct-data-with-squat)(ref:simple-rct-counterfactual-group-caption)</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> Athlete </th>
   <th style="text-align:right;"> Squat 1RM </th>
   <th style="text-align:left;"> Group </th>
   <th style="text-align:right;"> Pre-test (cm) </th>
   <th style="text-align:right;"> Post-test (cm) </th>
   <th style="text-align:right;"> Change (cm) </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Athlete 12 </td>
   <td style="text-align:right;"> 1.53 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 42.56 </td>
   <td style="text-align:right;"> 44.29 </td>
   <td style="text-align:right;"> 1.01 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 28 </td>
   <td style="text-align:right;"> 1.59 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 47.06 </td>
   <td style="text-align:right;"> 47.98 </td>
   <td style="text-align:right;"> 0.55 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 04 </td>
   <td style="text-align:right;"> 1.49 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 44.53 </td>
   <td style="text-align:right;"> 45.13 </td>
   <td style="text-align:right;"> 0.12 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 02 </td>
   <td style="text-align:right;"> 1.31 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 49.63 </td>
   <td style="text-align:right;"> 48.86 </td>
   <td style="text-align:right;"> -0.01 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 08 </td>
   <td style="text-align:right;"> 1.77 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 41.11 </td>
   <td style="text-align:right;"> 42.13 </td>
   <td style="text-align:right;"> -0.45 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 26 </td>
   <td style="text-align:right;"> 1.34 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 42.31 </td>
   <td style="text-align:right;"> 41.61 </td>
   <td style="text-align:right;"> -0.51 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 06 </td>
   <td style="text-align:right;"> 1.33 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 45.96 </td>
   <td style="text-align:right;"> 45.70 </td>
   <td style="text-align:right;"> -0.52 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 14 </td>
   <td style="text-align:right;"> 2.03 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 44.51 </td>
   <td style="text-align:right;"> 42.89 </td>
   <td style="text-align:right;"> -0.63 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 18 </td>
   <td style="text-align:right;"> 1.21 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 42.57 </td>
   <td style="text-align:right;"> 42.15 </td>
   <td style="text-align:right;"> -0.74 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 16 </td>
   <td style="text-align:right;"> 1.91 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 37.63 </td>
   <td style="text-align:right;"> 37.83 </td>
   <td style="text-align:right;"> -0.75 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 22 </td>
   <td style="text-align:right;"> 1.49 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 36.52 </td>
   <td style="text-align:right;"> 34.83 </td>
   <td style="text-align:right;"> -0.97 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 24 </td>
   <td style="text-align:right;"> 1.37 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 40.15 </td>
   <td style="text-align:right;"> 39.88 </td>
   <td style="text-align:right;"> -1.03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 30 </td>
   <td style="text-align:right;"> 1.04 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 39.34 </td>
   <td style="text-align:right;"> 38.34 </td>
   <td style="text-align:right;"> -1.21 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 20 </td>
   <td style="text-align:right;"> 1.58 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 38.94 </td>
   <td style="text-align:right;"> 36.97 </td>
   <td style="text-align:right;"> -1.72 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 10 </td>
   <td style="text-align:right;"> 1.67 </td>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:right;"> 36.77 </td>
   <td style="text-align:right;"> 34.15 </td>
   <td style="text-align:right;"> -2.26 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 01 </td>
   <td style="text-align:right;"> 2.05 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 37.98 </td>
   <td style="text-align:right;"> 52.86 </td>
   <td style="text-align:right;"> 14.93 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 27 </td>
   <td style="text-align:right;"> 2.05 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 44.79 </td>
   <td style="text-align:right;"> 58.50 </td>
   <td style="text-align:right;"> 13.43 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 19 </td>
   <td style="text-align:right;"> 1.87 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 46.77 </td>
   <td style="text-align:right;"> 59.76 </td>
   <td style="text-align:right;"> 12.99 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 25 </td>
   <td style="text-align:right;"> 1.97 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 38.90 </td>
   <td style="text-align:right;"> 49.58 </td>
   <td style="text-align:right;"> 10.81 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 03 </td>
   <td style="text-align:right;"> 1.79 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 41.29 </td>
   <td style="text-align:right;"> 51.41 </td>
   <td style="text-align:right;"> 10.34 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 23 </td>
   <td style="text-align:right;"> 1.44 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 48.41 </td>
   <td style="text-align:right;"> 57.57 </td>
   <td style="text-align:right;"> 8.58 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 17 </td>
   <td style="text-align:right;"> 1.21 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 44.81 </td>
   <td style="text-align:right;"> 51.41 </td>
   <td style="text-align:right;"> 7.85 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 21 </td>
   <td style="text-align:right;"> 1.49 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 37.14 </td>
   <td style="text-align:right;"> 44.95 </td>
   <td style="text-align:right;"> 7.37 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 15 </td>
   <td style="text-align:right;"> 1.43 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 46.69 </td>
   <td style="text-align:right;"> 52.73 </td>
   <td style="text-align:right;"> 6.14 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 29 </td>
   <td style="text-align:right;"> 1.22 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 42.77 </td>
   <td style="text-align:right;"> 47.38 </td>
   <td style="text-align:right;"> 5.02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 13 </td>
   <td style="text-align:right;"> 1.18 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 49.66 </td>
   <td style="text-align:right;"> 54.11 </td>
   <td style="text-align:right;"> 4.46 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 05 </td>
   <td style="text-align:right;"> 1.15 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 37.92 </td>
   <td style="text-align:right;"> 41.63 </td>
   <td style="text-align:right;"> 3.78 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 07 </td>
   <td style="text-align:right;"> 1.21 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 41.03 </td>
   <td style="text-align:right;"> 45.41 </td>
   <td style="text-align:right;"> 3.42 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 11 </td>
   <td style="text-align:right;"> 0.86 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 45.27 </td>
   <td style="text-align:right;"> 46.72 </td>
   <td style="text-align:right;"> 1.82 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 09 </td>
   <td style="text-align:right;"> 0.69 </td>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 47.61 </td>
   <td style="text-align:right;"> 45.62 </td>
   <td style="text-align:right;"> -1.57 </td>
  </tr>
</tbody>
</table>

Since the individual are randomized into Treatment and Control groups, we expect that there is no difference between Squat 1RM betweem them. Figure \@ref(fig:rct-squat) demonstrates that there is no difference between groups.

<div class="figure" style="text-align: center">
<img src="04-Causal-inference_files/figure-html/rct-squat-1.png" alt="(ref:rct-squat-caption)" width="90%" />
<p class="caption">(\#fig:rct-squat)(ref:rct-squat-caption)</p>
</div>

(ref:rct-squat-caption) **Squat 1RM scores for Control and Treatment groups. ** Since athletes are randomized, we expect them to be similar (i.e. no selection bias involved)

Before modeling this RCT data, let's check visually the relationship between Pre-test and Change (panel A), Squat 1RM and Change (panel B), and Pre-test and Squat 1RM (panel C) (Figure \@ref(fig:rct-change-relationships)).

<div class="figure" style="text-align: center">
<img src="04-Causal-inference_files/figure-html/rct-change-relationships-1.png" alt="(ref:rct-change-relationships-caption)" width="90%" />
<p class="caption">(\#fig:rct-change-relationships)(ref:rct-change-relationships-caption)</p>
</div>

(ref:rct-change-relationships-caption) **Relationship between predictors. A. ** Relationship between Pre-test and Change scores. Lines indicate linear regression model and are used to indicate relationship and interaction. Colored area represent *95% confidence intervals* (see [Statistical inference] section for more information). If there is no interaction between Pre-test and Change score, the lines would be parallel. As can be seen with the Treatment group, it seems that individuals with higher Pre-test demonstrates lower Change, and *vice versa* for the Control group. But if we check the confidence interval area, these are due to random chance. **B.** Relationship between Squat 1RM and Change scores. Visual analysis indicates that individuals in the Treatment group with the higher Squat 1RM demonstrates higher changes after the training intervention. **C.** Relationship between Squat 1RM and Pre-test predictors. It seems that, in both groups, athletes with higher Squat 1RM scores have lower Pre-test values. Checking confidence interval areas, this relationship is due to random chance. 

As can be seen from the Figure \@ref(fig:rct-change-relationships), there seems to be some *interaction* between Squat 1RM and Change. Other panels indicate that there might be some relationship (i.e. *interaction*), but *95% confidence intervals* around the regression lines indicate that these are due to *random chance* (see [Statistical inference] chapter for more info; for now you do not need to understand this concept).

What does interaction mean? If we check the panel C in the Figure \@ref(fig:rct-change-relationships), interaction refers to change in regression line slopes. If the Group regression lines are parallel, then the distance between them is due to effect of treatment (i.e. direct or *main* effect of treatment). Since they are not parallel, it means that Squat 1RM and treatment interact: the higher the Squat 1RM for the Treatment group, the higher the Change, while that is not the case for the Control group. Mathematically, interaction is a simple multiplication between two predictors, as cab be seen in the Table \@ref(tab:rct-interaction).

(ref:rct-interaction-caption) **RCT data with interaction between Group and Squat 1RM**

<table class="table" style="font-size: 10px; margin-left: auto; margin-right: auto;">
<caption style="font-size: initial !important;">(\#tab:rct-interaction)(ref:rct-interaction-caption)</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> Athlete </th>
   <th style="text-align:right;"> groupTreatment </th>
   <th style="text-align:right;"> Squat 1RM </th>
   <th style="text-align:right;"> groupTreatment:Squat 1RM </th>
   <th style="text-align:right;"> Pre-test (cm) </th>
   <th style="text-align:right;"> Change (cm) </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Athlete 12 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.53 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 42.56 </td>
   <td style="text-align:right;"> 1.01 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 28 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.59 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 47.06 </td>
   <td style="text-align:right;"> 0.55 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 04 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.49 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 44.53 </td>
   <td style="text-align:right;"> 0.12 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 02 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.31 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 49.63 </td>
   <td style="text-align:right;"> -0.01 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 08 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.77 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 41.11 </td>
   <td style="text-align:right;"> -0.45 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 26 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.34 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 42.31 </td>
   <td style="text-align:right;"> -0.51 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 06 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.33 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 45.96 </td>
   <td style="text-align:right;"> -0.52 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 14 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 2.03 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 44.51 </td>
   <td style="text-align:right;"> -0.63 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 18 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.21 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 42.57 </td>
   <td style="text-align:right;"> -0.74 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 16 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.91 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 37.63 </td>
   <td style="text-align:right;"> -0.75 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 22 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.49 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 36.52 </td>
   <td style="text-align:right;"> -0.97 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 24 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.37 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 40.15 </td>
   <td style="text-align:right;"> -1.03 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 30 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.04 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 39.34 </td>
   <td style="text-align:right;"> -1.21 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 20 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.58 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 38.94 </td>
   <td style="text-align:right;"> -1.72 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 10 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.67 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 36.77 </td>
   <td style="text-align:right;"> -2.26 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 01 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 2.05 </td>
   <td style="text-align:right;"> 2.05 </td>
   <td style="text-align:right;"> 37.98 </td>
   <td style="text-align:right;"> 14.93 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 27 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 2.05 </td>
   <td style="text-align:right;"> 2.05 </td>
   <td style="text-align:right;"> 44.79 </td>
   <td style="text-align:right;"> 13.43 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 19 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1.87 </td>
   <td style="text-align:right;"> 1.87 </td>
   <td style="text-align:right;"> 46.77 </td>
   <td style="text-align:right;"> 12.99 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 25 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1.97 </td>
   <td style="text-align:right;"> 1.97 </td>
   <td style="text-align:right;"> 38.90 </td>
   <td style="text-align:right;"> 10.81 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 03 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1.79 </td>
   <td style="text-align:right;"> 1.79 </td>
   <td style="text-align:right;"> 41.29 </td>
   <td style="text-align:right;"> 10.34 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 23 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1.44 </td>
   <td style="text-align:right;"> 1.44 </td>
   <td style="text-align:right;"> 48.41 </td>
   <td style="text-align:right;"> 8.58 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 17 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1.21 </td>
   <td style="text-align:right;"> 1.21 </td>
   <td style="text-align:right;"> 44.81 </td>
   <td style="text-align:right;"> 7.85 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 21 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1.49 </td>
   <td style="text-align:right;"> 1.49 </td>
   <td style="text-align:right;"> 37.14 </td>
   <td style="text-align:right;"> 7.37 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 15 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1.43 </td>
   <td style="text-align:right;"> 1.43 </td>
   <td style="text-align:right;"> 46.69 </td>
   <td style="text-align:right;"> 6.14 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 29 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1.22 </td>
   <td style="text-align:right;"> 1.22 </td>
   <td style="text-align:right;"> 42.77 </td>
   <td style="text-align:right;"> 5.02 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 13 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1.18 </td>
   <td style="text-align:right;"> 1.18 </td>
   <td style="text-align:right;"> 49.66 </td>
   <td style="text-align:right;"> 4.46 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 05 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1.15 </td>
   <td style="text-align:right;"> 1.15 </td>
   <td style="text-align:right;"> 37.92 </td>
   <td style="text-align:right;"> 3.78 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 07 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1.21 </td>
   <td style="text-align:right;"> 1.21 </td>
   <td style="text-align:right;"> 41.03 </td>
   <td style="text-align:right;"> 3.42 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 11 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 0.86 </td>
   <td style="text-align:right;"> 0.86 </td>
   <td style="text-align:right;"> 45.27 </td>
   <td style="text-align:right;"> 1.82 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Athlete 09 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 0.69 </td>
   <td style="text-align:right;"> 0.69 </td>
   <td style="text-align:right;"> 47.61 </td>
   <td style="text-align:right;"> -1.57 </td>
  </tr>
</tbody>
</table>

Bu as already alluded, the use of Change scores should be avoided. Let's see this exact graphs, but using Post-test (Figure \@ref(fig:rct-post-test-relationships)) as our variable of interest (i.e. outcome variable). 


<div class="figure" style="text-align: center">
<img src="04-Causal-inference_files/figure-html/rct-post-test-relationships-1.png" alt="(ref:rct-post-test-relationships-caption)" width="90%" />
<p class="caption">(\#fig:rct-post-test-relationships)(ref:rct-post-test-relationships-caption)</p>
</div>

(ref:rct-post-test-relationships-caption) **Relationship between predictors. A. ** Relationship between Pre-test and Post-test scores. Lines indicate linear regression model and are used to indicate relationship and interaction. Colored area represent *95% confidence intervals* (see [Statistical inference] section for more information). If there is no interaction between Pre-test and Post-test score, the lines would be parallel. As can be seen on the figure, there doesn't seem to be interaction involved. **B.** Relationship between Squat 1RM and Post-test scores. Visual analysis indicates that individuals in the Treatment group with the higher Squat 1RM demonstrates higher Post-test scores after the training intervention. **C.** Relationship between Squat 1RM and Pre-test predictors. It seems that, in both groups, athletes with higher Squat 1RM scores have lower Pre-test values. Checking confidence interval areas, this relationship is due to random chance. 

Let's apply linear regression model to these predictors. The parameters we are going to estimate are enlisted in the linear Equation \@ref(eq:rct-interaction-equation).

$$
\begin{equation}
  \widehat{Post} = \hat{\beta_0} + \hat{\beta_1}Group +  \hat{\beta_2}Pre + \hat{\beta_3}Squat\:1RM + \hat{\beta_4}Group:Squat\:1RM 
  (\#eq:rct-interaction-equation)
\end{equation}
$$

Estimated parameters for this linear model with interaction are enlisted in the Table \@ref(tab:rct-interaction-model-coef).

(ref:rct-interaction-model-coef-caption) **Estimated linear regression parameters for the RCT model with interaction between Group and Squat 1RM**

<table>
<caption>(\#tab:rct-interaction-model-coef)(ref:rct-interaction-model-coef-caption)</caption>
 <thead>
  <tr>
   <th style="text-align:right;"> Intercept </th>
   <th style="text-align:right;"> groupTreatment </th>
   <th style="text-align:right;"> Pre-test </th>
   <th style="text-align:right;"> Squat 1RM </th>
   <th style="text-align:right;"> Group:Squat 1RM </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> -5.21 </td>
   <td style="text-align:right;"> -8.03 </td>
   <td style="text-align:right;"> 1.1 </td>
   <td style="text-align:right;"> 0.26 </td>
   <td style="text-align:right;"> 10.82 </td>
  </tr>
</tbody>
</table>

If we check the estimated coefficient for the group Treatment in the Table \@ref(tab:rct-interaction-model-coef), which is equal to -8.03, can we conclude that this is the whole effect of treatment (i.e. plyometric treatment)? Luckily no! This coefficient represents *direct treatment effect*, but there are *indirect treatment effects* due to Squat 1RM and interaction between Squat 1RM and Group. This also assumes that we have not introduced bias in treatment effect estimation by *adjusting* (or by *not adjusting*) for covariates that we should not adjust for (or *vice versa*; for great applied paper please refer to @lubkeWhyWeShould2020). 

Figure \@ref(fig:rct-interaction-resid) depicts residuals of the RCT model with interactions. 

<div class="figure" style="text-align: center">
<img src="04-Causal-inference_files/figure-html/rct-interaction-resid-1.png" alt="(ref:rct-interaction-resid-caption)" width="90%" />
<p class="caption">(\#fig:rct-interaction-resid)(ref:rct-interaction-resid-caption)</p>
</div>

(ref:rct-interaction-resid-caption) **Residuals of the linear regression RCT model with interaction. **Grey band on both panels represents SESOI of ±2.5cm

As opposed to the Figures \@ref(fig:simple-rct-model-ba) and \@ref(fig:ancova-rct-model-ba), we can quickly see that this model have almost all residuals in the SESOI band. Table \@ref(tab:interaction-rct-model-perf-metrics) contains cross-validated model performance (using the same 10 repeats of 3 folds cross-validation splits as used in the simple and ANCOVA RCT models). 

(ref:interaction-rct-model-perf-metrics-caption) **Cross-validated predictive performance metrics for the RCT model with interaction**

<table>
<caption>(\#tab:interaction-rct-model-perf-metrics)(ref:interaction-rct-model-perf-metrics-caption)</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> metric </th>
   <th style="text-align:right;"> training </th>
   <th style="text-align:right;"> training.pooled </th>
   <th style="text-align:right;"> testing.pooled </th>
   <th style="text-align:right;"> mean </th>
   <th style="text-align:right;"> SD </th>
   <th style="text-align:right;"> min </th>
   <th style="text-align:right;"> max </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> MBE </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 0.03 </td>
   <td style="text-align:right;"> 0.03 </td>
   <td style="text-align:right;"> 0.51 </td>
   <td style="text-align:right;"> -1.12 </td>
   <td style="text-align:right;"> 0.92 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MAE </td>
   <td style="text-align:right;"> 0.90 </td>
   <td style="text-align:right;"> 0.82 </td>
   <td style="text-align:right;"> 1.17 </td>
   <td style="text-align:right;"> 1.17 </td>
   <td style="text-align:right;"> 0.26 </td>
   <td style="text-align:right;"> 0.50 </td>
   <td style="text-align:right;"> 1.58 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> RMSE </td>
   <td style="text-align:right;"> 1.08 </td>
   <td style="text-align:right;"> 1.01 </td>
   <td style="text-align:right;"> 1.42 </td>
   <td style="text-align:right;"> 1.40 </td>
   <td style="text-align:right;"> 0.27 </td>
   <td style="text-align:right;"> 0.67 </td>
   <td style="text-align:right;"> 1.87 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PPER </td>
   <td style="text-align:right;"> 0.97 </td>
   <td style="text-align:right;"> 0.99 </td>
   <td style="text-align:right;"> 0.92 </td>
   <td style="text-align:right;"> 0.88 </td>
   <td style="text-align:right;"> 0.06 </td>
   <td style="text-align:right;"> 0.76 </td>
   <td style="text-align:right;"> 0.99 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SESOI to RMSE </td>
   <td style="text-align:right;"> 4.64 </td>
   <td style="text-align:right;"> 4.95 </td>
   <td style="text-align:right;"> 3.51 </td>
   <td style="text-align:right;"> 3.74 </td>
   <td style="text-align:right;"> 0.94 </td>
   <td style="text-align:right;"> 2.68 </td>
   <td style="text-align:right;"> 7.44 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> R-squared </td>
   <td style="text-align:right;"> 0.97 </td>
   <td style="text-align:right;"> 0.98 </td>
   <td style="text-align:right;"> 0.95 </td>
   <td style="text-align:right;"> 0.95 </td>
   <td style="text-align:right;"> 0.03 </td>
   <td style="text-align:right;"> 0.85 </td>
   <td style="text-align:right;"> 0.99 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MinErr </td>
   <td style="text-align:right;"> -2.12 </td>
   <td style="text-align:right;"> -2.57 </td>
   <td style="text-align:right;"> -3.00 </td>
   <td style="text-align:right;"> -1.91 </td>
   <td style="text-align:right;"> 0.64 </td>
   <td style="text-align:right;"> -3.00 </td>
   <td style="text-align:right;"> -0.72 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MaxErr </td>
   <td style="text-align:right;"> 1.97 </td>
   <td style="text-align:right;"> 2.20 </td>
   <td style="text-align:right;"> 3.38 </td>
   <td style="text-align:right;"> 2.23 </td>
   <td style="text-align:right;"> 0.64 </td>
   <td style="text-align:right;"> 0.95 </td>
   <td style="text-align:right;"> 3.38 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MaxAbsErr </td>
   <td style="text-align:right;"> 2.12 </td>
   <td style="text-align:right;"> 2.57 </td>
   <td style="text-align:right;"> 3.38 </td>
   <td style="text-align:right;"> 2.48 </td>
   <td style="text-align:right;"> 0.41 </td>
   <td style="text-align:right;"> 1.57 </td>
   <td style="text-align:right;"> 3.38 </td>
  </tr>
</tbody>
</table>

As expected, predictive performance metrics are now much better. Let's inspect the athlete's residuals (Figure \@ref(fig:interaction-rct-subj-resid)).

<div class="figure" style="text-align: center">
<img src="04-Causal-inference_files/figure-html/interaction-rct-subj-resid-1.png" alt="(ref:interaction-rct-subj-resid-cation)" width="90%" />
<p class="caption">(\#fig:interaction-rct-subj-resid)(ref:interaction-rct-subj-resid-cation)</p>
</div>

(ref:interaction-rct-subj-resid-cation) **Observed Post-test in vertical jump for each athlete in the study.** Black dot indicate observed Post-test; vertical line indicate model prediction; colored bar indicated the residual magnitude compared to defined SESOI (±2.5cm in this example): grey for equivalent magnitude, green for lower magnitude, and red for higher magnitude; horizontal error bar represents cross-validated `RMSE` (see Table \@ref(tab:interaction-rct-model-perf-metrics), `RMSE` metric, column *testing.pooled*) and is used to indicate visually model predictive performance and uncertainty around model prediction

If we compare residuals from the simple model (Figure \@ref(fig:simple-rct-subj-resid)) and ANCOVA RCT model (Figure \@ref(fig:ancova-rct-subj-resid)), in RCT model with interactions the residuals are much smaller, indicating better prediction. You can also see that model predictions differ, as opposed to simple model that predicted same Change values for all athletes in the Control and Treatment groups. Athletes who are *flagged* (have residual bigger than SESOI) might need further inspection, since given training data and model, these demonstrate Post-test that is larger/smaller than expected taking their covariates into account (in this case Strength 1RM). 

Figure \@ref(fig:interaction-rct-subj-bias-var) depicts prediction errors during cross-validation for each athlete. This analysis, together with the Figure \@ref(fig:interaction-rct-subj-resid), can be used to detect athletes that are *troublesome* for the predictive model, and thus bear some further inspection. 

<div class="figure" style="text-align: center">
<img src="04-Causal-inference_files/figure-html/interaction-rct-subj-bias-var-1.png" alt="(ref:interaction-rct-subj-bias-var-caption)" width="90%" />
<p class="caption">(\#fig:interaction-rct-subj-bias-var)(ref:interaction-rct-subj-bias-var-caption)</p>
</div>

(ref:interaction-rct-subj-bias-var-caption) **Bias-variance across 10 times repeated 3-fold cross-validation for each athlete. ** This analysis can also be utilize to flag certain observations (i.e. athlete in this case) that are troublesome for the predictive model

Interpreting and understanding *direct* and *indirect* effects can be quite difficult, especially when causal structure becomes complex. Visualization techniques such as already mentioned PDP and ICE graphs can be utilized to understand the causal mechanism [@zhaoCausalInterpretationsBlackBox2019; @goldsteinPeekingBlackBox2013]. These techniques can also be implemented in observational studies, but with considerable domain knowledge and assumptions needed [@zhaoCausalInterpretationsBlackBox2019]. Although predictive analysis, particularly those using *black box* machine learning models, has been criticized to lack causal interpretation [@hernanSecondChanceGet2019; @pearlBookWhyNew2018; @pearlSevenToolsCausal2019], they can complement causal (or explanatory) analysis [@breimanStatisticalModelingTwo2001; @shmueliExplainPredict2010; @yarkoniChoosingPredictionExplanation2017]. Figure \@ref(fig:pdp-ice-interaction-rct) depicts PDP and ICE plots for Group and Strength 1RM predictors. 

<div class="figure" style="text-align: center">
<img src="04-Causal-inference_files/figure-html/pdp-ice-interaction-rct-1.png" alt="(ref:pdp-ice-interaction-rct-caption)" width="90%" />
<p class="caption">(\#fig:pdp-ice-interaction-rct)(ref:pdp-ice-interaction-rct-caption)</p>
</div>

(ref:pdp-ice-interaction-rct-caption) **PDP and ICE plots for Group and Strength 1RM predictors using RCT model with interaction**

Since this is RCT, we can give causal interpretation to PDP and ICE plot, particularly panel B in the Figure \@ref(fig:pdp-ice-interaction-rct). According to this analysis (given the data and the model), if one increase Squat 1RM, the effect of treatment (i.e. plyometric training) will be higher. This can be further analyzed using each athlete. The question we would like to answer (given the data collected and the model) is "How would particular athlete respond to the treatment if his strength was higher or lower?". Figure \@ref(fig:individual-ice) shows ICE plots in separate facets. This gives us the ability to analyze (given the data and the model) how would each athlete respond if his or her Squat 1RM changed. 

<div class="figure" style="text-align: center">
<img src="04-Causal-inference_files/figure-html/individual-ice-1.png" alt="(ref:individual-ice-caption)" width="90%" />
<p class="caption">(\#fig:individual-ice)(ref:individual-ice-caption)</p>
</div>

(ref:individual-ice-caption) **ICE plots for each athlete**

Figure \@ref(fig:interaction-rct-counterfactual-effects) depicts predicted ITE (i.e. what would happen if Groups *flipped* and everything else being equal). If we compare Figure \@ref(fig:interaction-rct-counterfactual-effects) with Figure \@ref(fig:ancova-rct-counterfactual-effects), we can quickly see that the ITEs differ and are not equal for every individual. If we calculate the `mean` of the ITEs (i.e. arrow lengths), we will get an estimate of `ATE`. `SD` of ITEs will give us estimate how variable the treatment effects are, or estimate of `VTE`. Since these are predicted with the model, I've used the terms `pATE` and `pVTE` indicating that they are estimated with the predictive model.   

<div class="figure" style="text-align: center">
<img src="04-Causal-inference_files/figure-html/interaction-rct-counterfactual-effects-1.png" alt="(ref:interaction-rct-counterfactual-effects-caption)" width="90%" />
<p class="caption">(\#fig:interaction-rct-counterfactual-effects)(ref:interaction-rct-counterfactual-effects-caption)</p>
</div>

(ref:interaction-rct-counterfactual-effects-caption) **Individual counterfactual prediction when the Group changes.** This way we can estimate model counterfactual predictions when the treatment changes (i.e. Controls receive treatment, and Treatment doesn't receive treatment). Arrows thus represent model predicted *Individual Treatment Effects* (ITE). Vertical line indicates observed Change

Table \@ref(tab:rct-models-performance-comparison) contains comparison between the ANCOVA RCT model ($\widehat{Post} = \hat{\beta_0} + \hat{\beta_1}Group + \hat{\beta_2}Pre$) and model with Squat 1RM and interaction term ($\widehat{Post} = \hat{\beta_0} + \hat{\beta_1}Group +  \hat{\beta_2}Pre + \hat{\beta_3}Squat\:1RM + \hat{\beta_4}Group:Squat\:1RM$) for few estimators that are insightful for the comparison. 

(ref:rct-models-performance-comparison-caption) **Comparison between two models using few estimators and performance metrics**

<table>
<caption>(\#tab:rct-models-performance-comparison)(ref:rct-models-performance-comparison-caption)</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> estimator </th>
   <th style="text-align:right;"> ANCOVA.model </th>
   <th style="text-align:right;"> Interaction.model </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Training RMSE </td>
   <td style="text-align:right;"> 3.31 </td>
   <td style="text-align:right;"> 1.08 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Training PPER </td>
   <td style="text-align:right;"> 0.54 </td>
   <td style="text-align:right;"> 0.97 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Training SESOI to RMSE </td>
   <td style="text-align:right;"> 1.51 </td>
   <td style="text-align:right;"> 4.64 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Training R-squared </td>
   <td style="text-align:right;"> 0.75 </td>
   <td style="text-align:right;"> 0.97 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> CV RMSE </td>
   <td style="text-align:right;"> 3.78 </td>
   <td style="text-align:right;"> 1.42 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> CV PPER </td>
   <td style="text-align:right;"> 0.49 </td>
   <td style="text-align:right;"> 0.92 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> CV SESOI to RMSE </td>
   <td style="text-align:right;"> 1.32 </td>
   <td style="text-align:right;"> 3.51 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> CV R-squared </td>
   <td style="text-align:right;"> 0.67 </td>
   <td style="text-align:right;"> 0.95 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SD Residual (Treatment) </td>
   <td style="text-align:right;"> 1.43 </td>
   <td style="text-align:right;"> 1.12 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SD Residual (Control) </td>
   <td style="text-align:right;"> 4.64 </td>
   <td style="text-align:right;"> 1.11 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> pATE (Treatment) </td>
   <td style="text-align:right;"> 7.85 </td>
   <td style="text-align:right;"> 7.57 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> pATE (Control) </td>
   <td style="text-align:right;"> 7.85 </td>
   <td style="text-align:right;"> 8.33 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> pVTE (Treatment) </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 4.62 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> pVTE (Control) </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 2.84 </td>
  </tr>
</tbody>
</table>

As can be seen in the Table \@ref(tab:rct-models-performance-comparison), linear model with interactions predicts Post-test in the vertical jump height better than the ANCOVA model. This means that the variance in the treatment effect (`SD Residual` in the Table \@ref(tab:rct-models-performance-comparison)) is now explained with additional variables and their interactions[^aleatory_uncertainty]. 

[^aleatory_uncertainty]: If you remember the footnote discussion on aleatory and epistemic uncertainty, this is example where what we believed to be aleatory uncertainty (inter-individual residual error or variation that we could not predict: `SD Residual` in the Table \@ref(tab:rct-models-performance-comparison)) was actually epistemic uncertainty that we reduced with introducing new variables (Squat 1RM and interaction term) to the model. Although the inter-individual variation in the treatment effect remains ($SD_{TE}$ or $SD_{IR}$), we are now able to predict individual responses more accurately.  

We are not only interested in prediction, but rather in the underlying causal structure and explaining this model performance when we *intervene* on variables (i.e. what happens to the target variable when I change X from *a* to *b*, while keeping other variables fixed - which is *ceteris paribus* condition). For this reason, PDP and ICE plots can be used to give causal interpretation of the model (see Figure \@ref(fig:pdp-ice-interaction-rct)). 
The main critique of Judea Pearl regarding the use of predictive models and techniques is the lack of counterfactual causal interpretation [@pearlBookWhyNew2018; @pearlSevenToolsCausal2019], particularly with observational studies. I agree that "the role of expert knowledge is the key difference between prediction and causal inference tasks" [@hernanSecondChanceGet2019, pp.44]  and that "both prediction and causal inference require expert knowledge to formulate the scientific question, but only causal inference requires causal expert knowledge to answer the question" [@hernanSecondChanceGet2019, pp.45], but this doesn't negate the need for providing predictive performance, as well as helping in interpreting the model when such data is available [@zhaoCausalInterpretationsBlackBox2019]. 

According to Zhao and Hastie [@zhaoCausalInterpretationsBlackBox2019, pp.1]: "There are three requirements to make causal interpretations: a model with good predictive performance, some domain knowledge in the form of a causal diagram and suitable visualization tools.". The common denominator among multiple experts is that for causal inference and causal interpretation there is a need for domain knowledge, particularly when RCTs are not available. This domain knowledge can be made more transparent by using DAGs and other structural diagrams, and thus help in falsifying assumptions [@gelmanSubjectiveObjectiveStatistics2017; @hernanCausalDiagramsDraw2017; @hernanCausalKnowledgePrerequisite2002; @hernanSecondChanceGet2019]. Adding prediction to explanatory models can be seen as complement, particularly since statistical analysis has been neglecting predictive analysis over explanatory analysis [@breimanStatisticalModelingTwo2001; @shmueliExplainPredict2010; @yarkoniChoosingPredictionExplanation2017].   

### Model selection

Besides applying model with a single interaction term, we can also apply models with more interactions, or quadratic or polynomial terms, or what have you. As we have seen in the [Prediction] chapter, these models can overfit quite easily. That is why we utilized cross-validation to check for model performance on the unseen data. But what if two or more models of different complexity perform similarly on the unseen data (i.e. when cross-validated)? This is the problem of *model selection and comparison*.  Generally, we want to select the simplest model that gives us reliable predictions. Discussions regarding the model comparison and model selection are beyond the scope of this book, although I will provide few examples in the second part of the book. 

The model definition should rely on pre-existing beliefs (i.e. subject-matter expert knowledge) around causal structure underlying intervention. If the statistical analysis is done to *confirm* the structural causal model, then this represents *confirmatory analysis*. Usually, these studies need to be *pre-registered* with the exact analysis workflow and assumption defined *before* data is collected. This is required because in the *exploratory analysis* one can *play* with the data, different models can be tried and the model that fits the data best can be selected. Exploratory analysis is useful for generating models and hypothesis for future studies, but also introduces *hindsight bias* since the model is selected *after* seeing the data or the results of multiple analyses. Very related is so called *p-harking* (Hypothesizing After Results are Known) which involves modifying the hypothesis or in this case causal model, after seeing the results, most often with the aim to reach *statistical significance* (discussed later in the [Statistical inference] section). In predictive analysis this hindsight bias is reduced by using hold-out sample, cross-validation, and evaluating the final model performance on the data that has not been seen by the model. 

## Ergodicity

*Ergodic* process is underlying DGP that is equivalent at *between-individual* (or inter-individual; or group-based analysis) and *within-individual* (or intra-individual or simply individual-based analysis) levels. Thus the causal effects identified using between-individual analysis can be applied to understand within-individual causation as well. *Non-Ergodic* process on the other hand differs between these two levels and effects identified at between-individual level cannot be inferred to within individual level. 

Few authors have already brought into question the generalizability of group-based research to individual-based prediction and causal inferences [@fisherLackGrouptoindividualGeneralizability2018; @glazierChallengingConventionalParadigms2018; @molenaarManifestoPsychologyIdiographic2004; @molenaarNewPersonSpecificParadigm2009]. Here is an interesting quote from Molenaar [@molenaarNewPersonSpecificParadigm2009, pp.112]: 

>"Most research methodology in the behavioral sciences employs inter-individual analyses, which provide information about the state of affairs of the population. However, as shown by classical mathematical-statistical theorems (the ergodic theorems), such analyses do not provide information for, and cannot be applied at, the level of the individual, except on rare occasions when the processes of interest meet certain stringent conditions. When psychological processes violate these conditions, the inter-individual analyses that are now standardly applied have to be replaced by analysis of intra-individual variation in order to obtain valid results."

The individual counterfactual predictions (ITEs) and ICE plots thus rely on ergodicity, which represents big assumption. This means that we should be cautious when generalizing model predictions and causal explanations from group-based level to individual-level. 

Data analysis at the individual level (i.e. collecting multiple data point for individuals) and identifying individual causal effects and predictions might be the step forward, but even with such an approach we are *retrodicting* under *ceteris paribus* conditions and we might not be able to predict future responses. For example, if we have collected responses to various interventions for a particular individual, we might not be able to estimate with certainty how he or she is going to respond to a familiar treatment in the future, since such a prediction relies on *stationarity* of the parameters or the underlying DGP [@molenaarNewPersonSpecificParadigm2009]. 

But this doesn't imply that all our efforts are useless. We just need to accept the uncertainty and the assumptions involved. For example, for completely novel subject, the best response estimate or prediction estimate is the expected response calculated by using the group-based analysis (i.e. average treatment effect). This represents the most likely response or prediction. But on top of providing these distribution-based or group-based estimates, one can provide expected uncertainties showing individual-based or response proportions as anchor-based (magnitude-based) estimates [@estradaStatisticsEvaluatingPrepost2019; @normanRelationDistributionAnchorBased2001]. It is important to note that these usually estimate the same information [@estradaStatisticsEvaluatingPrepost2019; @normanRelationDistributionAnchorBased2001]; e.g. the higher the `Cohen's d` the higher the proportion of higher responses. Thus reporting magnitude-based proportions as uncertainties together with expected responses using average-based approach at least help in communicating uncertainties much better than reporting solely average-based estimates. When warranted with the research question, researchers should also report predictive performances on unseen data, as well as underlying assumption using graphical models such as DAGs. 

It might be the best to conclude the section on causal inference with the quote from Andrew Gelman's paper [@gelmanCausalityStatisticalLearning2011, pp.965]:

>"Casual inference will always be a challenge, partly because our psychological intuitions do not always match how the world works. We like to think of simple causal stories, but in the social world, causes and effects are complex. We—in the scientific community—still have not come to any agreement on how best to form consensus on causal questions: there is a general acceptance that experimentation is a gold standard, but it is not at clear how much experimentation should be done, to what extent we can trust inference from observational data, and to what extent experimentation should be more fully incorporated into daily practice (as suggested by some in the “evidence-based medicine” movement)."
