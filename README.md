To test rocm57 ....

1.  make sure linux is rocm container (docker) ready  (see AMD rocm doc,  wsl or any sort of Linux VM will NOT work!)
2.  `cd rocm57`  then  `sh ./buildimage.sh`, this will create the `mlcllmrocm57:v0.1` base image; you can do a `docker images` to verify
3.  from top level,  `cd test` and see that the `cache` directory exists and that you have write permissions, this is where all the weights will be downloaded the first time, and will be managed for you
4.  run `sh startrocm57chat.sh gemma-2b-it-q4f16_1`
5.  this should download and convert all the weights (the first time you run it) and then start a chat session with the extra-small gemma model (on your rocm supported GPU)
6.  try the prompt `list the states in the usa` to get a somewhat stable reply 

## Containers for mlc_llm REST API and instant command line interface (CLI) chat with LLM

A set of docker container templates for your scaled production deployment of GPU-accelerated mlc_llms.  Based on the recent work on the SLM, jit flow, and OpenAI compatible APIs including function calling.

These containers are designed to be:

* minimalist - nothing non-essential is included;  you can layer on your own security policy for example
* non-opinionated - use CNCF k8s or docker compose or swarm or whatever you have for orchestration
* adaptive and composable - nobody knows what you intend to do with these containers, and we don't guess
* compatible - with multi-GPU support maturing and batching still in testing, these containers should suvive upcoming changes without needing to be severely revamped. 
* practical NOW - usable and deployable TODAY with 2024/2025 level workstation/consumer hardware and mlc-ai

###  Quick Start

1.  Make sure your system is one of the supported GPU drivers stack (cuda, rocm, and so on) and that you have the toolkit *and* container extension (docker support) already installed and tested
1.  Go to `/rocm/rocm57`  or `/cuda/cuda122` and build the base docker image according to the `README.md` there.
1. Run the command line chat or serve REST APIs immediately by running the scripts in the `/bin` directory, following the `README.md` there.

###  Structure

Base containers are segregated by GPU acceleration stacks.  See the README of the sub-folders for more information
```
cuda
|-- cuda122


rocm
|-- rocm57

bin

test
```

The `bin` folder has the template executables that will start the containers.

The `test` folder contains the tests.

####  Community contribution

This structure enables the greater community to easily contribute new tested templates for other cuda and rocm releases, for example.   

####  Greatly enhanced out-of-box UX

Managing the huge physical size of the weights for an LLM model is a major hurdle when deploying modern LLM in production / experimental environment at any scale.   Couple this with the need to compile NN network support library for every combination and permutation of GPU hardware vs OS supported - and an _impossibly frustrating_ out-of-box user experience is guaranteed.

The latest improvement in JIT and SLM flow for MLC_LLM specifically addresses this.   And these docker container templates further enhances the out-of-box UX, down to one single easy to use command line (with automatic cached LLM weights management). 

Users of such images can simply decide to run "llama2 7b on cuda 12.2" and in one single command immediately pull down an image onto their workstation running AI apps served by Llama 2 already GPU accelerated.    The weights are downloaded directly from huggingface and converted _specifically for her/his GPU hardware and OS_  the first time the command is executed;  any subsequent invocation can start _instantly_ using the already converted weights.

As an example the command to start an interactive chat with this LLM on Cuda 1.22 accelerated Linux is:

```
startcuda122chat.sh Llama-2-7b-chat-hf-q4f32_1
```

One container template is supplied for REST API serving, and another one is available for interactive command line chat with any supported LLMs.


##### Compatibility with future improvements

There is no loss of flexibility in using these containers, the REST API implementation already support batching - the ability to handle multiple concurrent inferences at the same time. And any future improvements in MLC_AI will be 

#### Tests

Tests are made global as they apply to mlc_ai running across any supported GPU configurations. 
 
