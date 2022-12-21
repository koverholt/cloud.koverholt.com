---
draft: false
date: 2022-12-21
---

# Managing Dialogflow Agents with Terraform

## Overview

[Dialogflow](https://cloud.google.com/dialogflow/cx/docs) is a powerful tool in
Google Cloud that you can use to design conversational chat and voice agents.

If you've ever wanted to get started with Dialogflow, you might have seen or ran
through the quickstart steps to
[build a shirt ordering agent](https://cloud.google.com/dialogflow/cx/docs/quick/build-agent)
that you can chat with to ask for the store location, get store hours, or make a
shirt order.

<center><img src="/assets/images/dialogflow-terraform-ui.png" alt="Dialogflow CX Shirt Ordering Agent" width="800px"></center>

In going through the quickstart steps, I wanted to find a way to codify all of
the Dialogflow components and settings to quickly spin up agents and change
their configuration. Terraform to the rescue!

As a result, I used the
[Terraform modules for Dialogflow CX](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/dialogflow_cx_agent)
and wrote up some Terraform + Dialogflow scripts to reproduced the agent
described in the "build a shirt ordering agent" quickstart.

<center><img src="/assets/images/dialogflow-terraform-graph.png" alt="Terraform Graph of Dialogflow CX Agent" width="800px"></center>

Try out the
[Terraform + Dialogflow scripts](https://github.com/koverholt/dialogflow-cx-terraform)
and spin up Dialogflow CX agents with a single command in your own Google Cloud
account!

## Setup

There's a few things that you'll need to set up before you run these Terraform
scripts.

1. Register for a
[Google Cloud account](https://cloud.google.com/docs/get-started).
1. Enable the
[Dialogflow API](https://cloud.google.com/dialogflow/cx/docs/quick/setup).
1. Install and initialize the
[Google Cloud `gcloud` CLI tool](https://cloud.google.com/sdk/docs/install).
1. Finally, install
[Terraform](https://developer.hashicorp.com/terraform/downloads). If you're
using macOS, I recommend installing Terraform with
[Homebrew](https://brew.sh/). Or, even better, install
[tfenv](https://github.com/tfutils/tfenv) using `brew install tfenv`, then run
`tfenv install` so you can easily switch between versions of Terraform!

## Usage

Once you've completed the setup on your local machine, you're ready to spin up
your own fully-configured Dialogflow agent in seconds:

1. Clone this repository and cd into the `terraform/` directory
1. Edit the values in `variables.tf` to specify your Google Cloud project ID
   along with your desired region and zone.
1. Run `terraform init`
1. Run `terraform apply`, the command that spins everything up!

Once you run `terraform apply` and confirm the proposed plan, you'll see
messages about all of the components that were provisioned:

```
google_dialogflow_cx_agent.agent: Creating...
google_dialogflow_cx_agent.agent: Creation complete after 2s
google_dialogflow_cx_entity_type.size: Creating...
google_dialogflow_cx_page.store_location: Creating...
google_dialogflow_cx_intent.store_hours: Creating...
google_dialogflow_cx_page.store_hours: Creating...
google_dialogflow_cx_page.order_confirmation: Creating...
google_dialogflow_cx_intent.store_location: Creating...
google_dialogflow_cx_intent.store_hours: Creation complete after 1s
google_dialogflow_cx_page.store_location: Creation complete after 1s
google_dialogflow_cx_page.order_confirmation: Creation complete after 1s
google_dialogflow_cx_page.store_hours: Creation complete after 1s
google_dialogflow_cx_intent.store_location: Creation complete after 1s
google_dialogflow_cx_entity_type.size: Creation complete after 1s
google_dialogflow_cx_page.new_order: Creating...
google_dialogflow_cx_intent.order_new: Creating...
google_dialogflow_cx_intent.order_new: Creation complete after 0s
google_dialogflow_cx_page.new_order: Creation complete after 0s
```

Now you're ready to view and test your agent in the
[Dialogflow Console](https://dialogflow.cloud.google.com/cx/projects)!

<center><img src="/assets/images/dialogflow-terraform-conversation.png" alt="Dialogflow CX Shirt Ordering Agent" width="500px"></center>

## How it works

We're using modules for
[Dialogflow from the Terraform registry](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/dialogflow_cx_agent)
to define a conversational agent. We've reproduced the agent described in the
[build a shirt ordering agent](https://cloud.google.com/dialogflow/cx/docs/quick/build-agent)
quickstart.

All of the agent's associated entity types, flows, intents, and pages are
created and managed with Terraform, so you can edit your Terraform scripts to
change certain parameters, run `terraform` apply, and see your changes instantly
reflected in the Dialogflow UI.

You might notice that the `terraform/flows.tf` file actually uses a `local-exec`
command to make a REST API call instead of using Terraform to define the flow.
This approach was used since Dialogflow creates a default flow when the agent is
created, which Terraform isn't aware of. So, we use a REST API call to PATCH the
default start flow and then modify messages and routes.

## Summary

It's nice to be able to manage conversational agents as code using Terraform in
Google Cloud. We get all of the benefits of Dialogflow with the convenience of
Terraform to manage everything in a stateful and version-control friendly way.

Take a look at the
[Terraform + Dialogflow scripts](https://github.com/koverholt/dialogflow-cx-terraform)
along with the
[Terraform modules for Dialogflow CX](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/dialogflow_cx_agent)
so you can spin up your own Dialogflow CX agents with a single command.

Coming soon, I'll share some best practices for working with Terraform and other
tools to represent "conversational agents as code", and I'll use these Terraform
scripts to build more solutions with agents and other Google Cloud tools.

Happy Terraforming!
