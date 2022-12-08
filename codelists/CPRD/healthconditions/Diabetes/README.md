# CPRD (Gold and Aurum) code lists for diabetes

These code lists were developed by Annie Jeffery, Division of Psychiatry, UCL.


"ALLDIABETESCODES" contains clinical codes to identify different diabetes types in CPRD Gold and Aurum. The code list was created based on combining clinical codes from exisiting lists ([CAMBRIDGE CHARLSON C08184 AND C07183](https://www.phpc.cam.ac.uk/pcu/research/research-groups/crmh/cprd_cam/codelists/v11/) and [the HDRUK Phenotype Library PH8](https://phenotypes.healthdatagateway.org/phenotypes/PH8/version/16/detail/)), and searching for additional terms relevent to diabetes in the CPRD medical code dictionary. This list builds on existing code lists firstly because it contains codes for both CPRD Gold (first column: "medcodeGold") and CPRD Aurum (second column "medcodeAurum"), and secondly because it contians additional relevant terms found in the medical code dictionary search. 

For type 2 diabetes: select codes with the categories "Type2", "Type2Maybe" and "NIDDM", depending on how sensitive/specific you want to be.

For type 1 diabetes: select codes with the categories "Type1", "Type1Maybe" and "IDDM", depending on how sensitive/specific you want to be.

Other diabetes types are included under the category "DiabetesOther" and "Gestational". When the diabetes type is unclear from the code, these are classed as "Diabetes". The category "Remove" contains codes that relate to pre-diabetes, diabetes education that may occur for people at risk, and other codes that do not identify diabetes.


"diabTestsA" and "diabTestsG" provides code lists for diabetes diagnostic testing in CPRD Aurum and Gold respectively. 


When you load the files, ensure you read the medcode and medcodeid variables as characters.


These lists have been used in the following research:

Jeffery A, Bhanu C, Walters K, Wong ICK, Osborn D, Hayes JF. Polypharmacy and Antidepressant Acceptability in Comorbid Depression and Type 2 Diabetes. British Journal of General Practice 31 October 2022. DOI: [10.3399/BJGP.2022.0295](https://doi.org/10.3399/BJGP.2022.0295)

Jeffery A, Bhanu C, Walters K, Wong ICK, Osborn D, Hayes JF. Association between polypharmacy and depression relapse in individuals with comorbid depression and type 2 diabetes: a UK electronic health record study. British Journal of Psychiatry. 2022 Dec 1:1-7. DOI: [10.1192/bjp.2022.160](https://www.cambridge.org/core/journals/the-british-journal-of-psychiatry/article/association-between-polypharmacy-and-depression-relapse-in-individuals-with-comorbid-depression-and-type-2-diabetes-a-uk-electronic-health-record-study/0E2777EA8E768BED2D11C5CCE6AFBB40)


♻️ License
---

This work is licensed under the MIT license (code) and Creative Commons Attribution 4.0 International license (for documentation).
You are free to share and adapt the material for any purpose, even commercially,
as long as you provide attribution (give appropriate credit, provide a link to the license,
and indicate if changes were made) in any reasonable manner, but not in any way that suggests the
licensor endorses you or your use, and with no additional restrictions.
