# Using GitHub API Tokens for programmatic access {#api-tokens}

In addition to caching HTTP or GIT credentials for _RStudio_ to connect to
GitHub, you may also want to store credentials so that your R _code_ can connect
to GitHub.  One reason to do this is to install R packages that are in private
GitHub repositories using the **devtools** package. The `devtools::install_github()`
function connects to GitHub's **A**pplication **P**rogramming **I**interface (API) to
install packages directly from GitHub repositories, and it needs credentials
if those repositories are not public.

The pattern described here is used by many R packages that connect to web
services that require login: we get an API _key_ or _token_ - essentially a long password -
from the web service, and store it in a file _outside our code repositories_.
This ensures that we never accidentally commit our key to someplace online
where others can see it. Whever R starts up, it reads this file and our keys
are stored as _environment variables_. These are like variables in our R session,
but they are stored separately in memory and are generally not changed
during sessions.  Environment variables are often set by your operating system,
letting R know what type of operating system it is working in.

## Step-by-step

-   One you have a GitHub account, go to <https://github.com/settings/tokens> to
    create a **p**ersonal **a**ccess **t**oken (PAT). 

    The PAT is a long random string
    of characters. It is your key. When creating this PAT, you'll have the option
    to provide different levels of access.  Be sure that "repo" is checked off (it
    should be by default) when you create your PAT.  Copy your PAT to the clipboard.

-   Now, in R, run the following, replacing `YOUR-PAT OF-40-RANDOM-LETTERS-AND-DIGITS-GOES-HERE`,
    with your token from the clipboard.

    ```
    cat("GITHUB_PAT=YOUR-PAT OF-40-RANDOM-LETTERS-AND-DIGITS\n",
        file=file.path(normalizePath("~/"), ".Renviron"),
        append=TRUE)
    ```

    This commands writes `GITHUB_PAT=YOUR-PAT OF-40-RANDOM-LETTERS-AND-DIGITS` to
    a hidden file called `.Renviron` in your home directory.  If the file already
    exists, at adds it to the end.

-   Restart R (Session > Restart R in the RStudio menu bar), as environment variables
    are loaded from `.Renviron` only [at the start of an R 
    session](http://stat.ethz.ch/R-manual/R-patched/library/base/html/Startup.html).
    Check that the PAT is now available by running this command, which finds the environment variable
    called `GITHUB_PAT`.

    
    ```r
    Sys.getenv("GITHUB_PAT")
    ```

    You should see your PAT print to screen. 

Now commands you run from the 
**devtools** package, which knows how to look for `GITHUB_PAT`, can access private
GitHub repositories to which you have access, and you can install them with
`devtools::install_github('username/reponame')`.

## A note about security

Many other R packages that access web services give the option of storing API
keys as environment variables in your `.Renviron` file.  These include
many packages for accessing online data sources such as [those maintained by
rOpenSci](http://ropensci.org/packages/). If you use these packages, your
`.Renviron` file will collect many keys and tokens. These keys, and this file,
should _never be comitted to GitHub, stored in Dropbox, or on any other online service_.
They should not be in your code or code somments.
This is why this file is stored _outside_ your project folder. Your keys can be
used to access your accounts [and wreak all kinds of havoc](https://securosis.com/blog/my-500-cloud-security-screwup). It's OK
if you lose keys; as long as you log on to the site via a browser, you should
be able to revoke keys and generate new ones.
