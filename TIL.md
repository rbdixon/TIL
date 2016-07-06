* /dev/ttyACM devices are for [cellular modems](https://www.rfc1149.net/blog/2013/03/05/what-is-the-difference-between-devttyusbx-and-devttyacmx/).
* Is a Linux process hogging a serial port that you wish you could interact with in its currently running state? Check it out with `strace` and then if you want to steal the port get [reptyr](https://github.com/nelhage/reptyr). You can re-parent a running process onto a new terminal (yours!). You can then grab the serial port that process used to use for yourself. Likely you could use `socat` to fork off a copy of the running traffic. [Details here.](https://blog.nelhage.com/2011/02/changing-ctty/).
* Hacker Pro-Tip: Have a silent serial port? After you get root `cat /dev/random > /dev/ttyS0` (or whatever) to make some noise on the port. Then probe without having to hope there's something on the port.
* Make a login macro with burp:
    - See this [article](http://fvaahe.com/creating-a-login-macro-for-burp-suite/)
    - Create new macro (Project Options -> Session -> Macro) to do a login
    - Make a new macro to make an innocuous request to check and see if session is valid.
    - You may need to disable using existing cookies for the request because you want a new cookie
    - In the "Use cookies from Burp's cookie Jar" session rule add a new Rule Action to check if session is valid
    - Run the check session macro, if you made one
    - Set check on session validity
    - Set to run login macro if not logged in
* Use Burp to test and see if login is invalidating old session tokens.
    - Set up a login macro. Make sure to configure the macro to NOT use old session cookies. You want a brand new one.
    - Set up a Session Handling Rule for Intruder that uses this macro so that before each Intruder request you log in.
    - Disable the standard "Use Cookies from Burp's Cookie Jar" Session Handling Rule for Intruder
    - Set up Intruder with a request, stocked with a newly valid collected session, for an authenticated URL
    - Make modify the request to include a `reqnumber=§1§` cookie to hold the payload
    - Set the payload to count from 1 to N
    - Set concurrent requests to 1
    - Launch the attack
    - Check the Logger++ view to make sure you are alternating between login and intruder requests and that the login request is always getting a new cookie and that the intruder request continues to use the old one.
    - A proper app should do a server invalidation on LRU cookies as new ones are invalidated.
* You can [encode IP addresses all sorts of weird ways](http://www.pc-help.org/obscure.htm) to obscure them from filters.
