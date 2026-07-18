# Portfolio Dashboard

Portfolio Dashboard is a private, read-only portfolio analytics site rendered inside Home Assistant.
It stores imported history, cached market data, and manually entered satellite portfolios in the
app's persistent `/data` directory.

## Installation

1. Wait for the private source repository's release workflow to publish the container image.
2. Keep the `home-portfolio-dashboard-addon` GHCR package private. Configure the Home Assistant
   `ghcr.io` registry credential with a dedicated GitHub token that has read-only `read:packages`
   access to that package.
3. Add `https://github.com/Arxxi/HomeLab-Apps` under **Settings -> Apps -> App store -> menu ->
   Repositories**.
4. Install **Portfolio Dashboard** and enter read-only exchange API credentials in Configuration.
5. Start the app, open **Portfolio** in the sidebar, then enable **Auto update**.

The public repository contains only neutral Home Assistant metadata and this installation guide.
Credentials are saved by Home Assistant as app options and are mapped to server-only environment
variables at startup. They are never included in the manifest or container image.

## Configuration

- **Exchange public key / secret key**: credentials restricted to read-only account and market-data
  access. Never grant trading, conversion, transfer, or withdrawal permissions.
- **CoinGecko demo API key**: optional. It improves market-data availability for satellite assets.
- **Public price refresh**: defaults to every 60 minutes and cannot be set below 15 minutes.
- **Account sync**: defaults to every 24 hours and cannot be set below 6 hours.
- **Timezone**: used by the container runtime; the default is `Europe/Kyiv`.

If a satellite asset has no current provider price, the dashboard marks it with an orange warning
and excludes it from current-value and profit/loss totals rather than using a misleading fallback.

## Backup and updates

Home Assistant cold backups include the persistent `/data` directory. Updates replace the app
container but retain that directory. Keep normal Home Assistant backups before importing or
rebuilding a large history.
