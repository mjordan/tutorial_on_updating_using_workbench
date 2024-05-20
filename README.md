# Hands-on tutorial on using Islandora Workbench to update content in your repository

## Overview and goals

During this 1.5 hour session, you will gain experience using Islandora Workbench to update your repository's content. After spending some time preparing your CSV and media files, you will move on to running Workbench to apply those changes to existing nodes and media.

## Prerequisites

- some experience using Islandora Workbench (doesn't matter what type of tasks) - important thing is that you know what configuration files are, what `--check` does, and that you're comfortable with the command line
- a working instance of Islandora Workbench on your computer (the more recent the better)

Sample data, and instances of Islandora, will be provided.

## Setup

Before we start practicing updates to content, we need to create some nodes and media we will later update.

Copy the following configuration file and save it in your "islandora_workbench" directory with the name "tutorial_create.yml". You may need to change the "host", "username", and "password" settings.

```
task: create
host: https://islandora.traefik.me
username: admin
password: password
input_csv: https://docs.google.com/spreadsheets/d/1nFwY-y5w0ljyvf510r4zX-ATnD7Oso3DhKERdblhcSY/edit?usp=sharing
standalone_media_url: true
allow_adding_terms: true
google_sheets_gid: 0
```

We also need to create a collection node, which we will use in an exercise later. We do this manually, without using Islandora Workbench.

## Exercises

### Updating node fields

First, "duplicate" the "update task" worksheet in the Google Sheet, labeling it with your name. After you modify this duplicated worksheet, you will use it as the input for your practice run of the "update" task.




### Replacing files


## Thank you

Thanks to Amy Blau for helping organize this tutorial, and to Rosie Le Faive for setting up the Islandora instances.




