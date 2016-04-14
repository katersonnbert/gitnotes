G-Node INfrastructure (GIN)
===========================

# General information about G-Node Go projects:
- Use tabs not whitespaces.
- Use snake case for variables.

# GIN-auth 
The GIN-auth is an OAuth2 server implementation within the G-Node infrastructure.

## General information
- For local development, check that all required go dependencies (check travis.yml) are installed.
- Check the `doc` folder for documentation first.
- The framework uses a POSTGRESQL database.
- DB config can be found in `conf.dbconf.yml`.
- Use goose for changing the database scheme; put all scripts into the `conf.migrations` package.
- All available clients can be found in both database and in `conf.clients.yml`.
- Run tests with `go tests ./...`
- Travis CI is used, it uses golint and go vet to check for bugs and goveralls for coverage.
- The go packages e.g. `data` and `util` need to be tested individually, goverall requires that the resulting 
profiles are assembled manually for a full coverage report.
- 

