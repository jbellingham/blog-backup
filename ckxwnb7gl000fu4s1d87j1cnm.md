## This Week I Learned: yarn why

## This Week I Learned

How to analyze an npm package's inclusion in a project.

### Context

I had a situation recently where I was trying to update an npm package in a yarn-based project I'm working on. I ran into trouble when I noticed that running `yarn upgrade [package name]` was not updating to the version I was expecting. I hadn't encountered this problem before, so I was a little stumped on it for a while. I was eventually able to track down a transitive dependency in the project that was pretty out of date and was limiting what version my package could be upgraded to.

### Introducing -- yarn why

I queried it with my teammates, and one of them suggested I run `yarn why [package name]` to see where it was coming from, here is a sample output:


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1641088932740/TkZ0URmQv.png)

Helpfully, the output of the command lists "Reasons this module exists", pointing to precisely where in your repo the package is imported, including when the dependency is a transitive dependency:


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1641089232354/Yk0MRhtiq.png)

Notice that the reason for `archiver`'s existence in this repo is because it is a dependency of `webdriverio`.

Yarn why will also list out if a package is referenced multiple times:

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1641090867781/gIyZyEYAW.png)

Useful for tracking down package versions across a monorepo.