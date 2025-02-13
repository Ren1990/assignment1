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

### **Result Discussion**
**Outcomes:**
1. Test with test_bom, all are classified correctly (only AC1 is found).
![alt text](data/outcome1.jpg)

2. Create new case1 (edge case) to generate all AC types.
![alt text](data/outcome2.jpg)

**Highlights and Learnings:**
1. Adding english translation (by LLM) improve classification for unseen chiness BOM headers. In test_bom, '淨重' and '毛重' are often confused during L2, but their english translation 'Net Weight' and 'Gross Weight' are perfectly matched with predefined BOM Header
![alt text](data/highlight1.jpg)

2. In L2 a strict criteria is defined: only the if the top retrieve predefined header has >=0.9 cosine similarity score, then it can used for matching category. Similarity retrieval using header description has better result than using header name, likely because of longer text to provide more distinction in vector space. In this example, '備註' and '重量單位' might not be classified correctly if header name is used for retrieval.
![alt text](data/highlight2.jpg)
