Step 1: Request a record

You begin by asking your computer to resolve a hostname, such as visiting 'https://www.gitlab.com' in a web browser. The first place your computer looks is its local DNS cache, which stores DNS information that the computer has recently retrieved.

Step 2: Ask the Recursive DNS servers

If the records are not stored locally, your computer queries (or contacts) your ISP's recursive DNS servers. These machines perform the legwork of DNS queries on behalf of their customers. The recursive DNS servers have their own caches, which they check before continuing with the query.

Step 3: Ask the Root DNS servers

If the recursive DNS servers do not have the record cached, they contact the root nameservers. These thirteen nameservers contain pointers for all of the Top-Level Domains (TLDs), such as '.com', '.net' and '.org'. If you are looking for 'www.gitlab.com.', the root nameservers look at the TLD for the domain - 'www.gitlab.com'- and direct the query to the TLD DNS nameservers responsible for all '.com' pointers.

Step 4: Ask the TLD DNS servers

The TLD DNS servers do not store the DNS records for individual domains; instead, they keep track of the authoritative nameservers for all the domains within their TLD. The TLD DNS servers look at the next part of the query from right to left - 'www.gitlab.com' - then direct the query to the authoritative nameservers for 'gitlab.com'.

Step 5: Ask the Authoritative DNS servers

Authoritative nameservers contain all of the DNS records for a given domain, such as host records (which store IP addresses), MX records (which identify nameservers for a domain), and so on. Since you are looking for the IP address of 'www.gitlab.com', the recursive server queries the authoritative nameservers and asks for the host record for 'www.gitlab.com'.

Step 6: Retrieving the record

The recursive DNS server receives the host record for 'www.gitlab.com' from the authoritative nameservers, and stores the record in its local cache. If anyone else requests the host record for 'www.gitlab.com', the recursive servers will already have the answer, and will not need to go through the lookup process again until the record expires from cache.

![Rails stack](https://cloud.githubusercontent.com/assets/4617609/18615157/9ce1407e-7dbc-11e6-811e-d3438bec1c28.png)

Step 7: Web Server

After the DNS gets resolved, the request hits a web server, which asks Rails what it has for that url. In this case the url being `https://gitlab.com/gitlab-org/gitlab-ce`. A web server is a program that takes a request to your website from a user and does some processing on it. Then, it might give the request to your Rails app. Nginx and Apache are the two big web servers. If the request is for something that doesn’t change often, like CSS, JavaScript, or images, your Rails app probably doesn’t need to see it. The web server can handle the request itself, without even talking to your app. It’ll usually be faster that way.
Web servers can handle SSL requests, serve static files and assets, compress requests, and do lots of other things that almost every website needs. And if your Rails app does need to handle a request, the web server will pass it on to your app server.

![web server](https://cloud.githubusercontent.com/assets/4617609/18615163/b7dd3ef0-7dbc-11e6-965e-953961d08005.png)

Step 8: Application Server

An app server is the thing that actually runs our Rails app. Our app server loads our code and keeps our app in memory. When our app server gets a request from our web server, it tells our Rails app about it. 
That’s probably what we do in development mode! In production, though, we usually have a web server in front. It’ll handle multiple apps at once, render our assets faster, and deal with a lot of the processing we'll do on every request.

There are a ton of app servers for Rails apps, including Mongrel (which isn’t used much anymore), Unicorn, Thin, Rainbows, and Puma. Each has different advantages and different philosophies. But in the end, they all accomplish the same thing – keeping your Rails app running and handling requests.

![app server](https://cloud.githubusercontent.com/assets/4617609/18615167/d556dbe4-7dbc-11e6-8785-870f7653ce73.png)

Step 9: Rack 

Rack is the magic that lets any of these app servers run our Rails app. (Or Sinatra app, or Padrino app, or…)
Rack can be imagined as a common language that Ruby web frameworks (like Rails) and app servers both speak. Because each side knows the same language, it means Rails can talk to Unicorn and Unicorn to Rails, without having either Rails or Unicorn know anything about the other.

![rack](https://cloud.githubusercontent.com/assets/4617609/18615171/f2432172-7dbc-11e6-8b25-292aa4750a50.png)

Step 10: Routes, controllers, models and views

Rails goes to the routes.rb file first, which takes the URL and calls a corresponding controller action. The controller goes and gets whatever stuff it needs from the database using the relevant model. With the data the controller got from the model, it uses the respective view to make some HTML. 

Sometimes we might want to force a particular controller to only be accessible via an HTTPS protocol for security reasons. We can use the force_ssl method in your controller to enforce that:

    class DinnerController
      force_ssl
    end
    
So, the URL will use https instead of http.

![controller](https://cloud.githubusercontent.com/assets/4617609/18615174/0d3a1e04-7dbd-11e6-9c30-f1d25c733390.png)

Step 11: The Answer!

Rails packs up the response and gives it to the web server. The web server delivers the response to the browser to display the page in the browser.

![response](https://cloud.githubusercontent.com/assets/4617609/18615175/1f7057c8-7dbd-11e6-98a7-ac8f29bff5bb.png)
