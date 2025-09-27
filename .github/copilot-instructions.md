# Copilot Instructions for Enphase Envoy Installer Integration

## Project Overview
This is a Home Assistant custom integration for Enphase Envoy devices (firmware v7+), focused on installer/DIY accounts but compatible with Home Owner accounts. It provides advanced monitoring and control features for solar, battery, relay, and grid profiles.

## Architecture & Key Components
- **custom_components/enphase_envoy/**: Main integration code. Entry point is `__init__.py` (async setup, coordinator, service registration).
- **envoy_reader.py**: Handles all communication with the Envoy device, including data fetching, streaming, and endpoint management.
- **PLATFORMS**: Integration supports multiple Home Assistant platforms (sensor, switch, number, select, binary_sensor).
- **DataUpdateCoordinator**: Used for periodic polling and state management. Realtime updates are handled via a background task and throttled callback.
- **Service Calls**: Custom Home Assistant services for grid profile management and API key retrieval are registered in `__init__.py`.
- **Persistent Storage**: Uses Home Assistant's `Store` for saving tokens and state between restarts.

## Developer Workflows
- **Installation**: Follows HACS custom integration workflow. See README for steps.
- **Testing**: No explicit test runner or test scripts found; test data is in `test_data/` for endpoint simulation. Use Home Assistant's dev tools for integration testing.
- **Debugging**: Logging is via `_LOGGER`. Enable debug logging in Home Assistant for detailed output.
- **Build/Release**: No build scripts; release is managed via GitHub and HACS.

## Project-Specific Patterns & Conventions
- **Async/Await**: All I/O and Home Assistant interactions are async. Use `async def` for all entry points and service handlers.
- **ConfigEntry Options**: Many behaviors (endpoints, polling, throttling) are controlled via config entry options. Always check `entry.options` for feature flags.
- **Entity Naming**: Entities are dynamically named based on device serials and types. See README for entity naming conventions.
- **Realtime Updates**: If enabled, a background task streams meter data and updates entities via throttled callback (`update_production_meters`).
- **Service Registration**: Custom services are registered using `hass.services.async_register` with domain-specific handlers.
- **Error Handling**: API errors raise `ConfigEntryAuthFailed` or `UpdateFailed` for Home Assistant to handle.

## Integration Points & External Dependencies
- **Home Assistant Core**: Uses config entries, event bus, service calls, and platform forwarding.
- **httpx**: For async HTTP requests to Envoy device.
- **numpy**: Used for some data processing (e.g., `isin`).
- **Persistent Storage**: Home Assistant's `Store` for token/state management.

## Examples & References
- **Entry Point**: See `custom_components/enphase_envoy/__init__.py` for setup, coordinator, and service registration.
- **Device Communication**: See `envoy_reader.py` for all Envoy API interactions.
- **Test Data**: See `test_data/envoy_metered/` for example endpoint responses.

## Conventions
- Use async/await for all I/O and Home Assistant API calls.
- Register new entities/platforms via `async_forward_entry_setups`.
- Use `DataUpdateCoordinator` for periodic polling and state management.
- Use throttled callbacks for high-frequency updates (realtime meter streaming).
- Store persistent state/tokens using Home Assistant's `Store`.
- Register custom services for advanced device control and diagnostics.

---
If any section is unclear or missing, please provide feedback or specify which workflows, patterns, or integration points need further documentation.
