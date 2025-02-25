# Designing Dapps

As you come up with ideas for dapps, you are going to make many design decisions about how to structure and organize your project. On the Internet Computer, there are a few design decisions that you should pay particular attention to as you plan the implementation for your app.

## Single or multiple canister architecture

One of the first decisions you might want to consider when designing your dapp is whether it should be encapsulated in a single canister or consist of multiple canisters.

For example, if you are writing a simple service with no frontend, you might want to use a single canister to simplify project management and maintenance and focus on adding features. If your dapp has both frontend assets and backend business logic, your project is likely to consist of at least two canisters, with one canister for managing user interface components and another canister for the backend services the application provides.

In planning, you might also consider placing some common reusable services in their own canister so that they can be imported and called from other more-specialized canisters or made available for other developers to use. The [LinkedUp](https://github.com/dfinity/linkedup) sample dapp illustrates this approach by splitting the professional service dapp into two canisters. In the LinkedUp example, the functions that establish social connections are defined in the `connectd` canister and separate from the functions used to set up professional profiles that are defined in the `linkedup` canister. It is easy to imagine extending the dapp with a third canister, for example to schedule events based on profile attributes or shared connections.

## Segregating actors from types and utilities

In planning the architecture for your project, one common practice is to place the code for the main actor in one file with separate additional files for defining the types you program uses and utility functions that don’t require an actor.

For example, you might set up the backend logic for your dapp to consist of the following files:

-   `Main.mo` or `main.rs` with the functions that require an actor to send query and update calls.

-   `Util.mo` or `util.rs` with helper functions that can be imported for the actor to use.

-   `Types.mo` or `types.rs` with all of the data type definitions for your dapp.

## Using query calls

As discussed in [Query and update methods](../../../concepts/canisters-code#query-update), queries return results faster than update calls. Therefore, explicitly marking a function as a `query` is an effective strategy for improving application performance. In the planning and design phase, you should consider how best to use query calls instead of functions that can perform queries or updates.

That is a good general rule to follow and can be applied broadly to most categories of dapps. However, you should also consider the security and performance trade-off that queries don’t go through consensus and do not appear on the blockchain. For some dapps, that trade-off might be appropriate. For example, if you are developing a blogging platform, queries that retrieve articles matching a tag probably don’t warrant going through consensus to ensure that a majority of nodes agree on the results. However, if your dapp is retrieving sensitive information—like financial data—you might want more assurance about your results than a basic query provides.

As an alternative to basic queries, the Internet Computer also supports **certified queries**. Certified queries enable you to receive **authenticated responses** that end users can trust. Using certified queries is an advanced technique that is not covered in the tutorials or other developer-focused documentation, but you can learn about how the authentication works and what you need to do to configure your program to return certified data in response to queries in the [Interface specification](../../../references/ic-interface-spec.md).

## Data storage and retrieval

The Internet Computer enables you to use **stable memory** to handle long-term data storage—often referred to as orthogonal persistence—and to use **query calls** to retrieve your data. Efficiently retrieving data using one or more keys can typically be achieved by using data structures like hash tables. It is also possible to implement a more traditional database inside a canister.
