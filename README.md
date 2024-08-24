# AI and GPT Bootcamp. Homework for week 1.

## Introduction

In this repo you will find the code and documentation of the **homework for week 1** of the 
**Encode Club AI and GPT Bootcamp Q3 2024**.

The main goal of the first assignment is to use the **OpenAI API**.

The author, [Luis José Sánchez](https://github.com/LuisJoseSanchez), belongs to the fantastic **Group 9**.

## Exercise statement

The exercise statement is in the [Weekend Project](https://github.com/Encode-Club-AI-Bootcamp/Generative-AI-Applications/tree/main/Lesson-04#weekend-project) section, at the end of **Lesson 4**.

The base code is in [ChefGPT.py](https://github.com/Encode-Club-AI-Bootcamp/Generative-AI-Applications/blob/main/Lesson-04/examples/ChefGPT.py)

The detailed instructions are here:
<https://github.com/Encode-Club-AI-Bootcamp/Generative-AI-Applications/blob/main/Lesson-04/exercises/07-Chef-GPT.md>

## Jupyter notebook

The jupyter notebook with all the python code is here:
<https://github.com/LuisJoseSanchez/encode-club-ai-and-gpt-bootcamp-q3-homework-week1/blob/master/Encode_Club_AI_and_GPT_Bootcamp_Q3_Homework_for_week_1.ipynb>

Notice that you will have to provide your own API key to get it work.

Please, give a ⭐ if you liked this repo!

## Python code explained

The first requirement is the installation of `openai` package. The python code has been written in Google Colab, so an exclamation mark is needed to install packages via `pip` command.

```python
# Install openai API package
!pip install openai
```

```console
Collecting openai
  Downloading openai-1.42.0-py3-none-any.whl.metadata (22 kB)
Requirement already satisfied: anyio<5,>=3.5.0 in /usr/local/lib/python3.10/dist-packages (from openai) (3.7.1)
Requirement already satisfied: distro<2,>=1.7.0 in /usr/lib/python3/dist-packages (from openai) (1.7.0)
Collecting httpx<1,>=0.23.0 (from openai)
.
.
.
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 318.9/318.9 kB 15.6 MB/s eta 0:00:00
Downloading h11-0.14.0-py3-none-any.whl (58 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 58.3/58.3 kB 2.9 MB/s eta 0:00:00
Installing collected packages: jiter, h11, httpcore, httpx, openai
Successfully installed h11-0.14.0 httpcore-1.0.5 httpx-0.27.0 jiter-0.5.0 openai-1.42.0
```

We need to import the class `OpenAI` and use it to instantiate a client with the API Key. Notice that the real API key has been removed from the code, you must provide yours in order to execute the code properly. 

```python
# Import the openai module
from openai import OpenAI
```

```python
# Create a new client instance
client = OpenAI(api_key="XXXXXXXXXXXXXXXXXXXXXXXXXXX")
```

OpenAI must be configured to give the proper answers. In this case it will be an Spanish chef specializing in vegetarian cuisine. Notice that we "prepare" OpenAI with all the information we want using the role `system`.

```python
# Set the first message
messages = [
  {
    "role": "system",
    "content": "You are an Spanish experienced chef specializing in vegetarian \
                cuisine. You are willing to help people by suggesting \
                detailed vegetarian recipes for dishes they want to cook. \
                If someone asks you about dishes or ingredients related to \
                meat or fish, politely tell them that you only handle \
                inquiries about vegetarian cuisine.",
  }
]
```


```python
# Add another system instruction
messages.append(
  {
    "role": "system",
    "content": "The client will ask you one of these three topics: \
                ingredient-based dish suggestions, recipe requests for \
                specific dishes or recipe critiques and improvement \
                suggestions. If you don't know the dish or the \
                ingredient, you should answer that you don't know the \
                dish or ingredient and end the conversation.",
  }
)
```


```python
# More instructions
messages.append(
  {
    "role": "system",
    "content": "If the client tell you an ingredient, suggest only dish \
                names without full recipes. If you are asked for dish \
                name then provide a detailed recipe. If the client tells \
                you a recipe, offer suggested improvements.",
  }
)
```

There are a few models we can use: GPT4, GPT3.5, GPT4 mini, etc. We must choose one of them to use with our API calls.

```python
# Specify the model used
model = "gpt-4o-mini"
```

A menu is shown so the user can choose one the actions required. According to the stament of the exercise, the clien can tell an ingredient, a dish or a recipe. The menu detects incorrect inputs, only options 1, 2 and 3 are allowed.

```python
# Menu
while True:
  print("1. I want the recipe for an specific dish")
  print("2. I want a recipe for an specific ingredient")
  print("3. I want suggestions to improve a recipe that I know")
  option = input("Choose an option (1-3): ")

  if option in ["1", "2", "3"]:
    break
  else:
    print("Invalid option. Please choose 1, 2, or 3.")
```

```console
1. I want the recipe for an specific dish
2. I want a recipe for an specific ingredient
3. I want suggestions to improve a recipe that I know
Choose an option (1-3): 77
Invalid option. Please choose 1, 2, or 3.
1. I want the recipe for an specific dish
2. I want a recipe for an specific ingredient
3. I want suggestions to improve a recipe that I know
Choose an option (1-3): 1
```

Once we know what the user wants to do, we have to ask the specific ingredient, dish or recipe.

```python
# Make the prompt
if option == "1":
  dish = input("Enter the name of the dish you want the recipe for: ")
  prompt = f"Suggest a detailed recipe for {dish}."
elif option == "2":
  ingredient = input("Enter the name of the ingredient you want a recipe for: ")
  prompt = f"Suggest a detailed recipe for {ingredient}."
else:
  recipe = input("Enter the recipe you want to improve:")
  prompt = f"Suggest improvements to the following recipe: {recipe}."
```

```console
Enter the name of the dish you want the recipe for: spanish omelette
```


Now we have all the necessary information to construct the phrase with which we will make the API request.

```python
# Add the user message
messages.append(
  {
    "role": "user",
    "content": prompt,
  }
)
```

Time to make the API call. Cross your fingers 🤞 and be sure you have enough credit 💰 in your OpenAI account.

```python
# Make the API call
response = client.chat.completions.create(
  model=model,
  messages=messages,
)
```

Finally, the `response` array have the answer.

```python
# Show the result
for choice in response.choices:
  print(choice.message.content)
```

Paste here the answer in markdown format:

Sure! Here’s a detailed recipe for a classic Spanish omelette, also known as "Tortilla Española."

### Ingredients:
- 4 medium-sized potatoes
- 1 medium onion (optional)
- 6 large eggs
- Olive oil
- Salt
- Pepper

### Instructions:

1. **Prepare Potatoes and Onion**:
   - Peel the potatoes and slice them thinly (about 1/8 inch thick).
   - If you're using onion, chop it finely.

2. **Cook Potatoes and Onion**:
   - Heat a generous amount of olive oil in a large frying pan over medium heat. You want enough oil to cover the bottom of the pan.
   - Once the oil is hot, add the sliced potatoes and cook them gently. Stir occasionally, and cook for about 20 minutes until they are soft but not browned.
   - If using onion, add it to the potatoes about halfway through cooking, and stir until the onion is translucent.

3. **Beat the Eggs**:
   - While the potatoes are cooking, crack the eggs into a large bowl. Add a pinch of salt and pepper, then whisk until well combined.

4. **Combine Potatoes with Eggs**:
   - Once the potatoes (and onion, if used) are cooked, use a slotted spoon to transfer them to the bowl with beaten eggs. Mix gently to combine, making sure the potatoes are coated with the egg.

5. **Cook the Omelette**:
   - Remove most of the oil from the frying pan, leaving just enough to coat the bottom. Heat the pan over medium heat.
   - Pour the potato and egg mixture into the pan, spreading it evenly. Cook for about 5-7 minutes, or until the bottom is set and lightly browned.
   - To flip the omelette, place a large plate over the pan and carefully invert the omelette onto the plate. Slide it back into the pan to cook the other side for another 5-7 minutes.

6. **Finish and Serve**:
   - Once both sides are golden and the omelette is cooked through, slide it onto a serving plate.
   - Let it cool for a few minutes, then slice into wedges and serve. It can be enjoyed warm or at room temperature.

### Tips for Improvement:
- **Variations**: Try adding veggies like bell peppers, herbs, or even cheese for different flavors.
- **Cooking Method**: Some people prefer to bake the tortilla in the oven for a less oily version.
- **Presentation**: Serve with a fresh side salad or a drizzle of aioli for added flavor.

Enjoy your Spanish omelette!

