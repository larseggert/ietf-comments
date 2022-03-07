
# The IETF Comments Processor

Handling comments from the IESG and multiple directorate reviews can be burdensome for document authors, because of the sheer number of comments, and because they come in an unstructured format that has to be manually processed.

This script defines a [markdown](https://commonmark.org)-based format for IETF comments, and can create GitHub issues for the comments it finds. When used properly, it can help automate a formerly tiresome task.

Area directors and directorate reviewers can use it to ease the burden for authors. Even when they don't, authors can reduce their work by editing submitted issues to fit the markdown format.


## Installation

To install ietf-comments, you'll need [Python 3](https://www.python.org/). Then, run:

> pip3 install ietf-comments

## Use

To validate a comments file and see the issues it contains, run:

> ietf-comments _filename_

To create a GitHub issue for each issue in the comments, set `GITHUB_ACCESS_TOKEN` in your environment to a [GitHub Personal Access Token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) and run:

> ietf-comments -g _owner/repo_ _filename_

... where `owner/repo` is the repo owner and name, separated by a slash.

If you'd like these issues to have a specific label, run:

> ietf-comments -g _owner/repo_ _filename_ -l _labelname_


## The IETF Comments Format

The IETF Comment Format is a set of restrictions on the [markdown](https://commonmark.org) format that facilitates identifying the issues raised in the comments and their types, so that tooling can more easily digest it.

See the [examples directory](https://github.com/mnot/ietf-comments/tree/main/examples) for examples of the format in use.


### Document Identity

The document should start with a header indicating the title of the review; for example:

~~~ markdown
# Security AD comments for draft-ietf-whatever-document-08
~~~

Note that it must:
* Be a `h1` header (i.e., one octothorp)
* Identifies the reviewer, either by name or position
* Identifies the subject of the review with the full draft name _with_ revision number
* Be the only `h1` header in the document


### Comment Sections

Then, the document can contain `discuss`, `comment`, and `nit` positions, each in their own subsection. For example:

~~~ markdown
## Comments

### Wrong references

The references in section 2.1 are not correct.

### Does it work that way?

The widget in s 5.4.3 doesn't seem well-specified; are you sure?
~~~

Note that:
* The type of comment is identified with `h2` headers (i.e., two octothorps)
* The type can be 'discuss', 'comment', or 'nit' with any capitalisation
* Each type header can occur exactly once in the document

### Issues

Individual issues within a comment section can be identified with `h3` headers, as they are above. Alternatively, if a comment section does not have `h3` headers, the text in that section will be considered to be a single issue.

Issue text can have the following features:

### Change Proposals

A change can be proposed by preceding two quoted sections with OLD and NEW; for example:

~~~ markdown
OLD

> The foo header is a bar [HTTP].

NEW

> The foo header field is a bar [HTTP].
~~~

Note that the quoted text should be in the textual format, not XML or Markdown.


#### Section Linking

Within issue text, section links are automatically added. It is assumed that the bare words 'section' and 's' are followed by a section number to be referenced in the document (as in the example above).
