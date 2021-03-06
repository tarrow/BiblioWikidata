Scripts for integrating bibliographic metadata into Wikidata from various sources, currently including PubMed, PubMed Central, and the DOI.org endpoint. Other APIs and datasets will be included in the future.

# Requirements

Python 3.5 or higher is required.

This makes use of the WikidataIntegrator library. You can easily install it with `pip3 install wikidataintegrator`.

# Setup

## Downloading and installing

Clone this project to the directory containing your Python scripts. `git clone https://github.com/harej/BiblioWikidata`

If your project is itself a git project, you should make BiblioWikidata a submodule. `git submodule add https://github.com/harej/BiblioWikidata`.

## Setting username and password
First, you must set your username and password. Make a duplicate of `site_credentials.py.sample` and save it as `site_credentials.py`. Fill in your Wikidata username and password. If you have two-factor authentication enabled, you will need to create a [bot password](https://www.wikidata.org/wiki/Special:BotPasswords). Remember to set the permissions to the file so that others can't read your username and password!

# Usage

There are docstrings throughout the code files. You may peruse those.

But generally, the only thing that's really been implemented at the moment is the ability to create new Wikidata items in bulk.

## Creating Wikidata entries on journal articles
Sample program: You have a list of DOIs saved at `doi_list.csv`.

```python
import csv
from BiblioWikidata import JournalArticles

def main():

	# Load the DOIs from file. VERY IMPORTANT: BiblioWikidata does *not* check
	# your list against Wikidata for existing entries! You must do this first.

	list_of_DOIs = []
	with open('doi_list.csv') as f:
		spreadsheet = csv.reader(f)  # One-column CSV of DOI strings
		for row in spreadsheet:
			list_of_DOIs.append(row[0])

	# Construct the "manifest"
	# The manifest is a list of dictionaries. The dictionaries have a syntax
	# like this:
	#
	#     {'doi': '10.100/abc', 'pmid': '1234', 'pmcid': '5678'}
	#
	# The PMIDs and PMCIDs are strings, and the PMCID string does not include
	# the 'PMC' prefix.
	#
	# You may optionally include a 'data' parameter including a list of Wikidata
	# statements you want to include in addition to the data generated by the
	# BiblioWikidata library. These statements must take the form of `WDBaseDataType`
	# children.

	manifest = [{'doi': x} for x in list_of_DOIs]

	JournalArticles.item_creator(manifest)

if __name__ == '__main__':
	main()
```