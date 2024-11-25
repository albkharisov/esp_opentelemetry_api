idf_component_register(
    PRIV_REQUIRES esp_opentelemetry_sdk
)

get_target_property(OPENTELEMETRY_API
    __idf_albkharisov__esp_opentelemetry_api
    OPENTELEMETRY_API_TARGET
)

target_link_libraries(${COMPONENT_LIB} INTERFACE ${OPENTELEMETRY_API})

