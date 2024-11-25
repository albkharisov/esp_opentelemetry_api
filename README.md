# esp_opentelemetry_api

ESP wrapper for API of [OpenTelemetry C++ Client](https://github.com/open-telemetry/opentelemetry-cpp).


## What is OpenTelemetry?

[OpenTelemetry](https://opentelemetry.io/) is a collection of APIs, SDKs, and tools. Use it to instrument, generate, collect, and export telemetry data (metrics, logs, and traces) to help you analyze your software’s performance and behavior.


## Dependencies

This package comes with `albkharisov/esp_opentelemetry_sdk`, as it depends
on it. But build will succeed even if you exclude linking to SDK part
in another component and forget to add exporters because API part consist
of default stub implementation.


## Description

This package provides a toolbox for instrumenting your code with OpenTelemetry.
You have to define dependency on it in each component you want to instrument.
Actually this package is only importer of a target from esp_opentelemetry_sdk.

## Example of instrumentation:

Logs:
```
#include "opentelemetry/logs/provider.h"

auto logger_provider = opentelemetry::logs::Provider::GetLoggerProvider();
auto logger = logger_provider->GetLogger("foo_library_logger", "foo_library");
```

Spans:
```
#include "opentelemetry/trace/provider.h"

auto tracer_provider = opentelemetry::trace::Provider::GetTracerProvider();
auto tracer = tracer_provider->GetTracer("foo_library", "1.0.0");
auto span = tracer->StartSpan("Write file 'data'");

{
    auto scope = tracer->WithActiveSpan(span);
}
```

Metrics:
```
#include "opentelemetry/metrics/provider.h"

auto meter_provider = opentelemetry::metrics::Provider::GetMeterProvider();
auto meter = meter_provider->GetMeter("my_metrics", "1.2.0");
auto double_counter = meter->CreateDoubleCounter("cycle_count");

std::map<std::string, std::string> labels = {{"key", "value"}};
auto labelkv = opentelemetry::common::KeyValueIterableView<decltype(labels)>{labels};

for (;;) {
    using namespace std::chrono_literals;
    std::this_thread::sleep_for(500ms);
    double_counter->Add(1.0, labelkv);
}
```

Instrumentation examples from original repo are [here](https://github.com/open-telemetry/opentelemetry-cpp/tree/main/examples/common/foo_library).
Documentation + examples can be found [here](https://opentelemetry-cpp.readthedocs.io/en/latest/api/GettingStarted.html) and [here](https://opentelemetry.io/docs/languages/cpp/instrumentation/).

