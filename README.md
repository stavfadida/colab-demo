# Colab demos

## `creative_ideas.ipynb`

A beginner-friendly Google Colab notebook that asks for an everyday object and uses the OpenAI Responses API (`gpt-6-luna`) to suggest three unusual, practical uses. A loop lets you try another object or exit.

### Run it

1. Open `creative_ideas.ipynb` in [Google Colab](https://colab.research.google.com/) (you can upload the file or open it from Google Drive).
2. Run the notebook cells from top to bottom. The first cell installs the OpenAI Python SDK.
3. When prompted, enter your OpenAI API key. The prompt uses `getpass`, which hides the key as you type; the key is not included in the notebook source.
4. Enter an everyday object to see three ideas. Enter another object to try again, or type `exit` to stop.

Keep your API key private. Do not paste it into a code cell, save it in the notebook, or commit it to Git. The API request requires an account and access to the specified model.
