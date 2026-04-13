#  API Ethics Assignment

## 👤 Name: Tupti Ashish Shetty

---

# Task 1 — Classification and Handling of PII Fields

## 🔹 Introduction
In healthcare datasets, protecting Personally Identifiable Information (PII) is essential to ensure privacy and ethical data sharing. PII is classified into:
- **Direct PII**: Directly identifies an individual
- **Indirect PII**: Can identify an individual when combined with other data

---

## 🔹 Field-wise Explanation

### 1. full_name
- **Type:** Direct PII  
- **Action:** Drop / Pseudonymize  
- **Justification:**  
  Full name directly identifies a person and is not required for analysis. It should be removed or replaced with a unique ID.

---

### 2. email
- **Type:** Direct PII  
- **Action:** Drop  
- **Justification:**  
  Email is a unique identifier and allows direct contact. It has no analytical value and poses high privacy risk.

---

### 3. date_of_birth
- **Type:** Indirect PII  
- **Action:** Mask / Generalize  
- **Justification:**  
  Date of birth can lead to identification when combined with other fields. Convert it into age or age groups.

---

### 4. zip_code
- **Type:** Indirect PII  
- **Action:** Mask / Generalize  
- **Justification:**  
  Exact location can help identify individuals. Keep only partial zip code (e.g., first 2–3 digits).

---

### 5. job_title
- **Type:** Indirect PII  
- **Action:** Keep / Generalize  
- **Justification:**  
  Useful for analysis but may identify individuals in rare cases. Can be grouped into categories.

---

### 6. diagnosis_notes
- **Type:** Indirect PII (Sensitive Data)  
- **Action:** Pseudonymize / Clean  
- **Justification:**  
  May contain sensitive health information and hidden identifiers. Remove names and personal details.

---

## 🔹 Final Summary Table

| Field | PII Type | Action | Reason |
|------|---------|--------|--------|
| full_name | Direct PII | Drop / Pseudonymize | Direct identification |
| email | Direct PII | Drop | High privacy risk |
| date_of_birth | Indirect PII | Mask | Prevent re-identification |
| zip_code | Indirect PII | Mask | Protect location privacy |
| job_title | Indirect PII | Generalize | Maintain usefulness |
| diagnosis_notes | Sensitive (Indirect) | Clean & Pseudonymize | Remove hidden identifiers |

---

## 🔹 Key Takeaways

- Direct PII should always be removed or replaced  
- Indirect PII should be masked or generalized  
- Sensitive data must be cleaned carefully  
- Goal is to balance privacy with data usefulness  

---



#  Task 2 — Audit the API Script for Ethical Compliance

## 🔹 Introduction
The given API script collects healthcare data but does not fully follow ethical and Terms of Service (TOS) guidelines. Below are key violations along with their fixes.

---

## 🔹 Violation 1: Hardcoded API Key

❌ Problem

for page in range(1, 101):

Fetches 100 pages continuously without delay

May exceed API rate limits

Can overload server and violate TOSb


✅ Corrected Code

import time
import requests
import os

API_URL = "https://healthstats-api.example.com/records"
API_KEY = os.getenv("API_KEY")

records = []

for page in range(1, 101):
    response = requests.get(API_URL, params={"page": page, "key": API_KEY})
    
    if response.status_code != 200:
        print("Error:", response.status_code)
        break
    
    data = response.json()
    records.extend(data.get("results", []))
    
    time.sleep(1)  # Add delay to respect API rate limits


✔️ Justification

Prevents overloading the API

Ensures compliance with API Terms of Service

Handles errors properly

Violation 3: Storing Raw PII Data

❌ Problem

save_to_database(records)


Stores sensitive personal data without cleaning

Violates privacy and ethical standards

✅ Corrected Code

def clean_record(record):
    record.pop("full_name", None)
    record.pop("email", None)
    return record

cleaned_records = [clean_record(r) for r in records]

save_to_database(cleaned_records)


✔️ Justification
Removes sensitive identifiers

Ensures safe data sharing

Follows ethical data handling practices

🔹 Key Takeaways

Never expose API keys in code

Always respect API rate limits and TOS

Clean and anonymize data before storing

Ethical data handling is critical in healthcare



