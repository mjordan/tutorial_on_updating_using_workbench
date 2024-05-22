# Hands-on tutorial on using Islandora Workbench to update content in your repository

## Overview and goals

During this 1.5 hour session, you will gain experience using Islandora Workbench to update your repository's content. Each follow-along exercise below defines one or more specific learning outcomes, and there are some bonus exercises to complete as well. At the end of the tutorial, you will be ready to use Workbench to update  nodes and media in your own Islandora repository.

## Prerequisites

- some experience using Islandora Workbench (doesn't matter what type of tasks) - important thing is that you know what configuration files are, what `--check` does, and that you're comfortable with the command line
- a working installation of Islandora Workbench on your computer (the more recent the better)

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

Then, run Workbench using this configuration file: `./workbench --config tutorial_create.yml --check`

If `--check` doesn't identify any significant issues, rerun Workbench without the `--check` option.

## Follow-along exercises

### 1. Updating node fields

> Learning outcomes of this exercise: 1) using the `google_sheets_gid` config setting, and 2) running a simple `update` task using provided CSV values.

First, duplicate the "update task" worksheet in the Google Sheets, labeling it with your name. You will use this worksheet as the input for running the following `update` task. Your config file will look like this (with your `host`, `username`, and `password` instead of the values below):

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

Save the config file in your islandora_workbench directory as "tutorial_exercise_1.yml". Note that your worksheet will have its own "gid" in the URL. You will need to register that value in your config file's `google_sheets_gid` setting. Once your config file is ready, run:

`./workbench --config tutorial_exercise_1.yml --check`

Then, if `--check` didn't report anything important, run:

`./workbench --config tutorial_exercise_1.yml`


### 2. More updating node fields

> Learning outcomes of this exercise: Building on the last exercise, you will 1) provide the metadata to be added to your nodes and 2) append values to existing fields rather than replace field values.

To start this exercise, we will look at the field structure of the Repository Item content type together, and then you will add some new columns to your worksheet and update your nodes using your own metadata values.

For this exercise, let's append values to fields rather than replace the values. To do this, we need to add `update_mode: append` to our configuration files.

Since you are working in your own worksheet, you don't need to modify your configuration file. All you need to do is run `./workbench --config tutorial_create.yml --check`.

### 3. Replacing files

> Learning outcome of this exercise: Replace some thumbnail files.

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

# We should all use the same GID for this exercise.
google_sheets_gid: 609339107
```

## Bonus on your own (or with a partner) exercises

1. Create a collection node (manually, via Drupal's admin GUI is probably easiest) and use Workbench to add some existing nodes to it. Hint: the member nodes need to have the collection's node ID in their `field_member_of`.
1. Use Workbench to set the publication status of a couple of nodes to unpublished. Hint: the column in your CSV should be named `published`, which takes either a `1` or `0` value.
   * If you have time, you may want to unpublish the nodes' media as well. This can be done via an `update_media` task ([docs](https://mjordan.github.io/islandora_workbench_docs/updating_media/)).
1. Use an `add_media` task ([docs](https://mjordan.github.io/islandora_workbench_docs/adding_media/)) to add a text file media to a couple of nodes, assigning the media use term "Extracted text".

## Thank you

Thanks to Amy Blau for helping organize this tutorial, and to Rosie Le Faive for setting up the Islandora instances.
