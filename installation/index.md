---
layout: landing
category: Installation
published: true
title: Installing Textpattern
description: These are detailed instructions for installing Textpattern.
---

# Installing Textpattern

These are detailed instructions for installing Textpattern. The instructions follow a process of using a (S)FTP client, which you presumably have ([Transmit](https://www.panic.com/transmit/) is a popular choice for Mac, and [WinSCP](http://sourceforge.net/projects/winscp/) is for Windows). If you're familiar with installing CMS software and prefer the quick notes, see the [README.txt](https://github.com/textpattern/textpattern/blob/master/README.txt) file that's included in the install package. If you're more of a command-line jockey, the bits in [working with the development branch](http://docs.textpattern.io/development/textpattern-source-code-repositories) should suit you.

On this page:

* [Create your database](#create-your-database)
* [Download and unpack Textpattern](#download-and-unpack-textpattern)
* [Upload package to web server](#upload-package-to-web-server)
* [Setup and configuration](#setup-and-configuration)
  * [Select language](#select-language)
  * [Step 1: Set database details](#step-1-set-database-details)
  * [Step 2: Add config file](#step-2-add-config-file)
  * [Step 3: Populate database](#step-3-populate-database)
  * [Step 4: Go!](#step-4-go)

## Create your database

Before you can install Textpattern, you will need a MySQL database ready, and a user account with 'write' privileges to go with it. It's a hurdle you jump on your web host's side of things, but now is a good time to take care of it so it's not holding up the Textpattern install/setup process later. 

Record this information when you create the database and keep it handy. It will be used later in the setup process:

* database login
* database password
* database name

Depending on your web host, your web host account username (or ID, whatever) may be prefixed on the database name and database login. If this is the case for you, it will be evident when creating the database with your web host. 

For example, if your web host ID was your last name (`lname`) and you named your database `oceans` and chose `me` as a database login, then your actual database name and login to use in [Step 1](#step-1-set-database-details) would be `lname_oceans` and `lname_me`, respectively. 

## Download and unpack Textpattern

Now we begin the Textpattern part of the deal:

1. Create a folder on your local drive and name it something recognizable (e.g. `txp-new`). 
2. [Download](http://textpattern.com/download the latest release package) (either `.zip` or `.tar.gz`, as you prefer). 
3. Move the zip package to the folder you created.
4. Unzip the package.

The zip package will have a folder/file tree matching that in the [Textpattern repo](https://github.com/textpattern/textpattern).[^1] Following are the folders and files you'll see, pay particular attention the to `.htaccess` file.[^2]

<div class="tabular-data" itemscope itemtype="http://schema.org/Table">
    <table>
        <thead>
            <tr>
                <th scope="col">Item</th>
                <th scope="col">Type</th>
                <th scope="col">Removable?</th>
                <th scope="col">What it’s for</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <th scope="row">files</th>
                <td>directory</td>
                <td>no</td>
                <td>Empty by default. It’s where content files (<code>.pdf</code>, <code>.docx</code>, <code>.rtf</code>, <code>.epub</code>, <code>.txt</code>, etc.) will go when/if you upload them in the <strong>Files</strong> panel. You may see a warning about the file's <code>chmod</code> status (editing rights) in the <strong>Diagnostics</strong> panel.</td>
            </tr>
            <tr>
                <th scope="row">images</th>
                <td>directory</td>
                <td>no</td>
                <td>Empty by default. It's where images will go when you upload them in the <strong>Images</strong> panel.</td>
            </tr>
            <tr>
                <th scope="row">rpc</th>
                <td>directory</td>
                <td>yes</td>
                <td>Contains the XML-RPC functionality, which is vestigial code from when ping-packs and such were popular with bloggers. If you don't use it, you can remove it, but make sure the "Enable XML-RPC server?" preference is set to "No" in the <strong>Preferences</strong> panel.</td>
            </tr>
            <tr>
                <th scope="row">sites</th>
                <td>directory</td>
                <td>yes</td>
                <td>Supports multi-site installations managed by one administration side. It contains a template and instructions (a README.txt) for doing so. You can safely remove this directory if not wanted.</td>
            </tr>
            <tr>
                <th scope="row">textpattern</th>
                <td>directory</td>
                <td>no</td>
                <td>Essential. Contains all the core scripting and functionality. Also where users log into site administration from the front side.</td>
            </tr>
            <tr>
                <th scope="row">.htaccess</th>
                <td>file</td>
                <td>no</td>
                <td>Important for handling many things, like default URL formatting, 301 redirects, specific use (or not) of "www.", and so forth. You can add to the file, but you should never alter or remove the default content, which Textpattern relies on.</td>
            </tr>
            <tr>
                <th scope="row">index.php</th>
                <td>file</td>
                <td>no</td>
                <td>Essential. It functions as the front end of your Textpattern site. It's where web users arrive and and interact with the dynamic publishing.</td>
            </tr>
            <tr>
                <th scope="row">css.php</th>
                <td>file</td>
                <td>no</td>
                <td>Negotiates the front-end stylesheets you create in the <strong>Styles</strong> panel.</td>
            </tr>
            <tr>
                <th scope="row">README.txt</th>
                <td>file</td>
                <td>yes</td>
                <td>The file providing brief instructions for installing and upgrading Textpattern.</td>
            </tr>
            <tr>
                <th scope="row">HISTORY.txt</th>
                <td>file</td>
                <td>yes</td>
                <td>The file showing the package release Changelog.</td>
            </tr>
            <tr>
                <th scope="row">LICENSE.txt</th>
                <td>file</td>
                <td>no</td>
                <td>The GNU GENERAL PUBLIC LICENSE agreement. It doesn't impact functionality, but leave it alone.</td>
            </tr>
        </tbody>
    </table>
</div>

[^1]: Do not move files in the tree or change their names. Doing so will render Textpattern useless.

[^2]: This type of file is a 'hidden' server file, meaning it won't appear in certain file managers unless the file manager is configured to show them. For example, if you setup local development on your Mac laptop, this file won't appear in Finder unless you turn hidden file functionality on. The same goes for certain FTP clients, which hide these files until you change settings to show them.

## Upload package to web server

If you intend to use Textpattern to manage the entire website, you'll upload the install package to your web root (i.e., the root of your web domain). For many hosts, the path to the root looks like this: */users/home/{username}/web/public/*, where {username} is your user account name. You would upload the Textpattern package to the `/public` directory (equal to `yourdomain.tld`). 

You may, of course, install it in a sub-directory if you're only using Textpattern to run a blog, for example, as part of a larger site (generally people who do that end up [moving the installation](http://docs.textpattrn.io/installation/moving-textpattern) later).

You will use FTP, or Secure-FTP (SFTP) if your web host requires it, to upload the package to your desired web server location. Two capable clients are WinSCP for Windows and Transmit for Mac.

When ready:

1. Connect to your web server via the FTP/SFTP client and navigate down to the directory where you intend to install Textpattern.
2. If not done automatically by the FTP/SFTP client, make a second connection to your local drive and navigate to where you have the unzipped Textpattern install package waiting.
3. Select the package file tree (the folders and files *inside* the unzipped version number folder) on your local drive and drag/drop them to your web server directory.

## Setup and configuration

Now you'll walk through the setup process, which is a series of screens. Have you're "database information":#sec1 ready. Assuming you uploaded the install file tree to your web root, open a browser and go to *http://domain.tld/textpattern/setup/*. (If you installed Textpattern in a subdirectory, the path would reflect that.)

### Select language

You should now be looking at the default installation screen, which shows the Textpattern logo, a language selection menu, and a **Submit** button.[^3] The language you select will be used for the setup process, and become the default for your Textpattern UI.[^4]

Choose the language you want and click the **Submit** button.

[^3]: If you don't see this screen, check your URL path. Should it be `https` or `www.`? Did you install in a sub-directory? Is there a typo?

[^4]: You can change the UI language any time after setup in the [**Preferences**](http://docs.textpattrn.io/administration/preferences-panel) panel.

### Step 1: Set database details

The next screen, the first of four installation steps, asks for your MySQL database details and site URL

#### MySQL

The MySQL details are those you should have noted down when creating the database. The data asked for is:

* login (database user name)
* password
* server (use `localhost`)
* database (database name)
* table prefix (optional)

As mentioned at the beginning of this doc, some web hosts automatically prefix `database_` and database `login_` names with your host account username. Be sure to check for this.

The *server* is almost always `localhost`. It would be unusual if it wasn't, so try that first if your not sure.

The screen will say you don't need to provide a *table prefix* if this will be your only installation of Textpattern. This assumes you only have one MySQL database available to you. If that were true, adding a prefix here (e.g. `txp1_`) would allow you, if desired in the future, to add another Textpattern installation (e.g. `txp2_`) to your database without conflict with the original tables. But it's common anymore for web hosts to provide many databases, in which case no prefix would be necessary. Either way, this field is optional. If you do use a prefix, a sensible convention is something short ending with an underscore.

#### Site URL

Textpattern should have filled this in automatically by detecting where your installation files are relative to your web root. Just make sure it's correct.

When ready, click the **Next** button.

If there was a problem with your database information, you'll be notified about it now and won't advance to the next screen. You won't be told what the exact problem is either, but it will either be a typographical error in the data you entered, or there's a problem with your database setup, in which case you should submit a support ticket to your web host.

### Step 2: Add config file

If all went well in the previous screen, you'll advance to screen 2, which is composed of two sections: **Checking database connection** and **Creating config.php file**.

#### Checking database connection

This part shows if Textpattern has actually communicated successfully with your database using the information from the previous screen. If so, you'll see this:

<span class="success">Connected</span> \\
<span class="success">Using {database} ({encoding})</span>

Where `{database}` is the name of your database and `{encoding}` is the character encoding your database is configured to use -- ideally `utf8`.[^5]

#### Creating config.php file

Then it's time to create your Textpattern configuration file in the */textpattern* directory. Textpattern makes this easy by generating the *config.php* file contents based on your successful connection details. The code will look like this, but with your own data values entered:

~~~ php
<?php
    $txpcfg['db'] = 'database';
    $txpcfg['user'] = 'login';
    $txpcfg['pass'] = 'password';
    $txpcfg['host'] = 'localhost';
    $txpcfg['table_prefix'] = '';
    $txpcfg['txpath'] = '/server/path/to/textpattern/install/directory';
    $txpcfg['dbcharset'] = 'utf8';
    $txpcfg['client_flags'] = 0;
?>
~~~

Proceed as follows:

1. Use your S/FTP client to connect to your web host, navigate to the installation directory, and open `/textpattern/config-dist.php` in your text editor of choice.
2. Copy the code that Textpattern generated for you in the current screen.
3. Paste the code into the `config-dist.php` file, completely overwriting its entire contents. 
4. Save the file, close it, then rename it by removing `-dist` from the file name, leaving `config.php`.

Return to the **Add config file** screen in your browser and click the **I did it** button.

[^5]: If you want to change your character encoding, you'll need to do that at your database. But finish installing first, then change the database encoding later.

### Step 3: Populate database

This screen, step 3, is where you create your Textpattern administrator account. Doing so also populates the database with the necessary Textpattern tables and the CMS–database bridge is complete. The screen has two sections: **Creating database tables** and **Site configuration**.

#### Creating database tables

To create the administrator account and generate the database tables, add the following information, and remember the login/password (you'll need it to log in):[^6]

* Full name (e.g. Jane Doe)
* Login name
* Password
* Email address

#### Site configuration

Under this area, you're asked to select the admin-side theme you want to use. The default theme is Hive. You can change the admin-side theme choice at any time in [Preferences](http://docs.textpattern.io/administration/preferences-panel) after logging in.

When ready click the **Next** button.

[^6]: The login name and password should *not* be the same login and password used for your database.

### Step 4: Go!

The final step is essentially a confirmation screen with a link to the administration login location for your new Textpattern site. Congratulations! You've installed one of the best open source CMS systems available.

The screen will also suggest deleting the `/setup` directory for security reasons, and troubleshooting your diagnostics. But when first logging in, Textpattern will land you at a location not mentioned in this screen, so here's the optimal course of action to take once you arrive at this confirmation step:

1. Use your S/FTP client to connect to your web server. Go to the `/textpattern/setup` directory, and delete it (contents too). This eliminates one potential diagnostic warning you would otherwise see later.
2. Return to your browser screen and click the "Main interface" link, which takes you to the administration login location. Log in using your new administrator account details (entered on the previous install screen), and check the box for remembering you, if you like. 
3. The **Go!** screen says to go to **Diagnostics**, but you'll actually land on the [Languages](http://docs.textpattern.io/administration/languages-panel) panel, where you add new languages and change the default, if desired.  
4. Proceed to the [Diagnostics](http://docs.textpattern.io/administration/) panel, as Textpattern suggested, to troubleshoot any warnings and errors you see.
5. Go to the public side of your website (the homepage), easy to do by clicking its name in the navigation bar of any admin-side location. Read the tips and suggestions provided on the default article titled, 'Welcome to your site'. They're based on the experience of veteran users and intended to fast-track your efforts at building your first Textpattern website.
