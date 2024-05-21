# Hands-on tutorial on using Islandora Workbench to update content in your repository

## Overview and goals

During this 1.5 hour session, you will gain experience using Islandora Workbench to update your repository's content. After spending some time preparing your CSV and media files, you will move on to running Workbench to apply those changes to existing nodes and media.

## Prerequisites

- some experience using Islandora Workbench (doesn't matter what type of tasks) - important thing is that you know what configuration files are, what `--check` does, and that you're comfortable with the command line
- a working instance of Islandora Workbench on your computer (the more recent the better)

Sample data, and instances of Islandora, will be provided.

## Setup

Before we start practicing updates to content, we need to create some nodes and media we will later update.

Copy the following configuration file and save it in your "islandora_workbench" directory with the name "tutorial_create.yml". You may need to change the `host`, `username`, and `password` settings.

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

## Follow along exercises

### 1. Updating node fields

First, duplicate the "update task" worksheet in the Google Sheet, labeling it with your name. After you modify this duplicated worksheet, you will use it as the input for your practice run of the `update` task.

```
task: update
host: https://islandora.traefik.me
username: admin
password: password
input_csv: https://docs.google.com/spreadsheets/d/1nFwY-y5w0ljyvf510r4zX-ATnD7Oso3DhKERdblhcSY/edit
allow_adding_terms: true

# Your GID will be different than this one.
google_sheets_gid: 1016713769
```

### 2. Replacing files

We will replace the thumbnail image for two of our nodes with this one:

![Pictures of several kinds of cats](https://raw.githubusercontent.com/mjordan/tutorial_on_updating_using_workbench/main/images/cats_tn.jpg)


```
task: update_media
host: https://islandora.traefik.me
username: admin
password: password
input_csv: https://docs.google.com/spreadsheets/d/1nFwY-y5w0ljyvf510r4zX-ATnD7Oso3DhKERdblhcSY/edit?usp=sharing
standalone_media_url: true
media_type: image

# We can all use the same GID for this exercise.
google_sheets_gid: 609339107
```

## On your own (or with a partner) exercises

1. Create a collection node (manually, via Drupal's admin GUI is OK) and use Workbench to add some existing nodes to it. Hint: the member nodes need to have the collection's node ID in their `field_member_of`.
1. Use Workbencbh to set the publication status of a couple of nodes to unpublished. Hint: the column in your CSV should be named `published`, which takes either a `1` or `0` value.
   * If you have time, you may want to unpublish the nodes' media as well. This can be done via an `update_media` task.
1. Use an `add_media` task ([docs](https://mjordan.github.io/islandora_workbench_docs/adding_media/)) to add a text file media to a couple of nodes, assigning the media use term "Extracted text".

## Thank you

Thanks to Amy Blau for helping organize this tutorial, and to Rosie Le Faive for setting up the Islandora instances.




