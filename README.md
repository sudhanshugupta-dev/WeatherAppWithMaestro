# TaskBit Weather App

TaskBit is an Expo/React Native app that surfaces current and hourly weather powered by the OpenWeather API. The free tier blocks the 7-day forecast endpoint, so the app focuses on reliable hourly data as a practical alternative.

## Features
- Current conditions and hourly outlook based on the user's location (requests runtime location permission).
- Swipeable hourly carousel with explicit test IDs for UI automation.
- Simple checkout demo flow used by the end-to-end test suite.
- Graceful fallback when the paid 7-day forecast API is unavailable.

## Getting Started
1. Install dependencies: `npm install`
2. Add your OpenWeather API key in `utils/api.js` (`API_KEY` constant). Consider moving this to environment config before production.
3. Start the dev server: `npx expo start`
4. Run on device: scan the QR in Expo Go, or use an emulator/simulator from the Expo CLI menu.

## Project Layout
- `app/` – Expo Router screens and navigation.
- `components/` – Shared UI pieces.
- `utils/api.js` – OpenWeather fetch helper (update `API_KEY` here).
- `.maestro/` – Maestro UI flow definitions.
- `debug_output/.maestro`, `maestro-debug/.maestro` – Saved Maestro artifacts (logs/screens).

## Screenshots
Captured images live in the repo root for easy GitHub preview:

![Home Screen](home_screen.png)
![Hourly Forecast](forecast_screen.png)
![Form Flow](blackscreen.png)
![Checkout Success](checkout_success.png)

In-app icons/art live under `assets/` (e.g., `assets/cloud.png`, `assets/sun.png`, `assets/images/icon.png`).

## Maestro Setup & Usage
Maestro drives the UI tests located in `.maestro/*.yaml` (e.g., `home_test.yaml`, `fullApp_test.yaml`, `forecast_test.yaml`).

1. Install Maestro (one-time): `curl -Ls https://get.maestro.mobile.dev | bash` and ensure `~/.maestro/bin` is on your `PATH`.
2. Prepare a device/emulator with the dev build that matches `appId: com.sudhanshu30602.TaskBit`:
   - Android: `expo run:android` to install a development build.
   - iOS: `expo run:ios` (or use an existing dev client build).
3. Start the Expo dev server with the dev client: `npx expo start --dev-client`.
4. Run a flow from project root (device/emulator must be unlocked and on the home screen):
   - `maestro test .maestro/home_test.yaml` – validates the hourly forecast UI.
   - `maestro test .maestro/fullApp_test.yaml` – full journey from weather view through checkout.
5. Collect richer debug output during a run: `maestro test .maestro/fullApp_test.yaml --output debug_output`. Artifacts will land under `debug_output/.maestro` and `maestro-debug/.maestro`.

Tips:
- Keep location services enabled so the weather request can resolve coordinates.
- If you change the `appId`, update it in each `.maestro/*.yaml`.
- The flows rely on test IDs such as `horizontal-flatlist`, `list-item-*`, and `Forecast`; ensure they stay stable when refactoring UI components.
