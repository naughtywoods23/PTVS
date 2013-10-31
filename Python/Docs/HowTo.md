How to write Documentation
==========================

File Format
-----------

Source documents are written in Markdown with, as usual for Markdown, a few variations specific to our documents.

Source files must have the `.md` extension and be UTF-8 encoded (BOM is optional).

All of the basic formatting elements, such as **bold** and *italics* exist, however, underscores are ignored, so that we can write __dunder__ names.

* Bulleted lists use an asterisk at the start of the line
 * Subitems use spaces before the bullet

1. Numbered lists use any leading number...
 1. ...can also be indented...
* or specified with an asterisk once you've started

`Monospace` uses backticks, and blocks of code are delimited with three adjacent backticks:

```
My monochromatic code block
```

Language names (such as `csharp` or `python`) can be specified immediately after the first three backticks to perform formatting:

With `python`:

```python
def foo():
    for x in range(10):
        print x
    return True
```

With `csharp`:

```csharp
bool foo() {
    for (int x = 0; x < 10; ++i) {
        Console.WriteLine(x);
    }
    return True
}
```

-

One or more hyphens or asterisks alone on a line will create a horizontal rule.

* * *

Links are specified using `[text](url)` format: [text](url)

For simpler links and emails, just put them in `<angle brackets>`: <vspython@microsoft.com>

Images are specified using `![text](url)` format: ![an image example](Images\EditHTML.png)

Or to include style, just use an `<img>` tag: <img src="Images\EditHTML.png" style="border: #0000ff 3px solid; vertical-align: middle" />

---

We have some special link formats:

* `[src:Full.Class.Name]` will create a link to the source file on CodePlex containing the class:
 * [src:Microsoft.VisualStudioTools.Project.CommonProjectNode]
* `[file:Filename.ext]` will create a link based the filename (if unambiguous) or the path from the root:
 * [file:Common\Product\SharedProject\ProjectNode.cs]
 * [file:PythonProjectNode.cs]
* `[wiki:Page Title]` will create a link to a page on the CodePlex wiki, and `[wiki:"New Title" Page Title]` will change the display text:
 * [wiki:Python Environments]
 * [wiki:"page about Python envs"Python Environments]
* `[issue:number]` will link to an issue, and `[issue:"New Title"number]` will change the display text:
 * [issue:1234]
 * [issue:"Our very first issue" 1]

Other special conversions include:

>> ![Floated to the right](Images\EditHTML.png)

* `--(` and `--)` to delimit a layout block. These get converted into a div that does not allow floats to escape.
* `>>` floats the line to the right, like the image.

Converting to HTML
------------------

The [file:update_html.py] script can be run with Python 3.3 to generate HTML files for all `*.md` files in the branch. This script handles filename mapping and the tweaks that we use for Markdown. It includes a shebang line that is recognized by the `py.exe` launcher, allowing it to be run directly.

Running the script looks like:

```
cd Python\Docs
py update_html.py
```

Requirements are:

* Python 3.3
* markdown2 (`pip install markdown2`)
* Pygments (`pip install pygments`)

The first time the script is run, it will generate a file `maps.cache`, which contains all of the mappings used for the `src` and `file` links. This file should be deleted when source files are added, removed, moved or renamed, but will significantly speed up later refresh times.

Uploading to CodePlex
---------------------

Generated documentation can be uploaded to CodePlex using the [file:upload_to_codeplex.py] script. This script requires a CodePlex username and password with permissions to edit the wiki. It must be run after [file:update_html.py] has been run.

Running the script looks like:

```
cd Python\Docs
py update_html.py
tf edit User\codeplex.map
py upload_to_codeplex.py --dir User --user MyName --password "MyPassword123"
```

The `codeplex.map` file in the uploaded directory may be modified and needs to be writable before running the script. If it is not writable, an error will be displayed. The contents of the file include URLs for images and hashes for files that have been uploaded before. These will be used to determine whether the file has changes since it was last published to CodePlex, and to skip unchanged files.