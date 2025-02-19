# OpenAI Function Call and Pydantic Integration Module

This Python module provides a powerful and efficient approach to output parsing when interacting with OpenAI's Function Call API. It leverages the data validation capabilities of the Pydantic library to handle output parsing in a more structured and reliable manner. This README will guide you through the installation, usage, and contribution processes of this module.
If you have any feedback, leave an issue or hit me up on [twitter](https://twitter.com/jxnlco). 


## Installation

To get started, clone the repository

```bash
git clone https://github.com/jxnl/openai_function_call.git
```

Next, install the necessary Python packages from the requirements.txt file:

```bash
pip install -r requirements.txt
```

## Contributing

Your contributions are welcome! If you have great examples or find neat patterns, clone the repo and add another example. 
The goal is to find great patterns and cool examples to highlight.

If you encounter any issues or want to provide feedback, you can create an issue in this repository. You can also reach out to me on Twitter at @jxnlco.

### Poetry

We also use poetry if you'd like

```bash
poetry build
```

Note that there's no separate pip install command for this module. Simply copy and paste the module's code into your application.

## Usage

This module simplifies the interaction with the OpenAI API, enabling a more structured and predictable conversation with the AI. Below are examples showcasing the use of function calls and schemas with OpenAI and Pydantic.

### Example 1: Function Calls

```python
import openai
from openai_function_call import openai_function

@openai_function
def sum(a:int, b:int) -> int:
    """Sum description adds a + b"""
    return a + b

completion = openai.ChatCompletion.create(
        model="gpt-3.5-turbo-0613",
        temperature=0,
        functions=[sum.openai_schema],
        messages=[
            {
                "role": "system",
                "content": "You must use the `sum` function instead of adding yourself.",
            },
            {
                "role": "user",
                "content": "What is 6+3 use the `sum` function",
            },
        ],
    )

result = sum.from_response(completion)
print(result)  # 9
```

### Example 2: Schema Extraction

```python
import openai
from openai_function_call import OpenAISchema

class UserDetails(OpenAISchema):
    """User Details"""
    name: str = Field(..., description="User's name")
    age: int = Field(..., description="User's age")

completion = openai.ChatCompletion.create(
    model="gpt-3.5-turbo-0613",
    functions=[UserDetails.openai_schema]
    messages=[
        {"role": "system", "content": "I'm going to ask for user details. Use UserDetails to parse this data."},
        {"role": "user", "content": "My name is John Doe and I'm 30 years old."},
    ],
)

user_details = UserDetails.from_response(completion)
print(user_details)  # UserDetails(name="John Doe", age=30)
```

## Advanced Usage

If you want to see more examples checkout the examples folder!

## License

This project is licensed under the terms of the MIT license.

For more details, refer to the LICENSE file in the repository.