# Notes on Using `.gitattributes` to Exclude Files from Language Stats

## What is `.gitattributes`?

The `.gitattributes` file defines how Git and GitHub should treat certain files in a repository.  
GitHub uses it to determine the language composition of a project by excluding or including files from language statistics.

## Why exclude files?

If a project includes third-party or external libraries (often written in different languages), they can distort GitHub’s language statistics. For instance, even if the main software is written in one language (like C++), GitHub may report a higher percentage of another language due to large dependency files (e.g., `.c`, `.js`, or `.py` files from libraries). This can misrepresent the actual tech stack of one's own codebase.

## How to exclude files from language stats

To fix this, mark those files or directories as "vendored" in a `.gitattributes` file. Vendored files are treated as external dependencies and are excluded from GitHub's language analysis.

### Create or edit `.gitattributes` in the root of your repository

```gitattributes
folder/thirdparty.extension linguist-vendored
```

This will exclude `thirdparty.extension` from language statistics.

### Exclude an entire folder

To ignore everything in a folder, use:

```gitattributes
folder/* linguist-vendored
```

or for any common vendor folder:

```gitattributes
vendor/** linguist-vendored
```
