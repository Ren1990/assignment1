# **BOM Header Classification: Demostration of LLM in Improving Schema Awareness.**
This is a Jupyter project demonstrate a hybrid solution to recognizing unseen BOM header uploaded by users into required categories.

### **Key Dependencies:**
1. Langchain Gemini-2.0-flash is used as the LLM to generate header description, which is later used for embedding and similarity retrieval.
2. Huggingface intfloat/multilingual-e5-large is used for text embedding.

### **Dataset Overview:**
1. 'Product Requirements Document' provides detailed requirements and relationship between Categories and BOM headers.
2. bom1.xlsx and bom2.xlsx, test_bom.xlsx are provided, containing both english and traditional chinese texts.
3. test_bom.xlsx is used as testing. Later on additional case1.xlsx is created to test to get all AC results (see below).

### **Objective:**
Design a solution to match the unseen uploaded BOM header with pre-defined categories. There are 4 Acceptance Criteria (AC) to measure the result:
1. Matched categories with its headers: solution has matched the uploaded BOM headers correctly.
2. Unmatched categories: categories has no matching uploaded BOM header. Assume each category should have at least one matching BOM header for every upload.
3. Matched categories with wrong headers: solution has matched the uploaded BOM headers incorrectly.
4. Unmatched bom headers: solution unable to match the uploaded BOM header into any category.

### **Solution Design Overview**
A hybrid solution maded of:
**Classification Logic:**
1. L1: header name matching check, required 100%. Apply english translation (supported by LLM) and text stripping when applicable.
2. L2: similarity retrieval, required >0.9 cosine similarity score and only the top result.

**Validation:**
1. V1: validate the datatype of classified uploaded column, if it incorrect, it will become AC3.

Solution Flow
1. **Data Preparation And Embedding:** Use LLM to generate header description on groun truth header, then embedde the header description for following similarity retrieval.
2. **Classification Logic**: Apply L1, then apply L2 on remaining unclassified uploaded BOM header.
3. **Validation**: Apply V1
4. **Output AC**: Generate AC result

### **Outcomes:**

1. Test with test_bom, all are classified correctly (only AC1 is found).
![alt text](data/outcome1.jpg)

2. Create new case1 (edge case) to generate all AC types.
![alt text](data/outcome2.jpg)

### **Highlights and Learnings:**
1. Adding english translation (by LLM) improve classification for unseen chiness BOM headers. In test_bom, '淨重' and '毛重' are often confused during L2, but their english translation 'Net Weight' and 'Gross Weight' are perfectly matched with predefined BOM Header
![alt text](data/highlight1.jpg)

2. In L2 a strict criteria is defined: only the if the top retrieve predefined header has >=0.9 cosine similarity score, then it can used for matching category. Similarity retrieval using header description has better result than using header name, likely because of longer text to provide more distinction in vector space. In this example, '備註' and '重量單位' might not be classified correctly if header name is used for retrieval.
![alt text](data/highlight2.jpg)

# **BOM Header Classification: Demonstration of LLM in Improving Schema Awareness**
This Jupyter project demonstrates a hybrid solution for recognizing unseen BOM headers uploaded by users and categorizing them correctly.

### **Key Dependencies:**
1. `Langchain Gemini-2.0-flash` is used as the LLM to generate header descriptions, which are later used for embedding and similarity retrieval.
2. `Huggingface intfloat/multilingual-e5-large` is used for text embedding.

### **Dataset Overview:**
1. The *Product Requirements Document* provides detailed requirements and defines the relationship between categories and BOM headers.
2. `bom1.xlsx`, `bom2.xlsx`, and `test_bom.xlsx` contain both English and Traditional Chinese texts.
3. `test_bom.xlsx` is used for testing. Additionally, `case1.xlsx` is created later to test all Acceptance Criteria (AC) results (see below).

### **Objective:**
Design a solution to match unseen uploaded BOM headers with predefined categories. There are four Acceptance Criteria (AC) to measure the results:
1. **Matched categories with their headers**: The solution correctly matches the uploaded BOM headers.
2. **Unmatched categories**: Categories without a matching uploaded BOM header. Assume each category should have at least one matching BOM header for every upload.
3. **Matched categories with incorrect headers**: The solution incorrectly matches the uploaded BOM headers.
4. **Unmatched BOM headers**: The solution is unable to match the uploaded BOM headers to any category.

### **Solution Design Overview**
A hybrid solution consists of:

#### **Classification Logic:**
1. **L1:** Header name matching check, requiring a 100% match. English translation (supported by LLM) and text stripping are applied when applicable.
2. **L2:** Similarity retrieval, requiring a cosine similarity score of >0.9, using only the top result.

#### **Validation:**
1. **V1:** Validate the data type of the classified uploaded column. If incorrect, it falls under AC3.

### **Solution Flow**
1. **Data Preparation and Embedding:** Use LLM to generate header descriptions for ground truth headers, then embed the header descriptions for similarity retrieval.
2. **Classification Logic:** Apply L1, then apply L2 to the remaining unclassified uploaded BOM headers.
3. **Validation:** Apply V1.
4. **Output AC:** Generate AC results.

### **Outcomes:**

1. Testing with `test_bom.xlsx`, all headers are classified correctly (only AC1 is found).
   ![alt text](data/outcome1.jpg)

2. Creating `case1.xlsx` (edge case) generates all AC types.
   ![alt text](data/outcome2.jpg)

### **Highlights and Learnings:**
1. Adding English translation (via LLM) improves classification for unseen Chinese BOM headers. In `test_bom.xlsx`, *淨重* and *毛重* are often confused during L2, but their English translations, *Net Weight* and *Gross Weight*, perfectly match the predefined BOM headers.
   ![alt text](data/highlight1.jpg)

2. In L2, a strict criterion is defined: Only if the top retrieved predefined header has a cosine similarity score of >=0.9 can it be used for matching categories. Similarity retrieval using header descriptions yields better results than using header names, likely because longer text provides more distinction in vector space. In this example, *備註* and *重量單位* might not be classified correctly if the header name alone is used for retrieval.
   ![alt text](data/highlight2.jpg)a
