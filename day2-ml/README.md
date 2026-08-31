# 🍕 AI Product Recommendation Engine for a Restaurant Chain

> **Name:** Syed Usman Ali<br>
> **Member 5 Component**: Zero-Shot Natural Language Menu Recommendation
> **Group:** 37
> **Primary Technology:** Hugging Face Transformers
> **Model:** `facebook/bart-large-mnli`

## 📌 Project Overview

This component is part of an **AI Product Recommendation Engine for a Restaurant Chain**.

It takes a user's natural-language food preference and uses a pre-trained Transformer model from Hugging Face to identify the most relevant menu category and recommend suitable menu items.

For example:

> **User:** "I want something crispy and spicy."

The system interprets the request, identifies the most relevant food category, searches within that category, and returns a suitable menu item along with its price and model score.

The component does not require a manually labeled training dataset because it uses **zero-shot classification** with a pre-trained Natural Language Inference (NLI) model.

---

## 🧠 Approach: Zero-Shot Classification

The system uses `facebook/bart-large-mnli`, a pre-trained BART model fine-tuned for Natural Language Inference.

Instead of training a new classifier specifically for restaurant data, the model compares the user's query against candidate labels and assigns scores based on how well the query matches those labels.

### Basic Flow

```text
User's Natural Language Query
              │
              ▼
     Hugging Face Pipeline
              │
              ▼
     Category Classification
              │
              ▼
       Filter Menu Items
              │
              ▼
    Item-Level Classification
              │
              ▼
       Recommended Items
              │
              ▼
       Item + Price + Score
```

---

## ⚙️ Two-Pass Classification

Initially, passing a large number of menu items directly to the zero-shot classifier produced weaker and less useful scores.

The menu contained **75+ items**, including multiple similar items within the same category. To make the classification process more focused, I implemented a **two-pass hierarchical approach**.

### Pass 1 — Category Classification

The user's query is first compared against broad restaurant categories such as:

* Burger
* Roll
* BBQ
* Pizza
* Pasta
* Cold Drink

The highest-scoring category is selected.

### Pass 2 — Item Classification

The menu is then filtered to items belonging to the selected category.

The same user query is compared against only those items, allowing the model to select the most relevant specific dish.

### Example

```text
User:
"I want a dripping cheesy burger"

             │
             ▼

       PASS 1
   Category Matching

       Burger
        91%

             │
             ▼

     Filter Burger Menu

             │
             ▼

       PASS 2
      Item Matching

   Beef Burger (Cheese)
        78%

             │
             ▼

       Final Result

   Beef Burger (Cheese)
          PKR 180
```

The two-pass design reduces the number of competing candidates during the second classification stage and makes the recommendation process more focused.

---

## 🛡️ Input Validation & Error Handling

The system includes basic validation for invalid or unsuitable inputs.

### Empty Input

Whitespace-only or empty queries are rejected instead of being sent to the model.

### Very Short Input

Queries below the minimum length are rejected because inputs such as `"hi"` do not provide enough information for meaningful food recommendations.

### Out-of-Scope Queries

The system attempts to identify non-food requests and refuses to recommend a menu item when the request is outside the intended domain.

Example:

```text
Input:
"I need a laptop for work."

Output:
Rejected — the request is outside the restaurant recommendation domain.
```

### Alternative Recommendations

The system can return multiple high-ranking menu items rather than only one result, allowing the user to choose between alternatives.

---

## 📊 Dataset

The recommendation component uses a menu dataset containing **75+ restaurant items**.

Each menu entry contains information such as:

* Item name
* Category
* Description
* Price in PKR

The dataset was constructed from a local restaurant menu and adapted for use in this prototype.

---

## 🧪 Testing

The system was tested using different natural-language queries covering specific preferences, general cravings, and invalid inputs.

| Test Query                                     | Category | Recommendation             |   Price | Model Score |
| ---------------------------------------------- | -------- | -------------------------- | ------: | ----------: |
| "I want a juicy beef burger with extra cheese" | Burger   | Beef Burger (Jumbo Cheese) | PKR 250 |       82.4% |
| "I'm craving spicy grilled chicken tikka"      | BBQ      | Chicken Bihari Tikka (Leg) | PKR 200 |       76.8% |
| "I want something crispy and spicy"            | Burger   | Zinger Burger              | PKR 160 |       68.2% |
| "Something quick to eat while walking"         | Roll     | Chicken Mayo Roll          | PKR 110 |       71.4% |
| "I want a loaded pizza with lots of meat"      | Pizza    | Supper Supreme             | PKR 550 |       79.6% |
| "I'm craving creamy pasta with cheese"         | Pasta    | Passta Large               | PKR 300 |       74.8% |
| "I need a new laptop for work"                 | —        | Rejected                   |       — |           — |
| `""`                                           | —        | Rejected — empty input     |       — |           — |
| `"hi"`                                         | —        | Rejected — input too short |       — |           — |
| "Give me something from the BBQ section"       | BBQ      | Top BBQ recommendation     |       — |           — |

> **Note:** The reported scores are the zero-shot classification scores produced by the model. They should not be interpreted as calibrated probabilities of recommendation correctness.

---

## 🧰 Technology Stack

* **Python 3.13**
* **Hugging Face Transformers**
* **PyTorch**
* **BART Large MNLI**
* Restaurant menu dataset

---

## 🚀 Installation

Install the required dependencies:

```bash
pip install torch transformers
```

The first execution downloads the required model from the Hugging Face Hub.

---

## ▶️ Running the Project

Run the recommendation script:

```bash
python recommend_engine.py
```

The system accepts a natural-language food request and returns the relevant category, recommended menu item(s), price, and model score.

---

## 📁 Project Structure

```text
restaurant-recommendation/
│
├── recommend_engine.py
├── menu_data.csv
├── README.md
└── screenshots/
```

---

## 🔍 Why Hugging Face Transformers?

Hugging Face provides access to a large ecosystem of pre-trained Transformer models without requiring a new model to be trained from scratch.

For this component, `facebook/bart-large-mnli` was selected because it supports **zero-shot classification**, allowing natural-language restaurant queries to be compared against candidate categories and menu items without requiring a task-specific labeled dataset.

This makes it useful for prototyping a recommendation system when a large collection of labeled user-order data is not available.

---

## 🔮 Future Improvements

Possible improvements to the current prototype include:

* Combine Transformer-based query understanding with **vector similarity search**.
* Use embeddings to improve semantic matching between user preferences and menu descriptions.
* Fine-tune a model using real restaurant ordering data.
* Add dietary and allergen preferences such as vegetarian, vegan, or gluten-free.
* Support multiple restaurants and larger menus.
* Add a web interface using FastAPI and a frontend framework.
* Integrate the component with the other modules of the complete recommendation engine.

---

## 🎯 Conclusion

This component demonstrates how a pre-trained Hugging Face Transformer can be used to interpret natural-language food preferences and map them to relevant restaurant menu items.

The main implementation uses a **two-pass zero-shot classification architecture**:

**User Query → Category → Filtered Menu → Specific Recommendation**

The approach provides a practical prototype without requiring a large labeled training dataset while leaving room for future improvements using embeddings, vector databases, fine-tuning, and larger-scale recommendation systems.

