# dontshoot-custom-server
Custom server for Dont Shoot - The Messenger

Don't Shoot - The Messenger
Creating a custom server.
Don't Shoot is a free messaging app built for ultimate privacy. Every message goes through multiple layers of SHA256 encryption. 
I call one of these layers the Conversation Encryption Layer, and it's the most unique of the layers. Each conversation has a key, and each member of the conversation must have the same key entered on their device. The key is NEVER sent over the internet. Getting the key from one member to another is all up to each member to figure out. Maybe send it through the mail in a paper letter, send it through smoke signals, whisper it when you're next to each other in person. Whatever you choose, the fact that the key never goes through the internet makes it super hard for anyone to get to your messages.
But before I describe the app more, (I'll leave that to another post) let me focus on the task at hand. The app also allows anyone to create their own server for managing the messages. Everything except notification handling is managed on a custom server. This adds yet another layer of security, as you can manage your own server and KNOW that it's secure, instead of relying on anyone else. (or big tech)
Super Secret MessagingWhat we'll be using:
Ubuntu Server (I'll show Linode, but you can use whatever you want)
NGINX, Nodejs and Express for running the web server backend
MongoDB for the database

Getting the Server -( Skippable)
You can skip this part if you already have a Linux server
You can use a $5/month server from Linode ( https://www.linode.com/ ) 
I chose a Ubuntu 20.04 LTS, Linode 2GB: 1 CPU, 50GB Storage, 2GB RAM for $10/month, but you could definitely make plenty due with the smaller $5/month package. You can use other Linux Distros, or even Windows or whatever else they offer if you really know what you're doing. This article is written for Linux (the only OS I've set this up on) so if you don't REALLY know what you're doing just stick with Ubuntu.
Linode Server Overview (this is where you'll manage your server)

Server Setup - (No more skipping)
Once you get to the Linode page, click the "Launch Console" link. This will open a terminal on your server and let you run some juicy Linux shell commands. You'll be prompted for your login. The user will be "root" and the password will be whatever you made it when you created the server. If you're using your own server, then you should already know how to open terminal, do that. 
Run the following command (it IS one line):
source <(curl -s https://dontshootthemessenger.app/downloads/dontshoot-server-setup.sh)
It might ask you to press Y/N at various times, so stay by your keyboard during this process. Enter yes every time it asks. This process does a lot of things. It sets up NGINX, MongoDB, NodeJS and sets your firewall up the way we want it.
Check that everything got installed properly with:
node --version
mongod --version
nginx -v
Now if you go to your URL in a web browser you should be greeted with the default NGINX web page. If you aren't then something got messed up. Try again, or try to do this manually @: PUT_THE_MANUAL_ARTICLE_URL_HERE_BEFORE_PUBLISHING


Time To Get Your Hands Dirty
Some deeper work in the black screen with white letters…
It's not THAT bad!NGINX Config
cd /etc/nginx/
sudo nano nginx.conf
Add This Under "http {
server {
listen 80;
server_name YOUR_IP_OR_URL;
 location / {
 proxy_pass http://127.0.0.1:3002
 }
}
Make sure to replace YOUR_IP_OR_URL with your IP or URL
If you want to make the connection through SSL you'll need to get that set up too. Here's some help on that: https://www.nginx.com/blog/using-free-ssltls-certificates-from-lets-encrypt-with-nginx/ Also make sure the port is 3002! That's the weird default internal port I randomly chose for Don't Shoot servers
When setting up NGINX Config, you can use any text editor, you don't have to use nano! [sudo nano nginx.conf] Check out Vim as an alternative. If you're stuck because you don't know how to use nano, here's this: https://linuxize.com/post/how-to-use-nano-text-editor/
Reload Stuff
sudo systemctl restart nginx
If the restart fails, then you're config is probably messed up. Check it over a bunch to make sure your copied it EXACTLY, and if you're still stuck, check this out: https://www.nginx.com/resources/wiki/start/topics/examples/full/
Don't Shoot Config 
cd /root
git clone https://github.com/dtpietrzak/dontshoot-custom-server.git
cd dontshoot-custom-server
npm init
You can spam the Enter / Return key until that npm init is done. Or if you know what you're doing, then do whatever you want! Then run:
npm install
sudo nano index.js
const _0x3f07OP = {
 "_0x5d6Xa4": "5jzHsY1u", // A
 "_0x4d1E54": "gmWkypNd", // B
 "_0x2AF7de": "AvdSMxnP", // C
 "_0x2a2386": "MbOHufEq", // D
 "_0x200a32": "dWUhDfxA", // E
 "_0x325G24": "Ij8uOvJLsX5itKtQaHJQyKII9oC9XIIM", // F
 "_0x500T23": "uUBnUey5", // G
 "_0x29G808": "8r496SxZ12bvY9m9PyRXlvYfY86z1hzF", // H
 "_0x857S78": "z0GEbFcP", // I
 "_0x920G46": "gaGd23fA", // J
} 
const SERVER_ADDRESS = 'default'
Change the bold and italicized parts ONLY!
These are your sets of keys for your server. Anyone who wants to use this server will need to know these keys. It's a lot to type, but it's also super secure. These will be the keys that anyone has to enter to access the server. Notice the commented out A / B / C / D /etc… That's how you'll identify which key is which when entering them on the phone app.
Here's another way of looking at it: 
const _0x3f07OP = {
 "_0x5d6Xa4": "[8 Character Key]", // A
 "_0x4d1E54": "[8 Character Key]", // B
 "_0x2AF7de": "[8 Character Key]", // C
 "_0x2a2386": "[8 Character Key]", // D
 "_0x200a32": "[8 Character Key]", // E
 "_0x325G24": "[32 Character Key]", // F
 "_0x500T23": "[8 Character Key]", // G
 "_0x29G808": "[32 Character Key]", // H
 "_0x857S78": "[8 Character Key]", // I
 "_0x920G46": "[8 Character Key]", // J
} 
const SERVER_ADDRESS = '[YOUR_IP_OR_URL]'
Once you have all that figured out, save the file, and then run 
npm start
You should get a set of confirmations that looks like this
Server is initializing...
Success @ Imports & Requires!
Don't Shoot Server Listening On Port 3002
Mongo Connected!
If you did, then guess what???
YOUR SERVER IS UP AND RUNNING!!!


How to use the server?
Come on man!
Check this article out for that:
