# Using Kubernetes
Presenting an example of getting setup with Kubernetes.

There are many detailed tutorials out there on doing this, and I'll be linking to resources I found especially useful. 

What I hope to contribute here is to fill in some of the gaps that I encounter as I grow my understanding and usage of Kubernetes.

# Installation
Kubernetes is for deploying and managing apps in a cluster. For local development and learning, `Minikube` is a way to run this cluster locally.
Once we've got the "cluster" available locally, then we can use `kubectl` and other resources to interact with the cluster.

I followed this well structured 
[guide](https://computingforgeeks.com/how-to-install-minikube-on-ubuntu-debian-linux/) 
to get Minikube setup on my Ubuntu 20.04 laptop.
The only thing that I had to do differently was after running `sudo apt install virtualbox virtualbox-ext-pack` I got a message saying I needed to install additional packages. 
I did that via
```
$ sudo apt install linux-headers-generic virtualbox-dkms 
```
and proceeded fine from there. Last thing was installing `kubectl` via the above guide.

# Configuration
Once installed, I setup the `kubectl` auto-completion to make it easier to work with this command line tool. 
First I verified I had `bash-completion` installed. This command will install it if missing:
```
$ sudo apt install bash-completion
```
Then running
```
$ kubectl completion -h
```
will spit out a bunch of information, of which the relevant bits for setting up in `bash` are:
```
  # Installing bash completion on Linux
  ## If bash-completion is not installed on Linux, please install the
'bash-completion' package
  ## via your distribution's package manager.
  ## Load the kubectl completion code for bash into the current shell
  source <(kubectl completion bash)
  ## Write bash completion code to a file and source if from .bash_profile
  kubectl completion bash > ~/.kube/completion.bash.inc
  printf "
  # Kubectl shell completion
  source '$HOME/.kube/completion.bash.inc'
  " >> $HOME/.bash_profile
  source $HOME/.bash_profile
```
I followed these instructions, and ran
```
$ source <(kubectl completion bash)

$ kubectl completion bash > ~/.kube/completion.bash.inc

$   printf "
→   # Kubectl shell completion
→   source '$HOME/.kube/completion.bash.inc'
→   " >> $HOME/.bash_profile
```
The 1st command includes the completions in the current session I'm using. 
The 2nd command adds the necessary commands to the `.kube/completion.bash.inc` file, and the last command adds to your `~/.bash_profile` (which is sourced on login) a reference to that new file.

# Try it out
With everything installed and configured, we can give it a test run. 

First, start the minikube "cluster". Note your output may differ a bit, as I've already started it once before.
```
$ minikube start
😄  minikube v1.14.2 on Ubuntu 20.04
✨  Using the docker driver based on existing profile
👍  Starting control plane node minikube in cluster minikube
🔄  Restarting existing docker container for "minikube" ...
🐳  Preparing Kubernetes v1.19.2 on Docker 19.03.8 ...
🔎  Verifying Kubernetes components...
🌟  Enabled addons: default-storageclass, storage-provisioner, dashboard
🏄  Done! kubectl is now configured to use "minikube" by default
```

With the cluster running we can get basic info via the `kubectl` command
```
$ kubectl version -o json
{
  "clientVersion": {
    "major": "1",
    "minor": "19",
    "gitVersion": "v1.19.3",
    "gitCommit": "1e11e4a2108024935ecfcb2912226cedeafd99df",
    "gitTreeState": "clean",
    "buildDate": "2020-10-14T12:50:19Z",
    "goVersion": "go1.15.2",
    "compiler": "gc",
    "platform": "linux/amd64"
  },
  "serverVersion": {
    "major": "1",
    "minor": "19",
    "gitVersion": "v1.19.2",
    "gitCommit": "f5743093fd1c663cb0cbc89748f730662345d44d",
    "gitTreeState": "clean",
    "buildDate": "2020-09-16T13:32:58Z",
    "goVersion": "go1.15",
    "compiler": "gc",
    "platform": "linux/amd64"
  }
}
```

The dashboard included in `minikube` can be handy for seeing your cluster.
However, at this time (Nov/2020) it is recommended to use the `kubectl` to interact with your cluster.
```
$ minikube addons list
|-----------------------------|----------|--------------|
|         ADDON NAME          | PROFILE  |    STATUS    |
|-----------------------------|----------|--------------|
| ambassador                  | minikube | disabled     |
| csi-hostpath-driver         | minikube | disabled     |
| dashboard                   | minikube | disabled     |
| default-storageclass        | minikube | enabled ✅   |
| efk                         | minikube | disabled     |
| freshpod                    | minikube | disabled     |
| gcp-auth                    | minikube | disabled     |
| gvisor                      | minikube | disabled     |
| helm-tiller                 | minikube | disabled     |
...
```
For this version of minikube, the dashboard is disabled. We can enable it and the access it via
```
$ minikube addons enable dashboard
 💡  Some dashboard features require the metrics-server addon. To enable all features please run:

	minikube addons enable metrics-server	

🌟  The 'dashboard' addon is enabled

$ minikube dashboard
🤔  Verifying dashboard health ...
🚀  Launching proxy ...
🤔  Verifying proxy health ...
🎉  Opening http://127.0.0.1:42099/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/ in your default browser...
Opening in existing browser session.
```
Works!

Lastly, we stop our cluster via
```
$ minikube stop
✋  Stopping node "minikube"  ...
🛑  Powering off "minikube" via SSH ...
🛑  1 nodes stopped.
```
