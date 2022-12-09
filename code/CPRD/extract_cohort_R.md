# Extract cohort from CPRD based on condition code list
author: Annie Jeffery, Division of Psychiatry, UCL


This document shows how to extract a list of patients from the CPRD who have a condition of interest, based on a  list of clinical codes. The condition of interest for this example is anxiety. 

Note - each time a file path is given, this needs to be updated to your relevant file path. File names have been kept to the file names given by CPRD, however, the project code has been updated to "0000". Files will need to be put into separate folders according to their type (e.g. Aurum observation files in one folder, etc.).

## Step 1: Load required packages
```{r}
library(tidyverse)
```

## Step 2: Load anxiety codelists

```{r}
anxCodesA <- read.delim("Filepath/anxietyCodesAurum.txt", header = TRUE, colClasses = "character")
head(anxCodesA)
anxCodesG <- read.delim("Filepath/anxietyCodesGold.txt", header = TRUE, colClasses = "character")
head(anxCodesG)
```

## Step 3.1: Load list of patients with registrationstart and end dates from CPRD Aurum
Load patient file (note - adjust colClasses depending if you have different columns - the below intends to read patid, regstartdate and regenddate - the rest are NULL):
```{r}
pats <- read.delim("//live.rd.ucl.ac.uk/ritd-ag-project-rd00qv-jfhay18/Annie/D1/CPRDAurum/patientsAurumD1.txt", header = TRUE, colClasses = c(rep("character", 1), rep("NULL", 5),  rep("character",1), rep("NULL", 1),  rep("character",1), rep("NULL", 2)))
```
Format dates (missing regenddate is imputed as end of follow-up period):
```{r}
pats$regstartdate <- as.Date(pats$regstartdate, format = "%d/%m/%Y")
pats$regenddate <- as.Date(pats$regenddate, format = "%d/%m/%Y")
pats$regenddate <- as.character(pats$regenddate)
pats <- pats %>% mutate(regenddate = case_when(is.na(pats$regenddate) ~ "2018-12-01", TRUE ~ regenddate))
pats$regenddate <- as.Date(pats$regenddate)
```


## Step 3.2: Extract people who have records for anxiety from CPRD Aurum observation files
List observation files within folder (note - if the file names contain "part1", "part2", these will need to be separated into different folders and the below steps repeated):
```{r}
obsA <- list.files(path = "Filepath/CPRDAurum/Observations")
num <- c(1:length(obsA))
num[num<100&num>9] <- paste0("0", num[num<100&num>9])
for (i in 1:9) {
  num[i] <- paste0("00", num[i])
}
```
Loop through each file to extract patients with anxiety records after patient registration starts/before it ends (note - adjust colClasses depending if you have different columns - the below intends to read patid, obsdate and medcodeid - the rest are NULL):
```{r}
anxietyObsA <- data.frame()
for (i in (1:length(num))) {
  name <- paste0("Filepath/CPRDAurum/Observations/0000_Aurum_Extract_Observation_", num[i], ".txt")
  temp <- read.delim(name, header = TRUE, colClasses = c(rep("character", 1), rep("NULL", 3),  rep("character",1), rep("NULL", 2),  rep("character",1), rep("NULL", 6)))
  temp <- temp[temp$medcodeid %in% anxCodesA$medcodeid,]
  temp$obsdate <- as.Date(temp$obsdate, format = "%d/%m/%Y")
  temp <- merge(x=temp, y=pats, by.x="patid", by.y = "patid", all.y = FALSE, all.x=head(FALSE))
  temp <- subset(temp, temp$obsdate >= temp$regstartdate & temp$obsdate <= temp$regenddate)
  temp$obsdate <- NULL
  temp$medcodeid <- NULL
  temp <- unique(temp)
  anxietyObsA <- rbind(anxietyObsA,temp)
  print(num[i])
}
rm(temp)
anxietyObsA <- unique(anxietyObsA)
```


## Step 4.1: Load list of patients with start and end dates from CPRD Gold
Load patient file (note - adjust colClasses depending if you have different columns - the below intends to read patid, frd and tod - the rest are NULL):
```{r}
pats <- read.delim("Filepath/CPRDGold/patientsGoldD1.txt", header = TRUE, colClasses = c(rep("character", 1), rep("NULL", 7),  rep("character",1), rep("NULL", 4),  rep("character",1), rep("NULL", 3)))
```

Format dates (missing tod is imputed as end of follow-up period):
```{r}
pats$frd <- as.Date(pats$frd, format = "%d/%m/%Y")
pats$tod <- as.Date(pats$tod, format = "%d/%m/%Y")
pats$tod <- as.character(pats$tod)
pats <- pats %>% mutate(tod = case_when(is.na(pats$tod) ~ "2018-12-01", TRUE ~ tod))
pats$tod <- as.Date(pats$tod)
```


## Step 4.2: Extract people who have records for anxiety from CPRD Gold clinical files
List clinical files within folder (note - if the file names contain "part1", "part2", these will need to be separated into different folders and the below steps repeated):
```{r}
clinG <- list.files(path = "Filepath/CPRDGold/Clinical")
num <- c(1:length(clinG))
num[num<100&num>9] <- paste0("0", num[num<100&num>9])
for (i in 1:9) {
  num[i] <- paste0("00", num[i])
}
```

Loop through each file to extract patients with anxiety records after patient registration starts/before it ends (adjust colClasses depending if you have different columns - the below intends to read patid, eventdate and medcode - the rest are NULL):
```{r}
anxietyClinG <- data.frame()
for (i in (1:length(num))) {
  name <- paste0("Filepath/CPRDGold/Clinincal/0000_GOLD_Extract_Clinical_", num[i], ".txt")
  temp <- read.delim(name, header = TRUE, colClasses = c(rep("character", 2), rep("NULL", 3),  rep("character",1), rep("NULL", 3)))
  temp <- temp[temp$medcode %in% anxCodesG$medcode,]
  temp$eventdate <- as.Date(temp$eventdate, format = "%d/%m/%Y")
  temp <- merge(x=temp, y=pats, by.x="patid", by.y = "patid", all.y = FALSE, all.x=head(FALSE))
  temp <- subset(temp, temp$eventdate >= temp$frd & temp$eventdate <= temp$frd)
  temp$eventdate <- NULL
  temp$medcode <- NULL
  temp <- unique(temp)
  anxietyClinG <- rbind(anxietyClinG,temp)
  print(num[i])
}
rm(temp)
anxietyClinG <- unique(anxietyClinG)
```


## Step 4.3: Extract people who have records for anxiety from CPRD Gold test files - note there may not be any of these depending on your condition code list
List test files within folder (note - if the file names contain "part1", "part2", these will need to be separated into different folders and the below steps repeated):
```{r}
testG <- list.files(path = "Filepath/CPRDGold/Test")
num <- c(1:length(testG))
num[num<100&num>9] <- paste0("0", num[num<100&num>9])
for (i in 1:9) {
  num[i] <- paste0("00", num[i])
}
```

Loop through each file to extract patients with anxiety records after patient registration starts/before it ends (adjust colClasses depending if you have different columns - the below intends to read patid, eventdate and medcode - the rest are NULL):
```{r}
anxietyTestG <- data.frame()
for (i in (1:length(num))) {
  name <- paste0("Filepath/CPRDGold/Test/0000_GOLD_Extract_Test_", num[i], ".txt")
  temp <- read.delim(name, header = TRUE, colClasses = c(rep("character", 2), rep("NULL", 3),  rep("character",1), rep("NULL", 9)))
  temp <- temp[temp$medcode %in% anxCodesG$medcode,]
  temp$eventdate <- as.Date(temp$eventdate, format = "%d/%m/%Y")
  temp <- merge(x=temp, y=pats, by.x="patid", by.y = "patid", all.y = FALSE, all.x=head(FALSE))
  temp <- subset(temp, temp$eventdate >= temp$frd & temp$eventdate <= temp$frd)
  temp$eventdate <- NULL
  temp$medcode <- NULL
  temp <- unique(temp)
  anxietyTestG <- rbind(anxietyTestG,temp)
  #Show progress through number of files
  print(num[i])
}
rm(temp)
anxietyTestG <- unique(anxietyTestG)
```


## Step 4.4: Extract people who have records for anxiety from CPRD Gold referral files - note there may not be any of these depending on your condition code list
List referral files within folder (note - if the file names contain "part1", "part2", these will need to be separated into different folders and the below steps repeated):
```{r}
refsG <- list.files(path = "Filepath/CPRDGold/Referrals")
num <- c(1:length(refsG))
num[num<100&num>9] <- paste0("0", num[num<100&num>9])
for (i in 1:9) {
  num[i] <- paste0("00", num[i])
}
```

Loop through each file to extract patients with anxiety records after patient registration starts/before it ends (adjust colClasses depending if you have different columns - the below intends to read patid, eventdate and medcode - the rest are NULL):
```{r}
anxietyRefsG <- data.frame()
for (i in (1:length(num))) {
  name <- paste0("Filepath/CPRDGold/Referrals/0000_GOLD_Extract_Referral_", num[i], ".txt")
  temp <- read.delim(name, header = TRUE, colClasses = c(rep("character", 2), rep("NULL", 3),  rep("character",1), rep("NULL", 6)))
  temp <- temp[temp$medcode %in% anxCodesG$medcode,]
  temp$eventdate <- as.Date(temp$eventdate, format = "%d/%m/%Y")
  temp <- merge(x=temp, y=pats, by.x="patid", by.y = "patid", all.y = FALSE, all.x=head(FALSE))
  temp <- subset(temp, temp$eventdate >= temp$frd & temp$eventdate <= temp$frd)
  temp$eventdate <- NULL
  temp$medcode <- NULL
  temp <- unique(temp)
  anxietyRefsG <- rbind(anxietyRefsG,temp)
  print(num[i])
}
rm(temp)
anxietyRefsG <- unique(anxietyRefsG)
```


## Step 5 Combine files, apply cohort limits and create demographics file
Combine files from CPRD Gold:
```{r}
anxietyG <- rbind(anxietyClinG,anxietyTestG,anxietyRefsG)
```


Import demographic variables - examples here used year of birth and gender (again, adjust colClasses for columns required):
```{r}
patsA <- read.delim("Filepath/CPRDAurum/patientsAurumD1.txt", header = TRUE, colClasses = c(rep("character", 1), rep("NULL", 1),  rep("character",2), rep("NULL", 7)))
patsG <- read.delim("Filepath/CPRDGold/patientsGoldD1.txt", header = TRUE, colClasses = c(rep("character", 1), rep("NULL", 1),  rep("character",2), rep("NULL", 13)))
anxietyObsA <- merge(x=anxietyObsA, y=patsA, by.x="patid", by.y = "patid", all.y = FALSE, all.x=head(FALSE))
anxietyG <- merge(x=anxietyG, y=patsG, by.x="patid", by.y = "patid", all.y = FALSE, all.x=head(FALSE))
```

Create cross-database unique identifier:
```{r}
anxietyObsA$DB <- "A"
anxietyObsA$UID <- paste0(anxietyObsA$patid,anxietyObsA$DB)
anxietyG$DB <- "G"
anxietyG$UID <- paste0(anxietyG$patid,anxietyG$DB)
```

Combine Gold and Aurum files:
```{r}
names(anxietyG)[2] <- "regstartdate"
names(anxietyG)[3] <- "regenddate"
anxPats <- rbind(anxietyObsA,anxietyG)
anxPats <- unique(anxPats)
```

Limit to people who were aged <75 at the start of the study period, and 18+ by the end of the study period
```{r}
anxPats <- subset(anxPats, anxPats$yob < 1999 & anxPats$yob > 1934)
```

## Step 6 Save patient demographics file
```{r}
write.table(anxPats, "Filepath/saved/patientMaster.txt", sep = "\t", col.names = TRUE, row.names = FALSE)
```
