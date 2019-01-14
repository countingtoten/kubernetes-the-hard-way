# Prerequisites

## Google Cloud Platform

This tutorial leverages the [DigitalOcean Droplets](https://www.digitalocean.com/) to streamline provisioning of the compute infrastructure required to bootstrap a Kubernetes cluster from the ground up. [Sign up](https://cloud.digitalocean.com/registrations/new).

[Estimated cost](https://cloud.google.com/products/calculator/#id=78df6ced-9c50-48f8-a670-bc5003f2ddaa) to run this tutorial: $0.22 per hour ($5.39 per day).

> The compute resources required for this tutorial exceed the Google Cloud Platform free tier.

## Google Cloud Platform SDK

### Install the DigitalOcean command-line client

Follow the DigitalOcean command-line client [documentation](https://www.digitalocean.com/community/tutorials/how-to-use-doctl-the-official-digitalocean-command-line-client) to install and configure the `doctl` command line utility.

### Generate a Personal Access Token

Follow the DigitalOcean [documentation](https://www.digitalocean.com/docs/api/create-personal-access-token/) to create a personal access token. Use that token to initialize doctl.

```
doctl auth -t Exampletoken1234 init
```

## Running Commands in Parallel with tmux

[tmux](https://github.com/tmux/tmux/wiki) can be used to run commands on multiple compute instances at the same time. Labs in this tutorial may require running the same commands across multiple compute instances, in those cases consider using tmux and splitting a window into multiple panes with `synchronize-panes` enabled to speed up the provisioning process.

> The use of tmux is optional and not required to complete this tutorial.

![tmux screenshot](images/tmux-screenshot.png)

> Enable `synchronize-panes`: `ctrl+b` then `shift :`. Then type `set synchronize-panes on` at the prompt. To disable synchronization: `set synchronize-panes off`.

Next: [Installing the Client Tools](02-client-tools.md)
