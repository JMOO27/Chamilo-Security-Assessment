# Phase 2: Chamilo Setup

Now that the underlying infrastructure was ready to go, it was finally time to clone the repository and get the LMS hosted on Apache2 so I could begin my investigation.

I started by downloading the archive file from the official GitHub repository, extracting it into Apache2's `/var/www` directory, creating a virtual host, and following the CLI quick-install instructions from the official repository.

Within 15 minutes of starting, I was finally able to connect to my server's local IP address through my web browser, where I was greeted by the installation wizard. You'd think the hard part was over, right? Wrong!

By step #2 of the installation wizard, I was greeted with a dashboard of dependencies and recommended configurations, some of which had not been fulfilled. According to the documentation, these were merely *suggestions*. However, attempting to press "Next" brought me back to the first step of the installation process. Trying again yielded the same result. Naturally, I thought perhaps the suggested configurations weren't merely suggestions after all. So, I did some research to figure out where I could find the `php.ini` file. As it turns out, this was a great exercise in using basic Linux CLI commands and managing file permissions.

After locating the file, I used Nano to apply the recommended configurations. I saved my changes and went back to see if I could proceed. Lo and behold... the same issue! At that point, I started thinking it might be some sort of session issue. I dug further into the configuration files, analyzed the browser's network responses through developer tools, and combed through the documentation. Nothing rang any bells.

After about an hour and a half of fruitless debugging, I was grasping at straws. I looked around the webpage and noticed that the installation wizard's logo was clickable and linked to another page. I clicked it, and suddenly I was brought to what appeared to be the correct version of the installation wizard. There were new buttons and, most importantly, a functioning "Next" button!

I made it through most of the installation steps until I encountered more issues, this time involving the MariaDB connection. After clicking "Next" on the database configuration page, I was met with multiple exceptions. Fortunately, these were much easier to debug. First, I checked whether MariaDB was even running using `systemctl status mariadb`. Everything looked good. Next, I checked whether I had actually created a database user account. Turns out, I hadn't!

I created the user account using some basic SQL commands I found online, granted it the necessary permissions for the Chamilo database, and thought I was all set. By this point, however, I kept encountering exception after exception. Although the debugging process made sense, I repeatedly had to navigate backward through the installation wizard to test whether I had resolved each error. Eventually, it wouldn't even let me do that.

I decided to restart the installation process entirely. But by some miracle, reentering the server's address brought me directly to a login screen.

It had... somehow worked?

I entered my administrator credentials and was granted access to the admin dashboard.