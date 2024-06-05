---
---
I'm a big fan of [Fossil](). The only reason I use other Git services like Github is because others use it. Though I have to admit that it's easier to click a button to create a repo than to add a new repo to a self-hosted server.

A couple nice features of Fossil are:

- You can run the full repo on your local computer and push your work later.
- You can use arbitrary CGI extensions written in the language of your choice. While you can use local scripting with any Git repo, it's not integrated with Github.

You can start the server with

```
export FOSSIL_HOME=/home/user
fossil server --nocgi --localhost --extroot=/home/user/repo/ext
```

The first line tells Fossil where to find the [configuration database](https://fossil-scm.org/home/doc/trunk/www/tech_overview.wiki#configdb). The second line can be used with `fossil ui` as well (it will open a new browser window, which you don't want, for instance, when using WSL on Windows). Option `--nocgi` tells Fossil that it's not being run as CGI on your web server. That makes life easier by not running in a chroot jail. Option `--localhost` restricts requests to come from the local computer. You don't need that option with `fossil ui` because it's on by default. `--extroot=` is the full absolute path to the directory that holds your CGI scripts.

You can add all kinds of functionality to your project with CGI extensions. For instance, you can add a viewer for arbitrary file types that are converted to HTML using Pandoc. Or you can use the database to store various types of information (like notes) and query them from inside the project.