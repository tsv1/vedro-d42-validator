# Vedro D42 Validator

[![Codecov](https://img.shields.io/codecov/c/github/tsv1/vedro-d42-validator/master.svg?style=flat-square)](https://codecov.io/gh/tsv1/vedro-d42-validator)
[![PyPI](https://img.shields.io/pypi/v/vedro-d42-validator.svg?style=flat-square)](https://pypi.python.org/pypi/vedro-d42-validator/)
[![PyPI - Downloads](https://img.shields.io/pypi/dm/vedro-d42-validator?style=flat-square)](https://pypi.python.org/pypi/vedro-d42-validator/)
[![Python Version](https://img.shields.io/pypi/pyversions/vedro-d42-validator.svg?style=flat-square)](https://pypi.python.org/pypi/vedro-d42-validator/)

This plugin integrates [d42 schema validation](https://d42.sh/docs/features/validation) into the Vedro testing framework, providing enhanced schema validation and error handling capabilities during test execution.

## Installation

<details open>
<summary>Quick</summary>
<p>

For a quick installation, you can use a plugin manager as follows:

```shell
$ vedro plugin install vedro-d42-validator
```

</p>
</details>

<details>
<summary>Manual</summary>
<p>

To install manually, follow these steps:

1. Install the package using pip:

```shell
$ pip3 install vedro-d42-validator
```

2. Next, activate the plugin in your `vedro.cfg.py` configuration file:

```python
# ./vedro.cfg.py
import vedro
import vedro_d42_validator

class Config(vedro.Config):

    class Plugins(vedro.Config.Plugins):

        class D42Validator(vedro_d42_validator.D42Validator):
            enabled = True
```

</p>
</details>

## Usage

Here is an example scenario demonstrating how to decode a base64 encoded string:

```python
# ./scenarios/decode_base64_encoded_string.py
import vedro
from base64 import b64decode
from d42 import schema

class Scenario(vedro.Scenario):
    subject = "decode base64 encoded string"

    def given(self):
        self.encoded = "Y3VjdW1iZXI="

    def when(self):
        self.result = {
            "result": b64decode(self.encoded)
        }

    def then(self):
        assert self.result == schema.dict({
            "result": schema.bytes(b"banana")
        })
```

Run the test using the command:

```shell
$ vedro run
```

If the expected and actual results don't match, a `ValidationException` will be raised, as illustrated below:

```shell
ValidationException:
 - Value <class 'bytes'> at _['result'] must be equal to b'banana', but b'cucumber' given
```
