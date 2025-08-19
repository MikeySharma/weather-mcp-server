# Weather MCP Server

A Model Context Protocol (MCP) server that provides weather alerts and forecasts using the National Weather Service (NWS) API.

## Features

- **Weather Alerts**: Get active weather alerts for any US state
- **Location Forecasts**: Retrieve detailed weather forecasts for specific coordinates
- **Real-time Data**: Pulls live data from the official NWS API
- **MCP Compliance**: Fully compatible with Claude Desktop and other MCP clients

## Installation

1. **Clone the repository**:
```bash
git clone https://github.com/MikeySharma/weather-mcp-server.git
cd weather-mcp-server
```

2. **Install dependencies**:
```bash
npm install
```

3. **Build the project**:
```bash
npm run build
```

## Configuration

### Claude Desktop Setup

Add the following to your Claude Desktop MCP settings:

**macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`  
**Windows**: `%APPDATA%/Claude/claude_desktop_config.json`

```json
{
  "mcpServers": {
    "weather": {
      "command": "node",
      "args": ["/absolute/path/to/weather-mcp-server/build/index.js"],
      "env": {
        "NODE_ENV": "production"
      }
    }
  }
}
```

### Environment Variables

The server uses the following environment variables (optional):

```bash
# For debugging
export DEBUG_MCP=true
export NODE_ENV=development
```

## Usage

### Available Tools

#### 1. Get Weather Alerts
```javascript
get_alerts(state: string)
```
- **state**: Two-letter US state code (e.g., "CA", "NY", "TX")
- **Returns**: Active weather alerts for the specified state

#### 2. Get Weather Forecast
```javascript
get_forecast(latitude: number, longitude: number)
```
- **latitude**: Geographic latitude (-90 to 90)
- **longitude**: Geographic longitude (-180 to 180)
- **Returns**: 7-day weather forecast for the location

### Example Usage with Claude

```
@weather get_alerts(state: "CA")
```

```
@weather get_forecast(latitude: 37.7749, longitude: -122.4194)
```

## API Reference

### National Weather Service API

This server uses the official NWS API:
- **Base URL**: `https://api.weather.gov`
- **Rate Limits**: Please respect rate limits (typically 10 requests/minute)
- **Data Coverage**: US territories only

### Response Formats

#### Alerts Response
```typescript
{
  features: Array<{
    properties: {
      event: string;
      severity: string;
      description: string;
      instruction: string;
      areaDesc: string;
      effective: string;
      expires: string;
    }
  }>
}
```

#### Forecast Response
```typescript
{
  properties: {
    periods: Array<{
      name: string;
      temperature: number;
      temperatureUnit: string;
      windSpeed: string;
      windDirection: string;
      shortForecast: string;
      detailedForecast: string;
    }>
  }
}
```

## Development

### Project Structure
```
weather-mcp-server/
├── src/
│   ├── index.ts          # Main server entry point
│   ├── tools/
│   │   └── weather-tools.ts  # Tool implementations
│   └── helpers/
│       └── weather-service.helpers.ts  # API helpers
├── build/                # Compiled JavaScript
├── package.json
└── tsconfig.json
```

### Building from Source

1. **Install TypeScript** (if not already installed):
```bash
npm install -g typescript
```

2. **Build the project**:
```bash
npm run build
```

3. **Development mode with watch**:
```bash
npm run dev
```

### Testing

Run the server in standalone mode for testing:
```bash
npm start
```

## Error Handling

The server includes comprehensive error handling for:
- Network timeouts
- API rate limiting
- Invalid coordinates
- Unsupported locations (non-US)
- Malformed responses

## Limitations

- **US Only**: NWS API only provides data for US territories
- **Rate Limits**: Respect NWS API rate limits (10 requests/minute)
- **Coordinate Precision**: Limited to 4 decimal places for grid points

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- [National Weather Service](https://www.weather.gov/) for providing the free API
- [Anthropic](https://www.anthropic.com/) for the Model Context Protocol specification
- [MCP SDK](https://github.com/modelcontextprotocol/sdk) developers

## Support

For issues and questions:
1. Check the [existing issues](https://github.com/MikeySharma/weather-mcp-server/issues)
2. Create a new issue with detailed description
3. Include error logs and reproduction steps

## Changelog

### v1.0.0
- Initial release with alerts and forecast functionality
- MCP protocol compliance
- TypeScript implementation
- Comprehensive error handling

---

**Note**: This server requires an internet connection to fetch weather data from the NWS API.