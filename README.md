## Integration of a Mathematical Calulations with a Chat Completion System using LLM Function-Calling

### AIM:
To design and implement a Python function for calculating the volume of a cylinder, integrate it with a chat completion system utilizing the function-calling feature of a large language model (LLM).

### PROBLEM STATEMENT:
The objective of this program is to use OpenAI's GPT-3.5-turbo model to calculate the volume of a cylinder when given its radius and height. The program leverages OpenAI's function calling capability to execute a predefined function, calculate_cylinder_volume, which computes the volume based on the formula:

**Volume = π × radius² × height**

The chatbot interacts with the user by receiving a query, determining the required function call, and generating a response with the computed volume.

### DESIGN STEPS:

#### STEP 1:
Import Libraries – Load necessary Python libraries such as os, openai, json, and dotenv for handling API calls and environment variables.
#### STEP 2:
Load OpenAI API Key – Retrieve the API key from environment variables to authenticate requests.
#### STEP 3:
Define Function for Cylinder Volume Calculation – Create a function that computes the volume of a cylinder using the formula 
𝑉=𝜋𝑟*2ℎ , h and returns the result in JSON format.
#### STEP 4:
Define OpenAI Function Schema – Specify the function name, description, required parameters (radius, height), and their expected data types to enable OpenAI's function calling.
#### STEP 5:
Prepare User Query – Create a user message asking for the volume of a cylinder, formatted as a chatbot request.
#### STEP 6:
Make Initial API Request – Send the user query to OpenAI’s GPT-3.5-turbo model along with the function schema, allowing the model to determine if a function call is needed.
#### STEP 7:
Process OpenAI Response – Check if the model requests function execution, extract the necessary parameters, and call the defined function to compute the volume.
#### STEP 8:
Update Message History – Append the function's output to the conversation history to simulate a real chatbot interaction.
#### STEP 9:
Send Final API Request – Send the updated conversation history back to OpenAI to generate a final response that includes the computed volume.
#### STEP 10:
Displaying the Output – Extract and print the final response, which provides the computed volume of the cylinder.

### PROGRAM:
```
import os
import openai
import json
from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv())
openai.api_key = os.getenv("OPENAI_API_KEY")
def calculate_cylinder_volume(radius, height):
    volume = 3.14159265359 * (radius ** 2) * height
    return json.dumps({"radius": radius, "height": height, "volume": volume})
functions = [
    {
        "name": "calculate_cylinder_volume",
        "description": "Calculate the volume of a cylinder given its radius and height.",
        "parameters": {
            "type": "object",
            "properties": {
                "radius": {"type": "number", "description": "The radius of the cylinder in meters."},
                "height": {"type": "number", "description": "The height of the cylinder in meters."}
            },
            "required": ["radius", "height"]
        }
    }
]
messages = [{"role": "user", "content": "What is the volume of a cylinder with radius 5 and height 10?"}]
response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=messages,
    functions=functions
)
response_message = response["choices"][0]["message"]
if "function_call" in response_message:
    args = json.loads(response_message["function_call"]["arguments"])
    result = calculate_cylinder_volume(**args)
    messages.append({
        "role": "function",
        "name": "calculate_cylinder_volume",
        "content": result,
    })
    final_response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=messages,
    )
    print(final_response["choices"][0]["message"]["content"])
```

### OUTPUT:
![image](https://github.com/user-attachments/assets/c9bec645-d9df-4057-96c0-03b7cd6c8fe2)

### RESULT:
The code enables LLM-driven cylinder volume calculation via function calling.
