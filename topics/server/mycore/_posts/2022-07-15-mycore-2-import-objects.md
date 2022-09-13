---
layout: post
title: MyCoRe - Part 2 - Importing and Publishing Objects
author: Ralf Eichinger
toc: true
---

In my previous Post ["MyCoRe - Part 1 - Installation, Configuration and Usage (on localhost)"]({% post_url topics/server/mycore/2022-06-13-mycore-1-installation %}) I described how to install,
configure and stop/start MyCoRe Digital Content Repository + MIR Webapp on top of it.
We ended up with a running webapp with no digital objects.

Now we want to import our digital objects to be managed and published in our MyCoRe.

The easiest way to test this is to use the webapp and create / import objects manually.

## Manual Import of Objects

Browse MIR webapp, e.g. <http://localhost:8081/mir> and log in with an user with publishing rights (e.g. the admin account).

In the main menu click "Submit Document - Publish".
Now you are requested to fill out the metadata informations for the object:

![MIR Submit Object](/assets/topics/server/mycore/mir-submit-object-01.png)

After filling (at least the required) fields, save it with button "Save".

On the next page you can upload files for the object.
In our case we had a PDF file at hand that we uploaded.

After this the object is stored in state "submitted". This means you can search and see the document
as long as you are logged in. But (not logged in) users won't see the object.

(Try this: log out and search (empty search). No documents will be found.)

## Publish Object

To finalize document submission and publish the object, log in, search for the object and click on the details for it.

In the right column click on "Actions" and choose "Edit in admin editor".

Set the "Publication state" to "published" and "Save" the document.

If you log out now and search, you will find the object.

![MIR Publish Object](/assets/topics/server/mycore/mir-submit-object-02.png)
