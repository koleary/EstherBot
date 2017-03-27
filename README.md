        2016-05-09T14:08:46.601444+00:00 app[web.1]: module.js:433
        2016-05-09T14:08:46.601454+00:00 app[web.1]:     throw err;
        2016-05-09T14:08:46.601455+00:00 app[web.1]:     ^
        2016-05-09T14:08:46.601456+00:00 app[web.1]:
        2016-05-09T14:08:46.601457+00:00 app[web.1]: SyntaxError: /app/script.json: Unexpected token }
        2016-05-09T14:08:46.601458+00:00 app[web.1]:     at Object.parse (native)
        2016-05-09T14:08:46.601458+00:00 app[web.1]:     at Object.Module._extensions..json (module.js:430:27)
        2016-05-09T14:08:46.601459+00:00 app[web.1]:     at Module.load (module.js:357:32)
        2016-05-09T14:08:46.601460+00:00 app[web.1]:     at Function.Module._load (module.js:314:12)
        2016-05-09T14:08:46.601460+00:00 app[web.1]:     at Module.require (module.js:367:17)
        2016-05-09T14:08:46.601461+00:00 app[web.1]:     at require (internal/module.js:20:19)
        2016-05-09T14:08:46.601462+00:00 app[web.1]:     at Object.<anonymous> (/app/script.js:6:21)
        2016-05-09T14:08:46.601473+00:00 app[web.1]:     at Module._compile (module.js:413:34)
        2016-05-09T14:08:46.601474+00:00 app[web.1]:     at Object.Module._extensions..js (module.js:422:10)
        2016-05-09T14:08:46.601474+00:00 app[web.1]:     at Module.load (module.js:357:32)

    Did you notice the `SyntaxError` part? It looks like there's a problem in my script.json. If I inspect that file in github I'll see that indeed, I have a stray comma at the end if the second to last line.

    ![image](/img/script-error.png)

    After I remove that comma and redeploy, everything will return to normal.

## How do I deploy my fixes to Heroku?

Great question! Now that you've found your bug and fixed it, you want to redeploy your app. With Heroku you can trigger a deployment using git. Without going into detail, git is a code versioning system it's where github gets its name. Git is the software, github.com is a separate company that hosts git code repositories. If you're using a Mac you should already have git installed. Although git is a very complex tool, it's worth [learning if you're eager](https://www.atlassian.com/git/tutorials/what-is-git), but for this guide's purposes we'll be using only the most basic concepts of git, `pull`ing changes from a remote github repository, `commit`ing changes, and then `push`ing those changes out to a remote repository.

1. To deploy using git you first have to download a copy of your heroku app's code, like so:

        git clone https://github.com/your-github-username/your-app

    Note that git will prompt you to enter your github credentials.

2. This will create a new git copy of your code in a new folder. You can go into that folder like so:

        cd your-app

3. Now you can use the heroku toolbelt to link this git copy up to your heroku app with the following command:

        heroku git:remote -a your-app

4. Once that's done, you can now deploy to heroku directly from this directory. If you've made any fixes on github directly, be sure to sync them here like so:

        git pull origin master
        git push heroku master

5. You can also make changes to your local copy of the code. To do this, edit whatever file you wish in your preferred text editor, and then commit and push them up to github. You'll add a commit message, which is a short sentence decribing what you changed.

        git commit -a -m 'Your commit message'
        git push origin master

    Then, you can deploy those changes to heroku in the same way:

        git push heroku master
