# AmbitionBox Web Scraper Refactoring Notes

## Overview

The original scraper was functional and successfully extracted company information from AmbitionBox. However, the code had several areas that could be improved for readability, maintainability, scalability, and correctness.

After analyzing the implementation, the code was refactored to follow cleaner Python practices and make it easier for other developers to understand and maintain.

---

# 1. Using Constants for Configuration

### Original Approach

```python
url = f"https://www.ambitionbox.com/list-of-companies?page={page}"
```

### Refactored Approach

```python
BASE_URL = "https://www.ambitionbox.com/list-of-companies?page={}"
```

### Why?

* Avoids repeating URLs throughout the code.
* Makes future URL changes easier.
* Improves readability by clearly separating configuration from logic.

---

# 2. Storing Data as Records Instead of Multiple Lists

### Original Approach

```python
name = []
rating = []
review = []
domain = []
loc = []
bad_review = []
```

### Refactored Approach

```python
companies_data = []
```

Each company is stored as a dictionary:

```python
companies_data.append({
    "Company Name": company_name,
    "Rating": rating,
    ...
})
```

### Why?

* Easier to understand.
* Prevents length mismatch errors.
* Directly compatible with Pandas DataFrames.
* More scalable when adding new fields.

---

# 3. Scraping Inside the Pagination Loop

### Original Issue

The pagination loop only visited pages:

```python
while True:
    ...
    page += 1
```

Actual data extraction occurred after the loop:

```python
company = soup.find_all(...)
```

As a result, only the final page was processed.

### Refactored Approach

```python
for page in range(1, 501):
    
    response = requests.get(...)
    
    soup = BeautifulSoup(...)
    
    company_cards = soup.find_all(...)
    
    for card in company_cards:
        ...
```

### Why?

* Every page is processed immediately.
* Prevents data loss.
* Ensures all company records are collected.

---

# 4. Meaningful Variable Names

### Original

```python
i
j
k
TL
```

### Refactored

```python
card
action
title
count
interlink
company_cards
```

### Why?

Meaningful names make code self-documenting and easier for developers to understand.

---

# 5. Handling Missing Data Safely

### Original

```python
i.find(...).text.strip()
```

Potential Error:

```python
AttributeError:
'NoneType' object has no attribute 'text'
```

### Refactored

```python
company_name.text.strip() if company_name else None
```

### Why?

* Prevents crashes.
* Handles incomplete records gracefully.

---

# 6. Simplifying Domain and Location Extraction

### Original

```python
TL = i.find(...).text.strip().split('|')

domain.append(TL[0].strip())
loc.append(TL[1].strip())
```

### Refactored

```python
parts = interlink.text.strip().split("|")

if len(parts) >= 2:
    domain = parts[0].strip()
    location = parts[1].strip()
```

### Why?

* Safer.
* Prevents index errors.
* Easier to read.

---

# 7. Improving Tertiary Information Extraction

## Original Problem

```python
j.find(
    'span',
    class_='companyCardWrapper__ActionCount'
)
```

This always returned the first count within the section.

As a result:

* Salary count could be incorrect.
* Interview count could be incorrect.
* Job count could be incorrect.

## Refactored Solution

```python
for action in action_wrappers:

    title = action.find(
        "span",
        class_="companyCardWrapper__ActionTitle"
    )

    count = action.find(
        "span",
        class_="companyCardWrapper__ActionCount"
    )

    stats[title.text.strip()] = count.text.strip()
```

### Why?

Each title is directly mapped to its corresponding value.

Example:

```python
{
    "Reviews": "1.2L",
    "Salaries": "10.2L",
    "Interviews": "11.8k",
    "Jobs": "4.1k",
    "Benefits": "10.8k",
    "Photos": "93"
}
```

This approach is both cleaner and more reliable.

---

# 8. Eliminating Long if-elif Chains

### Original

```python
if title == "Salary":
    ...
elif title == "Interviews":
    ...
elif title == "Jobs":
    ...
```

### Refactored

```python
stats[title] = count
```

### Why?

* Less code.
* Easier to maintain.
* Automatically supports new categories if the website changes.

---

# 9. Creating the DataFrame

### Original

```python
df = pd.DataFrame({
    'Company Name': name,
    'Rating': rating,
    ...
})
```

### Refactored

```python
df = pd.DataFrame(companies_data)
```

### Why?

* Cleaner.
* No need to manage multiple lists.
* Less chance of bugs.

---

# 10. Scalability Improvements

The refactored code can easily support:

* Additional company fields
* Multiple websites
* Database insertion
* API development
* Large-scale scraping projects

without major structural changes.

---

# Key Takeaways

The refactoring focused on four major software engineering principles:

### Readability

Using descriptive variable names and a cleaner structure.

### Maintainability

Reducing duplicated logic and simplifying future modifications.

### Reliability

Handling missing data and avoiding incorrect field extraction.

### Scalability

Building a structure that can grow as project requirements increase.

---

# Final Result

The refactored scraper:

✅ Processes every page correctly

✅ Extracts tertiary information accurately

✅ Prevents common scraping errors

✅ Produces a DataFrame directly

✅ Is easier for other developers to understand and maintain

✅ Follows cleaner Python coding practices
