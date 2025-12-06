# Things-Kit

**Core Framework for Modular Go Microservices**

Things-Kit is the core framework for building modular, opinionated microservices in Go, built on [Uber Fx](https://github.com/uber-go/fx). This repository contains the foundational components that all Things-Kit applications depend on.

## What's Included

This core repository provides:

- **App Runner** (`app/`) - Application lifecycle management and bootstrapping
- **Logger Interface** (`log/`) - Standard logging interface abstraction
- **Logger Implementation** (`logging/`) - Zap-based logger implementation
- **Configuration** (`viperconfig/`) - Viper-based configuration management

## Installation

```bash
go get github.com/things-kit/things-kit
```

## Quick Start

```go
package main

import (
	"github.com/things-kit/things-kit/app"
	"github.com/things-kit/things-kit/logging"
	"github.com/things-kit/things-kit/viperconfig"
)

func main() {
	app.New(
		viperconfig.Module,
		logging.Module,
		// Add your services here
	).Run()
}
```

## Core Modules

### App (`app/`)

The application runner wraps Uber Fx and provides:
- Application lifecycle management
- Clean startup/shutdown handling
- Helper functions for registering startup tasks

```go
app.New(
	viperconfig.Module,
	logging.Module,
	app.AsStartupFunc(RunMigrations), // Run before startup
).Run()
```

### Logger Interface (`log/`)

Defines the standard logging interface:

```go
type Logger interface {
	Info(msg string, fields ...Field)
	Error(msg string, err error, fields ...Field)
	Debug(msg string, fields ...Field)
	Warn(msg string, fields ...Field)
	
	// Context-aware methods for distributed tracing
	InfoC(ctx context.Context, msg string, fields ...Field)
	ErrorC(ctx context.Context, msg string, err error, fields ...Field)
	DebugC(ctx context.Context, msg string, fields ...Field)
	WarnC(ctx context.Context, msg string, err error, fields ...Field)
}
```

### Logger Implementation (`logging/`)

Zap-based implementation of the Logger interface with:
- Configurable log levels (debug, info, warn, error)
- Multiple output formats (json, console)
- Structured logging support
- Context-aware logging

Configuration example (`config.yaml`):
```yaml
logging:
  level: info
  encoding: json
  output_path: stdout
```

### Configuration (`viperconfig/`)

Viper-based configuration management with:
- YAML configuration files
- Environment variable overrides
- Automatic key replacement (`.` → `_` for env vars)
- Decentralized configuration (each module loads its own config)

Example:
```yaml
# config.yaml
logging:
  level: debug

grpc:
  port: 9090
```

Environment variable override:
```bash
export LOGGING_LEVEL=info
export GRPC_PORT=8080
```

## Architecture

Things-Kit follows these principles:

1. **Dependency Injection**: All components use Uber Fx for DI
2. **Interface-Based Design**: Program to abstractions, not implementations
3. **Modularity**: Each feature is a separate, optional module
4. **Lifecycle Management**: Graceful startup and shutdown
5. **Convention over Configuration**: Sensible defaults with override capability

## Additional Modules

The Things-Kit ecosystem includes separate modules for various infrastructure components:

- **[things-kit-http](https://github.com/things-kit/things-kit-http)** - HTTP server interface
- **[things-kit-httpgin](https://github.com/things-kit/things-kit-httpgin)** - Gin-based HTTP implementation
- **[things-kit-grpc](https://github.com/things-kit/things-kit-grpc)** - gRPC server
- **[things-kit-redis](https://github.com/things-kit/things-kit-redis)** - Redis cache implementation
- **[things-kit-sqlc](https://github.com/things-kit/things-kit-sqlc)** - Database connection pooling
- **[things-kit-kafka](https://github.com/things-kit/things-kit-kafka)** - Kafka consumer
- **[things-kit-cache](https://github.com/things-kit/things-kit-cache)** - Cache interface abstraction
- **[things-kit-messaging](https://github.com/things-kit/things-kit-messaging)** - Messaging interface abstraction
- **[things-kit-testing](https://github.com/things-kit/things-kit-testing)** - Testing utilities

## Examples

Complete example projects are available:

- **[things-kit-example](https://github.com/things-kit/things-kit-example)** - Basic HTTP server
- **[things-kit-example-db](https://github.com/things-kit/things-kit-example-db)** - HTTP + PostgreSQL with tests

## Documentation

For complete documentation and tutorials, visit [things-kit.dev](https://things-kit.dev).

## License

MIT License - see LICENSE file for details
