Site set up
===========

From scratch, clone and start a Python virtual environment. 

.. code-block::

   $ git clone https://github.com/wilsjame/weblog.git
   $ cd weblog
   $ python3 -m venv .venv
   $ source .venv/bin/activate

   Sweet! Expect your prompt to look something like this
   (.venv) sashabraus@laptop:~/weblog $

   $ pip install -r requirements.txt
   $ sudo apt install pandoc

   pandoc is a markup converter required by nbsphinx
   to output Jupyter Notebooks in html.

Build html
-------------

.. code-block::
   
   $ cd docs
   $ make html

   Everything to deploy a static site is in build/html
   Refresh pathto/weblog/docs/build/html/index.html in your browser!

Add entry
---------

1. Create a new .rst (reStructuredText) file in the source directory
2. Add the new .rst file to index.rst

.. code-block::
   
   $ cd docs/source
   $ vi sitesetup.rst
   $ vi index.rst

Given this index.rst snippet, sitesetup will appear after the binarysearch entry in the build html.

.. code-block:: rst
   :emphasize-lines: 6
   
   .. toctree::
      [...]

      welcome
      binarysearch
      sitesetup

Deploy
------

The current devops stack is:

local -> github -> travis ci -> aws s3 bucket -> public domain

Travis-CI
^^^^^^^^^

Sign up with your GitHub account and add a .travis.yml file to the root of the 
repository. Every main branch push to GitHub, Travis builds and deploys the
site html to a public facing S3 bucket. 

.. code-block:: yaml

   # working example .travis.yml
   os: linux
   dist: xenial
   language: python
   python:
     - "3.8"
   install:
     - pip install -r requirements.txt
   script:
     - cd docs &&
       make html
   deploy:
       provider: s3
       access_key_id: $AWS_ACCESS_KEY_ID
       secret_access_key: $AWS_SECRET_ACCESS_KEY
       bucket: "wilsja.me"
       edge: true # opt in to dpl v2
       local_dir: ./build/html
       on:
           branch: main

The AWS (IAM) access key environment variables are set on travis-ci.com from
the project's settings page.

Host and DNS
^^^^^^^^^^^^

The s3 -> domain step involves redirecting requests to a domain name, purchased
from a registrar, to the S3 bucket serving site html. Set up varies depending on
one's choice of hosting and domain name services. I registered this domain with
Namecheap, but consider keeping hosting and domain name services under the same
umbrella for likely (well, hopefully!), easier integration and maintenance.
