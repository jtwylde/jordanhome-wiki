Gollum is the app that runs the wiki (this one!).
Gollum is backed by a file-based store, with all content written in markdown pages. The repo for the wiki is public, located [here](https://github.com/jtwylde/jordanhome-wiki)

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
[Gollum repo](https://github.com/gollum/gollum)
[Gollum wiki](https://github.com/gollum/gollum/wiki)