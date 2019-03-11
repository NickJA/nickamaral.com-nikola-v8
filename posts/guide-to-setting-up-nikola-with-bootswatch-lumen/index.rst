.. title: Guide to setting up Nikola with Bootswatch Lumen
.. slug: guide-to-setting-up-nikola-with-bootswatch-lumen
.. date: 2019-03-10 22:09:29 UTC-04:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text

Guide to setting up Nikola with Bootswatch Lumen    
================================================

**Last Updated:** June 10, 2018

Overview
--------

This guide is a place for me to document the process I have 
gone through to use Nikola, Bootswatch, Github Pages and 
Google Domains to generate and host a static blog on nickamaral.com_.

.. _nickamaral.com: http://nickamaral.com

.. image:: /images/getnikola.png

I am writing this guide from my Dell XPS 13 9360 running Windows 10.  
I've installed the Windows Subsystem for Linux, Ubuntu, and Visual Studio Code, after which I installed Python 3.6, vitualenv and some other dependencies.  
I will document the process I went through to get WSL working in my next post.  

Update with Python now being in Windows store
---------------------------------------------

Trying to get this going using Python 3.7 from the Windows store.  After installing Python 3.7 from the Windows Store.

.. code:: python

    python -m venv nickamaral
    nickamaral\scripts\activate.bat
    pip3 install --upgrade pip
    pip install --upgrade "Nikola[extras]"

Installing Nikola
-----------------

This was my first project on my fresh WSL/Ubuntu setup so I needed to setup a virtualenv and virtualenvwrapper process to support my project workflow.
I was using a great guide_. from Liam at smallsec.  What I learned the most from the guide was the edits suggested for ~./bashrc

.. _guide: https://blog.smallsec.ca/2017/09/07/installing-python3-6-on-wsl/

Here is the code I ran to install python and virtualenv / virtualenvwrapper. 

.. code:: python

    sudo add-apt-repository ppa:jonathonf/python-3.6
    sudo apt update
    sudo apt install python3.6 python3-pip

    sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.5 1
    sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.6 2

    sudo -H pip3 install --upgrade pip
    sudo -H pip3 install virtualenv virtualenvwrapper

Edit your ~./bashrc.  In the below, nicho is my user folder in Windows and /mnt/c is specific to how WSL interacts with the Windows file system. 
These edits enable a lot of the magic in using virtualenv, via workon 'project name', especially on my WSL environment.  

.. code:: python

    sudo nano ~./bashrc

    export WORKON_HOME=$HOME/.virtualenvs
    export PROJECT_HOME=/mnt/c/Users/nicho/Code
    export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3.6
    source /usr/local/bin/virtualenvwrapper.sh

Next I needed to create my viritualenv for Nikola.   First create and navigate to the root of the project folder where you want to create the new viritualenv. 

.. code:: python

    mkdir newproject
    cd newproject
    mkvirtualenv newproject
    workon newproject

Once the new project is activated you can install nikola.  This step has a lot of dependencies so it will take a bit. 

.. code:: python

    pip install --upgrade "Nikola[extras]"

From here I followed the `Nikola - Getting Started`_ instructions. to setup a new nikola site in my project directory.  These steps could change, so I will just refer back to their guide.

.. _Nikola - Getting Started: https://getnikola.com/getting-started.html

Once the basic site was setup, I wanted to change the default template to `Bootswatch Lumen`_.  I ended up following this post_ to learn the steps involved in changing the basic template from bootstrap3.

.. _Bootswatch Lumen: https://bootswatch.com/lumen/
.. _post: https://necromuralist.github.io/posts/how-to-change-the-nikola-bootswatch-theme/

Here are the steps to get the theme up and going:

.. code:: python

    nikola install_theme bootstrap3
    nikola bootswatch_theme -s lumen
    nikola build

Ultimately, I wanted to host this site on github user pages.  Nikola has a great deploy process that integrates perfectly with github. 
I will write a separate post to explain the steps involved in setting up my github with ssh on WSL.  `Deploying to github`_ is very simple. 

.. _Deploying to github: https://getnikola.com/handbook.html#deploying-to-github

.. code:: python

    git init
    git remote add origin git@github.com:user/repository.git

Edit the conf.py for the following settings: 

- GITHUB_DEPLOY_BRANCH is the branch where Nikola-generated HTML files will be deployed. It should be gh-pages for project pages and master for user pages (user.github.io).
- GITHUB_SOURCE_BRANCH is the branch where your Nikola site source will be deployed. We recommend and default to src.
- GITHUB_REMOTE_NAME is the remote to which changes are pushed.
- GITHUB_COMMIT_SOURCE controls whether or not the source branch is automatically committed to and pushed. We recommend setting it to True, unless you are automating builds with Travis CI.

Create a .gitignore file. We recommend adding at least the following entries:

.. code:: python

    cache
    .doit.db
    __pycache__
    output

Run nikola github_deploy. This will build the site, commit the output folder to your deploy branch, and push to GitHub. 
In my case, the master branch contained my output.  I next needed to setup CNAME on Google Domains and my static files.  
The CNAME file needs to be added to the nikola files directory.  This will make sure it is uploaded to the output folder during deployment. 
In my case, the cname file contained the following: 

.. code:: python

    nickamaral.com
    www.nickamaral.com

And on Google Domains, I logged in, navigated to manage domains and clicked Configure DNS entry for the domain.  The entries I needed to add where the following: 

.. code:: python

    @ A 1h 192.30.252.153
    @ A 1h 192.30.25
    www CNAME 1h https://nickja.github.io.

And finally, in order to start a new post on the site:

.. code:: python

    nikola new_post

I will keep this post updated if I make any changes. 

-Nick