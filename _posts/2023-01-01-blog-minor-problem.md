---
title: Blog Minor Problem - Solved
date: 2023-01-01 12:00:00 +0800
category: meta
---

# Blog Minor Problem - Solved
New year's day and I made a new post. I pushed it to GitHub but there was an error in the build process.
```
Incremental build: disabled. Enable with --incremental
      Generating... 
  Liquid Exception: undefined method `tainted?' for "FastReport to PDF Demo":String in /home/runner/work/blog/blog/vendor/bundle/ruby/3.2.0/gems/jekyll-theme-chirpy-5.3.2/_layouts/post.html
                    ------------------------------------------------
      Jekyll 4.3.1   Please append `--trace` to the `build` command 
                     for any additional information or backtrace. 
                    ------------------------------------------------
/home/runner/work/blog/blog/vendor/bundle/ruby/3.2.0/gems/liquid-4.0.3/lib/liquid/variable.rb:124:in `taint_check': undefined method `tainted?' for "FastReport to PDF Demo":String (NoMethodError)

      return unless obj.tainted?
```

I started to worry for a bit because I do not know Ruby and I do not know for sure if I can debug the issue. 
So I searched Google for the error message from the build log.
```
undefined method `tainted?'
```

The search results basically tells me that Jekyll is not compatible with Ruby 3.2. Upon inspecting the build log, it says that the builder is using Ruby 3.2 which further establishes the aforementioned incompatibility.

So I began to search my project files for a way to configure the Ruby version to use. I found `.github\workflows\pages-deploy.yml` to be promising. It contains the following line:
``` .github\workflows\pages-deploy.yml
ruby-version: 3 # reads from a '.ruby-version' or '.tools-version' file if 'ruby-version' is omitted
```
We can see that the project is configured to be built with the "3" major version. The next thing that came to my mind was to include the minor version and the revision number in the configuration. But I have to find the particular Ruby revision that was used by the latest successful build. So I clicked the "Actions" tab for the project's GitHub repository and then clicked on the latest successful build. I found the following log line:
```
Found cache for key: setup-ruby-bundler-cache-v4-ubuntu-22.04-ruby-3.1.3-Gemfile.lock-
```
So, Ruby 3.1.3 is what we are looking for. Thus, I changed `pages-deploy.yml` to have the following line:
``` .github\workflows\pages-deploy.yml
ruby-version: 3.1.3 # reads from a '.ruby-version' or '.tools-version' file if 'ruby-version' is omitted
```
My next step was to push my changes back to GitHub. The project was now built successfully and site visitors may now read the latest update to my blog.