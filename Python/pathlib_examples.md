# Reading json file
```python
import datetime
import json
from pathlib import Path

file_contents = Path("my.log").read_text()  # one big string
file_lines = Path("my.log").read_text().splitlines()  # list of lines
path = Path(__file__).parent / "config.json"  # relative to this module

if not path.exists():  # Check for existence
    path.parent.mkdir(parents=True, exist_ok=True)  # Create directories
    path.write_text("{}")  # Writing creates a new file by default

config = json.loads(path.read_text())  # load/parse JSON
config["last_modified"] = str(datetime.datetime.utcnow())

path.write_text(json.dumps(config))  # save as JSON
```
