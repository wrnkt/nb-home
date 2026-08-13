# Rust Logging

#rust


```toml
[dependencies]
log = "0.4"
env_logger = "0.11"

[dependencies]
tracing = "0.1"
tracing-subscriber = "0.3"
```

```rust
use tracing::{trace, debug, info, warn, error};

fn main() {
    tracing_subscriber::fmt::init();

    tracing::info!("Application started");

    trace!("Very detailed diagnostic information");
    debug!("Debug information");
    info!("Normal application event");
    warn!("Something unexpected");
    error!("Something failed");

    info!(
        request_id = %request_id,
        duration_ms = duration,
        "request completed"
    );
}

use tracing::{info, info_span};

fn process_order(order_id: u64) {
    let span = info_span!("process_order", order_id);
    let _enter = span.enter();

    info!("Loading order");
    info!("Charging customer");
    info!("Sending confirmation");
}

use tracing::instrument;

#[instrument]
fn process_order(order_id: u64) {
    // ...
}

#[tracing::instrument]
async fn handle_request(request_id: String) {
    // ...
}



use log::{debug, error, info, warn};

fn main() {
    env_logger::init();

    info!("Application started");
    debug!("Debug information");
    warn!("Something looks suspicious");
    error!("Something went wrong");
}
```
