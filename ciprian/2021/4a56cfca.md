## -- title:       The `...` symlinks trick for "local aliasing"
## -- library:     ideas
## -- identifier:  4a56cfca
## -- format:      commonmark
## -- timestamp:   2021-09-11 18:31:02


## Summary

Local naming and resource aliasing is a major issue in publishing systems (like wikis);  however, I think it can be solved (and even implemented) with a simple trick borrowed from file-systems:  symlinks, more specifically what I call the `...` symlinks trick.

Although my main assumption here is that the wiki pages are stored on a file-system, thus their resources (images and such) are in fact files that must be resolved to paths during rendering, the same trick can still be applied (although without symlinks) to any publishing system that uses at least some form of "links".

For referring to this document, please use the following URL:
https://scratchpad.volution.ro/ciprian/992c7f2944456f18cdde77f683f49aa7/4a56cfca.html




## Context (for wikis)

When authoring a document, especially in wiki-like markup languages (like CommonMark, MoinMoin, etc.), one often needs to refer to other documents (for linking) or resources (for embedding, like images).

At the moment, with most wiki systems (all?) there are only a few options:
* refer to that resource by its "location" (be it relative to the authored document, or a URL);
* create a foot-note where one indicates the "link" (again by a "location"), and whenever one needs to refer to it, use the same footnote reference;
* use a sort of "macro" that when called expands into the "location";  (see [reStructuredText replacement directive](https://docutils.sourceforge.io/docs/ref/rst/directives.html#replacement-text) for an example;)

There are a few issues with all the above solutions:
* it easily leads to dead links;  if the referenced resource changes its "location", one needs to manually find-and-replace the URL in all documents;
* it only solves the problem locally (within the current document);  if one wants to author another document that references the same resource, the same solution has to be implemented once again;

(Note that the `...` symlinks trick doesn't completely solve this particular issue.  This section just describes a broader problem.  But, in particular when dealing with Markdown documents that span multiple directories and that are shared over GitHub, it could help at least with resolving resources, such as images.)




## Sidenote about symlinks

(Those that are accustomed to symlinks can skip this section.  Mind these are not Windows "shortcuts".)

Symlinks are extremely useful, especially in the UNIX family of OS's, because they can easily create "virtual file-systems" or "grafts" in a existing file-system structure.

Say one is working on two papers, that for some reason share the same images or perhaps common LaTeX files;  say these two papers are in different places on the file-system (for whatever reason).  It would be nice to have a way to share the files between these two papers without having to resort to absolute paths (that break once the file-system is re-arranged), and without a constant copy-paste from one place to another.

A nice solution would be this:  gather the common files in a third place (say in a handful of folders), and symlink those folders into the two papers' folders hierarchy.  For the publishing system the symlink is almost "invisible" and treated as what it points to.

For more details see [Wikipedia](https://en.wikipedia.org/wiki/Symbolic_link).




## The `...` symlink trick (for storage)

(This is a technique I often use with projects that should access folders on quite different storages, but I still want to keep paths relative to the root of the project.)

Have somewhere a `shortcuts` folder that contains symlinks to the various other folders a project should have access to;  for example:
* `temporary` -> `/tmp/some-project/temporary`;
* `database` -> `/mnt/some-ssd/some-project/database`;
* `artifacts` -> `/mnt/some-nfs/some-project/artifacts`;
* `artwork` -> `/mnt/some-disk/some-project-atrwork`;

Have in your project root folder a symlink called `...` that points to the previous folder.  Use this `...` as prefix of other symlinks it the current project to refer to the special folders;  for example:
* `...` -> `/home/some-user/some-project-shortcuts`  (this is the `shortcuts` folder described previously);
* `.outputs` -> `.../temporary/outputs`;  (say here our build system should write the compiled outputs;)
* `.logs` -> `.../temporary/logs`;  (we don't care about the log's persistence during development;)
* `.db` -> `.../database`;
* `.artifacts` -> `.../artifacts`; (here we copy the binaries for publishing;)
* `assets/images` -> `../.../artwork/images`;




## The `...` symlink trick for Markdown

Applying the same technique, one can have a `shortcuts` folder in a repository that will contain (relative) symlinks to other documents (part of the same repository) and resources (also part of the same repository).

Now in each folder that holds Markdown files, just create a symlink called `...` that points to this `shortcuts` folder.  Thus, when referring to a resource use the `.../picture-a.png` as a path.

For example:
* `shortcuts`:
  * `picture-a.png` -> `../images/2021/whatever.png`;
  * `picture-b.png` -> `../images/2019/something.png`;
  * `idea-1.md` -> `../documents/topic-x/document-1.md`;
  * `idea-2.md` -> `../documents/topic-z/document-2.md`;
* inside both `documents/topic-x` and `documents/topic-z`:
  * `...` -> `../../shortcuts`;

Note that one can have multiple `shortcuts` folders, perhaps for different topics, and that each `...` folder can point to one of these.
Moreover, symlinks from one shortcut folder can point to other symlinks in another shortcut folder.

All this should work even with GitHub's native Markdown renderer;  however, one can always modify the Markdown to HTML generator to treat these links specially and replace them with proper "locations".  (Also see the `some-document.url` suggestion in the next section.)




## The `...` symlink trick for HTTP (static) servers

This trick should definitively work with any static HTTP server (i.e. Apache, Nginx, etc.).

However, one could write a small static HTTP server that when it encounters a `...` path, it resolves it, and then can just redirect to the proper location.

Bonus points if one implements the following twist: if the `.../some-document.html` resolves to a folder like `redirects/some-document.url`, then take that file's contents (which should be a well formed URL) and use that as a redirect location.
