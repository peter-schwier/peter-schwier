# Translucent Databases | wayner.org

markdownload-timestamp:: 2023-04-14T09:00:50 (UTC -05:00)
markdownload-source:: https://www.wayner.org/node/46
markdownload-hostname:: www.wayner.org
author:: [[Peter Wayner]]
tags:: Database, Privacy, Security



## Excerpt
> Do you have personal information in your database?

---
Do you have personal information in your database?

Do you keep files on your customers, your employees, or anyone else?

Do you need to worry about European laws restricting the information you keep?

Do you keep copies of credit card numbers, social security numbers, or other information that might be useful to identity thieves or insurance fraudsters?

Do you deal with medical records or personal secrets?

Most database administrators have some of these worries. Some have all of them. That's why database security is so important.

This new book, Translucent Databases, describes a different attitude toward protecting the information. Most databases provide elaborate control mechanisms for letting the right people in to see the right records. These tools are well-designed and thoroughly tested, but they can only provide so much support. If someone breaks into the operating system itself, all of the data on the hard disk is unveiled. If a clerk, a supervisor, or a system administrator decides to turn traitor, there's nothing anyone can do.

Translucent databases provide better, deeper protection by scrambling the data with encryption algorithms. The solutions use the minimal amount of encryption to ensure that the database is still functional. In the best applications, the personal and sensitive information is protected but the database still delivers the information.

There's now a second edition of the best selling book about building computer systems that do useful work without holding any useful information. It comes with four new chapters and hundreds of revisions.

[To Order Click Here](https://www.wayner.org/node/39)

More Information


## Case Studies in Translucent Databases | wayner.org

> Translucent Databases contains several dozen examples  written in basic SQL and
Java.  The code is written to be easy-to-follow and  portable. All of the
code can  be extended and modified to fit a number of  different applications.
Here  are some of the examples:

---
_Translucent Databases_ contains several dozen examples written in basic SQL and  
Java. The code is written to be easy-to-follow and portable. All of the  
code can be extended and modified to fit a number of different applications.  
Here are some of the examples:  

-   A database that hides the position of Navy ships from enemies while  
    simultaneously providing accurate information to those with proper authorization.  
     
-   An anti-rape database that identifies trends without containing  
    any personal information.  
-   A babysitter scheduling service that matches parents with available  
    sitters while protecting the sitters' identities and locations'.  
-   A department store database that guards the modesty of customers.  
     
-   A private accounting system that detects fraud without revealing  
    information.  
-   A poker game for the Internet that prevents cheating.  
-   A pharmacy database for preventing dangerous drug interactions  
    while keeping medical records secure.  
-   A tool for travel agents to protect their clients from stalkers  
    and kidnappers.  
-   A stock exchange transaction mechanism designed to stop insider-trading. 
-   A website logfile tool that provides accurate counts of visitors  
    while protecting their identities.
-   A credit-card database for defending crucial e-commerce transactions.
-   A patent search tool that doesn't reveal the nature and focus of  
    the search.
-   A conference bulletin board that routes messages without helping  
    stalkers.
-   A tool for studying the radon concentration in homes without maintaining  
    personal information.
-   An anti-money laundering database.  
    
-   Privacy-preserving car tracking _(2nd edition)_
-   XML encoding and decoding _(2nd edition)_
-   Fuzzy and incomplete matching _(2nd edition)_

Zircon - This is a contributing Drupal Theme  
Design by [WeebPal](http://www.weebpal.com/).

## Can Libraries Protect Users' Privacy While Tracking Books? | wayner.org

> One of the bigger challenges for librarians is protecting the privacy of patrons while stopping them from stealing the books. In many cases, there's not much interesting in our reading choices, but sometimes there is. A spy might look for hints or clues in the list of books taken out by researchers from the nearby army base. A blackmailer may try to subvert some of the local police or security personnel by looking at what they read. This may be why some librarians are so careful to protect the choices of their customers.

---
One of the bigger challenges for librarians is protecting the privacy of patrons while stopping them from stealing the books. In many cases, there's not much interesting in our reading choices, but sometimes there is. A spy might look for hints or clues in the list of books taken out by researchers from the nearby army base. A blackmailer may try to subvert some of the local police or security personnel by looking at what they read. This may be why some librarians are so careful to protect the choices of their customers.

Is it possible to protect the reading choices of library patrons from hackers, insiders, and snoops while catching thieves? At first glance, this seems difficult because the library must keep track of the books on loan to defend itself against people who don't bring them back. Some libraries try to delete all records after a book is returned, but that doesn't stop the curious from looking at the list of books that are currently checked out.

The surprising result is that the library doesn't need to keep a list of what people are reading to stop theft. A few simple one way functions can lock out even the most adept snoops. (A good one-way function is the Secure Hash Algorithm or SHA and many toolkits now come with implementations that implement it and a more general, metaprotocol, the HMAC.)

One way functions scramble information so it is unreadable, but they don't remove all of the usefulness. If you want to see what functions like SHA do to text, you can try to scramble some book data here with [this Javascript](http://pajhome.org.uk/crypt/md5/) :

The results should be inscrutible if the one way function is working correctly. There should be no way to take the results of The trick is to pass the book title and the author through the one-way function, SHA(), before storing it away. That is, put SHA("All's Well That Ends Well/William Shakespeare") in the database instead of just the plaintext title "All's Well That Ends Well". Here's what a librarian's database might look like:

<table><tbody><tr><td><small><span>Name</span></small></td><td><small><span>SHA(book title)</span><br></small></td><td><small><span>Due Date</span><br></small></td><td><small><span>Replacement Cost</span><br></small></td></tr><tr><td><small>Bob Jones<br></small></td><td><small>19ded208e1d4f03f18f54bfead142edb3971632c</small></td><td><small>Jan 1<br></small></td><td><small>$20<br></small></td></tr><tr><td><small><br></small></td><td><small>201a0d9e68c174c0a8664b4d8510204ccad5583d<br></small></td><td><small>Jan 3<br></small></td><td><small>$21<br></small></td></tr><tr><td><small><br></small></td><td><small>873e637cc6d6eb2466ff2d0e02da8a54d1103cb7</small></td><td><small>Jan 3<br></small></td><td><small>$25<br></small></td></tr><tr><td><small>Mary Jones<br></small></td><td><small>51f33f597ecef6886e5426a304a2dec9b7b248f8<br></small></td><td><small>Jan 2<br></small></td><td><small>$15</small></td></tr><tr><td><small><br></small></td><td><small>4f697e91504dd8754afa0c9d29e040fb94b9e52c<br></small></td><td><small>Jan 3</small></td><td><small>$15<br></small></td></tr></tbody></table>

This table tells us that Bob Jones has out some book due on January 1. If he doesn't bring it back, he will owe $20. When he does return it, the library can examine the title, compute SHA(title/author), and delete the entry. Bob Jones is relieved of his responsibilities. If he doesn't return it, he can be billed.

This solution does have a few weaknesses. SHA(title/author) may be easy to guess. Someone can take the list of titles from Amazon or Books in Print and look for matches. There aren't too many books. Another solution is to give each book a unique, random ID number, something many libraries do already. If the number is long enough and chosen at random, then it is not possible for a spy or a blackmailer to try to guess the titles. Another solution is to add a password to the hashing equation by storing SHA(title/author/password) or SHA(title/author/patron's name). This significantly increases the complexity of any brute force attack, although it does not

Removing the Reader's Name

The system can be made a bit more secure by also locking away the identities. Instead of storing the name in the column, the system can store SHA('name') or better SHA('random id string'). When a person returns their book, they can present their libary card  
to delete the book from their record.

<table><tbody><tr><td><small><span>SHA(Name)</span></small></td><td><small><span>SHA(book title)</span><br></small></td><td><small><span>Due Date</span><br></small></td><td><small><span>Replacement Cost</span></small></td></tr><tr><td><small>06fda7eb8124bc3fa05ed70feb92c1c157d8fb23<br></small></td><td><small>19ded208e1d4f03f18f54bfead142edb3971632c</small></td><td><small>Jan 1<br></small></td><td><small>$20<br></small></td></tr><tr><td><small><br></small></td><td><small>201a0d9e68c174c0a8664b4d8510204ccad5583d<br></small></td><td><small>Jan 3<br></small></td><td><small>$21<br></small></td></tr><tr><td><small><br></small></td><td><small>873e637cc6d6eb2466ff2d0e02da8a54d1103cb7</small></td><td><small>Jan 3<br></small></td><td><small>$25<br></small></td></tr><tr><td><small>6980b62ba56b08d90e1f6103fdc42391856d12dc<br></small></td><td><small>51f33f597ecef6886e5426a304a2dec9b7b248f8<br></small></td><td><small>Jan 2<br></small></td><td><small>$15<br></small></td></tr><tr><td><small><br></small></td><td><small>4f697e91504dd8754afa0c9d29e040fb94b9e52c<br></small></td><td><small>Jan 3<br></small></td><td><small>$15<br></small></td></tr></tbody></table>

Removing the person's name from the record would require taking some anonymous bond or deposit for the loan-- a practice that might be too unwieldy. If people post a cash deposit with each loan, then it would be possible to remove the identity completely.

The library could also use some form of reputation database to see who was trustworthy and who wasn't. This isn't much different from what they do now.

If anyone has thoughts about the advantages and limitations of the approach taken here, please write.   --- Peter Wayner, p3 (a) wayner (dot) org

Zircon - This is a contributing Drupal Theme  
Design by [WeebPal](http://www.weebpal.com/).

## Case Study: Protecting Shoppers' Privacy | wayner.org

> How much information should a company retain about its customers? I started wondering about this question when I wrote   Translucent Databases  last year to explore the many ways that someone could build a database that kept no personal information yet still performed useful work. The technology is easy to implement, but the social matrix where the technology must live is hard to understand.

---
## Can Amazon Have A Feature-Rich Site _And_ Protect Sensitive Information?

How much information should a company retain about its customers? I started wondering about this question when I wrote [_Translucent Databases_](http://www.wayner.org/books/td/) last year to explore the many ways that someone could build a database that kept no personal information yet still performed useful work. The technology is easy to implement, but the social matrix where the technology must live is hard to understand.

Over the last few weeks, I've been discussing the usefulness of the approach with [Jon Udell.](http://weblog.infoworld.com/udell/) He sees the technical charm, but wonders whether anyone would ever want to use it. So as a challenge, I offered to show how [Amazon](http://www.amazon.com/) could use the techniques to offer almost all of their services without retaining any personal information about their customers. If someone breaks into their computers or someone on staff decides to get nosy, they can't find a customer's browsing habits, credit card number, address or anything. Yet, Amazon could still tune their offerings to you and suggest items related to your previous purchases.

Although it sounds a paradox for a database to contain no useful information and give valuable responses, the systems can be built. The right amount of encryption can scramble data without retaining it. One-way functions like SHA can turn names, social security numbers, and medical records into unintelligible 160-bit numbers. These 160-bit numbers can be used as surrogates for names, credit card numbers and what not, but they can't be reversed because, behold the magic of definition, the functions like SHA only work in one-way. They can't be inverted.

### Inscrutable Surrogates

The best way to continue is with an example. SHA is a _cryptographically secure hash function._ That means no one has publically described an efficient way to take SHA(x) and figure out a value of x that produced it. For instance, SHA("[pcw@flyzone.com](mailto:pcw@flyzone.com)/swordfish") produces the hexadecimal string 0185fd1f5137ec04a564fdd8ef043e12fd643511. If you start with 0185fd1f5137ec04a564fdd8ef043e12fd643511, though, it is practically impossible to find any string that generates the number, much less find "[pcw@flyzone.com](mailto:pcw@flyzone.com)/swordfish".

The reason I computed that string is that they're my email address (I can't possibly get more spam) and a password. Mixing them together with SHA produces a bag of 160 bits that can act as a surrogate for the email address and password. Anyone who knows the name and address can compute them, but someone who begins with the value 0185fd1f5137ec04a564fdd8ef043e12fd643511 can't go backwards. There's no public method for reversing them and it seems unlikely that there's any method at all.

You can try to scramble your own name and password here based on [this Javascript](http://pajhome.org.uk/crypt/md5/) :

These surrogates can replace your email address and password in the Amazon system. Every time the machines at Amazon store information about the items you examined or purchased, they can store them in their huge databases under the surrogate 0185fd1f5137ec04a564fdd8ef043e12fd643511.

When you return, you log into the system again with your email address and password. Amazon passes them through SHA, looks the result up in their database and starts tuning the experience again. They can look up what 0185fd1f5137ec04a564fdd8ef043e12fd643511 bought on the last trip and start posting similar items on the screen as temptation.

It should be obvious that a malicious hacker or a curious insider at Amazon can't find out what someone bought or even browsed for in the past. If someone wants to browse through the databases, all they find is inscrutable numbers like 0185fd1f5137ec04a564fdd8ef043e12fd643511.

### Offering Services

How many services offered by Amazon can be converted to use this system? A surprising amount. Without any real knowledge of the Amazon system beyond what I've experienced as a customer, I can divide their databases into these categories:

-   **Website Customization** \-- If Bob bought _New York on $100 a day_, offer him books like _A History of New York_ , recordings of Frank Sinatra singing "New York, New York" and maybe even movies like "The Muppets Take Manhattan".
-   **Purchase Analysis** \-- Do customers buy red shirts at Target in the same order as books like the _Communist Manifesto_ ? Someone in the marketting department may want to know.
-   **Fulfillment Services** \-- If someone orders something, get it to them toute de suite.
-   **Spam-like Services** \-- Did someone buy a book like [_Dow Jones 36,000_](http://www.amazon.com/exec/obidos/ASIN/0812931459/myhomepage0bc) several years ago? Send them some email and suggest a book about [accounting fraud](http://www.amazon.com/exec/obidos/ASIN/0385507879/myhomepage0bc) and [irrational exuberance.](http://www.amazon.com/exec/obidos/ASIN/0691050627/myhomepage0bc)

I think the first three of these can be accomplished with fully translucent databases that protect people's sensitive information without compromising any of the features. The last one can't be fixed permanently, but the dangers can be limited by creating a somewhat translucent proxy remailer.

-   **Solving Website Customization** -- When someone logs in with their email and password, compute the SHA surrogate and use it to track their progress. If they've visited before, use this value to look up their history and start offering them suggestions.
    
    A trickier problem is keeping a storehouse of addresses and credit card numbers to save customers from retyping them on the next visit. This information can be encrypted with a key based upon the email and password and then the key can be thrown away after the data is scrambled.
    
    SHA(password|name), for instance, is a good password for the encryption that bears no resemblance to SHA(name|password). You can't compute one from the other. So use one for the surrogate and one for the password that encrypts the personal information. Once this password is destroyed, insiders and hackers can't abuse it. The address and the credit card information can only be decrypted
    
    _after_ someone returns and presents the password and email address.
    
    Jon [points out](http://weblog.infoworld.com/udell/2003/05/29.html) that [Microsoft](http://www.microsoft.com/) offered a similar solution in Hailstorm. They encrypted the information but needed your permission to decrypt it. Hmmm.
    
-   **Solving Purchase Analysis** -- This is the easiest one of the four. The surrogates also link together people's purchases. Any analysis that required the person's name can compute the same results with the surrogate.
-   **Solving Fulfillment Services** -- This is a bit trickier. Sending a package requires collecting someone's address. This information, however, can be destroyed after the package arrives. Amazon could keep this information around until it ships and then forget it. If someone logged in to check the status of their purchase, they would log in with the email address and password to unlock the shipping records.
-   **Solving Spam-like Services** \-- One solution is to stop using them. Many people would be happy for them to end. But others might not be. I know one author who watched his new book sail into the Hot 100 after Amazon sent announcements to people who bought his last book. While many were inconvenienced, some were happy enough to buy.
    
    Still, I don't receive many of these messages despite buying books by repeat authors. It may be that Amazon finds that they're more trouble than they're worth. Many people report these messages as Spam, even though they chose to turn on the service in the past. They either forget or change their mind. This is a real hassle for Amazon because they get lumped in with random spammers. Dealing with so-called legitimate spam like this may be an even bigger practical problem then dealing with the illegitimate stuff.
    
    I can't think of any good way to offer these services without keeping the email addresses tied to the purchase records. It is possible to isolate this information on a separate server with special access restrictions. This will constrain hackers and insiders, but it won't remove any of the deeper problems.
    

### Social Problems

Amazon is a good choice for this demonstration because the company is constantly tweaking its [privacy policy](http://www.amazon.com/exec/obidos/tg/browse/-/468496/104-5971425-4550342) and [experimenting with different rules.](http://www.wired.com/news/politics/0,1283,38572,00.html) Some [users](http://www.noamazon.com/) were even prompted to start a [boycott](http://www.epic.org/privacy/internet/amazon/letter_pr.html) the firm. Of course, it's not fair to pick on Amazon alone. Many stores use the same techniques and may not even do as good a job protecting privacy. To some extent, I chose Amazon for this example because they have such a feature-rich site, not because they have a history of wrestling with customer privacy. If Amazon's site can be duplicated with these techniques, then they're pretty powerful.

Some argue that the success of Amazon shows that many people don't care about privacy. This assumption may be a bit of a leap because it could be that many people weigh the benefits of Amazon against the costs of giving up some of their privacy. Free shipping and a great selection outweigh having someone track what you read. The approach in this note shows that the company could give people most of the benefits **and** protect their personal information.

The techniques can also fight crime and terrorism, a big problem today. One man watched his credit card number get stolen and used to [send a night-vision scope](http://www.csmonitor.com/2002/0605/p01s04-%20wome.htm) to the Middle East. Insiders steal personal information all of the time. Others plunder databases filled with email and sell the lists to spammers. While I'm sure the majority of folks at Amazon are clean cut citizens, there may be someone who isn't. A translucent database can stop the insiders with malice aforethought.

### More

This note is only meant to address a challenge that Jon Udell made to help determine the utility of translucency. He seemed skeptical that the ideas would find widespread use. I hope this note will at least make it clear that a fully-functional, highly customized store like Amazon can be built with a few very simple techniques. Amazon can have its cake and eat most of it too.

Of course, just because an idea is simple and stops terrorism (among other things), doesn't mean that it can or will be widely adopted. I think the resistence in ourselves is deeply buried, perhaps even below our logical layer. Many people still feel a packrat's instinct with data. They feel that this information should be kept around, _just in case._

This is a natural human wish, but it should also be balanced by the just as natural aversion to responsibility. Most businesses don't have to pay the price if a customer's identity gets stolen, their credit cards get cloned, or their bank account is raided. This may change as more people and businesses become aware of the danger of misused information and the responsibility to protect it.

If anyone has thoughts about the advantages and limitations of the approach taken here, please write.

\--- Peter Wayner,  
p3 (at) wayner (dooot) org
