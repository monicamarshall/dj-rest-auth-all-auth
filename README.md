# dj-rest-auth, django-allauth, djangorestframework-simplejwt, RS256 algorithm
demo project that demonstrates protecting restful endpoints with dj-rest-auth, django-allauth, djangorestframework-simplejwt AUTH libraries.
This project uses the gmail email server for verifying users' emails when registering.  A valid email is part a necessary piece for user registration besides a valid username and password.  User accounts are managed by the django user management system.

Documentation for dj-rest-auth, django-allauth, djangorestframework-simplejwt is available at:

https://pypi.org/project/djangorestframework-simplejwt/

https://django-rest-framework-simplejwt.readthedocs.io/en/latest/getting_started.html#

https://www.django-rest-framework.org/api-guide/authentication/

https://github.com/iMerica/dj-rest-auth

https://dj-rest-auth.readthedocs.io/en/latest/introduction.html

https://django-allauth.readthedocs.io/en/latest/

https://dev.to/a_atalla/django-rest-framework-custom-jwt-authentication-5n5

https://github.com/J-rayX/drfauthproject

Example projects:

https://dev.to/jkaylight/django-rest-framework-authentication-with-dj-rest-auth-4kli

https://simpleisbetterthancomplex.com/tutorial/2018/12/19/how-to-use-jwt-authentication-with-django-rest-framework.html

https://medium.com/django-rest/django-rest-framework-jwt-authentication-94bee36f2af8

https://blog.miguelgrinberg.com/post/json-web-tokens-with-public-key-signatures

# django-allauth and dj-rest-auth capabilities

Login (account_login)

Signup (account_signup)

Logout (account_logout)

Password Management

Password Reset (account_reset_password)

E-mails Management (account_email)

E-mail Verification

Social Connections (socialaccount_connections)

# Prerequisite: private/public key 

A private/public key must be created for enabling the rest_framework_simplejwt 4.6.0 AUTH library to create/refresh JWT tokens with the RS256 algorithm.

In the environment where the cryptography library is installed run the command to create a private and public key. Specify the location of the public/private key in settings.py ( JWT_PUBLIC_KEY_PATH = './publicKey.pem' JWT_PRIVATE_KEY_PATH = './privateKey.pem')

  ssh-keygen -t rsa -b 4096

Documentation on python cryptography:

https://blog.miguelgrinberg.com/post/json-web-tokens-with-public-key-signatures

# Prerequisite: Email server configuration

dj-rest-auth, django-allauth require a valid email for users wanting to register.  Email server configuration is a necessary prerequisite for the auth capabilities to work.  Email configuration settings are located in the application configuration file settings.py for demo purposes. 

EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'

EMAIL_HOST = 'smtp.gmail.com'

#EMAIL_HOST = 'smtp.mail.yahoo.com'

EMAIL_USE_TLS = True

EMAIL_PORT = 587

#EMAIL_PORT = 465

EMAIL_HOST_USER = 'jwtdemoapp@gmail.com'  ( This is the webapp admin email address that sends an email to users registering with their email for users' email verification )

EMAIL_HOST_PASSWORD = 'jwtdemoapps@ula'    ( This is the webapp admin email account password for sending an email to users registering with their email for users' email verification )

ACCOUNT_EMAIL_REQUIRED = True

ACCOUNT_AUTHENTICATION_METHOD = 'email'

ACCOUNT_EMAIL_VERIFICATION = 'mandatory'

ACCOUNT_CONFIRM_EMAIL_ON_GET = True

LOGIN_URL = 'http://localhost:8090/users/login'

# To run this project:

1. create a virtual environment and activate it
2. Install the following libraries with the command pip install <library-name>:

  Django==3.2
  
  django-filter==2.4.0

  djangorestframework==3.12.4

  djangorestframework-simplejwt==4.6.0   (JWT token create/refresh/verify)
  
  dj-rest-auth==2.1.4  ( User account management for JWT access to protected resources via Rest URI )
  
  django-allauth==0.44.0  ( User account management for JWT access to protected resources via Rest URI )

  cryptography==3.4.7   (create private/public keys)
  
  psycopg2==2.8.6 (connect to postgres DB)
  
3. Check out the project
4. Create a database with name 'databasename'.  Postgres configuration is in settings.py
5. Set up a gmail account for the application admin user to send registration/email verification emails to users wanting to register.  Make sure to allow (move the button to the right) less secure apps in the security section of the newly created gmail account.  
6. Set up a gmail account for a test user that registers with the application.  The user's email account will be verified by the app admin.  Make sure to allow (move the button to the right) less secure apps in the security section of the newly created gmail account. 

7. cd to the directory that contains manage.py and issue the commands:

  python manage.py makemigrations in the security section of the newly created gmail account.  
  
  python manage.py migrate

  python manage.py createsuperuser (enter the username & password to use when requesting a token)

  python manage.py runserver 8090
8. Access the admin console with the superuser name and password ( step 7 ) and select sites.  Modify the the DOMAIN NAME to 127.0.0.1:8080 ( host address and port number where the application is running) and the DISPLAY NAME (name of the application ).  The domain name and display name will appear in the email that the app admin sends to registering users to verify their email address.
  
# Test protected resources by django-allauth and dj-rest-auth endpoints

$ curl -X GET  http://localhost:8090/api/students/

{"detail":"Authentication credentials were not provided."}

$ curl -X GET  http://localhost:8090/hello/

{"detail":"Authentication credentials were not provided."}
monica@DESKTOP-0MV2FVN ~

$ curl -H "Content-Type: application/json" -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjIwMTM4MzkzLCJqdGkiOiJmMGRmYTZjMGVmNDI0NTFmYTgzMDkxZTdmMGNmYWYyZCIsInVzZXJfaWQiOjIsImF1ZCI6Ik1wZGJVc2VycyIsImlzcyI6Ik1wZGJSZXN0QVBJIn0.XJap7x55O-V1SonnqoLarY3egXSVjr0qSUBJu06PokeNMJXAZmlNAT1QqrWXbyJHpW0GAqBqjW2bA5HgE10J-w" -X GET  http://localhost:8090/hello/

{"message":"Hello, World!"}

$ curl -H "Content-Type: application/json" -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjIwMTM4MzkzLCJqdGkiOiJmMGRmYTZjMGVmNDI0NTFmYTgzMDkxZTdmMGNmYWYyZCIsInVzZXJfaWQiOjIsImF1ZCI6Ik1wZGJVc2VycyIsImlzcyI6Ik1wZGJSZXN0QVBJIn0.XJap7x55O-V1SonnqoLarY3egXSVjr0qSUBJu06PokeNMJXAZmlNAT1QqrWXbyJHpW0GAqBqjW2bA5HgE10J-w" -X GET  http://localhost:8090/api/students/

[{"pk":1,"first_name":"Henry","last_name":"Royal","email":"royal@gmail.com","classroom":"Data Structures Java"},{"pk":2,"first_name":"Diana","last_name":"Prin
cess","email":"diana@gmail.com","classroom":"Lambdas"}]


$ curl -H "Content-Type: application/json" -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjIwMTM4MzkzLCJqdGkiOiJmMGRmYTZjMGVmNDI0NTFmYTgzMDkxZTdmMGNmYWYyZCIsInVzZXJfaWQiOjIsImF1ZCI6Ik1wZGJVc2VycyIsImlzcyI6Ik1wZGJSZXN0QVBJIn0.XJap7x55O-V1SonnqoLarY3egXSVjr0qSUBJu06PokeNMJXAZmlNAT1QqrWXbyJHpW0GAqBqjW2bA5HgE10J-w" -X GET  http://localhost:8090/hello/

{"detail":"Given token not valid for any token type","code":"token_not_valid","messages":[{"token_class":"AccessToken","token_type":"access","message":"Token
is invalid or expired"}]}

$ curl -H "Content-Type: application/json" -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjIwMTM4MzkzLCJqdGkiOiJmMGRmYTZjMGVmNDI0NTFmYTgzMDkxZTdmMGNmYWYyZCIsInVzZXJfaWQiOjIsImF1ZCI6Ik1wZGJVc2VycyIsImlzcyI6Ik1wZGJSZXN0QVBJIn0.XJap7x55O-V1SonnqoLarY3egXSVjr0qSUBJu06PokeNMJXAZmlNAT1QqrWXbyJHpW0GAqBqjW2bA5HgE10J-w" -X GET  http://localhost:8090/api/students/

{"detail":"Given token not valid for any token type","code":"token_not_valid","messages":[{"token_class":"AccessToken","token_type":"access","message":"Token
is invalid or expired"}]}

# Test django-allauth and dj-rest-auth endpoints for Login, Logout, User Registration, Email Verification, Password Reset:

http://localhost:8090/users/login/

http://localhost:8090/users/logout/

http://localhost:8090/users/register/

http://localhost:8090/password-reset/


# Test AUTH with rest_framework_simplejwt RS256 restful endpoints:

1. Obtain an access token.( with valid username and password ).  This url will return an access token and a refresh token to refresh the access token when it expires.  Remember that a new access token can be generated using the refresh token if the refresh token has not expired.  The lifetime of a refresh token is typically longer than the lifetime of an access token.  If the refresh token is expired a new access token can be obtained by submitting login credentials on the request token url.  The url for obtaining an access/refresh token (server running at port 8088 ): http://localhost:8090/api/token/

2. Refresh access token ( using the refresh token obtained in step 1 ) The url for a new access token by posting the refresh token (server running at port 8090 ): http://localhost:8090/api/token/refresh/

3. Verify token ( using the access token or the refresh token from step 1 or step 2 ). The url for verifying a token (server running at port 8090 ): http://localhost:8090/api/token/verify/
   If the token is valid or not expired, an empty response is returned.  Otherwise, an error message is returned.

You can easily test if the endpoint returns a response by submitting the following commands in your terminal.  Remember that you must have a user created with the username admin and password password123 for the request to work.  
Create an account with username and password by issuing the command:  
python manage.py createsuperuser in the project directory where manage.py is located.  
Follow the prompts and add a superuser with name admin and password password123.

Test Obtain Token:

$ curl -X POST -d "username=admin&password=password123" http://localhost:8090/api/token/

{"refresh":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJ0b2tlbl90eXBlIjoicmVmcmVzaCIsImV4cCI6MTYxOTcxMjM3MiwianRpIjoiMGYyMTUwNmM1ZWE4NGE3M2EyZDMwYzRhMzEwNDQ1ODEiL
CJ1c2VyX2lkIjoxLCJ1c2VybmFtZSI6Im1vbmljYSIsInN1YiI6Ik1wZGJVc2VyIiwiYXVkIjoiTXBkYlVzZXJzIiwiaXNzIjoiTXBkYlJlc3RBUEkifQ.FGFY4SjvjqU852IBHAhCpqjww21dvcML8AqgavXY
kRYYShsTH9Egp3vYwHR0DsmQr9chbDuLDyRDxkO_Q9HxoQ","access":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjE5NzExNzcyLCJqdGkiOiI
zODYxODNkMjNhNDY0MTYxOWUwNjZmMmUwMjdkZTAwMyIsInVzZXJfaWQiOjEsInVzZXJuYW1lIjoibW9uaWNhIiwic3ViIjoiTXBkYlVzZXIiLCJhdWQiOiJNcGRiVXNlcnMiLCJpc3MiOiJNcGRiUmVzdEFQS
SJ9.kY5kRcT-FKg6z4hfS8Yt7RoNpBsWhptre_f5MMLBxGcD51dPAMs88d2TXi63D2F5LDTaVoQiNvr687rBDxHiiQ"}

Test Obtain Token:

$ curl -X POST -H "Content-Type: application/json" -d '{"username":"admin","password":"password123"}' http://localhost:8090/api/token/

{"refresh":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJ0b2tlbl90eXBlIjoicmVmcmVzaCIsImV4cCI6MTYxOTcxMjQ5MCwianRpIjoiY2QzMjAzYTg2MzA5NGRhMmE1NDc2ZmEzYzRmYjY3MzQiL
CJ1c2VyX2lkIjoxLCJ1c2VybmFtZSI6Im1vbmljYSIsInN1YiI6Ik1wZGJVc2VyIiwiYXVkIjoiTXBkYlVzZXJzIiwiaXNzIjoiTXBkYlJlc3RBUEkifQ.XL6pk-_eG72xY-rmn8c-zE1d6KI9qkgjv7bvW2q6
jBRjKe4qUcIDhWEssPchYYa79oKAHsPofGPFfxS7FSUNCA","access":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjE5NzExODkwLCJqdGkiOiJ
lNzA3MGM3ZjBkZjM0ZTg1OGRlZmRmY2RiOTQxZDBkMCIsInVzZXJfaWQiOjEsInVzZXJuYW1lIjoibW9uaWNhIiwic3ViIjoiTXBkYlVzZXIiLCJhdWQiOiJNcGRiVXNlcnMiLCJpc3MiOiJNcGRiUmVzdEFQS
SJ9.DvEd2CnEp6cHR2Oj63jMf66WZ572jkD41qUWY4Iqb4ZrYdSu492TwwxDpV1Lf2l6-XHOpnAiQSAwxkx1f0NpNA"}

Test Token Refresh by posting the refresh token from Otain Token:

$ curl -X POST -H "Content-Type: application/json" -d '{"refresh":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJ0b2tlbl90eXBlIjoicmVmcmVzaCIsImV4cCI6MTYxOTcxMjQ5MCwianRpIjoiY2QzMjAzYTg2MzA5NGRhMmE1NDc2ZmEzYzRmYjY3MzQiLCJ1c2VyX2lkIjoxLCJ1c2VybmFtZSI6Im1vbmljYSIsInN1YiI6Ik1wZGJVc2VyIiwiYXVkIjoiTXBkYlVzZXJzIiwiaXNzIjoiTXBkYlJlc3RBUEkifQ.XL6pk-_eG72xY-rmn8c-zE1d6KI9qkgjv7bvW2q6jBRjKe4qUcIDhWEssPchYYa79oKAHsPofGPFfxS7FSUNCA"}' http://localhost:8090/api/token/refresh/

{"access":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjE5NzEyMjY4LCJqdGkiOiIyODlkMjkwY2RiNjY0MTJjYTUzNzVjMDcwMGY0ODUwMCIsIn
VzZXJfaWQiOjEsInVzZXJuYW1lIjoibW9uaWNhIiwic3ViIjoiTXBkYlVzZXIiLCJhdWQiOiJNcGRiVXNlcnMiLCJpc3MiOiJNcGRiUmVzdEFQSSJ9.ppPOSPGZf4tDkijRfBjuMutpu0yhtK2Z_ZdOyTqsbU1
q5tRUZsaciysmFUEKKwjvg4LQaYPF7BPyKeqR6-_U3w","refresh":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJ0b2tlbl90eXBlIjoicmVmcmVzaCIsImV4cCI6MTYxOTcxMjg2OCwianRpIjoiN
DVkMDJjZmU3YzAzNGEzNmI0N2Y0ODhlOGQwZDlmMDYiLCJ1c2VyX2lkIjoxLCJ1c2VybmFtZSI6Im1vbmljYSIsInN1YiI6Ik1wZGJVc2VyIiwiYXVkIjoiTXBkYlVzZXJzIiwiaXNzIjoiTXBkYlJlc3RBUEk
ifQ.v0_ZHtRGBeaLW1IDcxcJeTDK1YT0Ne4QPgnrYQwnKkj_diDc8LB9SPwmJeN4kBvmH57miHR0QnW26ScA9us74g"}

Test access token verify:

$ curl -X POST -H "Content-Type: application/json" -d '{"token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJ0b2tlbl90eXBlIjoicmVmcmVzaCIsImV4cCI6MTYxOTcxMjQ5MCwianRpIjoiY2QzMjAzYTg2MzA5NGRhMmE1NDc2ZmEzYzRmYjY3MzQiLCJ1c2VyX2lkIjoxLCJ1c2VybmFtZSI6Im1vbmljYSIsInN1YiI6Ik1wZGJVc2VyIiwiYXVkIjoiTXBkYlVzZXJzIiwiaXNzIjoiTXBkYlJlc3RBUEkifQ.XL6pk-_eG72xY-rmn8c-zE1d6KI9qkgjv7bvW2q6jBRjKe4qUcIDhWEssPchYYa79oKAHsPofGPFfxS7FSUNCA"}' http://localhost:8090/api/token/verify/

{}

Test refresh token verify:

$ curl -X POST -H "Content-Type: application/json" -d '{"token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjE5NzEyMjY4LCJqdGkiOiIyODlkMjkwY2RiNjY0MTJjYTUzNzVjMDcwMGY0ODUwMCIsInVzZXJfaWQiOjEsInVzZXJuYW1lIjoibW9uaWNhIiwic3ViIjoiTXBkYlVzZXIiLCJhdWQiOiJNcGRiVXNlcnMiLCJpc3MiOiJNcGRiUmVzdEFQSSJ9.ppPOSPGZf4tDkijRfBjuMutpu0yhtK2Z_ZdOyTqsbU1q5tRUZsaciysmFUEKKwjvg4LQaYPF7BPyKeqR6-_U3w"}' http://localhost:8090/api/token/verify/

{}

Test an invalid/expired token:

$ curl -X POST -H "Content-Type: application/json" -d '{"token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjE5NzEyMjY4LCJqdGkiOiIyODlkMjkwY2RiNjY0MTJjYTUzNzVjMDcwMGY0ODUwMCIsInVzZXJfaWQiOjEsInVzZXJuYW1lIjoibW9uaWNhIiwic3ViIjoiTXBkYlVzZXIiLCJhdWQiOiJNcGRiVXNlcnMiLCJpc3MiOiJNcGRiUmVzdEFQSSJ9.ppPOSPGZf4tDkijRfBjuMutpu0yhtK2Z_ZdOyTqsbU1q5tRUZsaciysmFUEKKwjvg4LQaYPF7BPyKeqR6-_U3"}' http://localhost:8090/api/token/verify/

{"detail":"Token is invalid or expired","code":"token_not_valid"}

Prerequisite for the obtaining a token:

The username and password for the user requesting a new token must already exist in the django Users database.  For this, run the 'python manage.py createsuperuser' command
to create a user first.

The project also offers 2 services:
1. The hello service ( returns the message Hello )
2. The students service ( view/update/delete/modify students).

# To be noted:
The framework will authorize access to the hello and student service in curl commands if the RS256 token is set in the Authorization Bearer header of the request and will return an error when the curl requests for a protected resource are submitted without a Bearer Token.
The error message returned is:

{"detail":"Authentication credentials were not provided."}

The framework DOES NOT automatically authorize access to the hello and student services when requests for the hello and students resources are submitted via browser. When these endpoints are accessed via browser, an error message is returned:  "detail": "Authentication credentials were not provided."

1. http://localhost:8090/hello/
2. http://localhost:8090/students/
3. http://localhost:8090/students/1

# Test protection of api resources with Bearer JWT Token:

In order to access protected resource via restufl api urls you must include the Authorization: Bearer <your_token> header.  The default prefix can be overridden in settings.py with JWT_AUTH_HEADER_PREFIX = <YOUR_AUTH_HEADER>.

$ curl -H "Content-Type: application/json" -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjE4OTYxNzM0LCJqdGkiOiIyZTMxNzc1NzNjOWM0MzgxOTYwMGNlNmIxNjNjNzUwNSIsInVzZXJfaWQiOjUsImF1ZCI6Ik1wZGJVc2VycyIsImlzcyI6Ik1wZGJSZXN0QVBJIn0.nwKjo4jGWbegZmz0dGoUIieqxeoryGdJlmN9gd33__Co1iIho6H2YbtAXp5eLyE-K7ZdhUnnVbyCNtxka4wQpQ" -X GET  http://localhost:8090/hello/

{"message":"Hello, World!"}

Test protection of the hello resource WITHOUT Bearer JWT Token:

$ curl -X GET http://localhost:8090/hello/

{"detail":"Authentication credentials were not provided."}

Test protection of the students list resource with Bearer JWT Token:

$ curl -H "Content-Type: application/json" -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjE4OTYxNzM0LCJqdGkiOiIyZTMxNzc1NzNjOWM0MzgxOTYwMGNlNmIxNjNjNzUwNSIsInVzZXJfaWQiOjUsImF1ZCI6Ik1wZGJVc2VycyIsImlzcyI6Ik1wZGJSZXN0QVBJIn0.nwKjo4jGWbegZmz0dGoUIieqxeoryGdJlmN9gd33__Co1iIho6H2YbtAXp5eLyE-K7ZdhUnnVbyCNtxka4wQpQ" -X GET  http://localhost:8090/api/students/

[{"pk":1,"first_name":"Alberto","last_name":"Dinardi","email":"dinardi@gmail.com","classroom":"Data Structures Java"},{"pk":2,"first_name":"Gemma","last_name":"Crane","emai
l":"voci@gmail.com","classroom":"Advertising"}]

Test protection of the students list resource WITHOUT Bearer JWT Token:

$ curl -X GET http://localhost:8090/api/students/

{"detail":"Authentication credentials were not provided."}

Test protection of a student (pk required) resource with Bearer JWT Token:

$ curl -H "Content-Type: application/json" -H "Authorization: Bearer 
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjE4OTYxNzM0LCJqdGkiOiIyZTMxNzc1NzNjOWM0MzgxOTYwMGNlNmIxNjNjNzUwNSIsInVzZXJfaWQiOjUsImF1ZCI6Ik1wZGJVc2VycyIsImlzcyI6Ik1wZGJSZXN0QVBJIn0.nwKjo4jGWbegZmz0dGoUIieqxeoryGdJlmN9gd33__Co1iIho6H2YbtAXp5eLyE-K7ZdhUnnVbyCNtxka4wQpQ" -X GET  http://localhost:8090/api/students/1

[{"pk":1,"first_name":"Alberto","last_name":"Dinardi","email":"dinardi@gmail.com","classroom":"Data Structures Java"},{"pk":2,"first_name":"Gemma","last_name":"Crane","emai
l":"voci@gmail.com","classroom":"Advertising"}]

Test protection of a student (pk required) resource with Bearer JWT Token:

$ curl -X GET http://localhost:8090/api/students/1

{"detail":"Authentication credentials were not provided."



