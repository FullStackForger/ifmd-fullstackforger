# HapiJS plugins you should know

## Glue

> Glue provides configuration based composition of hapi's Server object. Specifically it wraps
> * `server = new Hapi.Server(Options)`
> * one or more `server.connection(Options)`
> * zero or more `server.register(Plugin, Options)`
> calling each based on the configuration generated from the Glue manifest.

Github: [hapijs/glue](https://github.com/hapijs/glue)

## Rejoice
> Rejoice is a CLI tool for hapi which requires a js/json file with the config. It relies on the composer library called glue

Github: [hapijs/rejoice](https://github.com/hapijs/rejoice)

## Confidence

> Confidence is a configuration document format, an API, and a foundation for A/B testing. The configuration format is designed to work with any existing JSON-based configuration, serving values based on object path ('/a/b/c' translates to a.b.c). In addition, confidence defines special $-prefixed keys used to filter values for a given criteria.

Github: [hapijs/confidence](https://github.com/hapijs/confidence)
