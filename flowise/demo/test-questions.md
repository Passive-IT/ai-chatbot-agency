# Demo Test: Bella Cucina Restaurant Chatbot

## Setup Instructions

1. In Flowise, duplicate the `rag-chatflow-template.json` chatflow
2. Rename it to `Bella Cucina Demo`
3. In the **Faiss Vector Store** node, set `basePath` to `/root/.flowise/vectorstore/bella-cucina`
4. In the **Conversational Retrieval QA Chain** node, update the system prompt:
   ```
   You are a helpful assistant for Bella Cucina restaurant. Answer questions only using the provided documents. If the answer is not in the documents, say: "I don't have that information. Please contact us at (555) 234-5678."

   Be concise, friendly, and professional. Do not make up information.

   ## Human Handoff
   If the user asks to speak to a human, real person, agent, or says "help me" in a way that indicates they want human assistance, respond with:
   "I'd be happy to connect you with our team! To make sure someone gets back to you quickly, could you please share:
   1. Your name
   2. Your phone number or email

   Once you provide those details, our team will reach out to you shortly."

   After the user provides their contact information, confirm receipt and say: "Thank you! Our team at Bella Cucina will contact you soon. Is there anything else I can help with in the meantime?"
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

## Human Handoff Test Questions

**Q6: I want to talk to a real person**
Expected: Chatbot asks for name and phone number/email to connect with the team.

**Q7: Can I speak to a human please?**
Expected: Same handoff response — asks for contact details.

**Q8: Help me, I need to speak to someone**
Expected: Same handoff response — asks for contact details.

**Q9: (After Q6) My name is John, my phone is 555-123-4567**
Expected: Chatbot confirms receipt and says the Bella Cucina team will contact them soon.

---

## Pass Criteria

- Q1–Q5: Answered correctly using only document content. No hallucination.
- Q6–Q8: Handoff triggered — chatbot asks for name and phone/email.
- Q9: Contact info acknowledged and confirmation message sent.
- End-to-end (requires n8n): After Q9, a lead email notification is received by the business.
