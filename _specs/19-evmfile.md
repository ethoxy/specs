---
title: 19-EVMFILE
subtitle: Filesystem Library on Ethereum Virtual Machine
author: Wei Tang <hi@that.world>
status: Raw
type: Standard Track
category: Interface
abstract: >
  This RFC provides a filesystem specification that can be built on top of
  the Ethereum Virtual Machine.
---

This RFC provides a filesystem specification that can be built on top of
the Ethereum Virtual Machine.

Introduction
------------

The filesystem is built using EVM\'s account storage. Pathes of files
are hashed to get the storage key, and the contents of files are stored
as the storage value.

Filesystem Prefix
-----------------

The implementation library should have a pre-set prefix string that
avoids conflicts with other libraries that also use account storage.
This prefix string is implementation-specific.

Storing Files
-------------

When storing a file, `keccak256("{prefix}:{path}:{len}")` is used to
store the length of the file. If the length of the file is 0, the file
does not exist.

If the length of the file is greater than 0,
`keccak256("{prefix}:{path}:{index}")` is used as the key to store
contents of the file of every 32 bytes.

Folders
-------

Folders are normal files with contents of the list of files stored
inside. The list of files are a list of UTF-8 strings ended with `\0`
and padded in 32 bytes.

File Metadata
-------------

File metadata are normal files stored with path `{file_path}#metadata`.
The format of metadata is implementation-specific.
