# Open source Chinese dictionary proposal
CC-CEDICT is a widely used open source Chinese-English translation dictionary maintained by MDBG. It is licensed under *Creative Commons Attribution-Share Alike 3.0*. Edits of the dictionary are submitted via a form on [mdbg.net](https://cc-cedict.org/editor/editor.php). 

The database is distributed as a textual export file with entries in the format of:
```
Traditional Simplified [pin1 yin1] /American English equivalent 1/equivalent 2/
漢字 汉字 [han4 zi4] /Chinese character/CL:個|个/
```
This export file is distributed through [mdbg.net](https://www.mdbg.net/chindict/chindict.php?page=cedict). There is no API for CC-CEDICT integration(neither for submitting edits nor consuming the data). The database export is distributed on a mdbg.net webpage stating that scraping is not allowed.

CC-CEDICT is distributed for free but is not very open in a sense. This is a proposal to create a fork of CC-CEDICT which is more open and modernized. The goal is to make the data more accessible both for consuming and contributing.

The idea is to make the dictionary more accessible to encourage more people to join the collaborative effort and to set up a working group of people who together plan the future direction of the dictionary.

The main components of this proposal are:

* Providing API for
  * Editing - submitting edits
  * Download dictionary exports
  * Information/statistics
* Website
  * Editing through website
  * Download dictionary exports
  * Information/statistics
* GitHub integration
  * Accepting/rejecting and commenting edits is done through pull requests
  * Edits submitted through API/website are automatically turned into git pull requests
  * Exports distributed on GitHub
* Independence
  * Not tightly connected to any organization
* Encourage contributions

  *These components are explained in more detail in below sections.*

## API
### Editing
Editing support - enable edit submission via API so that developers can integrate edit submission into their applications/services.


### Download dictionary exports
CC-CEDICT is distributed in a text format. The proposal is to keep this export format but perhaps also provide other format(s). SQL dump, CSV? The export format CC-CEDICT use was introduced 1991. Can it be improved?

Other than just providing the export download also provide information about the export.

For example the following request:

```
GET /export/latest
```

Returns:

```json
{
	"export":
	{
		"id": 1068,
		"version": "1.0.0.43",
		"exportDate": "2016-12-12 23:55:00",
		"totalEntries": 112004,
		"previousExportDiff":
		{
			"newDefinitions": 15,
			"editedDefinitions": 3,
			"newEntries": 0
		},
		"export":
		{
			"CEDICT-format":
			{
				"zip": "http://domain.com/legacy.zip",
				"gzip": "http://domain.com/legacy.tar.gz"
			},
			"sql":
			{
				"zip": "http://domain.com/sql.zip",
				"gzip": "http://domain.com/sql.tar.gz"
			}
		}
	}
}
```

#### Export formats
The dictionary is mainly distributed in an export format to be read by applications consuming the database. Could also provide exports of the dictionary to be used with popular translation applications, for example these formats:

 * AppleDict *(Compatible with Apple's Dictionary application)*
 * StarDict

### Information/statistics
Provide information about database exports which could be used for statistics.

#### List exports information
For example the request:

```http
GET /export
```

Returns something like:

```json
{
	"exports":
	[
		{
			"id": 1070,
			"version": "1.0.0.45",
			"exportDate": "2016-12-14 23:55:00",
			"totalEntries": 112004,
			"previousExportDiff":
			{
				"newDefinitions": 9,
				"editedDefinitions": 20,
				"newEntries": 0
			}
		},
		{
			"id": 1069,
			"version": "1.0.0.44",
			"exportDate": "2016-12-13 23:55:00",
			"totalEntries": 112004,
			"previousExportDiff":
			{
				"newDefinitions": 22,
				"editedDefinitions": 7,
				"newEntries": 0
			}
		},
		{
			"id": 1068,
			"version": "1.0.0.43",
			"exportDate": "2016-12-12 23:55:00",
			"totalEntries": 112004,
			"previousExportDiff":
			{
				"newDefinitions": 15,
				"editedDefinitions": 3,
				"newEntries": 0
			}
		}
	]
}
```

## Website
Other than being informative the website should provide same functionality as the API - submit edits, download exports, get statistics etc.

## GitHub integration
### Edits as pull requests
Edits done through website/API are automatically turned into pull requests on a git repo. The repo will be in a textual format representing the dictionary's data. (Could be the CEDICT format or an improved textual format).

Edits are discussed and accepted/rejected on GitHub.

### Exports distributed on GitHub
The latest exports are distributed through GitHub.


## Independence
CC-CEDICT is tightly connected to MDBG which is making money from it's closed source software. CC-CEDICT is even hosted on MDBG's website. We want to give credit where credit is due but not be tightly connected to any organization.

## Encourage contributions
Aiming for encouraging people to contribute to the database. Both by submitting dictionary contributions and technical contributions.

### Dictionary contributions by users
By providing an API for both consuming the database and submitting contributions we open up possibilities for developers to add support for this in their applications/services. For example dictionary applications can allow their users to submit new translations and edits.

### Technical contributions
The API is one step towards encouraging technical contributions - it enables developers to create technical contributions such as frameworks, tools, different kinds of integrations.
