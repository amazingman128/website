---
title: "Basic Auth"
weight: 8010
menu:
  book:
    parent: "middlewares"
---


```rust
use salvo::extra::basic_auth::BasicAuthHandler;
use salvo::extra::serve::StaticDir;
use salvo::routing::Router;
use salvo::Server;

#[tokio::main]
async fn main() {
    tracing_subscriber::fmt().init();
    
    let validator = |user_name, password| -> bool {
        user_name == "root" && password == "pwd"
    };
    let auth_handler = BasicAuthHandler::new(validator);

    let router = Router::new()
        .before(auth_handler)
        .get(StaticDir::new(vec!["examples/static/boy", "examples/static/girl"]));
    Server::new(TcpListener::bind(([0, 0, 0, 0], 7878))).serve(router).await.unwrap();
}
```