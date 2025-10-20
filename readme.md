## Self-Host a private AI Assistant using Ollama Open WebUI and Twingate

This will allow you to access your own LLM from up to 5 devices on different networks for free.

### Why would you want to Self-Host your own LLM?

If you're workflow only needs AI to quickly sketch out ideas, assist in small cumbersome problems or structure out your tasks, then you probably won't need the most powerful model on hand.
In that case the cheaper and more private option is to run the LLM yourself.

For help with the selection refer to the table below: 

| Model    | Purpose    | Upsides    | Downsides    |
|---|---|---|---|
| deepseek-r1  | In depth search/structuring    | handles complex tasks and analysis, state of the art model | no control over data, token limits, prices could increase, server outages |
| deepseek-r1 1.5b params, **local**  | quick and simple tasks   | prompts not exposed, no token limit, very little cost | more parameters require more powerful hardware |

### Prerequisites
 * PC or Instance that can run Docker aswell as your desired LLM
 * A Microsoft, Google, Github or LinkedIn Account to use when signing up for Twingate

If you won't sign up for Twingate you'll only have access to your local LLM via your machine.
### Setup

Go to https://ollama.com/ download and install Ollama.

Once it is installed open the terminal/command prompt and check the installation via `ollama --version` (this should show you the version number, ollama needs to be running in the background for this to work.) 

Download your prefered model from Ollama via ollama run "modelname" for example ` ollama run gemma3:1b ` to install the Gemma3 model with 1 billion parameters.

Check that the model is working by prompting it directly in the terminal/command prompt. Exit via /bye. 

![gemma_in_cmd](https://raw.githubusercontent.com/sOnnenen/Self-Host-private-AI-Assitant/refs/heads/main/images/gemma_in_cmd.png)

Next, make sure docker desktop is running, go to your terminal and run the following command:

```bash 
docker run -d -p 3000:8080 --name open-webui --add-host=host.docker.internal:host-gateway -v open-webui:/app/backend/data ghcr.io/open-webui/open-webui:main
```

This will create and start a new container `-d` has the container run in the background `-p` maps port 3000 on the computer to 8080 in the container. `--add-host=` will allow the web-ui in the container to talk to the ollama service `-v` creates a persistent volume, so the chat history is saved. This command will also install Open WebUI if you don't have it already.

You can now sign up in Open WebUI via http://localhost:3000. Note that the account you create here is a local account on your computer.

![open_webui](https://raw.githubusercontent.com/sOnnenen/Self-Host-private-AI-Assitant/refs/heads/main/images/open_webui.png)

To make this local LLM accessible from other devices (phone laptop in other networks) we use Twingate. 
Once signed up, login to Twingate and add a remote Network and select Location `On Premise`.

![add_network_twingate](https://raw.githubusercontent.com/sOnnenen/Self-Host-private-AI-Assitant/refs/heads/main/images/add_network_twingate.PNG)

Click on your Network and select `Deploy Connector` and select `Docker` for Deployment Method.

Then select `generate Tokens` (you might be prompted to authenticate again) and after that copy and run the Docker command Twingate generates for you using your Tokens in your terminal/command prompt.

THe connection is now established and we need to add the LLM as a resource.
To do that click `create Resource` in the Network tab and put in your IP address in your local network. (Run `ipconfig` in your terminal to get the IP address if necessary.)
Add Port Restrictions (3000) to limit access to the Port your LLM sits on.
Click create resource and select `Everyone`.

![add_resource_twingate](https://raw.githubusercontent.com/sOnnenen/Self-Host-private-AI-Assitant/refs/heads/main/images/add_resource_twingate.png)

Using the Twingate App you are now able to access your local LLM from any Network. 

![phone_access](https://raw.githubusercontent.com/sOnnenen/Self-Host-private-AI-Assitant/refs/heads/main/images/phone_access.jpeg)
