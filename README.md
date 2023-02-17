# Exchange Format for Serlo Content

M2.1 Entwurf für die allgemeine Grundstruktur des Aufgaben- und Austauschformats
steht und ist in einem Open Source Repository veröffentlicht.

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
instance, you can create and display H5p files. Those H5p files packages all
necessary data into a single file, which can then be uploaded to another
platform. The main components of an H5p file are shown in the following image:

![grafik](https://user-images.githubusercontent.com/13507950/217875199-b2b1584e-8d0b-4ee5-9dbd-8893cf168b0d.png)

The primary content, such as texts and plugins, is stored in a JSON file. The
format of the file is defined by the Serlo Editor and has a built-in mechanism
for migrations to ensure backward compatibility, as described HERE. Therefore we
aim to use H5p as a container in which we store the educational material itself
in the JSON file format of the Serlo editor.

Additionally, all media is included in the H5p file, making it possible to
upload it to another platform. To render the content as intended, the code for
displaying the content is also provided.

## Details

### The H5p container format

#### The main idea behind H5p

H5p defines a container format for educational material. It is compariable to a
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
edit the document. In comparision H5p ships the code for rendering the
educational content as JavaScript and Css files inside the ZIP file:

```
       ┌───────────────────────────────┐
       │┌──────┐   ┌───────┐   ┌──────┐│
H5p =  ││ Data │ + │ Media │ + │ Code ││
       │└──────┘   └───────┘   └──────┘│
       │ZIP-file                       │
       └───────────────────────────────┘
```

So you can think of each H5p file as an individual App which displays a
particular educational content. This has some advantages:

- ...

#### Specification of H5p

TODO: Short overview of the specification (file-structure, some insights into
the files) + Link zu specification

### The content format of the Serlo editor

TODO: Link to documentation of Anna with describing the main ideas

## Ptotypes

description and links to prototypes

## Limitations

There is a small tradeoff, because the file includes the renderer, there is a
certain overhead to it -> but its great for reuse

Need for installation from the admin (one time)

Limited editability
