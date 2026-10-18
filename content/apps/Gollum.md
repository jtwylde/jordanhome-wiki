Gollum is the app that runs the wiki (this one!).
Gollum is backed by a file-based store, with all content written in markdown pages. The repo for the wiki is public, located [here](https://github.com/jtwylde/jordanhome-wiki)

## Sidebars, Footers, Headers
Gollum supports creating special 'subpages' that, when present in a directory, will add sidebar, header, or footer onto all pages in that directory. They can be created/editing by naming the page **_Sidebar, _Header, or _Footer**

## Tags
The following tags/links are supported:
- '[[page]]: links to an internal page within Gollum
- '[[title\|page]]: links to an internal page within Gollum with title
- '[[page#anchor]]: links to a specific anchor on the page (i.e a heading)
- '[[https://google.com]]: links to a website
- '[[google\|https://google.com]]: links to a website with title
- '[[_TOC_]]: shows a pages table of contents


## Macros
Macros are additional functions that substitute in content. Gollum ships with default macros, and custom ones can also be [[created|https://github.com/gollum/gollum/wiki#custom-macros]].

See the [[Gollum wiki|https://github.com/gollum/gollum/wiki/Standard-Macros]] for a detailed list of default macros.

## App Setup
In the cluster, Gollum is configured to clone the repo on first startup using an `initContainer`. 
The main pod running Gollum is configured with a hook to push to the repo on all changes.
```
Gollum::Hook.register(:post_commit, :hook_id) do |committer, sha1|
  committer.wiki.repo.git.pull('origin', committer.wiki.ref)
  committer.wiki.repo.git.push('origin', committer.wiki.ref)
end
```

## References
- [Gollum repo](https://github.com/gollum/gollum)
- [Gollum wiki](https://github.com/gollum/gollum/wiki)