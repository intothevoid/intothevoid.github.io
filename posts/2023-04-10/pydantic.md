---
title: "Type safety in Python with Pydantic"
date: 2023-04-10T16:56:00+09:30
description: "An introduction to Pydantic, and type checking in Python"
tags: ["python", "languages", "programming"]
featured_image: "/images/python.png"
---

## Introduction to Pydantic

Pydantic is a Python library that provides data validation and settings management, using Python type annotations. It is designed to make it easy to define and validate data models, allowing you to catch errors and handle them gracefully, making your code more robust and easier to maintain.

Pydantic is particularly useful for handling data that is passed between different parts of a system, such as between a front-end web form and a back-end API, or between different microservices. By defining a clear schema for the data, you can ensure that it is consistent and valid, regardless of where it comes from.

## Features of Pydantic

One of the key features of Pydantic is the use of Python type annotations. These annotations provide a way to specify the expected type of a variable or argument, which can be used to validate the data and catch errors early in the development process. Pydantic also provides a number of built-in validators, such as "email" and "url", that can be used to ensure that data conforms to specific formats.

To define a Pydantic model, you simply create a class that inherits from the BaseModel class. In this class, you define the fields that make up the model, using Python type annotations to specify the expected types. You can also specify default values and validation rules for each field.

## Example

```python
from pydantic import BaseModel

class User(BaseModel):
    id: int
    name: str
    email: str
    password: str

```
In this model, we have defined four fields: id, name, email, and password. The id field is expected to be an integer, while the other fields are expected to be strings.

To create an instance of the User model, we simply pass in the data as a dictionary:

```python
data = {
    "id": 1,
    "name": "Alice",
    "email": "alice@example.com",
    "password": "secretpassword",
}

user = User(**data)

```
Pydantic will automatically validate the data and ensure that it conforms to the expected types and validation rules. If any errors are found, Pydantic will raise a ValidationError exception, which includes details about the error and the field that caused it.

Pydantic also provides a number of other features, such as support for custom validation functions, automatic conversion of data types, and support for parsing and serializing data to and from JSON.

Pydantic is a powerful library that provides an easy and efficient way to validate data and handle errors in your Python applications. By using Pydantic to define and validate your data models, you can catch errors early in the development process and ensure that your code is more robust and easier to maintain.

## Validation in Pydantic

In Pydantic, to get error details, you need to use a try/except block. The error type will be pydantic.error_wrappers.ValidationError.

Here is an example:

```python
from pydantic import BaseModel, ValidationError

class User(BaseModel):
    id: int
    name: str
    email: str
    password: str

try:

    data = {
        "id": 1,
        "name": "Alice",
        "email": "alice@example.com",
        "password": "secret",
        }

    user = User(**data)

except ValidationError as e:
    print(e.json())
``` 

## Custom Validation

Pydantic provides a number of built-in validators, such as "email" and "url", that can be used to ensure that data conforms to specific formats. However, you can also define your own custom validation functions, which can be used to validate data in any way you like.

To define a custom validation function, you simply create a function that accepts a single argument, which will be the value of the field being validated. The function should raise a ValueError if the value is invalid, or return the value if it is valid.

Here is an example of a custom validation function that checks whether a string is a valid email address:

```python
from pydantic import BaseModel, ValidationError, validator
from email_validator import validate_email, EmailNotValidError

class User(BaseModel):
    id: int
    name: str
    email: str
    password: str

    @validator("email")
    def email_validator(cls, v):
        try:
            validate_email(v)
        except EmailNotValidError as e:
            raise ValueError("Invalid email address")
        return v

try:
    user = User(**data)
    pprint(user)

except ValidationError as e:
    print(e.json())
```

## Conclusion

In this article, we have looked at Pydantic, a Python library that provides data validation and settings management, using Python type annotations. We have also looked at some of the features of Pydantic, including the use of Python type annotations to specify the expected types of fields, and the use of custom validation functions to validate data in any way you like.

Pydantic is a powerful library that provides an easy and efficient way to validate data and handle errors in your Python applications. By using Pydantic to define and validate your data models, you can catch errors early in the development process and ensure that your code is more robust and easier to maintain.

## References

- [Pydantic](https://pydantic-docs.helpmanual.io/)