# Qri Recipes

## What's a recipe?

A recipe is a plug-and-play script that automates adding data to qri. Recipes can be run multiple times to update a dataset with new data. Recipes can be written in any language that fits in a docker container, and you’re highly encouraged to write and share recipes that fit your data needs. 


## Example: Craigslist Apartments

[Craigslist Apartments](https://github.com/qri-recipes/craigslist_apartments) creates a dataset from craigslist apartment listings. Running the recipe will spin up a python script that visits craigslist.com web pages turning HTML into entries in the dataset. The recipe includes a dataset structure, and fills in relevant metadata for you, so all you have to do is run it.


## Recipe Configuration

Recipes can be made configurable to broaden their applicability. In the craigslist apartments example, the recipe can be configured to grab apartments from different cities. A well-written recipe has well-documented configuration options to make recipes applicable in a number of places.


## Running Recipes Multiple Times

Recipes are intended to be run multiple times keep the dataset in sync with the source material. Each time a recipe is run, it will generate a new change in the qri dataset, marking the update. How often a recipe is run depends on the task at hand, but as a good starting point, think in days or weeks instead of minutes or seconds. If the recipe is monitoring very fast changing data, feel free to update more frequently. For now you have to schedule and (re) run recipes yourself, but we have plans to improve on this (see the roadmap section).

How a recipe manipulates the existing data is up to the recipe itself, but when it comes to running recipes multiple times, there are three general approaches to storing changes: overwrite, append, and merge.

#### Overwrite
overwrite re-writes the dataset each time with the latest data. This is more useful than it seems, because qri keeps versions of datasets. New recipes should consider overwrite “default” recipe, creating the highest degree of flexibility. It’s possible to generate append and merge style datasets from a qri history by combining prior changes.

#### Append
Append reads in the previous dataset and adds all data the recipe finds to a script.
A classic example of this is time-series data,

#### Merge
Merge reads in the previous dataset and makes "intelligent" updates to the dataset by cross referencing new data with the existing dataset based on an _identifier_.
For merge to work, the schema of the dataset must specify at least one identifier property that the recipe can use for smart merging.

Recipe maintainers should list which of these categories their recipe fits within.


## Recipes Naturally Interoperate

One of the most exciting features of recipes is their natural interoperability. If you run the craigslist recipe for apartments in Twin Falls, Idaho, and I run the same recipe for for Chicago, Illinois our datasets can be compared seamlessly. 


## Alternatives to recipes

Recipes work best when the task generalizes well, but recipes are by no means the only way to add data to qri. If the data source you’re looking at is more “one off” than a repeatable thing, it’s perfectly fine to use, say a jupyter notebook, or maybe bash scripts that run `qri add` for you. Don’t feel like you have to use or write a recipe, but recipes do have the distinct upside of being easier to share & generalize.


## List of Available Recipes

The list of public recipes is kept as a dataset at `qri/recipes`, which is a mirror of the recipes.csv in this overview repo. To add your own recipe, submit a Pull Request to this repo that updates the list.


## Writing your own recipe


Recipe Project Organization

Recipe bases should all draw on the same dataset template directory, which will be stored in the repo of each recipe.


    templates/
      - structure.json
      - schema.json
      - meta.json
    params.yaml
    recipe.py
    dockerfile
    README.md

the templates directory contains starting templates for recipe ingestion. recipe bases should/will check the templates directory for json files that

the dockerfile is a docker container file that will run the recipe with `docker run`


## Configuration Via Environment Variables

recipes should accept configuration via environment variables.


## Recipe base packages

A recipe base is a helper package for building a recipe. Recipe bases are in the works for a number of packages. We’ll update this repo once they’re ready. If you’re interested in writing a recipe base for your language or framework of choice, feel free to file an issue on this repo and we’ll try to help!

## Recipes Roadmap

In time, we plan to integrate recipes directly into qri as dataset transformations. The upside of this will be letting qri handle scheduling when recipes execute, and creating recipes that depend on datasets within qri to make new, derivative datasets. More on that in the coming months.