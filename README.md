
<!-- TABLE OF CONTENTS -->
## Table of Contents

* [About the Project](#about-the-project)
* [Getting Started](#getting-started)
  * [Prerequisites](#prerequisites)
  * [Installation](#installation)
* [Usage](#usage)




<!-- ABOUT THE PROJECT -->
## About The Project

This is my attempt to spin up kubernetes clusters quickly while studying for the CKA certification.
I found that using docker desktop and minikube was not complete enough for some type of testing.



<!-- GETTING STARTED -->
## Getting Started

Install the pre-reqs and clone the repo.  You are ready to build your cluster.

### Prerequisites

1. install virtual box
2. install vargrant

On a MAC 
```
brew cask install virtualbox
brew cask install vagrant
```


### Installation

1. Clone the repo
    ```sh
    git clone https://github.com/rscottwatson/k8s-vagrant
    ```
2. Decide on what version and network plug-in to use and how many worker nodes you want
3. Start up your VMs
    ```
    vagrant up
    ```



<!-- USAGE EXAMPLES -->
## Usage
```
Supported environment variables
K8S_VERSION        # version of kubernetest to deploy format is package release
                     eg. 1.19.2-00, 1.18.0-00 
K8S_WORKER_COUNT   # number of worker nodes to deploy default 1
K8S_CNI            # What network plugin to deploy. 
                     Supported CNI plugins are weavenet, calico and flannel calico is the default
```

1. Basic startup
    ```
    vagrant up
    ```
2. Set all variables
    ```
    export K8S_VERSION=; export K8S_CNI=flannel; export K8S_WORKER_COUNT=2; vagrant up
    ```




