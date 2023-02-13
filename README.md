# Exchange Format for Serlo Content

M2.1 Entwurf für die allgemeine Grundstruktur des Aufgaben- und Austauschformats
steht und ist in einem Open Source Repository veröffentlicht.

## Introduction

Most learning content is created on a specific platform, but sometimes it is
necessary to transfer it from one platform to another or to make a backup. To
facilitate this sharing, a common exchange format is required between platforms.
This document outlines a draft of Serlo's strategy for making learning content
transferable across different systems.

## Overview

It would not be efficient to create a new format. Instead, we will utilize an
existing and widely used format as a foundation. This format is defined by h5p.
H5p provides a platform for sharing interactive HTML5 content. With an H5p
instance, you can create and display H5p files. H5p also has an external file
format that packages all necessary data into a single file, which can then be
uploaded to another platform. The main components of an H5p file are shown in
the following image:

![grafik](https://user-images.githubusercontent.com/13507950/217875199-b2b1584e-8d0b-4ee5-9dbd-8893cf168b0d.png)

The primary content, such as texts and plugins, is stored in a JSON file. The
format of the file is defined by the Serlo Editor and has a built-in mechanism
for migrations to ensure backward compatibility, as described HERE.

Additionally, all media is included in the H5p file, making it possible to
upload it to another platform. To render the content as intended, the code for
displaying the content is also provided.

## Details

## Limitations

There is a small tradeoff, because the file includes the renderer, there is a
certain overhead to it -> but its great for reuse

Need for installation from the admin (one time)

Limited editability
