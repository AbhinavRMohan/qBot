README

qBot - a simple single-turn medical question and answer chatbot using Google's T5-base model on a custom QA dataset. The model is trained to generate natural language answers given input questions.

-----------------------------------------------------------------------------------------------------

This Notebook uses both a Retriever methodology and a Generative methodology. For this particular dataset, the Retriever methodology worked better.

Overarching Assumptions - Retrieval:
 - Dataset Coverage: The data sufficiently represents the types of user questions expected in real usage. Unknown or highly novel questions beyond the dataset may not be answered accurately
 - Answer Accuracy: Each answer in the dataset is factually correct to be returned as-is
 - Single-Turn Conversations: The system is designed for single-turn queries (question-answer) and does not save a chat history
 - Text Preprocessing: Lowercase, white space removal port/starboard, punctuation resetting is sufficient to normalize questions and answers for similarity matching

Retrieval Model:
 - Model matches a user query to the closest stored question/answer
 - Pros:
   -- Safer to implement based on the dataset size, since it will return a real answer from the dataset
   -- Easier to implement and scales well with dataset size
   -- No hallucinations, since it is a direct retrieval
 - Cons:
   -- Cannot generate new text and is limited to what is in the dataset
   -- We did not semantically clean the q/a dataset; that means questions can be VERY similarly worded and give DIFFERENT answers still! We only aggregated by exact phraseology and thus, questions may give different answers at times

Overarching Assumptions - Generation:
 - Data:
   -- A subset of the data will be used for training and assumes that the entirety of the subset used contains sufficient information for training across different subject areas in medicine
 - Training:
   -- Limited computational use on free Google Colab - the base model may not be the best usecase here. A larger model would be more appropriate, also more trainability and finetuning would be required
   -- Adafactor optimizer chosen for efficiency
 - Use Case:
   -- Answers were truncated for training, so unsure how informative they would be (I ran into a LOT of issues while determining the model to use, setting it up on my dataset, where to prune, etc)
 - Evaluation:
   -- Same as Dataset Coverage above

Generative Model:
 - Model creates answers based on training data
 - Pros:
   -- Can produce answers even for questions not in the dataset (reasoning)
   -- Can combine information from multiple examples
 - Cons:
   -- Requires large datasets or pre-trained LLM fine-tuning
   -- Risk of hallucinations, especially since the data set is NOT large enough for such a task, and this is dangerous in the medical field (dataset is too small)

-----------------------------------------------------------------------------------------------------

Strengths and Weaknesses of the T5 Model

Strengths: ability to generalize, output relatively coherent text, and can be used for generation tasks. Model can also scale, is trainable

Weaknesses: high computational cost, heavy data dependency and very prone to hallucination

-----------------------------------------------------------------------------------------------------

Metrics Evaluated 

ROUGE - Rouge assesses the response summarization by looking at the response generation at the token level between the reference and generated response. 

BERT Score - BERT looks at the semantic relationship between the generated and reference text and looks at precision and recall - both the token match as well as the semantic match - i.e. does the response capture most of the semantic essence of the ground truth?

-----------------------------------------------------------------------------------------------------

Future Improvements

Hop on the LLM Train -

RAG based model
 - This is a significant step up from even a generative model, since we can evaluate the precision and recall of the retriver through context relevancy and accuracy through answer relevancy and faithfulness

Multiturn Conversation:
 - Storing the chat history will improve UX with them able to have more interactions with the chat bot

Dataset expansion
 - While we could have augmented the dataset with other data, this is something we can consider for the future as well to improve the robustness of the model and coverage

User input handling:
 - Handle typos, misspellings, slangs, phraseology, etc. with better NLU methods (parts of speech recognition, name entity recognition, intent classifier)
 - Handle data quality management better, through more data exploration and analysis (visualization)

There is a lot of scope for improvement even within the model used - using a model like Flan-T5, T5-Large, etc.

-----------------------------------------------------------------------------------------------------

SAMPLE RESPONSES

Question: treatments for copd
- Generated Answer: treatments for copd include - a combination of drugs and physical therapy to treat copd - injections of copd drugs into the blood - oral copd tablets - topical copd injections - intravenous copd medications copd is one of the most common types of chronic obstructive pulmonary disease in the united states.

Question: causes of glaucoma
- Generated Answer: glaucoma is one of the most common causes of blindness. it occurs in about 1 in 50,000 people worldwide.

Question: Tell me about vaccines
- Generated Answer: tell me about vaccines. vaccines are medicines that protect against diseases such as polio, tetanus, rubella, and rubella. if you have a vaccine, give it to someone you know who has a virus in your body. you can also take it to a doctor's office for a test of your immune system. Vaccines are safe, effective, and contain no side effects. you should not take vaccines unless you are pregnant or planning to be pregnant. - don't give me any side effects - do not give me a second chance to bevaccinated - tell me what you are doing - how much you are

Question: What is hypertension?
- Generated Answer: hypertension is a type of arterial hypertension. it occurs in people of all ages and ethnic backgrounds. it is caused by a number of factors, including - the fact that a person's blood pressure is too high or too low to breathe - a lack of oxygen in the blood - lack of exercise - poor diet - inability to control symptoms of hypertension, such as diabetes, heart disease, and stroke - increased risk of certain cancers in people with diabetes and other serious health problems such as heart attack, stroke, and other health problems that affect people with certain diseases such as stroke, diabetes, and diabetes. nih: national heart, lung, and blood institute

Question: What is diabetes?
- Generated Answer: diabetes is a condition that affects the blood sugar in your body. it occurs in people of all ages and ethnic backgrounds.

