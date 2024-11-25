idf_component_register(
    PRIV_REQUIRES esp_opentelemetry_sdk
)

target_link_libraries(${COMPONENT_LIB} INTERFACE opentelemetry_api)

