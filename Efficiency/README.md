# Edit in VIM
```
1.
In our BV plugin,
# from pydantic import BaseModel, Field, model_validator # pydantic v2 works for Azure
from langchain_core.pydantic_v1 import BaseModel,Field # pydantic v1 works for Bhishan

When we deploy, we need to commend v1 and use v2.
- vi util_models.py
/BaseModel hit enter
^   go to the first letter
i   go to insert mode (edit the file)
:w  write the saved changes.

```
