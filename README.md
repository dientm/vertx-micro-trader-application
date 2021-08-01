# vertx-micro-trader-application
A fake financial app making (virtual) money. The application is composed of a set of microservices:

- The quote generator - this is an absolutely unrealistic  simulator that generates the quotes for 3 fictional companies `MacroHard, Divinator, and Black Coat`. The market data is published on the Vert.x event bus.

- The traders - these are a set of components that receives quotes from the quote generator and decides whether to by or sell a particular share. To make this decision, they rely on another component called the `portfolio` service.

- The portfolio - this service manages the number of shares in our portfolio and their monetary value. It is exposed as a `service proxy`, i.e. an asynchronous RPC service on top of the Vert.x event bus. For every successful operation, it sends a message on the event bus describing the operation. It uses the quote generator to evaluate the current value of the portfolio.

- The audit - that's the legal side. We need to keep a list of all our operations (yes, that's the law). The audit component receives operations from the portfolio service via an event bus and address. It then stores theses in a database. It also provides a REST endpoint to retrieve the latest set of operations.

- The dashboard - some UI to let us know when we become rich.
