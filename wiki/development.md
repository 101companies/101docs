### Installation

Clone the repo code:

```
git clone https://github.com/101companies/101rails.git
```

Move into the wiki folder

```
cd 101rails
```

Install native dependencies:

```
apt-get install curl nodejs build-essential libxslt-dev libxml2-dev mongodb zlib1g-dev libreadline-dev libssl-dev libcurl4-openssl-dev bundler
```

Bundle, here you might need your system password:

```
bundle install
```

#### load database dump

Clone the repo containing the zip

```
git clone https://github.com/101companies/101zip.git
```

Restore the dumped zip file

```
mongorestore --directoryperdb 101zip/full_backup.zip --drop
```

#### Starting the server

You can start the server by running 

```
rails s
```

Alternatively you can access the console of rails by using

```
rails c
```

For further info, consult the according [Rails Doc](http://guides.rubyonrails.org/v3.2.19/)

By default rails runs in development mode, in development you can easily gain admin acess by clicking
the local login button in the task bar. After you gained an admin session you can access the admin area
by opening [locahost](http://localhost:3000).

