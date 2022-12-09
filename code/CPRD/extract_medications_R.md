# Extract medications for cohort from medication code list
author: Annie Jeffery, Division of Psychiatry, UCL

This document shows how to extract a list of medications from the CPRD based on a list of medication product codes. The medications for this example are antidepressants.

Note - each time a file path is given, this needs to be updated to your relevant file path. File names have been kept to the file names given by CPRD, however, the project code has been updated to "0000". Files will need to be put into separate folders according to their type (e.g. Aurum observation files in one folder, etc.).

## Step 1 Load packages
```{r}
library(lubridate)
library(tidyverse)
```

## Step 2 Load code lists
```{r}
antidepCodes <- read.delim("Filepath/codelists/antidepressants.txt", header = TRUE, colClasses = "character")
antiDepA <- subset(antidepCodes, antidepCodes$database == "Aurum")
antiDepG <- subset(antidepCodes, antidepCodes$database == "Gold")
```

# Step 3 Load patient lists
```{r}
Load patient file (note - adjust colClasses depending if you have different columns - the below intends to read patid, regstartdate (Aurum only), regenddate (Aurum only), frd (Gold only), tod (Gold only) - the rest are NULL):
pats <- read.delim("Filepath/CPRDAurum/patientsAurum.txt", header = TRUE, colClasses = c(rep("character", 1), rep("NULL", 5),  rep("character",1), rep("NULL", 1),  rep("character",1), rep("NULL", 2)))
pats <- read.delim("/Filepath/CPRDGold/patientsGold.txt", header = TRUE, colClasses = c(rep("character", 1), rep("NULL", 5),  rep("character",1), rep("NULL", 1),  rep("character",1), rep("NULL", 2)))
```

## Step 4 Extract antidepressant prescriptions for patient list from Aurum, from years 2009-2018
List prescription files within folder (note - if the file names contain "part1", "part2", these will need to be separated into different folders and the below steps repeated):
```{r}
drugsA <- list.files(path = "Filepath/CPRDAurum/Drugs")
num <- c(1:length(drugsA))
num[num<100&num>9] <- paste0("0", num[num<100&num>9])
for (i in 1:9) {
  num[i] <- paste0("00", num[i])
}
```

Loop through each file to extract relevant medications (note - adjust colClasses depending if you have different columns - the below intends to read patid, issuedate and prodcodeid - the rest are NULL):
```{r}
antidepA1 <- data.frame()
for (i in (1:length(num))) {
  name <- paste0("Filepath/CPRDAurum/Drugs/0000_Aurum_Extract_DrugIssue_", num[i], ".txt")
  temp <- read.delim(name, header = TRUE, colClasses = c(rep("character", 1), rep("NULL", 4),  rep("character",1), rep("NULL", 1),  rep("character",1), rep("NULL", 4)))
  temp <- temp[temp$patid %in% patsA$patid,]
  temp <- temp[temp$prodcodeid %in% antiDepA$productcode,]
  temp$issuedate <- as.Date(temp$issuedate, format = "%d/%m/%Y")
  temp$year <- year(temp$issuedate)
  temp <- subset(temp, temp$year >= 2009 & temp$year <= 2018)
  antidepA1 <- rbind(antidepA1,temp)
  print(num[i])
}
rm(temp)
antidepA1 <- unique(antidepA1)
```


## Step 5 Extract antidepressant prescriptions for patient list from Aurum, from years 2009-2018
List prescription files within folder (note - if the file names contain "part1", "part2", these will need to be separated into different folders and the below steps repeated):
```{r}
drugsG <- list.files(path = "Filepath/CPRDGold/Drugs")
num <- c(1:length(drugsG))
num[num<100&num>9] <- paste0("0", num[num<100&num>9])
for (i in 1:9) {
  num[i] <- paste0("00", num[i])
}
```

Loop through each file to extract relevant medications (note - adjust colClasses depending if you have different columns - the below intends to read patid, eventedate and prodcode - the rest are NULL; also columns are renamed to match Aurum to combine later):
```{r}
antidepG1 <- data.frame()
for (i in (1:length(num))) {
  name <- paste0("Filepath/CPRDGold/Drugs/0000_GOLD_Extract_Therapy_", num[i], ".txt")
  temp <- read.delim(name, header = TRUE, colClasses = c(rep("character", 2), rep("NULL", 2),  rep("character",1), rep("NULL", 7)))
  names(temp)[2] <- "issuedate"
  names(temp)[3] <- "prodcodeid"
  temp <- temp[temp$patid %in% patsG$patid,]
  temp <- temp[temp$prodcodeid %in% antiDepG$productcode,]
  temp$issuedate <- as.Date(temp$issuedate, format = "%d/%m/%Y")
  temp$year <- year(temp$issuedate)
  temp <- subset(temp, temp$year >= 2009 & temp$year <= 2018)
  antidepG1 <- rbind(antidepG1,temp)
  print(num[i])
}
rm(temp)
antidepG1 <- unique(antidepG1)
```

## Step 6 Combine Gold and Aurum
Create unique cross-databaste identifiers for patients and drugcodes, then combine files
```{r}
antiDepA1$DB <- "A"
anxietyObsA$UID <- paste0(antiDepA1$patid,antiDepA1$DB)
anxietyObsA$UMD <- paste0(antiDepA1$prodcodeid,antiDepA1$DB)
antiDepG1$DB <- "G"
antiDepG1$UID <- paste0(antiDepG1$patid,antiDepG1$DB)
antiDepG1$UMD <- paste0(antiDepG1$prodcodeid,antiDepG1$DB)
ALLantideps <- rbind(antiDepA1, antiDepG1)
```

# Step 7 Save file
```{r}
write.table(ALLantideps, "Filepath/saved/ALLantideps.txt", sep = "\t", col.names = TRUE, row.names = FALSE)
```
