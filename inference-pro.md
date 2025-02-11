---
title: Inference for PROs
thumbnail: /blog/assets/inference_pro/thumbnail.png
authors:
  - user: osanseviero
  - user: pcuenq
  - user: victor
---

# Inference for PROs


![Inference for PROs image](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/inference-for-pros/Inference-for-pros.png)

Today, we're introducing Inference for PRO users - a community offering that gives you access to APIs of curated endpoints for some of the most exciting models available, as well as improved rate limits for the usage of free Inference API. Use the following page to [subscribe to PRO](https://huggingface.co/subscribe/pro). 

Hugging Face PRO users now have access to exclusive API endpoints for a curated list of powerful models that benefit from ultra-fast inference powered by [text-generation-inference](https://github.com/huggingface/text-generation-inference). This is a benefit on top of the free inference API, which is available to all Hugging Face users to facilitate testing and prototyping on 200,000+ models. PRO users enjoy higher rate limits on these models, as well as exclusive access to some of the best models available today.

## Contents

- [Supported Models](#supported-models)
- [Getting started with Inference for PROs](#getting-started-with-inference-for-pros)
- [Applications](#applications)
  - [Chat with Llama 2 and Code Llama 34B](#chat-with-llama-2-and-code-llama-34b)
  - [Chat with Code Llama 70B](#chat-with-code-llama-70b)
  - [Code infilling with Code Llama](#code-infilling-with-code-llama)
  - [Stable Diffusion XL](#stable-diffusion-xl)
- [Messages API](#messages-api)
- [Generation Parameters](#generation-parameters)
  - [Controlling Text Generation](#controlling-text-generation)
  - [Controlling Image Generation](#controlling-image-generation)
  - [Caching](#caching)
  - [Streaming](#streaming)
- [Subscribe to PRO](#subscribe-to-pro)
- [FAQ](#faq)

## Supported Models

In addition to thousands of public models available in the Hub, PRO users get free access and higher rate limits to the following state-of-the-art models:

| Model                          | Size                                                                                                                                                                                       | Context Length | Use                                                          |
|--------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------|--------------------------------------------------------------|
| Meta Llama 3.1 405B Instruct FP8 | [405B](https://huggingface.co/meta-llama/Meta-Llama-3.1-405B-Instruct-FP8)                                                       | 128k tokens      | High quality multilingual chat model with large context length |
| Meta Llama 3 Instruct          | [8B](https://huggingface.co/meta-llama/Meta-Llama-3-8B-Instruct), [70B](https://huggingface.co/meta-llama/Meta-Llama-3-70B-Instruct)                                                       | 8k tokens      | One of the best chat models                                  |
| Mixtral 8x7B Instruct          | [45B MOE](https://huggingface.co/mistralai/Mixtral-8x7B-Instruct-v0.1)                                                                                                                     | 32k tokens     | Performance comparable to top proprietary models             |
| Nous Hermes 2 Mixtral 8x7B DPO | [45B MOE](https://huggingface.co/NousResearch/Nous-Hermes-2-Mixtral-8x7B-DPO)                                                                                                              | 32k tokens     | Further trained over Mixtral 8x7B MoE                        |
| Zephyr ORPO 141B A35B          | [141B MOE](https://huggingface.co/HuggingFaceH4/zephyr-orpo-141b-A35b-v0.1)                                                                                                                | 65k tokens     | A high-quality conversational model with high context length |
| Zephyr 7B β                    | [7B](https://huggingface.co/HuggingFaceH4/zephyr-7b-beta)                                                                                                                                  | 4k tokens      | One of the best chat models at the 7B weight                 |
| Llama 2 Chat                   | [7B](https://huggingface.co/meta-llama/Llama-2-7b-chat-hf), [13B](https://huggingface.co/meta-llama/Llama-2-13b-chat-hf) | 4k tokens      | One of the best conversational models                        |
| Mistral 7B Instruct v0.2       | [7B](https://huggingface.co/mistralai/Mistral-7B-Instruct-v0.2)                                                                                                                            | 4k tokens      | One of the best chat models at the 7B weight                 |
| Code Llama Base                | [7B](https://huggingface.co/codellama/CodeLlama-7b-hf) and [13B](https://huggingface.co/codellama/CodeLlama-13b-hf)                                                                        | 4k tokens      | Autocomplete and infill code                                 |
| Code Llama Instruct            | [34B](https://huggingface.co/codellama/CodeLlama-34b-Instruct-hf)                                                                                                                          | 16k tokens     | Conversational code assistant                                |
| Stable Diffusion XL            | [3B UNet](https://huggingface.co/stabilityai/stable-diffusion-xl-base-1.0)                                                                                                                 | -              | Generate images                                              |
| Bark                           | [0.9B](https://huggingface.co/suno/bark)                                                                                                                                                   | -              | Text to audio generation                                     |

Inference for PROs makes it easy to experiment and prototype with new models without having to deploy them on your own infrastructure. It gives PRO users access to ready-to-use HTTP endpoints for all the models listed above. It’s not meant to be used for heavy production applications - for that, we recommend using [Inference Endpoints](https://ui.endpoints.huggingface.co/catalog). Inference for PROs also allows using applications that depend upon an LLM endpoint, such as using a [VS Code extension](https://marketplace.visualstudio.com/items?itemName=HuggingFace.huggingface-vscode) for code completion, or have your own version of [Hugging Chat](http://hf.co/chat).

## Getting started with Inference For PROs

Using Inference for PROs is as simple as sending a POST request to the API endpoint for the model you want to run. You'll also need to get a PRO account authentication token from [your token settings page](https://huggingface.co/settings/tokens) and use it in the request. For example, to generate text using [Meta Llama 3 8B Instruct](https://huggingface.co/meta-llama/Meta-Llama-3-8B-Instruct) in a terminal session, you'd do something like:

```bash
curl https://api-inference.huggingface.co/models/meta-llama/Meta-Llama-3-8b-Instruct \
    -X POST \
    -d '{"inputs": "In a surprising turn of events, "}' \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer <YOUR_TOKEN>"
```

Which would print something like this:

```json
[
  {
    "generated_text": "In a surprising turn of events, 2021 has brought us not one, but TWO seasons of our beloved TV show, \"Stranger Things.\""
  }
]
```

You can also use many of the familiar transformers generation parameters, like `temperature` or `max_new_tokens`:

```bash
curl https://api-inference.huggingface.co/models/meta-llama/Meta-Llama-3-8b-Instruct \
    -X POST \
    -d '{"inputs": "In a surprising turn of events, ", "parameters": {"temperature": 0.7, "max_new_tokens": 100}}' \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer <YOUR_TOKEN>"
```

For more details on the generation parameters, please take a look at [_Controlling Text Generation_](#controlling-text-generation) below.

To send your requests in Python, you can take advantage of `InferenceClient`, a convenient utility available in the `huggingface_hub` Python library:

```bash
pip install huggingface_hub
```

`InferenceClient` is a helpful wrapper that allows you to make calls to the Inference API and Inference Endpoints easily:

```python
from huggingface_hub import InferenceClient

client = InferenceClient(model="meta-llama/Meta-Llama-3-8b-Instruct", token=YOUR_TOKEN)

output = client.text_generation("Can you please let us know more details about your ")
print(output)
```

If you don't want to pass the token explicitly every time you instantiate the client, you can use `notebook_login()` (in Jupyter notebooks), `huggingface-cli login` (in the terminal), or `login(token=YOUR_TOKEN)` (everywhere else) to log in a single time. The token will then be automatically used from here.

In addition to Python, you can also use JavaScript to integrate inference calls inside your JS or node apps. Take a look at [huggingface.js](https://huggingface.co/docs/huggingface.js/index) to get started!

## Applications

### Chat with Llama 2 and Code Llama 34B

Models prepared to follow chat conversations are trained with very particular and specific chat templates that depend on the model used. You need to be careful about the format the model expects and replicate it in your queries.

The following example was taken from [our Llama 2 blog post](https://huggingface.co/blog/llama2#how-to-prompt-llama-2), that describes in full detail how to query the model for conversation:

```Python
prompt = """<s>[INST] <<SYS>>
You are a helpful, respectful and honest assistant. Always answer as helpfully as possible, while being safe.  Your answers should not include any harmful, unethical, racist, sexist, toxic, dangerous, or illegal content. Please ensure that your responses are socially unbiased and positive in nature.

If a question does not make any sense, or is not factually coherent, explain why instead of answering something not correct. If you don't know the answer to a question, please don't share false information.
<</SYS>>

There's a llama in my garden 😱 What should I do? [/INST]
"""

client = InferenceClient(model="codellama/CodeLlama-13b-hf", token=YOUR_TOKEN)
response = client.text_generation(prompt, max_new_tokens=200)
print(response)
```

This example shows the structure of the first message in a multi-turn conversation. Note how the `<<SYS>>` delimiter is used to provide the _system prompt_, which tells the model how we expect it to behave. Then our query is inserted between `[INST]` delimiters.

If we wish to continue the conversation, we have to append the model response to the sequence, and issue a new followup instruction afterwards. This is the general structure of the prompt template we need to use for Llama 2:

```
<s>[INST] <<SYS>>
{{ system_prompt }}
<</SYS>>

{{ user_msg_1 }} [/INST] {{ model_answer_1 }} </s><s>[INST] {{ user_msg_2 }} [/INST]
```

This same format can be used with Code Llama Instruct to engage in technical conversations with a code-savvy assistant!

Please, refer to [our Llama 2 blog post](https://huggingface.co/blog/llama2#how-to-prompt-llama-2) for more details.

### Code infilling with Code Llama

Code models like Code Llama can be used for code completion using the same generation strategy we used in the previous examples: you provide a starting string that may contain code or comments, and the model will try to continue the sequence with plausible content. Code models can also be used for _infilling_, a more specialized task where you provide prefix and suffix sequences, and the model will predict what should go in between. This is great for applications such as IDE extensions. Let's see an example using Code Llama:

```Python
client = InferenceClient(model="codellama/CodeLlama-13b-hf", token=YOUR_TOKEN)

prompt_prefix = 'def remove_non_ascii(s: str) -> str:\n    """ '
prompt_suffix = "\n    return result"

prompt = f"<PRE> {prompt_prefix} <SUF>{prompt_suffix} <MID>"

infilled = client.text_generation(prompt, max_new_tokens=150)
infilled = infilled.rstrip(" <EOT>")
print(f"{prompt_prefix}{infilled}{prompt_suffix}")
```

```
def remove_non_ascii(s: str) -> str:
    """ Remove non-ASCII characters from a string.

    Args:
        s (str): The string to remove non-ASCII characters from.

    Returns:
        str: The string with non-ASCII characters removed.
    """
    result = ""
    for c in s:
        if ord(c) < 128:
            result += c
    return result
```

As you can see, the format used for infilling follows this pattern:

```
prompt = f"<PRE> {prompt_prefix} <SUF>{prompt_suffix} <MID>"
```

For more details on how this task works, please take a look at https://huggingface.co/blog/codellama#code-completion.

### Stable Diffusion XL

SDXL is also available for PRO users. The response returned by the endpoint consists of a byte stream representing the generated image. If you use `InferenceClient`, it will automatically decode to a `PIL` image for you:

```Python
sdxl = InferenceClient(model="stabilityai/stable-diffusion-xl-base-1.0", token=YOUR_TOKEN)
image = sdxl.text_to_image(
    "Dark gothic city in a misty night, lit by street lamps. A man in a cape is walking away from us",
    guidance_scale=9,
)
```

![SDXL example generation](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/inference-for-pros/sdxl-example.png)

For more details on how to control generation, please take a look at [this section](#controlling-image-generation).

## Messages API

All text generation models now support the Messages API, so they are compatible with OpenAI client libraries, including LangChain and LlamaIndex. The following snippet shows how to use the official `openai` client library with Llama 3.1 405B Instruct (FP8):

```py
from openai import OpenAI
import huggingface_hub

# Initialize the client, pointing it to one of the available models
client = OpenAI(
    base_url="https://api-inference.huggingface.co/v1/",
    api_key=huggingface_hub.get_token(),
)
chat_completion = client.chat.completions.create(
    model="meta-llama/Meta-Llama-3.1-405B-Instruct-FP8",
    messages=[
        {"role": "system", "content": "You are a helpful an honest programming assistant."},
        {"role": "user", "content": "Is Rust better than Python?"},
    ],
    stream=True,
    max_tokens=500
)

# iterate and print stream
for message in chat_completion:
    print(message.choices[0].delta.content, end="")
```

For more details about the use of the Messages API, please [check this post](https://huggingface.co/blog/tgi-messages-api).

## Generation Parameters

### Controlling Text Generation

Text generation is a rich topic, and there exist several generation strategies for different purposes. We recommend [this excellent overview](https://huggingface.co/blog/how-to-generate) on the subject. Many generation algorithms are supported by the text generation endpoints, and they can be configured using the following parameters:

- `do_sample`: If set to `False` (the default), the generation method will be _greedy search_, which selects the most probable continuation sequence after the prompt you provide. Greedy search is deterministic, so the same results will always be returned from the same input. When `do_sample` is `True`, tokens will be sampled from a probability distribution and will therefore vary across invocations.
- `temperature`: Controls the amount of variation we desire from the generation. A temperature of `0` is equivalent to greedy search. If we set a value for `temperature`, then `do_sample` will automatically be enabled. The same thing happens for `top_k` and `top_p`. When doing code-related tasks, we want less variability and hence recommend a low `temperature`. For other tasks, such as open-ended text generation, we recommend a higher one.
- `top_k`. Enables "Top-K" sampling: the model will choose from the `K` most probable tokens that may occur after the input sequence. Typical values are between 10 to 50.
- `top_p`. Enables "nucleus sampling": the model will choose from as many tokens as necessary to cover a particular probability mass. If `top_p` is 0.9, the 90% most probable tokens will be considered for sampling, and the trailing 10% will be ignored.
- `repetition_penalty`: Tries to avoid repeated words in the generated sequence.
- `seed`: Random seed that you can use in combination with sampling, for reproducibility purposes.

In addition to the sampling parameters above, you can also control general aspects of the generation with the following:

- `max_new_tokens`: maximum number of new tokens to generate. The default is `20`, feel free to increase if you want longer sequences.
- `return_full_text`: whether to include the input sequence in the output returned by the endpoint. The default used by `InferenceClient` is `False`, but the endpoint itself uses `True` by default.
- `stop_sequences`: a list of sequences that will cause generation to stop when encountered in the output.

### Controlling Image Generation

If you want finer-grained control over images generated with the SDXL endpoint, you can use the following parameters:

- `negative_prompt`: A text describing content that you want the model to steer _away_ from.
- `guidance_scale`: How closely you want the model to match the prompt. Lower numbers are less accurate, very high numbers might decrease image quality or generate artifacts.
- `width` and `height`: The desired image dimensions. SDXL works best for sizes between 768 and 1024.
- `num_inference_steps`: The number of denoising steps to run. Larger numbers may produce better quality but will be slower. Typical values are between 20 and 50 steps.

For additional details on text-to-image generation, we recommend you check the [diffusers library documentation](https://huggingface.co/docs/diffusers/using-diffusers/sdxl).

### Caching

If you run the same generation multiple times, you’ll see that the result returned by the API is the same (even if you are using sampling instead of greedy decoding). This is because recent results are cached. To force a different response each time, we can use an HTTP header to tell the server to run a new generation each time: `x-use-cache: 0`.

If you are using `InferenceClient`, you can simply append it to the `headers` client property:

```Python
client = InferenceClient(model="meta-llama/Meta-Llama-3-8b-Instruct", token=YOUR_TOKEN)
client.headers["x-use-cache"] = "0"

output = client.text_generation("In a surprising turn of events, ", do_sample=True)
print(output)
```

### Streaming

Token streaming is the mode in which the server returns the tokens one by one as the model generates them. This enables showing progressive generations to the user rather than waiting for the whole generation. Streaming is an essential aspect of the end-user experience as it reduces latency, one of the most critical aspects of a smooth experience.

<div class="flex justify-center">
    <img 
        class="block dark:hidden" 
        src="https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/tgi/streaming-generation-visual_360.gif"
    />
    <img 
        class="hidden dark:block" 
        src="https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/tgi/streaming-generation-visual-dark_360.gif"
    />
</div>

To stream tokens with `InferenceClient`, simply pass `stream=True` and iterate over the response.

```python
for token in client.text_generation("How do you make cheese?", max_new_tokens=12, stream=True):
    print(token)

# To
# make
# cheese
#,
# you
# need
# to
# start
# with
# milk
```

To use the generate_stream endpoint with curl, you can add the `-N`/`--no-buffer` flag, which disables curl default buffering and shows data as it arrives from the server.

```
curl -N https://api-inference.huggingface.co/models/meta-llama/Meta-Llama-3-8b-Instruct \
    -X POST \
    -d '{"inputs": "In a surprising turn of events, ", "parameters": {"temperature": 0.7, "max_new_tokens": 100}}' \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer <YOUR_TOKEN>"
```

## Subscribe to PRO

You can sign up today for a PRO subscription [here](https://huggingface.co/subscribe/pro). Benefit from higher rate limits, custom accelerated endpoints for the latest models, and early access to features. If you've built some exciting projects with the Inference API or are looking for a model not available in Inference for PROs, please [use this discussion](https://huggingface.co/spaces/huggingface/HuggingDiscussions/discussions/13). [Enterprise users](https://huggingface.co/enterprise) also benefit from PRO Inference API on top of other features, such as SSO.

## FAQ

**Does this affect the free Inference API?**

No. We still expose thousands of models through free APIs that allow people to prototype and explore model capabilities quickly.

**Does this affect Enterprise users?**

Users with an Enterprise subscription also benefit from accelerated inference API for curated models.

**Can I use my own models with PRO Inference API?**

The free Inference API already supports a wide range of small and medium models from a variety of libraries (such as diffusers, transformers, and sentence transformers). If you have a custom model or custom inference logic, we recommend using [Inference Endpoints](https://ui.endpoints.huggingface.co/catalog).
