It is advised to familiar yourself with the basic concepts of 101worker
https://github.com/101companies/101worker/blob/master/README.md

The following installation guide assumes the usage of Ubuntu Linux. MacOs users should find 
the packages themselves. Windows is not supported at this point.

**Install Ubuntu Linux**

* sudo apt-get install git
* sudo apt-get install openjdk-7-jdk openjdk-7-jre-lib 
* sudo apt-get install haskell-platform 
* sudo apt-get install php5 
* sudo apt-get install python-pip
* sudo apt-get install gcc python-dev r-recommended
* sudo pip install nltk

git clone https://github.com/101companies/101worker
cd 101worker
make init

make production.debug -- this runs the modules in a VERBOSE mode, i.e you can
see their console output

the full list of modules is configured 
https://github.com/101companies/101worker/blob/master/configs/production.json


**Fixing issues step-by-step**

* sudo pip install mailer
* sudo pip install jinja2
* sudo pip install simplejson html2text inflect
* sudo pip install sqlparse tinycss pymongo

cabal update (get's the package list, needed for the valrious Haskell-based tools)

* sudo apt-get install mono-complete

DB Username & Password - put into ENV variable

sudo vi /etc/environment

* MONGODB_USER='CONSULT PRIVATE DOC'
* MONGODB_PWD='CONSULT PRIVATE DOC'

In case pull101 fails -- go into the admin panel of the 101wiki and check that there 
are no zombie contributions that don't exist on github. Normally, this can be
checked, by filtering the contributions w/o accociated wiki pages.

clone 101integrate separately

INSTALL NLTK

python
import nltk
nltk.download()

type all + ENTER
cd ${integrateDir}/../ ; git clone https://github.com/101companies/101integrate.git

Once the modules are tested, configure daily chronjob
crontab -e
0 20 * * * cd /home/worker/101worker && make production.run
0 23 * * * cd /home/worker/101worker && make report

TODO: 
Martin, explain how to configure services on Apache
