---
title: "ML System Design Example Sepsis Detection"
date: 2026-01-03
draft: false
tags: ["ML", "system design", "interview preparation", "job hunt"]
summary: "Example of sepsis detection with ML"
---

Design a sepsis detection ML pipeline.

## 1. Clarifying requirements

**Business objective**
Early detection brings more saved lives, and fewer ER rooms would be used

**Features the system needs to support**?

**Data**
Where will the data come from? How large a dataset? Is the data labeled or not? How much data cleaning do we need to do?

**What are our constraints?**
How much computing power is available? Is it a cloud-based system or should the system work on a device? Is the model supposed to improve over time?

**Scale of the system** 
How many users do we have?

**Performance**
How fast must prediction be? Is a real-time solution expected? What is more preferable, accuracy over latency?


## 2. The problem:
Design a sepsis detection ML pipeline. Data available from the person who is already in a medical facility, having checked in, and based on the input criteria algorithm, should send a warning to the doctor system or raise a flag indicating a high possibility of sepsis.

## 3. Data preparation.

The parameters can be relevant:

**Heart Rate:** An elevated heart rate, known as tachycardia, can be an early sign of sepsis. Monitoring heart rate trends over time can help identify abnormalities and prompt further evaluation.

**Blood Pressure:** Sepsis can cause hypotension, or low blood pressure, which is a critical indicator of septic shock. Regular monitoring of blood pressure can help identify sudden drops that may warrant immediate medical attention.

**Body Temperature:** Sepsis can result in either high body temperature (fever) or low body temperature (hypothermia). Monitoring temperature patterns can provide valuable information for early detection.

**Respiratory Rate:** Rapid or shallow breathing, known as tachypnea or bradypnea, respectively, can be associated with sepsis. Monitoring respiratory rate can help identify abnormal breathing patterns.

**Oxygen Saturation:** Sepsis can lead to decreased oxygen levels in the blood.

The idea is that if all 5 parameters below a certain level, then send an alarm that it is a sepsis, if 3-4 then raise a warning, and otherwise do not raise anything.

I would suppose that we have data with all these 5 parameters, and whether sepsis was detected or not from this. If we have more parameters, it is even better.

We need to split available data into train, val, and test splits. I would do 80/10/10 splits.

## 5. Model development.
Since we have labels, we can use supervised ML with multiclass classification for 3 classes:
1. High risk of sepsis
2. Moderate risk of sepsis
3. Not a sepsis

To work with supervised learning, we need to follow the ETL process. Which means Extract -> Transform -> Load.

We are getting data from the Database of patients, logs, and maybe other hospital files. Then we need to transform them into labeled data, and after that, work on how it will be efficiently loaded into the system. 

Most of the data are numerical values or could be transformed from categorical data into numerical categories. We're gonna evaluate missing values and work with them as we go. If too many missing data points for a label, it will probably drop this label. With concentration on the main 5, which we described before.

For the model, I think a decision tree could be a good choice.
If we did not have enough data, I would consider sampling.

For evaluation, I would avoid accuracy for imbalanced data and focus on recall, precision, and PR–AUC.

## 6. Deployment and serving.
The main question is whether it is on cloud or on device deployment. I would suppose that it is in a local cluster since it is sensitive data. This solution brings us a cheaper cost since no billing from an outside company, but there is a cost in maintenance, and it needs to be upgraded on a regular basis.
Latency could also be faster than a base solution. Cons tough that ML could run slower and more general constraints, like we can increase memory immediately, some services needed to be added to the in-house system.


## 7. Monitoring and infrastructure.

Check for ML data drifts. Monitor prediction distribution and confidence scores. Track actual sepsis outcomes vs predictions. Adjust thresholds for performance degradation.

Monitoring input-output. Input distribution drift (vital signs outside normal ranges). Feature drift in case of population demographics change, and consider how to deal if the missing data rates increase.

Integration of clinical monitoring in case of a false positive rate is increasing. Alert clinical staff if the rate is increasing.
Monitor patient outcomes: Mortality, ICU length of stay.

Possibly add feedback loops:

- Clinician feedback on predictions (useful/not useful)
- Regular model retraining schedule (monthly/quarterly)
- Active learning: Flag uncertain cases for expert review

By the end, if we have time to calibrate the system, it's definitely worth considering bias and fairness to see performance across demographic groups. It is also important that clinical staff understand why alerts fire. So we need to run tests on the staff.
Consider liability too, who is responsible if the model misses sepsis?
Important: do not disrupt clinical workflow - integration should be smooth, and if possible, run collaboration with other hospitals' data.