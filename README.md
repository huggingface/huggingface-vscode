# 🤗 VSCode extension for testing open source code completion models

It was forked from [tabnine-vscode](https://github.com/codota/tabnine-vscode) & modified for making it compatible with open source code models on [hf.co/models](https://huggingface.co/models). 

** Announcement (Aug 25, 2023): latest version of this extension supports [codellama/CodeLlama-13b-hf](hf.co/codellama/CodeLlama-13b-hf). Find more info [here](#code-llama) how to test Code Llama with this extension.

** Announcement (Sept 4, 2023): latest version of this extension supports [Phind/Phind-CodeLlama-34B-v2](hf.co/Phind/Phind-CodeLlama-34B-v2) and [WizardLM/WizardCoder-Python-34B-V1.0](hf.co/WizardLM/WizardCoder-Python-34B-V1.0). Find more info [here](#phind-and-wizardcoder) how to test those models with this extension.

We also have extensions for:
* [neovim](https://github.com/huggingface/hfcc.nvim)
* [jupyter](https://github.com/bigcode-project/jupytercoder)

The currently supported models are:

* [StarCoder](https://huggingface.co/blog/starcoder) from [BigCode](https://www.bigcode-project.org/) project. Find more info [here](https://huggingface.co/blog/starcoder).
* [Code Llama](http://hf.co/codellama) from Meta. Find more info [here](http://hf.co/blog/codellama).

## Installing

Install just like any other [vscode extension](https://marketplace.visualstudio.com/items?itemName=HuggingFace.huggingface-vscode).

By default, this extension is using [bigcode/starcoder](https://huggingface.co/bigcode/starcoder) & [Hugging Face Inference API](https://huggingface.co/inference-api) for the inference. However, you can [configure](#configuring) to make inference requests to your custom endpoint that is not Hugging Face Inference API. Thus, if you are using the default Hugging Face Inference AP inference, you'd need to provide [HF API Token](#hf-api-token).

#### HF API token

You can supply your HF API token ([hf.co/settings/token](https://hf.co/settings/token)) with this command:
1. `Cmd/Ctrl+Shift+P` to open VSCode command palette
2. Type: `Hugging Face Code: Set API token`

<img src="https://github.com/huggingface/huggingface-vscode/raw/master/assets/set-api-token.png" width="800px">

## Testing

1. Create a new python file
2. Try typing `def main():`

<img src="https://github.com/huggingface/huggingface-vscode/raw/master/assets/ext-working.png" width="800px">

#### Checking if the generated code is in [The Stack](https://huggingface.co/datasets/bigcode/the-stack)

Hit `Cmd+shift+a` to check if the generated code is in in [The Stack](https://huggingface.co/datasets/bigcode/the-stack).
This is a rapid first-pass attribution check using [stack.dataportraits.org](https://stack.dataportraits.org).
We check for sequences of at least 50 characters that match a Bloom filter.
This means false positives are possible and long enough surrounding context is necesssary (see the [paper](https://dataportraits.org/) for details on n-gram striding and sequence length).
[The dedicated Stack search tool](https://hf.co/spaces/bigcode/search) is a full dataset index and can be used for a complete second pass. 


## Developing
Make sure you've [installed yarn](https://yarnpkg.com/getting-started/install) on your system.
1. Clone this repo: `git clone https://github.com/huggingface/huggingface-vscode`
2. Install deps: `cd huggingface-vscode && yarn install --frozen-lockfile`
3. In vscode, open `Run and Debug` side bar & click `Launch Extension`

## Checking output

You can see input to & output from the code generation API:

1. Open VSCode `OUTPUT` panel
2. Choose `Hugging Face Code`

<img src="https://github.com/huggingface/huggingface-vscode/raw/master/assets/ext-output.png" width="800px">

## Configuring

You can configure: endpoint to where request will be sent and special tokens.

<img src="https://github.com/huggingface/huggingface-vscode/raw/master/assets/set-configs.png" width="800px">

Example:

Let's say your current code is this:
```py
import numpy as np
import scipy as sp
{YOUR_CURSOR_POSITION}
def hello_world():
    print("Hello world")
```

Then, the request body will look like:
```js
const inputs = `{start token}import numpy as np\nimport scipy as sp\n{end token}def hello_world():\n    print("Hello world"){middle token}`
const data = {inputs, parameters:{max_new_tokens:256}};  // {"inputs": "", "parameters": {"max_new_tokens": 256}}

const res = await fetch(endpoint, {
    body: JSON.stringify(data),
    headers,
    method: "POST"
});

const json = await res.json() as any as {generated_text: string};  // {"generated_text": ""}
```

## Code Llama

To test Code Llama 13B model:
1. Make sure you have the [latest version of this extension](#installing).
2. Make sure you have [supplied HF API token](#hf-api-token)
3. Open Vscode Settings (`cmd+,`) & type: `Hugging Face Code: Config Template`
4. From the dropdown menu, choose `codellama/CodeLlama-13b-hf`

<img src="https://github.com/huggingface/huggingface-vscode/raw/master/assets/set-code-llama.png" width="600px">

Read more [here](https://huggingface.co/blog/codellama) about Code LLama.

## Phind and WizardCoder

To test [Phind/Phind-CodeLlama-34B-v2](https://hf.co/Phind/Phind-CodeLlama-34B-v2) and/or [WizardLM/WizardCoder-Python-34B-V1.0](https://hf.co/WizardLM/WizardCoder-Python-34B-V1.0) :
1. Make sure you have the [latest version of this extension](#installing).
2. Make sure you have [supplied HF API token](#hf-api-token)
3. Open Vscode Settings (`cmd+,`) & type: `Hugging Face Code: Config Template`
4. From the dropdown menu, choose `Phind/Phind-CodeLlama-34B-v2` or `WizardLM/WizardCoder-Python-34B-V1.0`

<img src="https://github.com/huggingface/huggingface-vscode/raw/master/assets/set-phind-wizardcoder.png" width="600px">

Read more about Phind-CodeLlama-34B-v2 [here](https://huggingface.co/Phind/Phind-CodeLlama-34B-v2) and WizardCoder-15B-V1.0 [here](https://huggingface.co/WizardLM/WizardCoder-15B-V1.0).
## Community

| Repository | Description |
| --- | --- |
| [huggingface-vscode-endpoint-server](https://github.com/LucienShui/huggingface-vscode-endpoint-server) | Custom code generation endpoint for this repository |
