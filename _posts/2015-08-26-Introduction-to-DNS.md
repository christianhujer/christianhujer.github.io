---
layout: post
title: Introduction to DNS
---

DNS, the Domain Name System, is the service which makes it possible that you don't need to remember cryptic ip-addresses like `5.35.240.170` or even worse `2a01:488:66:1000:523:f0aa:0:1`.
Instead you can remember a name, like `www.nelkinda.com`.
But how does it work?

## The DNS Service
The DNS service consists of a tree of DNS servers which listen to UDP port 53 to incoming DNS resolution requests.
If they know the answer, they reply with an address.

For example, when you connect your browser to `www.nelkinda.com`, what happens first is that your computer sends a DNS request to its DNS server asking it for the address of `www.nelkinda.com`.
The DNS server might then answer that the address is `5.35.240.170`.
Then your browser connects to `5.35.240.170`.

### Is it the same when I'm using a Proxy Server?
Almost.
There's just a small difference.
Your browser will connect with the proxy server, and the DNS request for the final destination will be performed by the proxy server, not your browser.

### How does my computer know the DNS servers?
In most cases, your computer will know them through DHCP from your DHCP server.
And in many cases your DHCP server will know them from your provider via PPP.
So you don't really need to worry about that.

## DNS for Admins
If you are a system administrator, DNS is of special interest to you.
You use DNS for:

* Making sure that your company website is reachable via name for IPv4 as well as IPv6.
* Configure multi-hosting (aka virtual hosting) to host mutliple virtual websites on a single ip address.
* Make sure that mail can be sent to your domain.
* Provide alias names for your services so users can remember nicer names.
* Authenticate your setup towards third parties like Google.
And maybe more.

### The record types
This is advanced stuff for people who're not just using DNS, but making entries to the DNS themselves.

<table class="bordertable">
<caption>DNS Record Types</caption>
<thead>
<tr>
<th>Record Type</th>
<th>Purpose</th>
<th>Example</th>
</tr>
</thead>
<tbody>
<tr><td>A          </td><td>IPv4 Address Resolution </td><td><code>nelkinda.com</code> -> <code>5.35.240.170</code>                  </td></tr>
<tr><td>AAAA       </td><td>IPv6 Address Resolutoin </td><td><code>nelkinda.com</code> -> <code>2a01:488:66:1000:523:f0aa:0:1</code> </td></tr>
<tr><td>MX         </td><td>Mail Exchange           </td><td><code>nelkinda.com</code> -> <code>aspmx.l.google.com</code>            </td></tr>
<tr><td>CNAME      </td><td>Alias Name              </td><td><code>mail.nelkinda.com</code> -> <code>ghs.googlehosted.com</code>     </td></tr>
<tr><td>NS         </td><td>Name Server             </td><td><code>nelkinda.com</code> -> <code>ns1.whois.com</code>                 </td></tr>
<tr><td>TXT        </td><td>Textual Meta Information</td><td><code>nelkinda.com</code> -> <code>"google-site-verification=..."</code></td></tr>
<tr><td>SRV        </td><td>Service Lookup          </td><td><code>_sip._tcp.nelkinda.com</code> -> <code>example.com:5060</code>    </td></tr>
</tbody>
</table>

### DNS and Redirection
Imagine you use [Slack](http://slack.com/) for team communication, and you want that your team members can reach slack easily.
The address provided by slack would be `teamname.slack.com` but maybe you want that your team can use `slack.companyname.com` instead.

For this you need two things:

* An A record pointing `slack.companyname.com` to your webserver's IP address.
* A redirection setup in your webserver.

Here's an example redirection setup for the Apache httpd, which you could store in, depending on your system, i.e. `/etc/apache2/sites-enabled/teamname.conf`:

{% highlight apache linenos %}
<VirtualHost *:80>
    ServerName slack.companyname.com
    Redirect 303 / https://teamname.slack.com/
</VirtualHost>
{% endhighlight %}

Replace `slack.companyname.com` and `teamname.slack.com` with whatever is appropriate for you.

The same can be used for other services as well, like Bitbucket, Github, Mingle, whatever you like.

### DNS Redirection in the case of Google
Google is a bit special in the sense of redirection.
The above concept would work for Google.
Google thinks this type of redirection is important for user convenience.
And so, Google provides a simpler way.
Google takes care of the webserver redirection part with its host `ghs.googlehosted.com`.
All you need to do is setup the name.

Because in this case you don't point your name to an ip address but to another name, the record type to setup is not `A` / `AAAA` but `CNAME`.


### Censorship with DNS - and how to Workaround
From time to time some governments think they can censor the Internet.
The actual censorship is executed by the Internet providers of that country.
Often, providers use DNS for censorship because it is quite cheap to implement.
They just need to forge the DNS records in their own DNS servers and redirect the DNS lookups to an alternate address.

The disadvantage of that technique is that it only works on the level of complete sites.
Imagine, whatever the government wants to censor is on a site like <http://github.com/>.
With DNS-based censorship, the whole GitHub site would be unreachable, cutting off all the developers of that country from one of their most important sites.
Given that GitHub is also used by lots of companies, it means even software development businesses are affected.

You think this wouldn't happen in your country?
Maybe you're wrong.
Even in India, the largest democracy of the world, the government sometimes thinks that it knows better what the people should not see than the people themselves.

* In August 2012, India blocked Facebook and Twitter [[1]][1].
* From 2014-12-31 to 2015-01-02 GitHub, Vimeo, DailyMotion, a lot of pastebins and a lot of other sites were blocked in India [[2]][2], [[3]][3].
* On 2015-08-03, India blocked 857 pornography websites, and that despite the fact that the Supreme Court declined a request to do so, because of the request of one activist [[4]][4]

If sites are blocked via DNS, the workaround is simple.
And it is needed.
Imagine your government blocks GitHub again, but your business depends on it!

There are at least five, maybe more workarounds:

* Access the site by directly accessing its IP address. This only works if the ban is only on the domain name, not on the IP address. Because of multihosting, bans are often on domain names only. But then, without a plugin, your browser will not work with multihosting sites.
* Write the IP address in your computer's `/etc/hosts` file. Many operating systems use this file first before using DNS. The format of that file is self-explanatory, you can edit it with any text-editor.
* Use a different DNS server. Often the one from Google would still work, the IP addresses of the Google DNS servers are `8.8.8.8` and `8.8.4.4`.
* Setup your own DNS server.
* Use something like the Tor network. **Warning** *Use Tor at your own risk! There's a lot of bad stuff going on inside the Tor network.*


## References
[1]: http://mediashift.org/2012/08/india-blocks-facebook-twitter-mass-texts-in-response-to-unrest241/ "Mediashift: India Blocks Facebook, Twitter, Mass Texts in Response to Unrest"
[2]: http://www.zdnet.com/article/india-blocks-32-websites-including-github-internet-archive-pastebin-vimeo/ "ZDNet: India blocks 32 Websites including Github, Internet Archive, Pastebin, Vimeo"
[3]: https://twitter.com/pranesh_prakash/status/550196008416600064/photo/1?ref_src=twsrc%5Etfw "Twitter: Insane! Govt orders blocking of 32 websites including @internetarchive @vimeo @github @pastebin #censorship #FoEx"
[4]: http://www.nytimes.com/2015/08/04/world/asia/india-orders-blocking-of-857-pornography-websites-targeted-by-activist.html "New York Times: India Blocks 857 Pornography Websites, Defying Supreme Court Decision"
* [[1]] <http://mediashift.org/2012/08/india-blocks-facebook-twitter-mass-texts-in-response-to-unrest241/> Mediashift: India Blocks Facebook, Twitter, Mass Texts in Response to Unrest
* [[2]] <http://www.zdnet.com/article/india-blocks-32-websites-including-github-internet-archive-pastebin-vimeo/> ZDNet: India blocks 32 Websites including Github, Internet Archive, Pastebin, Vimeo
* [[3]] <https://twitter.com/pranesh_prakash/status/550196008416600064/photo/1?ref_src=twsrc%5Etfw> Twitter: Insane! Govt orders blocking of 32 websites including @internetarchive @vimeo @github @pastebin #censorship #FoEx
* [[4]] <http://www.nytimes.com/2015/08/04/world/asia/india-orders-blocking-of-857-pornography-websites-targeted-by-activist.html> New York Times: India Blocks 857 Pornography Websites, Defying Supreme Court Decision
