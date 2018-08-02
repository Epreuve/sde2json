# sde2json
Simple FuzzySteve MySQL SDE to JSON conversion.

### Edits by Ep

I came across this project and loved the idea of having the SDE tables in JSON form for a thing I'm working on.  Unfortunately, it seemed this was built as a component of a larger project and didn't initially work following the setup steps.  I'm in no way a PHP developer, but after a bit of hacking at it, I managed to get this to work as a stand-alone utility which was perfect for my needs.

The steps to get this working are as follows:

* Clone the Repo
* `cd sde2json`
* Install [composer](https://getcomposer.org/download/) if you haven't already, then run `php composer.phar install`.
    * If you don't have a MySQL DB, follow these steps first.
    * Download [MySQL](https://dev.mysql.com/downloads/mysql/) for your system.
    * Setup a DB to use for the conversion. You're just creating the DB, No need to create tables or whatnot.
    * Be sure to confirm that the `mysql` command works on your command line, you might need to add it to your path manually.
* Change `config.json` to match your DB configuration.  If you need to specify a socket to connect, you can add another key `dbSock` to set it, and the `MyDB.php` script will pick it up and act accordingly.
* Make one small change to `sde/update.sh` to add your username/password as needed to line `21`.  This will cause a warning later, but you're an adult.  Make smart life choices.

## Warning
The script doesn't use workers or chunks to break up and process the huge tables in the SDE.  This causes huge spikes in memory usage during the script.  You will very likely need to bump the `memory_limit` for your PHP install to complete the script.  I've included the `php_info.php` file in the root directory that should tell you what your current limit is, and where your `php.ini` file is so you can increase it if need be. You can run it with a simple `php php_info.php` to see what you're working with. 

I tried up to 768mb and still had issues, then went nuts and bumped it to 4gb (All on a local machine, not a server - lots to spare) and it finished fine, although I'm sure that was overkill.  You might need to tinker with it to find the right amount depending on your setup.

* Run `./sde/update.sh` and wait.
* Once you see `Conversion Complete!`, you should have all your `.json` files waiting in your `public` folder (Assuming nothing blew up).
* Enjoy!

A big thanks to cvweiss for taking the time to put the original project together.  It definitely saved me a lot of time.
    
### Original Setup Steps

After your ```git checkout cvweiss/sde2json``` modify config.json to your setup.

Run ```update.sh``` in the sde directory.

If you'd like automated updates setup the cronjob (configure to your system, this is mine)

    0       *       *       *       *       ~/sde.zzeve.com/sde/update.sh

Make sure your apache/nginx/whatever points to the public directory. 
