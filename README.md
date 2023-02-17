# Exchange Format for Serlo Content

*M2.1 Entwurf für die allgemeine Grundstruktur des Aufgaben- und Austauschformats
steht und ist in einem Open Source Repository veröffentlicht.*

## Introduction

We want to achieve that learning content can be shared and exchanged between
learning platforms and that they are not tied to a specific platform. It is
necessary to transfer it from one platform to another. For example a teacher
want to use a learning material from a content repository like serlo.org in his
learning management system or he wants to make a backup. To facilitate this
sharing, a common exchange format is required between platforms. This document
outlines a draft of Serlo's strategy for making learning content transferable
across different systems.

## Overview

Instead of reinventing the wheel we first researched and analyzed current
solutions for exchanging interactive educational material. With H5p there is
already an existing and widely used format as a foundation which we aim to
support as well.

H5p provides a platform for sharing interactive HTML5 content. With an H5p
instance, you can create and display H5P files. Those H5P files packages all
necessary data into a single file, which can then be uploaded to another
platform. The main components of an H5P file are shown in the following image:

![grafik](https://user-images.githubusercontent.com/13507950/217875199-b2b1584e-8d0b-4ee5-9dbd-8893cf168b0d.png)

The primary content, such as texts and plugins, is stored in a JSON file. The
format of the file is defined by the Serlo Editor and has a built-in mechanism
for migrations to ensure backward compatibility, as described in our [migration algorithm repository](https://github.com/serlo/lenabi-migration-algorithm). Therefore we
aim to use H5P as a container in which we store the educational material itself
in the JSON file format of the Serlo editor.

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

Therefore you need an already installed programm on your computer to view and
edit the document. In comparision H5P ships the code for rendering the
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
particular educational content. You  This has some advantages:

- Portability: The file itself is enough to install all necessary software on your platform. This way, you don't need to find a separate download, but you are able to install the necessary libraries yourself on the platform.

- Consistency: The content is displayed as you have created it, there are no differences across different system.

- Standardisation: HTML5 is a standardized and widely used format. (any more?)

#### Specification of H5P

The [H5P file format specification](https://h5p.org/documentation/developers/h5p-specification) consists of 5 key components: the package itself, [the file tree structure](https://h5p.org/specification), the [package definition file](https://h5p.org/documentation/developers/json-file-definitions), the content structure, and the code libraries. The content structure is optional and includes media files and a content.json file. The code libraries must specify their name, dependencies, and other metadata in the [Library Definition file](https://h5p.org/library-definition), and if it's a runnable content type, it must also include a [Semantics Definition file](https://h5p.org/semantics) that describes the content's structure.

Refer to the official documentation for more information...

### The content format of the Serlo editor

TODO: Link to documentation of Anna with describing the main ideas

## Ptotypes

description and links to prototypes

## Limitations

There is a small tradeoff, because the file includes the renderer, there is a
certain overhead to it -> but its great for reuse

Need for installation from the admin (one time)

Limited editability
