# Making Sense Out of Unstructured Patient Data

## Business Understanding
This project aims to analyze unstructured Electronic Medical Records (EMR) data to extract key information such as patient demographics, diagnoses, medications, and vital statistics. The primary goal is to derive meaningful insights from the raw EMR text corpora.

## Data Understanding
The data source is a text file containing unstructured medical records. The chosen dataset was selected for its relevance to diabetes, and the analysis focused on extracting critical health information. Future enhancements may include additional analysis or integration with structured datasets.

## Results
- **Patient Demographics:**
  - Number of male patients: 32
  - Number of female patients: 2
  - Total number of patients: 34
  - Mean age of patients: 50.20

- **Diagnosis:**
  - Type 1 DM

- **Medications:**
  - Full list: ['glyburide - 5 mg daily', 'Potassium Chl - 25 meq po qd', 'glimepiride - 4 mg daily', 'Insulin Nph - 80 1 80 UNITS SC AC', 'Insulin Nph - 20 1 20 UNITS SC AC', 'Potassium Chl - 25 meq po qd', 'Insulin', 'Insulin', 'glimepiride - 4 mg daily', 'Insulin Nph - 20 1 20 UNITS SC AC', 'glyburide - 5 mg daily', 'Insulin', 'glimepiride - 4 mg daily']
  - Unique Medications: ['Insulin Nph - 80 1 80 UNITS SC AC', 'glyburide - 5 mg daily', 'glimepiride - 4 mg daily', 'Insulin Nph - 20 1 20 UNITS SC AC', 'Potassium Chl - 25 meq po qd', 'Insulin']

- **Vital Statistics:**
  - Height: 68.0 cm
  - Weight: 168.0 lbs
  - Mean Height: 68.0 cm
  - Mean Weight: 168.0 lbs
  - Mean BMI: 25.54

## Technologies
- Python
- Visual Studio Code
- Regular Expressions (Regex)


## Status
The project is currently in the complete.

#### Importing regex as re
import re

#### Assuming your medical records are stored in a text file named 'medical_records.txt'
file_path = 'D:\Text_Sample_Diabetes(RM).txt'

#### Read the text file into a list of strings (each string represents a line in the file)
with open(file_path, 'r') as file:
    records = file.readlines()

print(file_path)

#### Define regular expressions to identify males and females in the records
male_pattern = re.compile(r'\b(?:M|Male|man|boy|male)\b', re.IGNORECASE)
female_pattern = re.compile(r'\b(?:F|Female|woman|girl|female)\b', re.IGNORECASE)

#### Counters for males and females
male_count = 0
female_count = 0

#### Iterate through the records and apply regular expressions
for record in records:
    if male_pattern.search(record):
        male_count += 1
    elif female_pattern.search(record):
        female_count += 1

#### Total number of patients
total_patients = male_count + female_count

#### Print the results
print(f"Number of male patients: {male_count}")
print(f"Number of female patients: {female_count}")
print(f"Total number of patients: {total_patients}")

#### Define the regular expression to extract ages
age_pattern = re.compile(r'\b(\d{1,3})\s*(?:yr|year|yrs)\b', re.IGNORECASE)

#### List to store extracted ages
ages = []

#### Iterate through the records and apply the regular expression to extract ages
for record in records:
    match = age_pattern.search(record)
    if match:
        # Extracted age is in the first capturing group
        age = int(match.group(1))
        ages.append(age)

#### Calculate the mean age
mean_age = sum(ages) / len(ages) if ages else 0

#### Print the result
print(f"Mean age of patients: {mean_age:.2f}")

#### Read the content from the file
with open(file_path, 'r') as file:
    medical_record_text = file.read()

#### Extract Diagnosis
diagnosis_match = re.search(r"a:(.*?)(?=(?:p:|s:|$))", medical_record_text)
if diagnosis_match:
    diagnosis = diagnosis_match.group(1).strip()
    print(f"Diagnosis: {diagnosis}")

#### Extract Administered Medication (Multiple occurrences)
medication_matches = re.finditer(r"p:administered\s(.*?)(?=(?:,|;|$))", medical_record_text)
medications = [match.group(1).strip() for match in medication_matches]
print(f"Medications: {medications}")

#### Get unique medications using a set
unique_medications = list(set(medications))

print("Unique Medications:", unique_medications)

#### Extract Height
height_match = re.search(r"Height\s(\d+\.?\d*)\s*(?:cm|in)\b", medical_record_text)
if height_match:
    height = float(height_match.group(1).strip())
    print(f"Height: {height} cm")

#### Extract Weight
weight_match = re.search(r"Weight\s(\d+\.?\d*)\s*lbs\b", medical_record_text)
if weight_match:
    weight = float(weight_match.group(1).strip())
    print(f"Weight: {weight} lbs")

#### Calculate Mean Height and Weight
heights = [height]  # Assuming a list in case there are multiple heights
weights = [weight]  # Assuming a list in case there are multiple weights

mean_height = sum(heights) / len(heights) if heights else None
mean_weight = sum(weights) / len(weights) if weights else None

#### mean_height is in inches and mean_weight is in pounds
#### Convert height to meters and weight to kilograms
mean_height_meters = mean_height * 0.0254  # Convert from inches to meters
mean_weight_kg = mean_weight * 0.453592  # Convert from pounds to kilograms

#### Calculate BMI
mean_bmi = round(mean_weight_kg / (mean_height_meters ** 2), 2)

print(f"Mean Height: {mean_height} cm")
print(f"Mean Weight: {mean_weight} lbs")
print(f"Mean BMI: {mean_bmi}")

