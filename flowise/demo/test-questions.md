# Demo Test: Bella Cucina Restaurant Chatbot

## Setup Instructions

1. In Flowise, duplicate the `rag-chatflow-template.json` chatflow
2. Rename it to `Bella Cucina Demo`
3. In the **Faiss Vector Store** node, set `basePath` to `/root/.flowise/vectorstore/bella-cucina`
4. In the **Conversational Retrieval QA Chain** node, update the system prompt:
   ```
   You are a helpful assistant for Bella Cucina restaurant. Answer questions only using the provided documents. If the answer is not in the documents, say: "I don't have that information. Please contact us at (555) 234-5678."

   Be concise, friendly, and professional.
   ```
5. In the **Plain Text** node, paste the entire contents of `bella-cucina-faq.txt`
6. Click **Upsert** to index the documents
7. Run the test questions below in the chat window

---

## Test Questions and Expected Answers

**Q1: What are your opening hours on Saturday?**
Expected: Saturday 11:30 AM – 11:00 PM

**Q2: Do you have gluten-free options?**
Expected: Yes, gluten-free pasta is available as a substitute for any pasta dish (+$3).

**Q3: How much does the Tagliatelle Bolognese cost?**
Expected: $22

**Q4: How do I make a reservation for a large group?**
Expected: Call (555) 234-5678 or visit the website; credit card required for 8+ guests.

**Q5: Is there parking nearby?**
Expected: Free street parking on Oak Street after 6 PM weekdays / all day weekends; Oak Street Garage is 2 minutes away (paid).

---

## Pass Criteria

All 5 questions answered correctly using only the document content. No hallucination or made-up information.
