# Creating a code list for medication group
author: "Annie Jeffery"

This file explains how to create a CPRD code list for a given group of medications. The example used in this file is for opioids.

## Step 1 create list of medications based on BNF or ATC chapters
Example, opioid analgesics BNF 4.7.2:
  - Buprenorphine (0407020B0)
  - Codeine phosphate (0407020C0)
  - Dextromoramide tartrate (0407020D0)
  - Dextropropoxyphene (0407020E0)
  - Diamorphine hydrochloride (Systemic) (0407020K0)
  - Dihydrocodeine tartrate (0407020G0)
  - Dipipanone hydrochloride (0407020H0)
  - Fentanyl (0407020A0)
  - Hydromorphone hydrochloride (040702050)
  - Meptazinol hydrochloride (0407020L0)
  - Methadone hydrochloride (0407020M0)
  - Morphine hydrochloride (0407020P0)
  - Morphine (Opium Tincture) (0407020W0)
  - Morphine sulfate (0407020Q0)
  - Morphine tartrate and cyclizine tartrate (040702020)
  - Nalbuphine hydrochloride (0407020Y0)
  - Oxycodone (0407020Z0)
  - Oxycodone hydrochloride (0407020AD)
  - Oxycodone hydrochloride/naloxone hydrochloride (0407020AF)
  - Papaveretum (0407020AB)
  - Pentazocine hydrochloride (0407020T0)
  - Pentazocine lactate (0407020U0)
  - Pethidine hydrochloride (0407020V0)
  - Tapentadol hydrochloride (0407020A
  - Tapentadol phosphate (0407020AH)
  - Tramadol hydrochloride (040702040)

## Step 2 load CPRD product dictionaries
```{r}
prodA <- read.delim("Filepath/productA.txt", colClasses = "character")
prodG <- read.delim("Filepath/productG.txt", colClasses = "character")
```

## Step 3 extract codes for each drug substance from CPRD Aurum (combine where possible, e.g. morphine and diamorphine will be picked up under "morphine")
Note: grep is a function which takes any row containing the word in quotation marks within the specified variable that follows; ignore.case includes regardless of capital letters
```{r}
buprA <- prodA[grep("Buprenorphine", prodA$DrugSubstanceName,  ignore.case = TRUE),]
codeA <- prodA[grep("Codeine", prodA$DrugSubstanceName,  ignore.case = TRUE),]
dexmA <- prodA[grep("Dextromoramide", prodA$DrugSubstanceName,  ignore.case = TRUE),]
dexpA <- prodA[grep("Dextropropoxyphene", prodA$DrugSubstanceName,  ignore.case = TRUE),]
morpA <- prodA[grep("morphine", prodA$DrugSubstanceName,  ignore.case = TRUE),]
dipiA <- prodA[grep("Dipipanone", prodA$DrugSubstanceName,  ignore.case = TRUE),]
fentA <- prodA[grep("Fentanyl", prodA$DrugSubstanceName,  ignore.case = TRUE),]
hydrA <- prodA[grep("Hydromorphone", prodA$DrugSubstanceName,  ignore.case = TRUE),]
meptA <- prodA[grep("Meptazinol", prodA$DrugSubstanceName,  ignore.case = TRUE),]
methA <- prodA[grep("Methadone", prodA$DrugSubstanceName,  ignore.case = TRUE),]
nalbA <- prodA[grep("Nalbuphine", prodA$DrugSubstanceName,  ignore.case = TRUE),]
oxycA <- prodA[grep("Oxycodone", prodA$DrugSubstanceName,  ignore.case = TRUE),]
papaA <- prodA[grep("Papaveretum", prodA$DrugSubstanceName,  ignore.case = TRUE),]
pentA <- prodA[grep("Pentazocine", prodA$DrugSubstanceName,  ignore.case = TRUE),]
pethA <- prodA[grep("Pethidine", prodA$DrugSubstanceName,  ignore.case = TRUE),]
tapeA <- prodA[grep("Tapentadol", prodA$DrugSubstanceName,  ignore.case = TRUE),]
tramA <- prodA[grep("Tramadol", prodA$DrugSubstanceName,  ignore.case = TRUE),]
```

Combine all in final codelist for Aurum:
```{r}
opioidCodesA <- rbind(buprA, codeA, dexmA, dexpA, morpA, dipiA, fentA, hydrA, meptA, methA, nalbA, oxycA, papaA, pentA, pethA, tapeA, tramA)
```

## Step 4 extract codes for each drug substance from CPRD Gold
```{r}
buprG <- prodG[grep("Buprenorphine", prodG$drugsubstance, ignore.case = TRUE),]
codeG <- prodG[grep("Codeine", prodG$drugsubstance,  ignore.case = TRUE),]
dexmG <- prodG[grep("Dextromoramide", prodG$drugsubstance,  ignore.case = TRUE),]
dexpG <- prodG[grep("Dextropropoxyphene", prodG$drugsubstance,  ignore.case = TRUE),]
morpG <- prodG[grep("morphine", prodG$drugsubstance,  ignore.case = TRUE),]
dipiG <- prodG[grep("Dipipanone", prodG$drugsubstance,  ignore.case = TRUE),]
fentG <- prodG[grep("Fentanyl", prodG$drugsubstance,  ignore.case = TRUE),]
hydrG <- prodG[grep("Hydromorphone", prodG$drugsubstance,  ignore.case = TRUE),]
meptG <- prodG[grep("Meptazinol", prodG$drugsubstance,  ignore.case = TRUE),]
methG <- prodG[grep("Methadone", prodG$drugsubstance,  ignore.case = TRUE),]
nalbG <- prodG[grep("Nalbuphine", prodG$drugsubstance,  ignore.case = TRUE),]
oxycG <- prodG[grep("Oxycodone", prodG$drugsubstance,  ignore.case = TRUE),]
papaG <- prodG[grep("Papaveretum", prodG$drugsubstance,  ignore.case = TRUE),]
pentG <- prodG[grep("Pentazocine", prodG$drugsubstance,  ignore.case = TRUE),]
pethG <- prodG[grep("Pethidine", prodG$drugsubstance,  ignore.case = TRUE),]
tapeG <- prodG[grep("Tapentadol", prodG$drugsubstance,  ignore.case = TRUE),]
tramG <- prodG[grep("Tramadol", prodG$drugsubstance,  ignore.case = TRUE),]
```

Combine all in final codelist for Gold:
```{r}
opioidCodesG <- rbind(buprG, codeG, dexmG, dexpG, morpG, dipiG, fentG, hydrG, meptG, methG, nalbG, oxycG, papaG, pentG, pethG, tapeG, tramG)
```
