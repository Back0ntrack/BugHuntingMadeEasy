# Prerequisites

## Introduction to Azure Cloud Shell

Azure Cloud Shell is a browser-accessible command-line experience for managing Azure resources. It provides the flexibility of choosing the shell experience that best suits the way you work, either Bash or PowerShell.

## Azure Resource Manager (ARM)

Azure Resource Manager is the deployment and management service for Azure. Think of it as the central "control plane" or the ultimate middleman for everything you do in the Azure cloud.

Whether you are using the Azure Portal (the web interface), Azure CLI, PowerShell, or REST APIs, your request is sent to the Azure Resource Manager API. ARM authenticates and authorizes the request, and then routes it to the appropriate Azure service to be executed.

### ARM Template&#x20;

An ARM template is a JavaScript Object Notation (JSON) file that defines the infrastructure and configuration for your project. This is Azure's native implementation of Infrastructure as Code (IaC).

Instead of manually clicking through the Azure Portal to create a server, a database, and a virtual network, you write a text file describing those resources. You then hand that file to ARM, and ARM builds the environment exactly as described.

#### Core concepts of ARM Templates:

* Declarative Syntax: You specify _what_ you want to create, not _how_ to create it. You declare, "I want a virtual machine with 4GB of RAM," and ARM figures out the underlying steps to make that happen.
* Idempotency: You can deploy the same template multiple times and get the same result. If a resource already exists and matches the configuration, ARM will leave it alone. If it’s missing or configured incorrectly, ARM will create or update it.

