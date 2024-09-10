# How to design URL?
- URLs should include nouns, not verbs.
- User plural nouns only for consistency (no singular nouns).
- Use HTTP methods (HTTP/1.1) to operate on these resources:
- Use HTTP response status codes to represent the outcome of operations on resources.

# URI normalization
To transform a URI into a normalized URI have a Goal:   
the normalization is possible to determine if two syntactically diffrent URIs may be equivalent.

1. Converting percent-encoded triplets to uppercase.   
http://example.com/foo%2a → http://example.com/foo%2A

2. Converting the scheme and host to lowercase.  
HTTP://User@Example.COM/Foo → http://User@example.com/Foo

3. Decoding percent-encoded triplets of unreserved characters.  
Percent-encoded triplets of the URI in the range of    
ALPHA, DIGIT, hyphen, period, underscore, or tilde ... should be decoded ...   
http://example.com/%7Efoo → http://example.com/~foo

4. Removing dot-segment.  
http://example.com/foo/./bar/baz/../qux → http://example.com/foo/bar/qux

5. Converting an empty path to a "/" path.  
http://example.com → http://example.com/

6. Removing the default port.  
http://example.com:80/ → http://example.com/




Reference [[1]('https://apiguide.readthedocs.io/en/latest/build_and_publish/use_RESTful_urls.html')]
