# Confounding by indication

At our December 2022 Winter Meeting we discussed the issue of confounding by indication in the context of polypharmacy. 

The following page describes the issue and notes from our workshop discussion. We have now opened the competition to contribute resources on methods used to address confounding by indication in pharmacoepidemiology. We will be offering a voucher worth £50 to £100 for the best contribution! More details below... :)

## Background on pharmacoepi research in polypharmacy

Polypharmacy is described by the King’s Fund as a “the concomitant use of multiple medications by one individual”. For many people, the use of multiple medications can be essential for extending life expectancy and improving quality of life. However, when medications are prescribed inappropriately, polypharmacy can be problematic. There is a growing body of evidence showing that polypharmacy can increase an individual’s risk of drug side effects, drug-drug or drug-disease interactions, treatment burden, poor quality of life and non-adherence to medications. However, there is also conflicting evidence that people who take more medications may have better engagement with care and better adherence to treatments. 

Strict inclusion criteria and the complex characteristics of people who use multiple medications make the effects of polypharmacy difficult to study in clinical trials. This makes polypharmacy an important area for research using real-world data and pharmacoepi methods. However, a key problem when studying the effects of polypharmacy in real-world data is confounding by indication.

## The problem of confounding by indication

Confounding by indication occurs when the observed effect of a medication is not caused by the medication itself, but by the condition for which the medication is prescribed. In the case of polypharmacy, observed outcomes such as adverse effects, poor adherence, or poor quality of life, may in fact be a result of multimorbidity (either specific conditions or the cumulative burden of multiple conditions), rather than the medications themselves. As the condition for which a medication is prescribed is likely to be highly correlated with the prescribing of the medication, adjusting for the condition as a confounder may cause overfitting of our models, and not resolve the issue of confounding by indication. On the other hand, clinical characteristics that might lead to the prescribing of a medication, such as disease severity, may not be available in the data, and so may lead to unmeasured confounding by indication. 

## Further reading
- [Kyriacou D, Lewis R J. *Confounding by indication in clinical research.* JAMA. 2016;316(17):1818-1819. DOI: 10.1001/jama.2016.16435](https://jamanetwork.com/journals/jama/fullarticle/2576568)
- [Sendor R, Stürmer T.  *Core concepts in pharmacoepidemiology: Confounding by indication and the role of active comparators.* Pharmacoepidemiol Drug Saf. 2022; 31( 3): 261- 269. DOI: 10.1002/pds.5407](https://onlinelibrary.wiley.com/doi/full/10.1002/pds.5407)

## Contribution competition

**Do you know any examples methods that can adequately address confounding by indication in pharmacoepi research using secondary data?**

We are offering vouchers of £50-100 for the best contribution to these pages of examples of how confounding by indication has been addressed in pharmacoepi research using secondary data. The vouchers are redeemable at a number of major online UK retailers. The contributions should meet these criteria:
- For £50 prize:
  - In slide format (converted to PDF)
  - A example of research in the area of pharmacoepi using secondary data, where specific methods have been used to address confounding by indication
  - A description of how these methods have addressed the issue of confounding by indication
  - Strengths and limitations of the methods
  - Any relevant references where the research has been published
  - Examples of previous presentations that have won our methods competitions or been discussed in our meetings can be found [here](https://github.com/Pharmacoepi-Data-Collaborative/home/tree/Main/information_resources/statistical_methods).
- For £100 prize:
  - All of the above, plus contribution of code / code lists that have been used to address the issue

Details of how to contribute can be found [here](https://github.com/Pharmacoepi-Data-Collaborative/home/blob/Main/HOW_TO_CONTRIBUTE.md). Or if you are not comfortable using Github, you can email contributions directly to [annie.jeffery.09@ucl.ac.uk](mailto:annie.jeffery.09@ucl.ac.uk)

## Notes from the workshop discussion
- Question 1: What are some examples you have come across of confounding by indication in pharmacoepi research:
  - Some medications used in diabetes may cause skin conditions as an adverse effect, but how skin conditions may also be a result of diabetes itself
  - Increasing the dose of a medication or switching treatment may lead to adverse effects, however, these effects may also be a symptom of an uncontrolled condition, which prompted the dose change/medication switch
  - Some medications may have side effects that result in suicide, however, suicide may also be the result of the psychological condition for which the medication is prescribed
- Question 2: What are some methods that we can use to address confounding by indication in pharmacoepi research:
  - In clinical practice, we can see if symptoms resolve when a medication is discontinued, which may mean the symptoms are caused by the condition rather than the medication - this could be an option to explore confounding by indication in pharmacoepi research
  - Active comparators may be a solution to address confounding by indication, where drugs are compared to other drugs, rather than to taking no drug
  - Self-controlled case series may be an option to compare medication effects according to the timing of when they occur within an individual
  - Target trials that compare treatment strategies in individuals who meet the threashold for treatment
