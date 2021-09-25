# Django-CRM

This is a fork of [Django-CRM](https://github.com/MicroPyramid/Django-CRM), to do the following:

* Enhance installation instructions in general
* Add MacOS instructions
* Add admin support (go to <http:127.0.0.1:8000/admin>)

<http://django-crm.readthedocs.io> for latest documentation

# TODOs

* [x] Document token authentication
* [ ] Detailed walkthrough of the `env.md`setup
* [ ] Detailed description of the components needed (whether for stand-alone or Docker)

# Enhancements

## `admin/` support

Django Admin support has been enabled.  The upstream codebase had some support (e.g., the
`*/admin.py` files), but those were not exposed.  My fork exposes those via the standard
mechanisms (e.g., <http://localhost:8000/admin>).

## show_urls

I find it convenient to be able to see the routes, and added support for the following:
```
  python manage.py show_urls
```

# Installation Notes

* SASS: the official documentation says to install ruby, ruby-dev, and gem.  However, you do
  not need that if you install SASS in another way (e.g., `npm install sass`)

* There is no `python manage.py create_blog_user` task; however, the standard 
  `python manage.py createsuperuser` is enabled.


## Minimum requirements

The upstream install notes are the following:

```
sudo apt install postgresql xvfb libfontconfig wkhtmltopdf git libpq-dev python3-dev python3-pip gem ruby ruby-dev b
uild-essential libssl-dev libffi-dev python3-venv redis-server redis-tools virtualenv -y

sudo gem install sass

sudo apt-get install postfix
```

However, you also need to set up the following:
1. Postgres db setup and configuration in `env.md`
2. AWS S3 setup and configuration in `env.md`
3. STATIC and MEDIA setup.  Note that the `media` directory does not exist in the upstream
  release, and the `crm/server_settings.py` sets that to the `/media` directory in your S3 bucket.
4. Redis setup and configuration in `env.md` (if you are using Tasks -- otherwise not required)

# Usage Notes

Note in `crm/urls.py` that there is swagger, and `api/` support; however, the UI of Django-CRM
is not in the codebase.

Navigate to <http://127.0.0.1:8000/swagger> to see the Swagger pages.

## Authentication

### Getting a token

Using httpie (e.g., first `pip install httpie`):

```
http POST http://127.0.0.1:8000/api/auth/login/ email=<your email from registration> password=<password set at registration>

```
or navigate to <http://127.0.0.1:8000/api/auth/login>
and provide the JSON like `{"email":"your email", "password":"your password"}` and POST.

Either of those methods will you give a token (in JSON) like the following):

```
{
    "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpZCI6MSwiZW1haWwiOiJzdGV2ZW4uamVua2luc0BnbWFpbC5jb20iLCJyb2xlIjoiQURNSU4iLCJoYXNfc2FsZXNfYWNjZXNzIjpmYWxzZSwiaGFzX21hcmtldGluZ19hY2Nlc3MiOmZhbHNlLCJmaWxlX3ByZXBlbmQiOiJ1c2Vycy9wcm9maWxlX3BpY3MiLCJ1c2VybmFtZSI6InN0ZXZlbiIsImZpcnN0X25hbWUiOiIiLCJsYXN0X25hbWUiOiIiLCJpc19hY3RpdmUiOnRydWUsImlzX2FkbWluIjp0cnVlLCJpc19zdGFmZiI6dHJ1ZX0.9Zw7_WJfqU_Q1IBs8BfpdKC-qggUHeGyv-cgX9JxC8Q",
    "error": false
}
```

Use that token to authenticate, with an Authentication header set to that token prepended with 'jwt' and a space.  

For example, via `curl`:
```
$ curl  -H "Content-Type: application/json" -H "Authorization: jwt eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpZCI6MSwiZW1haWwiOiJzdGV2ZW4uamVua2luc0BnbWFpbC5jb20iLCJyb2xlIjoiQURNSU4iLCJoYXNfc2FsZXNfYWNjZXNzIjpmYWxzZSwiaGFzX21hcmtldGluZ19hY2Nlc3MiOmZhbHNlLCJmaWxlX3ByZXBlbmQiOiJ1c2Vycy9wcm9maWxlX3BpY3MiLCJ1c2VybmFtZSI6InN0ZXZlbiIsImZpcnN0X25hbWUiOiIiLCJsYXN0X25hbWUiOiIiLCJpc19hY3RpdmUiOnRydWUsImlzX2FkbWluIjp0cnVlLCJpc19zdGFmZiI6dHJ1ZX0.9Zw7_WJfqU_Q1IBs8BfpdKC-qggUHeGyv-cgX9JxC8Q" -X GET http://127.0.0.1:8000/api/users/


{"per_page":10,"active_users":{"active_users_count":1,"next":null,"previous":null,"page_number":1,"active_users":[{"id":1,"file_prepend":"users/profile_pics","username":"steven","first_name":"","last_name":"","email":"steven.jenkins@gmail.com","is_active":true,"is_admin":true,"is_staff":true,"date_joined":"2021-09-07T05:02:44.091174+05:30","role":"ADMIN","profile_pic":null,"has_sales_access":false,"has_marketing_access":false}]},"inactive_users":{"inactive_users_count":0,"next":null,"previous":null,"page_number":1,"inactive_users":[]},"admin_email":"steven.jenkins@gmail.com","roles":[["ADMIN","ADMIN"],["USER","USER"]],"status":[["True","Active"],["False","In Active"]]}
```

At this point, you can use Swagger to further explore the CRM.

