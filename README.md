# Hands-on tutorial on using Islandora Workbench to update content in your repository

## Overview

During this 1.5 hour session, you will gain experience using Islandora Workbench to update your repository's content. Each guided exercise below defines one or more specific learning outcomes, and there are some bonus exercises to complete as well.

At the end of the tutorial, you will be ready to use Workbench to update nodes and media in your own Islandora repository.

## Prerequisites

- some experience using Islandora Workbench (doesn't matter what type of tasks) - important thing is that you know what configuration files are, what `--check` does, and that you're comfortable with the command line
- the ability to create and edit Workbench confguration files - any text editor will suffice
- a working installation of Islandora Workbench on your computer (the more recent the better)

Sample data, and instances of Islandora, will be provided.

## A note about the sample data and configuration files

The input CSV we will use is a shared Google Sheet whose URL appears in all of the configuration files below. Within the Google Sheet, specific worksheets are identifed in the configuration files by using the `google_sheets_gid` setting.

The "file" column in each worksheet points to an image in this Github repo's `images` directory. In addition to the image files that will end up as "Original file" media on the nodes we will create and update, the directory contains a thumbnail image, which we will use in exercise 4.

You will not need to clone this Github repo to participate in the tutorial. Copying the sample configuration files, and in a couple cases editing them slightly, is all that is required.

## Setup

Before we start practicing updates to content, we need to create some nodes and media.

Copy the following configuration file and save it in your "islandora_workbench" directory with the name "tutorial_create.yml". You may need to change the `host`, `username`, and `password` settings.

```
task: create
host: https://islandora.traefik.me
username: admin
password: password
input_csv: https://docs.google.com/spreadsheets/d/1nFwY-y5w0ljyvf510r4zX-ATnD7Oso3DhKERdblhcSY/edit?usp=sharing
allow_adding_terms: true
google_sheets_gid: 0
```

Then, run Workbench using your newly saved configuration file: `./workbench --config tutorial_create.yml --check`

If `--check` doesn't identify any significant issues, rerun Workbench without the `--check` option. Workbench will create six nodes and accompanying media.

## Guided exercises

### Exercise 1. Replacing values in node fields

> [!NOTE]
> Learning outcomes of this exercise: 1) using the `google_sheets_gid` config setting, and 2) running a simple `update` task using provided CSV values.

1. Duplicate the "update task" worksheet in the shared Google Sheet, renaming it so it includes "update" and your initials.
2. Update the values in the `node_id` column of your duplicated worksheet to match the node IDs of the items you created above.
3. note the value of the `gid` parameter in the URL of your worksheet. You will need to add to `google_sheets_gid` setting in your Workbench config file.

You will use this worksheet as the input for running the following `update` task. Your config file will be based on this template (with your `host`, `username`, `password`, and `google_sheets_gid` instead of the values below):

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

Save the config file in your islandora_workbench directory as "tutorial_update.yml". Note that your worksheet will have its own "gid" in the URL. You will need to register that value in your config file's `google_sheets_gid` setting. Once your config file is ready, run:

`./workbench --config tutorial_update.yml --check`

Then, if `--check` didn't report anything important, run:

`./workbench --config tutorial_update.yml`

The values in your worksheet should have replaced the original values in the respective node fields. This is because the default `update_mode` is "replace", telling Workbench to replace whatever values are in the node fields with the values in the CSV data.

### Exercise 2: Appending values to node fields

> [!NOTE]
> Learning outcome of this exercise: Appending values to existing fields rather than replacing field values.

Now let's append values to data in fields rather than replace the values.

1. In each row of your CSV, replace the contents of the `field_subject_general` column with a subject heading of your own, prepending the subject heading with "subject:" to tell Workbench which vocabulary to add the new value to.
2. Add `update_mode: append` to your configuration file (same on you used in exercise 1). This overrides the default value of `update_mode` ("replace"), telling Workbench to append the values in the CSV file to the values existing in the respective node fields.

Rerun `./workbench --config tutorial_update.yml --check`, and if no major problems are reported, `./workbench --config tutorial_update.yml`.


### Exercise 3. Populating empty fields

> [!NOTE]
> Learning outcome of this exercise: Inserting new columns to your worksheet, thereby adding the CSV data to your nodes.

To start this exercise, 

1. Look at the field structure of the "Repository Item" content type using the "Manage Fields" list for the "Repository Item" content type at `/admin/structure/types/manage/islandora_object/fields`.
2. Choose a field and add its machine name to your Google CSV workbsheet.
3. Remove the `title`, `field_subject_general`, and `field_coordinates` columns from your CSV, or, if you want to keep them, add the following to your configuration file" `ignore_csv_columns: ['title', 'field_subject_general', 'field_coordinates']`.
4. Populate the new column with metadata values.

Since you are using the same worksheet as in the previous exercises and the `update_mode` setting can remain as "append", you don't need to modify your configuration file. All you need to do is run `./workbench --config tutorial_update.yml --check`, and then, if no major problems are reported, `./workbench --config tutorial_update.yml`.

### Exercise 4. Replacing files

> [!NOTE]
> Learning outcome of this exercise: Replace some thumbnail files.

Updating content not only involves updating node field data, it can also involve replacing image or other files. In this exercise, we will replace the thumbnail image for two of our nodes with this one:

![Pictures of several kinds of cats](https://raw.githubusercontent.com/mjordan/tutorial_on_updating_using_workbench/main/images/cats_tn.jpg)

1. Before we run Workbench, perform a search on your Drupal website for "cats" and note that the thumbnail images are derived from the original files used when we created the nodes and media.
2. Duplicate the "update task" worksheet in the shared Google Sheet, renaming it so it includes "replace file" and your initials.
3. Find the media IDs of the two thumbnail media whose file you want to replace and update your worksheet's `media_id` column with them.
4. Save the following config file in your islandora_workbench directory as "tutorial_exercise_replace_thumbnails.yml" (with your own `host`, `username`, and `password` values):

```
task: update_media
host: https://islandora.traefik.me
username: admin
password: password

input_csv: https://docs.google.com/spreadsheets/d/1nFwY-y5w0ljyvf510r4zX-ATnD7Oso3DhKERdblhcSY/edit?usp=sharing
media_type: image

# Replace with the gid of your "replace files" worksheet.
google_sheets_gid: 609339107
```

Save your configuration file and run `./workbench --config tutorial_exercise_replace_thumbnails.yml --check`, and if no major problems are reported, then run `./workbench --config tutorial_exercise_replace_thumbnails.yml`.

To see the new thumbnails, rerun you search for "cats".

Note that we only replaced the file, not the entire media entity. These files remain "thumbnail" images due to their media having that Islandora Media Use value. If we wanted to add a thumbnail media to a node that didn't have one, we would use a `add_media` task.

## Bonus on-your-own (or with-a-partner) exercises

1. Create a collection node (manually, via Drupal's admin GUI is probably easiest) and use Workbench to add some existing nodes to it. Hint: the member nodes need to have the collection's node ID in their `field_member_of`.
1. Use Workbench to set the publication status of a couple of nodes to unpublished. Hint: the column in your CSV should be named `published`, which takes either a `1` (published) or `0` (unpublished) value.
   * If you have time, you may want to unpublish the nodes' media as well. This can be done via an `update_media` task ([docs](https://mjordan.github.io/islandora_workbench_docs/updating_media/)).
1. Use an `add_media` task ([docs](https://mjordan.github.io/islandora_workbench_docs/adding_media/)) to add a plain text file media to a couple of nodes, assigning the media use term "Extracted text".

## Thank you

Thanks to Amy Blau for helping organize this tutorial, and to Rosie Le Faive for setting up the Islandora instances.
