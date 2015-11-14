# NodeJS Microservices: 
# Appendix 2: Resources

3rd party tools and services
----------------------------
__Commercial__
* [mongolab](https://mongolab.com/): Fully managed MongoDB-as-a-Service, with first 500MB data for free 
* [docker](https://www.docker.com/): An open platform for distributed applications for developers and sysadmins
* [nginx](http://nginx.org/) monitoring available with official
[nginx+](https://www.nginx.com/products/) or 3rd party
[scout](https://scoutapp.com/plugin_urls/72-nginx-monitoring)

__Open source__
* [strider](https://github.com/Strider-CD/strider): Open Source Continuous Integration & Deployment Server
* [grafana](http://grafana.org/): Graph and dashboard builder for visualizing time series metrics
* [beanstalkd](http://kr.github.io/beanstalkd/): Beanstalk is a simple, fast work queue
* [munin](http://munin-monitoring.org/): Munin is a networked resource monitoring tool 

NodeJS tools
------------
* [guvnor](https://www.npmjs.com/package/guvnor): A node process manager with web interface and security options
* [Seneca][seneca-url] - seneca is a microservices toolkit for Node.js
* [winston](http://github.com/winstonjs/winston): A multi-transport async logging library for node.js 
* [apidoc](http://apidocjs.com/): Inline Documentation for RESTful web APIs

Http plugins
------------

* [chairo](https://github.com/hapijs/chairo) for Hapi
* [seneca-web](https://github.com/senecajs/seneca-web) for Express.


[Seneca][seneca-url] examples and plugins 
-------------------------------------------------------
_by Richard Rodger (CTO of nearForm)_

* [seneca-examples][seneca-examples]: Node.js seneca module usage examples
* [nodezoo][nodezoo]: A Search Engine for Node.js Modules
* [data-entities][data-entities]: Using multiple databases at the same time for different kinds of entities.
* [simple-plugin](simple-lugin): Create a simple Seneca plugin, including unit tests.
* [api-server](api-server): building a REST server with Seneca
* [plugin-web](plugin-web): creating plugins that expose web user interfaces
* [micro-services](micro-services): create a small micro-services system   
* [shopping-cart](shopping-cart): A shopping cart example, showing how plugins expose additional HTTP APIs
* [seneca-auth](seneca-auth): A user authentication plugin, using [PassportJS](http://passportjs.org).
* [user-accounts](user-accoutns): A user account system, showing login/logout logic.
* [seneca-beanstalk-transport](https://github.com/rjrodger/seneca-beanstalk-transport)

[seneca-url]: http://senecajs.org
[seneca-examples]: https://github.com/rjrodger/seneca-examples
[nodezoo]: https://github.com/rjrodger/nodezoo
[data-entities]: http//github.com/rjrodger/seneca-examples/tree/master/data-entities
[api-server]: http://github.com/rjrodger/seneca-examples/tree/master/api-server
[plugin-web]: http://github.com/rjrodger/seneca-examples/tree/master/plugin-web
[micro-services]: http://github.com/rjrodger/seneca-examples/tree/master/micro-services
[shopping-cart]: http://github.com/rjrodger/seneca-examples/tree/master/shopping-cart
[seneca-auth]: https://github.com/rjrodger/seneca-auth
[user-accounts]: https://github.com/rjrodger/seneca-examples/tree/master/user-accounts