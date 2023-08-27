Docker Image
If you have Docker installed you can pull and run the Community Edition with one docker command:

docker run -d --name kk9600 -p 8780:8780 -p 8783:8783 konakart/konakart_9600_ce
This docker image contains the KonaKart demo store (Community Edition) with a pre-populated PostgreSQL database. After running the docker run command open a browser and see the storefront at:

http://localhost:8780/konakart/

The Admin Application is at:

http://localhost:8780/konakartadmin/

(login using “admin@konakart.com” as the username and “princess” as the password)

Helpful tips
Note that the Set-up programs can also be used in a “silent” (or “unattended”) mode if you prefer (or need to because you have no graphics environment). See the Installation instructions for more details.
For a summary of changes made between releases you can check the CHANGES.txt file which is included in each package.
Check the Release Notes for details on what to look out for when upgrading from previous releases.
Check Known Problems for the release that you’re planning to install.
Download Manager Users : Please note that we only allow a limited number of simultaneous connections so if you use a download manager to download these files, please restrict the number of connections you use to about 10. Thanks.
