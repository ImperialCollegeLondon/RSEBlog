---
date: 2026-09-14
authors:
  - jamesturner246
categories:
  - Technology
tags:
  - LLM
  - HPC
  - Runai
---

# Deploying an LLM Inference Server

For better or worse, the age of the Large Language Model (LLM) has officially arrived. With it comes fresh new opportunities and problems for humanity, such as increased worker productivity alongside massively increased energy demand and job displacement. The widespread adoption of LLMs from just a few key suppliers brings other issues, such as vendor dependence and data security concerns. Besides this, private research and development of these models continues at a blistering pace. Because of the needs of researchers and private vendors, and perhaps also the other issues, a new trend has emerged whereby private instances of LLMs are deployed locally on personally owned or rented infrastructure. New software stacks, such as the open source vLLM, or Nvidia’s proprietary NIM, have appeared to fill the niche of LLM deployment and administration.

In this article, we explore the process of private LLM deployment using Nvidia Run:ai on Imperial College London’s new HX3 cluster for AI computing. We begin with a short overview of the HX3 cluster and its usage, followed by a quick look over the software stack we will be utilising for our deployment, including its web and command line interfaces. Finally, we get down to business and deploy our own model onto the HX3 cluster and play around with it a little.

## Imperial HX3

HX3 is Imperial College London’s brand new HPC cluster dedicated entirely to GPU-accelerated compute workloads for AI teaching and research. This guide is mainly targeted at students and staff members at Imperial; If you are not an Imperial student or staff member, then try contacting your own institution’s HPC department; you may have access to equivalent infrastructure with which to follow along with this guide.

### Accessing the HX3 Compute Cluster

You can request access to this compute environment by reaching out to your department’s HPC team. Depending on your requirements, you may be allocated a personal 'project' workspace, with which you can submit compute workloads using your personal quota. Otherwise, you may instead be added to a department-specific project with its own compute quota which is shared with others in your department.

Once appropriate access has been granted, you may proceed to the web interface of HX3, used to submit and manage workloads. For Imperial users, the web interface is accessible by pointing your web browser to runai.hx3.hpc.ic.ac.uk. NB you will need to either be connected directly to Imperial’s local network or connected via the Zscaler proxy service to access the interface.

### The Run:ai Interface

HX3 uses Nvidia’s Run:ai platform to submit and manage AI compute workloads. Run:ai has two interfaces: the web interface discussed already, which you can connect to by following the link above, and the Command Line Interface (CLI), which we will be using throughout the bulk of this guide. Before that, though, take a second to familiarise yourself with the web interface. From the landing page, you will see a page describing your submitted workloads, along with controls for starting, managing and deleting them. Deleted workloads may be accessed and restarted by following the 'Deleted workloads' on the navigation panel on the left. User settings, including API token generation, is accessed by clicking on your profile avatar in the top right of the page.

Since we will be using the CLI client, click the question mark icon in the top right of the page, near to your avatar icon, and click 'Researcher Command Line Interface'. Follow the instructions on the popup window to download and install your personal CLI client, which will be made available as `runai` on the command line. Now open a fresh terminal so that Run:ai can properly initialise, and then login with the client by running `runai login`. If successful, you now have a fully working CLI client to submit jobs. Now as a final configuration step, make sure to point the client to whichever compute project you were assigned to by running `runai project set <project_name>`. You can check the name of your project by running `runai project list`. Now we can move on to something more interesting.

## Deploying the LLM

Run:ai workloads can be classified as one of three things: either 'Workspace', 'Training' or 'Inference'. Workspaces are useful as spaces for developing and running Jupyter notebooks in an isolated container environment. The training workload, as you might expect, is a specialised environment from which AI models can be trained in a controlled and repeatable manner. Since the LLM models we will be using have already been fully trained, we will be using the inference workload type, used for deploying fully trained AI models and exposing them to users via a web API.

Let’s begin by listing our submitted workloads with `runai workload list`. If you are part of a department-wide project group, you may see several workloads submitted by others in your group, along with their status. If you have been allocated a fresh project group, the returned list will be empty as we have not submitted anything yet. Let’s fix that. Decide a name for the workload and set the environment variable `WORKLOAD` with it. I’ll use `export WORKLOAD=my-test-llm`, but you may need to pick another one if this one is already taken. Our next command is as follows:

```bash
runai inference submit ${WORKLOAD} \
   -i vllm/vllm-openai:latest \
   --gpu-devices-request 1 \
   --serving-port 8000 \
   -- Qwen/Qwen3-0.6B
```

We can follow the startup process of the model by running `runai inference logs ${WORKLOAD} --follow` and wait until the container is fully deployed and awaiting requests.

It’s worth unpacking the previous command a bit, as several things are happening under the hood at once. First, let’s discuss the command itself. The first part: `runai inference submit` tells the Run:ai platform that we want to submit a request for an 'inference' type workload, as discussed above. Next, we give the workload name, which we have already saved in our `WORKLOAD` environment variable. This will be the name visible when running `runai workload list`. Next up is `-i vllm/vllm-openai:latest`. This instructs Run:ai to load the vLLM and OpenAI container, which is a pre-built environment containing the inference server itself, which is vLLM with an OpenAI API layer. We will discuss the API later. Following this, `--gpu-devices-request 1` requests a single GPU for the workload, and `--serving-port 8000` exposes the workload’s vLLM inference server on port 8000. Finally, `Qwen/Qwen3-0.6B` is the huggingface.co identifier for the model we are using.

NB there are some good reasons you might want to request a different number of GPUs. You may have noticed when calling `runai project list` that the returned table has a column named 'Allocated GPUs'. Depending on your quota, two other scenarios may exist: your GPU quota may be less than, or greater than one. Additionally, if your project is shared with other users, you may find that there is not enough quota left after other workloads for your job to request a whole GPU. If there is not enough quota for your job, you may request 'fractional' GPU usage, where some fraction of a non-full GPU is requested instead. Although only proportionate GPU memory is reserved, it’s a viable option when not enough GPU quota exists. For a fractional half GPU request, for example, replace `--gpu-devices-request 1` in your `submit` call with `--gpu-request-type portion --gpu-portion-request 0.5`, and your workload will run on half of a shared GPU. On the other hand, if your remaining GPU quota is above one, you might consider requesting more to run a bigger model faster. This, however, is outside the scope of this guide.

By now, our LLM should be up and running on a HX3 node, as evident in the workload’s log. For testing purposes, we will forward the remote port (8000) from the HX3 node to localhost, allowing us to transmit HTTP requests through a tunnel to our vLLM inference server. To do so, run the following in a separate command line terminal: `runai inference port-forward ${WORKLOAD} --port 8000:8000`. We may now pass our HTTP requests to the vLLM server via localhost port 8000. With that, we can now move on to passing some queries.

## Sending Requests to the LLM

Let’s start simple. We will use the command line program `curl` to send HTTP requests to our inference server's API. Run the following command in your terminal:

```bash
curl http://localhost:8000/v1/chat/completions \
    -H 'Content-Type: application/json' \
    -d '{
        "model": "Qwen/Qwen3-0.6B",
        "messages": [
            {
                "role": "user",
                "content": "In one sentence, what is Imperial College London?"
            }
        ],
        "max_tokens": 50
    }'
```

You’ll see that the request is structured as JSON, with fields compatible with the OpenAI API format, which is quite standard in LLM inference. We first select the model we set up earlier, and then construct our query message’s content, using the user role. Note also our `max_tokens` choice of 100. The response you receive might look a little like this:

```text
"role": "assistant",
"content": "<think>\nOkay, the user is asking for a one-sentence definition of Imperial College London. Let me start by recalling what I know about the university.\n\nImperial College London is a prestigious institution located in London. It's known for its research and",
```

As you can see, there was a slight issue with the response. It seems the `Qwen/Qwen3-0.6B` model we selected has a 'think' ability, but this 'thinking' is taking much of our token allowance. We have a couple of options. The first is to disable this thinking behaviour by passing another option flag with our JSON query, like so:

```bash
curl http://localhost:8000/v1/chat/completions \
    -H 'Content-Type: application/json' \
    -d '{
        "model": "Qwen/Qwen3-0.6B",
        "messages": [
            {
                "role": "user",
                "content": "In one sentence, what is Imperial College London?"
            }
        ],
        "max_tokens": 50,
        "chat_template_kwargs": {
            "enable_thinking": false
        }
    }'
```

Here’s the response I got:

```text
"role": "assistant",
"content": "Imperial College London is a world-renowned British university located in London, providing a wide range of academic and professional opportunities.",
```

That’s more like it! It’s a little short and basic, though. Let’s turn thinking back on and increase the token quota:

```bash
curl http://localhost:8000/v1/chat/completions \
    -H 'Content-Type: application/json' \
    -d '{
        "model": "Qwen/Qwen3-0.6B",
        "messages": [
            {
                "role": "user",
                "content": "In one sentence, what is Imperial College London?"
            }
        ],
        "max_tokens": 1000
    }'
```

The response I get now is:

```text
"role": "assistant",
"content": "<think>\nOkay, the user is asking for a one-sentence definition of Imperial College London. Let me start by recalling what I know about the institution. Imperial College London is a prestigious university located in London, UK, known for its strong academic programs and research. I need to make sure the sentence is concise and includes key details like location, type of institution, and notable aspects.\n\nFirst, I should mention the university's name. Then, its location. Next, the type of institution. Also, the reputation or achievements. Let me check if there's any specific information I should include. For example, the fact that it's a public institution or its focus areas. Maybe include something about its reputation in the field of education or research.\n\nWait, the user wants it in one sentence. So I need to combine these elements without adding unnecessary words. Let me structure it: \"Imperial College London is a prestigious UK university located in London, offering a wide range of academic programs and research opportunities.\" That seems to cover location, type, and purpose. Let me double-check if there's any other important detail that's missing. No, that should be sufficient. I think that's the correct answer.\n</think>\n\nImperial College London is a prestigious UK university located in London, offering a wide range of academic programs and research opportunities.",
```

Interesting... We can see that this model in particular uses many of its allocated tokens on the 'thinking' phase, before starting to write its final response. Clearly some experimentation is called for! There is a huge selection of models to try from Hugging Face and other AI container sources we can try. The `Qwen/Qwen3-14B` model is a Slightly bigger and more capable version of the model we are using now, or alternatively there are many more to choose from... Before we do that, though, let’s look at a nifty little way to use our LLM: agentic coding in our IDE of choice.

## Using the LLM for Agentic Coding

For the purposes of checking agentic coding out using a private LLM deployment, let’s make use of the 'Visual Studio Code' IDE, which is freely available on most operating systems, and already supports using custom LLM servers straight out of the box.

TODO: agentic coding with vscode

## Final Notes

Thanks for staying with us until the end! You should hopefully feel a little more confident about configuring and deploying your own instances of LLM models on HX3 and trying some new models. Before you leave us, though, just a few extra points that should be kept in mind whilst experimenting. First, please be a good citizen! Shut down your LLMs once you are done with them to free up resources for someone else. Second, if doing anything more than testing a model for a short while, you are strongly advised to add token-based authentication to your model, to prevent others from accessing and abusing it. Reach out to your department’s HPC representatives if you are unsure whether you need it or require assistance in setting it up. Finally, if you are deploying the same model over and over, consider setting up persistent caching to prevent Run:ai from downloading and compiling the same model over and over, saving bandwidth and speeding up deployment significantly. And now, with that done, go and have some fun!
