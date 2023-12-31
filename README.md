# ConvoGen
A framework for generating synthetic conversational data from seed data as small as a single message.

## Usage

### Installation

Follow the steps to [install miniconda.](https://docs.conda.io/projects/miniconda/en/latest/miniconda-install.html) ( assuming you don't have it already 😉 )

To create a new environment
```bash
$ conda create -n <some-cool-name> python=3.10
```

Change your active env to the one you just created.
```bash
$ conda activate <some-cool-name>
```

Navigate to the folder you downloaded this repo to.
```bash
$ cd <arbitrary-path-on-your-computer>/convogen
```

Now install a small set of libraries you will need using `pip`.
```bash
$ pip install -r requirements.txt
```

Sit back this will take a while.....

## Data download

Create a Twitter account and sign-up on the [Developer Portal](https://developer.twitter.com/en/portal/dashboard).

Use the notebook `./notebooks/scraping.ipynb` to scrape tweets based on the `tweet_ids`.

## Model-Checkpoint Download
We use [this implementation](meta-llama/Llama-2-70b-chat) of MetaAI's Llama v2.

To use this model you will need to obtain licensing from MetaAI as detailed [here](https://ai.meta.com/resources/models-and-libraries/llama-downloads/). Fill out the form and submit it, the nice folks at Meta will accept you instantly.

Having said that, the hardware requirements for the model are significant. I ran it on an 80 GB Nvidia A100 GPU. Using lower GPUs would mean a difference in execution time.

To log-in to the HuggingfaceHub using the `huggingface-cli` follow [these steps](https://huggingface.co/docs/huggingface_hub/quick-start).

Feel free to explore smaller models like [MistralAI's 7B](https://huggingface.co/mistralai/Mistral-7B-Instruct-v0.2) model which is 1/10th the size (and no sign-up needed). However, I must warn you about the significant degradation of the generated conversational quality.

You are now good to run any of the existing experiments.

## Repo structure

Save your downloaded data in the `./datasets` sub-folder.

All prompts can be found in the `/datasets/inst-templates\<experiment-subfolder>`

The User `dispositions` and `motivations` can be found under `/datasets/inst-templates/<experiment-subfolder>/disposition` and `/datasets/inst-templates/<experiment-subfolder>/motivations` respectively.

Two main notebooks in `./notebooks`:
  1. `generate-conversations.ipynb`: Generates conversations based on a given seed `tweet`, a user `motivation`, and a user `disposition`.
      - Generated conversations stored under `/datasets/inst-templates/<experiment-subfolder>/conversations`
  3. `restyle-conversations.ipynb`: Rephrases the generated conversation based on the twitter discourse style.
      - Restyled conversations stored under `/datasets/inst-templates/<experiment-subfolder>/restyled-conversations`

All conversations are stored in a flat `json`. You can read it in programmatically with the `json` library.
```python
import json
import pathlib

PATH_TO_CONV = pathlib.Path("some-path/some-convo.json")
conv_json = json.loads(PATH_TO_CONV.read_text())

conversation = conv_json['conv'] # Only works if your `json` was generated by the generation notebook.
restyled_conversation = conv_json['restyled_conv'] # Only works if your `json` was generated by the restyling notebook.
```


## Set up new experiments

Create a new folder under `./datasets/inst-templates`, for instance, `new-expt`.
```bash
$ mkdir ./datasets/inst-templates/new-expt
```

Copy from the existing experiments everything that you want to keep the same.  Configure the `expt-setup.json` based on what you want to do.

Then alter `expt_name` in the `generate-conversations.ipynb` and `restyle-conversations.ipynb` to `new-expt` (or whatever else you call your experiment).

Good to go 😼.
----

