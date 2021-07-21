## Why I switched from a T-SQL database to a GIT based one

***I lost all my posts*** ... that's more or less the root of my decision. A mix of carelessness and incompetence on my part that could have been avoided through the use of a different technology.

If you don't care about my personal history, just skip this whole section

### The old architecture

I had a blog before this one, which was hosted on managed VPS with a SQL Server database running on its own VPS. At some point, all of my SQL Server's data disappeared (for reasons I shall not detail here) and I of course had not made a single backup, ever. 

#### Story time !

This reminded me of the olden days, when I was not yet a "professional developper" and still deployed my code through a direct FTP upload (not even secured !). The "subversion" I did back then was zipping my code and storing these archives on my hard drive. 'Till I formatted that HDD and incidentally lost a good chunk of my projects. This event pushed me to search for a better solution, so that this wouldn't happen ever again. 
GIT solved that particular problem, and so when something very similar happened to me with my blog's database, I had the same reaction of searching for a way to ensure it wouldn't happen again. Of course, I first though about actually making backups of my database, but that's a lot of hassle just to keep using a "proper" database.

So, I pondered over the actual benefits of this Transact SQL database I was using, what where the alternatives and what were the tradeoffs. Thus, I realized that my blog didn't *need* an RDBMS, just a way to store a few markdown files and some JSON. Git suddenly seemed like a **very** sensible idea: It's mainly used as a versioned text-based storage after all.

 Incidentally, that questioning of SQL Server lead me to migrating my other sites toward PostgreSQL, but that story will be for an other post !

### The current architecture

This blog is, at the time of writing this post, a .NET Core 3.1 Blazor server-side web application running inside a Docker container on Kubernetes. It has no attached resources: No database container, no volume of any kind. The local repository lives within the container itself and is re-cloned on startup then periodically pulled. This makes for an extremely simple architecture and cost-effective hosting method with very few possible points of failure. 

## A fully-featured DB isn't always needed

My former blog used a SQL Server database, which is what I consider to be a "fully-featured" database. Incidentally, it required the host machine to have a minimum of 2GB of memory available **just to start the application**. That's a lot of dedicated resources for a simple blog with just a few posts. For this reason and as mentioned above, my other applications are being switched to PostgreSQL which is an other fully-featured DB, simply a tad lighter to run. 

Database servers such as SQL Server & Postgres provide a lot of amazing advanced features, but they come at a cost. The concurrent access, high search and write performances, entities relationships, complex querying and other such features are entirely superfluous when all you need is a single application to do nothing more than list and display plain text that would be represented as a single table. 

## Plain text files suffice, and might actually be the most sensible approach

A blog article is, essentially, a text file and at the core a blog is little more than a collection of articles. Building a whole admin with a WYSIWYG is a lot of hassle just to write some text files. Not building an admin if the blog content is hosted in a RDBMS would make the creation and edition of blogs a fairly unpleasant experience and so ,for all these reasons, I came to the conclusion that basing the core of my blog's content on plain text files, namely markdown and JSON is the best option.

### Lighter application, more reliability

By avoiding having to create a full administration area I once again remove a part of complexity reducing the amount of custom code that requires testing and maintenance. Relying on other people's code to handle the edition of my content lets me take advantage of the time and energy that has already been spent in making their software stable and reliable, giving my blog an other level of simplicity and reducing once again the number of possible points of failures.

### Accessibility & portability of plain text format

By using plain text in hosted GIT repository, currently on Gitlab, makes it easy to access, create and alter posts on my blog from any device without the hassle of building a responsive administration section. This also provides me with the issue tracker feature that serves as a todo-list and all the repository statistics. Additionally a GIT repository is very easy to switch solutions if I ever decided to use Github, a self-hosted remote or any other solution.

### Flexible edition enhancement 

Why use a WYSIWYG when technologies such as markdown exist that allow us to reach more or less the same level of rich content but with a much wider range of tools available for editorial work ? I'm currently typing this blog in Typora, a software dedicated to markdown edition. Once again, taking advantage of someone else' hard work to make this software stable, reliable and easy to use. 

By using markdown I get more freedom in what interface I use to create content and by **not** saving in the database "compiled" HTML (as most WYSIWYG) would I have far more freedom in post-processing my content via markdown plugins.

### Offline availability 

I'm writing this blog post using my "normal" environment despite not being connected to the internet and without having built an offline-ready AMP web application or desktop application. I can just write and commit files as per usual and I'll push them to the repo as soon as I get access to a network. This means I have no fear of losing work while typing in the train and don't have to disrupt my workflow by switching to a degraded offline-mode editor, as my editor is itself offline by nature.

## Then why custom-make a blog ?

You'd be totally justified in asking this question, considering I've used about a thousand word to say "use as much pre-build free stuff as possible". Here are the reasons:

### I am not bound to a particular company or technology

My git repository is hosted on Gitlab and the blog's code is running in a docker container on a Kubernetes cluster provided by Digital Ocean. If I chose to move to github for my repo and an Azure Web Application Service for my hosting it could be achieved in under an hour. In one word: flexibility.

### I have full control over the application's front-end

This one might sound surprising as this blog right now has about 23 lines of custom CSS. But the "front-end control" is not so much about design as it is about user experience. I am still working on the core of the blog and have some big rarely seen features planned that I couldn't have found anywhere else.

## Let's wrap it up !

I moved away from SQL Server specifically because I found it too heavy compared to other relational databases.

I move my blog away from relational databases altogether because I found they were too feature-rich for my needs, which made them more complex to setup and use than made sense. Removing the relational DB also reduced the amount of infrastructure required, which lead to cheaper hosting & more reliability. 

I used a GIT repository as my source of data & truth as keeping its current state is the core functionality of git. Being based on plain text files it was very easy to re-purpose as a database back-end.