## Development of a Named Entity Recognition (NER) Prototype Using a Fine-Tuned BART Model and Gradio Framework

### AIM:
To design and develop a prototype application for Named Entity Recognition (NER) by leveraging a fine-tuned BART model and deploying the application using the Gradio framework for user interaction and evaluation.

### PROBLEM STATEMENT:
Accessing and synthesizing information from multiple documents is crucial for research, but manual analysis is time-consuming. A multidocument retrieval agent can automate this process by:

   1.Parsing and indexing multiple research articles.
   
   2.Enabling users to ask queries in natural language.
   
   3.Providing synthesized, concise, and accurate responses from the indexed documents.
   
The effectiveness of the system will be evaluated through diverse queries to test its accuracy and relevance.

### DESIGN STEPS:

## STEP 1: Load and Parse Research Articles
Use LlamaIndex's document loaders to read and parse multiple research articles in PDF or text format.

## STEP 2: Create a Unified Index
Combine and index content from all documents using LlamaIndex to enable cross-document retrieval.

## STEP 3: Set Up a Query Engine
Configure a query engine to allow natural language questions and retrieve relevant content.

## STEP 4: Implement the Retrieval Agent
Build a retrieval agent that extracts and synthesizes information from the index.

## STEP 5: Evaluate the Agent
Test the agent with diverse queries to evaluate the quality of its responses.

### PROGRAM:
```
Developed By: M.Someshvaran
Reg No:212225230270
# Helper function
import requests, json

#Summarization endpoint
def get_completion(inputs, parameters=None,ENDPOINT_URL=os.environ['HF_API_SUMMARY_BASE']): 
    headers = {
      "Authorization": f"Bearer {hf_api_key}",
      "Content-Type": "application/json"
    }
    data = { "inputs": inputs }
    if parameters is not None:
        data.update({"parameters": parameters})
    response = requests.request("POST",
                                ENDPOINT_URL, headers=headers,
                                data=json.dumps(data)
                               )
    return json.loads(response.content.decode("utf-8"))


API_URL = os.environ['HF_API_NER_BASE'] #NER endpoint
text = "My name is Andrew, I'm building DeepLearningAI and I live in California"
get_completion(text, parameters=None, ENDPOINT_URL= API_URL)

def ner(input):
    output = get_completion(input, parameters=None, ENDPOINT_URL=API_URL)
    return {"text": input, "entities": output}

gr.close_all()
demo = gr.Interface(fn=ner,
                    inputs=[gr.Textbox(label="Text to find entities", lines=2)],
                    outputs=[gr.HighlightedText(label="Text with entities")],
                    title="NER with dslim/bert-base-NER",
                    description="Find entities using the `dslim/bert-base-NER` model under the hood!",
                    allow_flagging="never",
                    #Here we introduce a new tag, examples, easy to use examples for your application
                    examples=["My name is somesh and I live in Chennai", "my hobby is playing football"])
demo.launch(share=True, server_port=int(os.environ['PORT3']))
```


### OUTPUT:
<img width="542" height="766" alt="Screenshot 2026-09-01 141216" src="https://github.com/user-attachments/assets/c8d38e88-5ebb-477f-816d-5fa45ee8451f" />

<img width="1155" height="613" alt="Screenshot 2026-09-01 141231" src="https://github.com/user-attachments/assets/dd0894b2-00d1-44cb-a60f-7ef56c3e77d9" />



### RESULT:
Thus, a multidocument retrieval agent using LlamaIndex to extract and synthesize information from multiple research articles is designed and implemented successfully.
