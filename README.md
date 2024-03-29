# Decidim app by OSP

Citizen Participation and Open Government application.
## How to deploy with Terraform?

Check the list of make commands in the Makefile. Each command corresponds to a provider and a specific need.

- To deploy a new infrastructure with Scaleway

```make
make deploy-scw
```

## Setting up the application

You will need to do some steps before having the app working properly once you've deployed it:

1. Open a Rails console in the server: `bundle exec rails console`
2. Create a System Admin user:
```ruby
email = <your email>
password = <a secure password>
user = Decidim::System::Admin.new(email: email, password: password, password_confirmation: password)
user.save!
```
3. Visit `<your app url>/system` and login with your system admin credentials
4. Create a new organization. Check the locales you want to use for that organization, and select a default locale.
5. Set the correct default host for the organization, otherwise the app will not work properly. Note that you need to include any subdomain you might be using.
6. Fill the rest of the form and submit it.

You're good to go!

## Running tests

This application has a functional testing suite. You can easily run locally the tests as following :

Create test environment database 

`bundle exec rake test:setup`

And then run tests using `rspec`

`bundle exec rspec spec/`

## Docker
### How to use it? 
You can boot a Decidim environment in Docker using the Makefile taht will run docker-compose commands and the last built image from the Dockerfile.
Three context are available : 

- **Clean Decidim**

An environment running the current Decidim version (from Gemfile) without any data.
```make
make start-clean-decidim
```

- **Seeded Decidim**

An environment running the current Decidim version (from Gemfile) with generated seeds
```make
make start-seeded-decidim
```

- **Dumped Decidim**

An environment running the current Decidim version (from Gemfile) with real data dumped from an existing platform to simulate a Decidim bump version before doing in the real production environment.
```make
make start-dumped-decidim
```
***Warning : you need to get a psql dump on your local machine to restore it in your containerized database***
***Warning2 : you need to set organization host to 0.0.0.0 with the rails console***


### How to stop and remove it? 

To get rid off your Docker environmnent : 

- Shut down Docker environmnent
```make
make stop
```

- Delete resources
```make
make delete
```
### Troubleshooting

Make commands are available to help you troubleshoot your Docker environment

- Start Rails console
 ```make
make rails-console
```
- Start bash session to app container
```make
make connect-app
```

## Database architecture (ERD)

![Architecture_decidim](https://user-images.githubusercontent.com/52420208/133789299-9458fc42-a5e7-4e3d-a934-b55c6afbc8aa.jpg)
