# Exchange Format for Serlo Content

## Introduction

We want to achieve that learning content can be shared and exchanged between
learning platforms and that they are not tied to a specific platform. It is
necessary to transfer it from one platform to another. For example a teacher
want to use a learning material from a content repository like
[serlo.org](https://de.serlo.org/) in his learning management system or he wants
to make a backup. To facilitate this sharing, a common exchange format is
required between platforms. This document outlines a draft of Serlo's strategy
for making learning content transferable across different systems.

## Overview

Instead of reinventing the wheel we first researched and analyzed current
solutions for exchanging interactive educational material. With H5p there is
already an existing and widely used format as a foundation which we aim to
support as well.

H5p provides a platform for sharing interactive HTML5 content. With an H5p
instance, you can create and display H5P files. H5P packages all
necessary data into a single file, which can then be uploaded to another
platform. The main components of an H5P file are shown in the following image:

![grafik](https://user-images.githubusercontent.com/13507950/217875199-b2b1584e-8d0b-4ee5-9dbd-8893cf168b0d.png)

The primary content, such as texts and plugins, is stored in a JSON file. The
format of the file is defined by the Serlo Editor and has a built-in mechanism
for migrations to ensure backward compatibility, as described in our
[migration algorithm repository](https://github.com/serlo/lenabi-migration-algorithm).
Therefore we aim to use H5P as a container in which we store the educational
material itself in the JSON file format of the Serlo editor.

Additionally, all media is included in the H5P file, making it possible to
upload it to another platform. To render the content as intended, the code for
displaying the content is also provided.

## Details

### The H5P container format

#### The main idea behind H5P

H5P defines a container format for educational content. It is comparable to a
[Word document](https://en.wikipedia.org/wiki/Office_Open_XML) since both file
formats are [ZIP files](<https://en.wikipedia.org/wiki/ZIP_(file_format)>)
containing the media and the data of the document. However there is a major
difference. A word document only contains the data of the document as well as
the media files (like images included in the word document):

```
       ┌────────────────────┐
       │┌──────┐   ┌───────┐│
Word = ││ Data │ + │ Media ││
       │└──────┘   └───────┘│
       │ZIP-file            │
       └────────────────────┘
```

Therefore you need an already installed program on your computer to view and
edit the document. In comparison H5P ships the code for rendering the
educational content as JavaScript and CSS files inside the ZIP file:

```
       ┌───────────────────────────────┐
       │┌──────┐   ┌───────┐   ┌──────┐│
H5p =  ││ Data │ + │ Media │ + │ Code ││
       │└──────┘   └───────┘   └──────┘│
       │ZIP-file                       │
       └───────────────────────────────┘
```

So you can think of each H5P file as an individual App which displays a
particular educational content. This has some advantages:

- Portability: The file itself is enough to install all necessary software on
  your platform. This way, you don't need to download addition software, but you
  are able to install the necessary libraries yourself on the platform with the
  file.

- Consistency: The content is displayed as you have created it, there are no
  differences across different system.

- Compatibility: Besides the official implementation of H5P for Drupal and
  Moodle, there are new systems that implement the existing H5P specification.
  Serlo Content stored in this exchange format can be added to these new systems
  easily.

#### Specification of H5P

The
[H5P file format specification](https://h5p.org/documentation/developers/h5p-specification)
describes a H5P file as a ZIP file with the ending `.h5p`. In case of a Serlo
content the [file tree structure](https://h5p.org/specification) will be:

```
.
├── h5p.json               # Meta / package file describing the content and file structure
│
├── content                # Directory with content and media files
│   │
│   ├── images                # Directory with media files
│   │   └── explain.png
│   │
│   └── content.json          # JSON file describing the content
│                               in the Serlo Editor content format
│
├── Serlo.Editor           # Main used library with source code and CSS
│   ├── ...
│   ├── index.js
│   ├── style.css
│   ├── library.json
│   └── semantics.json
│
└── FontAwesome            # Additional libraries which are used (optional)
    ├── ...
    └── library.json
```

Meta data about the educational content (for displaying / using them in the H5P
repositories) as well as describing how the educational can be technically
embedded are stored in the `h5p.json` file. It is called the
[package definition file](https://h5p.org/documentation/developers/json-file-definitions)
and it will look like:

```json
{
  "title": "Example content with the Serlo Editor",
  "language": "de",
  "mainLibrary": "Serlo.Editor",
  "embedTypes": ["div", "iframe"],
  "authors": [
    {
      "name": "Julia Sprothen",
      "role": "Author"
    }
  ],
  "source": "https://serlo.org/1656",
  "license": "CC BY-SA",
  "licenseVersion": "4.0",
  "changes": [
    {
      "date": "02-07-19 11:27:00",
      "author": "Julia Sprothen",
      "log": "Erstellung eines Beispielinhalts"
    }
  ],
  "preloadedDependencies": [
    {
      "machineName": "Serlo.Editor",
      "majorVersion": "1",
      "minorVersion": "0"
    }
  ]
}
```

The content will be stored in the `content` directory. In case there are
attached media files they will be stored in `images`. All major image and video
are allowed (see
[Allowed File Extensions](https://h5p.org/allowed-file-extensions)).

The code libraries must specify their name, dependencies, and other meta data in
the [Library Definition file](https://h5p.org/library-definition), and if it's a
runnable content type, it must also include a
[Semantics Definition file](https://h5p.org/semantics) that describes the
content's structure. In case of the Serlo editor the library definition will
look like

```json
{
  "title": "Serlo editor",
  "description": "A WYSIWYG editor for educational material",
  "machineName": "Serlo.Editor",
  "majorVersion": 1,
  "minorVersion": 0,
  "patchVersion": 0,
  "runnable": 1,
  "author": "Serlo Education e.V.",
  "license": "Apache License 2.0",
  "coreApi": {
    "majorVersion": 1,
    "minorVersion": 0
  },
  "preloadedCss": [{ "path": "styles.css" }],
  "preloadedJs": [{ "path": "index.js" }],
  "preloadedDependencies": [
    {
      "machineName": "FontAwesome",
      "majorVersion": 3,
      "minorVersion": 0
    }
  ]
}
```

Refer to the official documentation for up-to-date information.

### The content format of the Serlo editor

The educational material itself will be described in the file
`content/content.json`. The root is an object which contains meta information as
well as information about the used version. We have documented this structure in
depth in our
[documentation for the migration algorithm](https://github.com/serlo/lenabi-migration-algorithm):

```json
{
  "type": "https://serlo.org/editor",
  "version": 1,
  "content": {
    "plugin": "article",
    "state": {}
  }
}
```

It content itself is described in the article
[content format of the Serlo editor](https://github.com/serlo/documentation/wiki/Content-format)
in the wiki of our Serlo Editor. It represents educational material as nested
blocks of educational content which we call `plugin`:

![Plugin structure of the content format](https://raw.githubusercontent.com/serlo/documentation/main/images/main-content.png)

Those plugins are stored in a JSON format so that they can be read and used in
many languages. The basic structure of a `plugin` is

```JavaScript
{
  "plugin": "...", // name of the plugin
  "state": ... // necessary information to describe the plugin
}
```

The article
[content format of the Serlo editor](https://github.com/serlo/documentation/wiki/Content-format)
contains a detailed description of all plugins together with a description of
their state.

## Prototypes

To make sure that this approach will work, we have created two prototypes. The
first one is located in [h5p-serlo-poc](https://github.com/serlo/h5p-serlo-poc).
This prototype defines a code library for H5P that is build with React and
Typescript, the same foundation as the rendering of Serlo Content. The prototype
implements a simple renderer that takes a JSON input and displays is as react
component. This shows, that the approach in this document is workable and Serlo
Content can be exported as `.h5p`-files and opened on a supported platform.

We can go further than only displaying the content. The second prototype located
at [h5p-editor-serlo-poc](https://github.com/serlo/h5p-editor-serlo-poc) adds
another library that defines an editing widget that is rendered with React and
Typescript. This way, it would be also possible to include the Serlo Editor into
the file and make the file editable. We are still evaluating the possibilities
here.

## Limitations

There are some limitations to this approach that we are still researching. The
first one is that the target platform still needs to install the library once
and update it regularly. There are some ways to handle this:

- Installation is simple and can be done quickly. We hope that this will
  encourage platform administrators to install and update our library.
- There are some major platforms that we have contact with. This way, we can
  ensure that the library is installed and updated.

On a more technical basis, bundling all code into the file has great benefits
for portability, but it will also increase the file size. However, as a
transport format, we expect that the upsides will outweigh the downsides. Since
each H5P file is basically an App / another website you include via iframe in
your website you should only use those files from trusted sources and authors.

Editing the content on the other platform will need an integration of the Serlo
Editor, which can be accomplished with another library. The technical foundation
is available, as shown in the prototype. But there is still work needed to
clearly define the scope of this possibility.

## License

<img src="https://github.com/serlo/lenabi-exchange-format/raw/main/assets/cc-by-sa.svg" alt="Logo CC-BY-SA 4.0 license" title="CC-BY-SA 4.0" align="right" width="150" />

This specification is licensed under the
[CC-BY-SA 4.0 license](https://creativecommons.org/licenses/by-sa/4.0/). Please
provide [Serlo Education e.V.](https://serlo.org) together with the link to this
repository as the source attribution.

## Förderung

<img src="https://github.com/serlo/lenabi-exchange-format/raw/main/assets/bmbf.png" alt="Logo BMBF" title="BMBF" align="right" width="150" />

Das diesem Bericht zugrunde liegende Vorhaben wurde mit Mitteln des
Bundesministeriums für Bildung und Forschung unter dem Förderkennzeichen LENABI2
gefördert. Die Verantwortung für den Inhalt dieser Veröffentlichung liegt bei
der Autorin/beim Autor.
