# Broken pipe from
[broken pipe issue]('https://code.djangoproject.com/ticket/4444')

It's not really an issue. 
A 'Broken Pipe' occurs when data didn't change, the browser fourcefully closes the connection because it does not need more data. 

The browser reads from socket -> didn't change? oh then close it! -> The other end of the socket raises a socket exception 'Broke the socket pipe'

