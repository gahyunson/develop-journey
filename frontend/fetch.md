```javascript
fetch(url)
.then(response => responnse.json())
.then(data => console.log(data))
.catch(error => console.log(error))
.finally()
```

If you want to redirect after request,
```javascript
.then(response => {
    if (response.ok){
        window.location.href = "{% url 'url_name' %}";
    } 
```

