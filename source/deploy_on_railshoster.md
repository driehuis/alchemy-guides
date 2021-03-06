Railshoster Alchemy deployment
------------------------------

[RailsHoster](http://www.railshoster.com) offers durable rails hosting
for good money. In this guide you will learn how to deploy your Alchemy
Site on your RailsHoster server. Please be sure that you have read the
[Getting Started Guide](/getting_started) to be familar with the Alchemy
installation steps. After finishing this guide you will know:

-   How to initialize your local Alchemy installation so it is ready to
    be deployed on RailsHoster
-   How to deploy changes you’ve made to your server

endprologue.

### Prerequisites

We assume that you already have ordered a hostingplan on RailsHoster.

If not please go to
[railshoster.com](http://www.railshoster.com/web_hosting) and choose a
plan that suites your needs.

INFO: Alchemy runs with all plans, even the smallest one starting a only
€ 4,95.

### Install the railshoster gem

<shell>\
\$ gem install railshoster\
</shell>

### Create a remote git repo

Create a new repository on [GitHub](https://github.com) or, if you have
one, into your own Gitolite/Gitosis server or where ever you have your
git remote repositories.

TIP: DO NOT EVER STORE ANY CRITICAL INFORMATIONS, LIKE DATABASE OR SSH
PASSWORDS INTO YOUR GIT REPOSITORY, UNLESS YOU HAVE A PRIVATE ONE!<br>\
Remember: GitHub repositories from the free plan are open to the public.

#### Init your project as git repository

Inside your projects folder enter:

<shell>\
git init .\
</shell>

Open the <code>.gitignore</code> file in your editor and add:

<shell>\
index/\***/**\
uploads/\***/**\
</shell>

TIP: You can pass <code>—scm=git</code> option to the Alchemy installer
while creating a new project. That option inits your project as git
repository and sets all ignores for you.

#### Add the remote repository

##### GitHub Example

<shell>\
git remote add origin git@github.com:repository-name.git\
</shell>

##### Repository server with a non standard ssh port

<shell>\
git remote add origin
ssh://git@your-reposerver.de:SSHPORT/repository-name.git\
</shell>

### Initialize your project for railshoster

<shell>\
railshoster init -a YOUR\_APP\_KEY .\
</shell>

NOTE: You received your app key for your rails app with your order
confirmation from railshoster.

### Add Alchemy and Rails 3.1 deploy tasks

Open the <code>config/deploy.rb</code> file with an editor and add:

<ruby>\
require ‘alchemy/capistrano’\
load ‘deploy/assets’\
</ruby>

#### If your repository server has a non standard ssh port

Change

<ruby>\
set :repository, “git@your-reposerver.de/repository-name.git”\
</ruby>

to

<ruby>\
set :repository,
“ssh://git@your-reposerver.de:SSHPORT/repository-name.git”\
</ruby>

### Commit and push deploy settings

<shell>\
\$ git commit -am ‘Added deploy settings’\
\$ git push origin master\
</shell>

### Run the setup task

<shell>\
railshoster deploy:setup\
</shell>

NOTE: If prompted, enter the mount point of Alchemy to the same you have
in your <code>config/routes.rb</code> file.

### Make your first cold deploy

<shell>\
railshoster deploy:cold\
</shell>

TIP: Sometimes the ssh-key forwarding does not work. If the deploy
script wants you to enter a password for the repository server then
something is wrong with the key forwarding. [RailsHoster has a
guide](http://help.railshoster.com/kb/getting-started/showcase-deploy-an-alchemy-cms-site-using-the-railshoster-gem)
how to fix this in their knowledge base.

### Check the installation

Open a browser and enter the domain the railshoster gem just showed you
after succesfull deploy.

TIP: You can always get your domain via the railshoster gem. Just run
<code>railshoster appurl</code> while in your sites folder.

### Next steps

Now you can start to customize your Alchemy site.

Just follow one of our [guides](/).

Everytime you have made a change you want to publish onto your server
just run:

<shell>\
railshoster deploy\
</shell>
