# forms
- preparing and restructuring data to make it ready for rendering
- creating HTML forms for the data
- receiving and processing submitted forms and data from theclient

### HTML forms
- a collection of elements inside <form>...</form>
- send the information (input) back to the server 
- Other complex things ... pops up a date picker or move a slider: HTML forms + 
*Javascript and CSS*

A form must have two informations
1. where? 
The URL to which the data corresponding
2. how?
The HTTP method (the data should be returned by)

Can use the HTTP methods with forms are GET and POST.

```python
<form action="/" method="POST">
    {% csrf_token %}
    <label for="message">Your Message:</label>
    <textarea id="message" name="message" placeholder="Write your message here..."></textarea>
    <button type="submit" class="btn btn-outline-success" onclick="location.href='main'">Send Message</button>
</for>
```
Now I need a view corresponding to that "/" URL which will find the appropriate key/value pairs in the request.

forms.py
```python
class ContactForm(forms.Form):
    message = forms.CharField(label='message', max_length=1024)
```






Reference [django document]('https://docs.djangoproject.com/en/4.0/topics/forms/')