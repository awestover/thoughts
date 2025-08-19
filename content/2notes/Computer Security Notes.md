Inference server: 
- Has an API which is how you (usually) run inference on the AI. 
- This API will have monitoring and stuff. 
- Note: this def contains model weights

Scaffold: 
- It's just a python loop that can make calls to inference API, or to execute jobs on execution server.

Execution server:
- Where the AI does stuff. 
- Or actually: scaffold adds a task to the slurm task queue. 

Lab internal tools
- internal github
- slack

Internet:
- Connected to Execution server (and scaffold say)

**Some problems:**
- An employee, or the AI can post your algorithmic secrets on the internet.
- Upload weights to internet?
- Commit compromised code to internal github

RID's:
- Rogue scaffold on the execution server (if it has weights) and use those weights for inference.
- If you don't have the weights you can just talk to the inference server from the execution server.
	- But probably we can catch this.
- Compromise the inference server (via committing compromised code to the internal git repo)
