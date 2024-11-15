# How to upgrade library version?
1. Set version
```python
flake8>=4.0.1,<4.1
```

2. `docker compose build`

3. It will tell us any issuses.
`docker compose run --rm app sh -c "python manage.py wait_for_db && python manage.py test && flake8"`
