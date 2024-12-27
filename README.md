# Construction Safety Incident Retrieval System with RAG

This project implements a Retrieval-Augmented Generation (RAG) system to improve recommendations for construction safety by utilizing real-world incident data, semantic search, and language generation. The goal is to provide construction safety engineers with actionable safety recommendations based on past incidents, helping to mitigate risks and ensure a safer work environment.

## Key Features

Incident Data Processing: The system processes a dataset containing detailed incident summaries from various construction sites, focusing on workplace accidents and injuries.


Vectorized Database: Uses Qdrant, a vector database, to store incident summaries as vector embeddings generated using Sentence Transformers, enabling semantic search functionality.


Semantic Search: Allows users to query the system using natural language and retrieve the most relevant past incidents based on the content of their query.


Safety Recommendations: Integrates a local language model from HF, to generate context-specific safety guidelines based on the retrieved incidents, helping users to follow best practices and reduce risks.

## Technologies Used

Python: The primary programming language used for building the system.


Qdrant: Vector database used to store and search incident data.


Sentence Transformers: Used for generating vector embeddings of incident summaries to enable semantic search.


OpenAI API: Used for completing language generation tasks with the Llama-3.2 model.

## How It Works

Users can input queries describing construction safety scenarios (e.g., “incidents involving wet surfaces”). The system retrieves the top relevant incidents based on the semantic similarity between the query and incident summaries. The retrieved incident summaries are passed to the Llama-3.2 model, which generates safety recommendations based on the context of the incidents. For example, it may suggest precautions for working on slippery surfaces based on past incidents involving falls.

## Example Usage

### Query for Incidents Involving Water and Rain
```user_prompt = "incidents involving water and rain"

hits = qdrant.search(
    collection_name="incidents",
    query_vector=encoder.encode(user_prompt).tolist(),
    limit=3
)

#The system searches for the most relevant incidents based on the query and returns summaries such as:

{
  "Summary": "on may 9 2000 employee #1 was performing abrasive blasting operations on the roof of a water tower. employee #1 had not donned his fall protection equipment and was subsequently not tied off. when he got to the edge of the tower he slipped or stumbled and fell 150 ft to the ground. employee #1 was killed.",
  "Result": 1
}

#Generate Safety Recommendations
#Based on the retrieved incidents, the system generates a safety recommendation:

completion = client.chat.completions.create(
    model="LLaMA_CPP",
    messages=[
        {"role": "system", "content": "You are a bot, an construction safety engineer. Your top priority is to help guide fellow engineers to follow safety protocols to reduce incidents at workplace based on the some incident reports"},
        {"role": "user", "content": "create me recommendations to work safely"},
        {"role": "assistant", "content": str(search_results)}
   ]
)


```
## Output
```
As a construction safety engineer,
I'd like to emphasize the importance of working safely on slippery and wet surfaces.

Here are some guidelines:

Pre-Job Precautions
Conduct a hazard assessment: Identify potential slippery and wet areas on the job site.
Clean up spills immediately: Ensure that any spills or leaks are cleaned up promptly to prevent slipping hazards.
Use warning signs: Post warning signs to alert workers of the potential hazard.


Personal Protective Equipment (PPE)
Wear non-slip shoes: Ensure that workers wear shoes with good traction to prevent slipping.
Use slip-resistant gloves: Provide workers with gloves to prevent slipping.
Use safety harnesses: Ensure that workers are properly tied off to a safety line when working on wet surfaces.


Work Practices
Use proper ladder safety: Ensure that ladders are properly positioned and secured on wet surfaces to prevent slipping.
Use scaffolding safely: Ensure that scaffolding is properly erected and secured on wet surfaces.
Use fall protection equipment: Ensure that workers are equipped with fall protection gear.
```
## Another output based on incidents involving heights and heavy equipment

```
Construction Safety Recommendations

Based on the incident reports provided, I've identified common safety hazards and recommended measures to prevent similar incidents:

1. Personal Protective Equipment (PPE)

* Ensure all employees wear required PPE, including hard hats, safety glasses, ear protection, and steel-toed boots.
* Regularly inspect PPE for damage or wear and tear.

2. Ladder Safety

* Ensure ladders are properly inspected and maintained before use.
* Use ladder safety devices, such as ladder levelers or stabilizers, when working at heights.
* Establish a "three-point rule" for ladder use: one hand and two feet, or two hands and one foot, or two feet and one hand.

3. Chainsaw Safety

* Always maintain a safe distance from the chainsaw's kickback zone.
* Keep loose clothing and long hair tied back when operating a chainsaw.
* Never reach over or under the chainsaw while it's in operation.

4. Fall Protection

* Ensure all employees are properly trained on fall protection procedures.
* Use fall protection equipment, such as harnesses and lanyards, when working at heights.
* Foster a safety-first culture within the organization.
* Recognize and reward employees for their safety contributions.
```
