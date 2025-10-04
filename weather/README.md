# Weather MCP Server

A Model Context Protocol (MCP) server that provides weather forecasts and alerts from the National Weather Service (NWS) API.

## Features

- 🌤️ Get detailed weather forecasts for any US location
- ⚠️ Check active weather alerts by state
- 🚀 Fast and lightweight using FastMCP
- 🐳 Docker support for easy deployment

## MCP Tools

### `get_forecast(latitude: float, longitude: float)`
Get a detailed 5-period weather forecast for a specific location.

**Parameters:**
- `latitude`: Latitude of the location
- `longitude`: Longitude of the location

**Example:** `get_forecast(38.5816, -121.4944)` for Sacramento, CA

### `get_alerts(state: str)`
Get active weather alerts for a US state.

**Parameters:**
- `state`: Two-letter US state code (e.g., "CA", "NY", "TX")

**Example:** `get_alerts("CA")` for California alerts

### `get_timezone(latitude: float, longitude: float)`
Get timezone information for a specific location.

**Parameters:**
- `latitude`: Latitude of the location
- `longitude`: Longitude of the location

**Example:** `get_timezone(39.3292, -82.1013)` for Athens, OH

## Installation Options

### Option 1: Local Installation with uv (Recommended for Development)

1. **Install uv** (if not already installed):
   ```bash
   curl -LsSf https://astral.sh/uv/install.sh | sh
   ```

2. **Clone and setup:**
   ```bash
   cd weather
   uv sync
   ```

3. **Run the server:**
   ```bash
   uv run weather
   ```

4. **Configure in your MCP client** (e.g., Cursor's `~/.cursor/mcp.json`):
   ```json
   {
     "mcpServers": {
       "weather": {
         "command": "uv",
         "args": [
           "--directory",
           "/path/to/weather",
           "run",
           "weather"
         ],
         "description": "Weather tools to get forecasts and alerts"
       }
     }
   }
   ```

### Option 2: Docker (Recommended for Distribution)

#### Build the Docker image:

```bash
# Build locally
docker build -t weather-mcp:latest .

# Or use docker-compose
docker-compose build
```

#### Run the container:

```bash
# Run with Docker
docker run -i weather-mcp:latest

# Or use docker-compose
docker-compose up
```

#### Configure in your MCP client:

```json
{
  "mcpServers": {
    "weather": {
      "command": "docker",
      "args": [
        "run",
        "-i",
        "--rm",
        "weather-mcp:latest"
      ],
      "description": "Weather tools to get forecasts and alerts"
    }
  }
}
```

### Option 3: Share via Docker Hub

#### Push to Docker Hub:

```bash
# Tag your image
docker tag weather-mcp:latest yourusername/weather-mcp:latest

# Login to Docker Hub
docker login

# Push the image
docker push yourusername/weather-mcp:latest
```

#### Others can pull and use:

```bash
# Pull the image
docker pull yourusername/weather-mcp:latest

# Configure in MCP client
{
  "mcpServers": {
    "weather": {
      "command": "docker",
      "args": [
        "run",
        "-i",
        "--rm",
        "yourusername/weather-mcp:latest"
      ]
    }
  }
}
```

## Requirements

### Local Installation:
- Python 3.12+
- uv package manager

### Docker Installation:
- Docker 20.10+
- Docker Compose 2.0+ (optional)

## Data Source

This server uses the [National Weather Service API](https://www.weather.gov/documentation/services-web-api), which provides:
- Free weather data for US locations
- No API key required
- Real-time forecasts and alerts
- Official government weather data

## Example Usage

Once configured with an MCP-compatible AI assistant:

```
User: "What's the weather in Sacramento?"
AI: [Calls get_forecast(38.5816, -121.4944)]
AI: "Sacramento will be sunny with a high of 77°F today..."

User: "Are there any weather alerts in California?"
AI: [Calls get_alerts("CA")]
AI: "There is a Freeze Warning for Northern California..."
```

## Development

### Project Structure:
```
weather/
├── weather.py          # Main MCP server code
├── pyproject.toml      # Python project configuration
├── uv.lock            # Dependency lock file
├── Dockerfile         # Docker image definition
├── docker-compose.yml # Docker Compose configuration
└── README.md          # This file
```

### Adding New Features:

Add new tools by decorating async functions with `@mcp.tool()`:

```python
@mcp.tool()
async def your_new_tool(param: str) -> str:
    """Tool description for the AI.
    
    Args:
        param: Parameter description
    """
    # Your implementation
    return "result"
```

## Troubleshooting

**Server not appearing in MCP client:**
- Ensure the server is properly configured in your MCP client's config file
- Check that the command path is correct
- Restart your MCP client after configuration changes

**Docker connection issues:**
- Ensure Docker is running: `docker ps`
- Check if the image exists: `docker images | grep weather-mcp`
- View container logs: `docker logs weather-mcp-server`

**API request failures:**
- NWS API requires a User-Agent header (already configured)
- Coordinates must be within the US
- Check your internet connection

## License

This project uses the National Weather Service API, which is in the public domain.

## Contributing

Contributions are welcome! Feel free to:
- Add new weather-related tools
- Improve error handling
- Enhance documentation
- Submit bug reports

## Support

For issues or questions:
1. Check the troubleshooting section above
2. Review the MCP documentation
3. Ensure your coordinates/state codes are valid

