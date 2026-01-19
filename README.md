# OpenWeatherMap MCP Server

A comprehensive Model Context Protocol (MCP) server for OpenWeatherMap API integration, providing weather data, forecasts, air quality information, and location services.

## Features

- 🌤️ **Current Weather** - Real-time weather data for any location
- 📅 **5-Day Forecast** - Detailed weather predictions with 3-hour intervals
- 🌍 **Location Search** - Find coordinates for any city name
- 📮 **ZIP Code Support** - Weather lookup by postal codes
- 💨 **Air Quality** - Pollution data and AQI levels
- 💾 **Smart Caching** - Reduce API calls with intelligent caching
- 🔄 **Unit Conversion** - Support for metric, imperial, and standard units

## Quick Start

### Prerequisites

- Python 3.8+
- OpenWeatherMap API key (included in .env)
- pip or uv package manager

### Installation

1. Clone or navigate to the project:
```bash
cd /home/jez/Documents/fastmcp/weather-mcp-server
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. The `.env` file is pre-configured with the API key

### Running the Server

```bash
# Run with default STDIO transport
python weather_server.py

# Run with HTTP transport
TRANSPORT=http PORT=8000 python weather_server.py

# Run with FastMCP CLI
fastmcp run weather_server.py

# Development mode with inspector
fastmcp dev weather_server.py

# Test mode
python weather_server.py --test
```

## Available Tools

### 1. `get_current_weather`
Get current weather conditions for any location.

**Parameters:**
- `location` (str): City name (e.g., "London") or coordinates ("51.5,-0.1")
- `units` (str): "metric" (°C), "imperial" (°F), or "standard" (K)
- `include_details` (bool): Include extended information

**Example:**
```python
{
    "location": "London",
    "units": "metric",
    "include_details": true
}
```

### 2. `get_forecast`
Get 5-day weather forecast with 3-hour intervals.

**Parameters:**
- `location` (str): City name or coordinates
- `days` (int): Number of days (1-5)
- `units` (str): Temperature units

**Example:**
```python
{
    "location": "New York",
    "days": 3,
    "units": "imperial"
}
```

### 3. `search_location`
Find locations and their coordinates by name.

**Parameters:**
- `query` (str): Location name to search
- `limit` (int): Maximum results (1-5)

**Example:**
```python
{
    "query": "Paris",
    "limit": 3
}
```

### 4. `get_weather_by_zip`
Get weather by ZIP/postal code.

**Parameters:**
- `zip_code` (str): ZIP or postal code
- `country_code` (str): ISO country code (e.g., "US", "GB")
- `units` (str): Temperature units

**Example:**
```python
{
    "zip_code": "10001",
    "country_code": "US",
    "units": "imperial"
}
```

### 5. `get_air_quality`
Get air pollution data and AQI for a location.

**Parameters:**
- `location` (str): Coordinates ("lat,lon") or city name

**Example:**
```python
{
    "location": "Beijing"
}
```

**AQI Levels:**
- 1 = Good (green)
- 2 = Fair (yellow)
- 3 = Moderate (orange)
- 4 = Poor (red)
- 5 = Very Poor (purple)


## Resources

### `weather://api/status`
Get API status, usage statistics, and cache information.

### `weather://cache/stats`
Get detailed cache statistics.

### `weather://units/info`
Information about available unit systems.

## Prompts

### `weather_analysis`
Generate comprehensive weather analysis for a location.

### `travel_weather`
Create travel weather comparison for multiple destinations.

## Configuration

Edit `.env` file to customize:

```env
# API Configuration
API_KEY=your_api_key_here
DEFAULT_UNITS=metric      # metric, imperial, or standard
DEFAULT_LANG=en          # Language code

# Cache Settings
CACHE_TTL=600           # Cache duration in seconds

# Server Settings
LOG_LEVEL=INFO          # DEBUG, INFO, WARNING, ERROR
MAX_DAILY_CALLS=1000    # API rate limit
```

## Deployment

### Local Installation (Claude Desktop)

```bash
fastmcp install claude-desktop weather_server.py
```

### FastMCP Cloud Deployment

1. Push to GitHub:
```bash
git init
git add .
git commit -m "OpenWeatherMap MCP Server"
gh repo create weather-mcp-server --public
git push -u origin main
```

2. Deploy on [fastmcp.cloud](https://fastmcp.cloud):
   - Sign in with GitHub
   - Create new project
   - Select repository
   - Set entrypoint: `weather_server.py`
   - Add environment variables from `.env`
   - Deploy

3. Connect to Claude:
```json
{
  "mcpServers": {
    "weather": {
      "url": "https://your-project.fastmcp.app/mcp",
      "transport": "http"
    }
  }
}
```

## Usage Examples

### Get Weather for Multiple Cities
```
"What's the weather like in London? And what about Paris and Berlin?"
```

### Plan a Trip
```
"Compare the 5-day forecast for Rome, Barcelona, and Athens. Which has the best weather for tourism?"
```

### Check Air Quality
```
"Check the air quality in Beijing and let me know if it's safe for outdoor activities."
```

### Find a Location
```
"Find all cities named Springfield and show me their coordinates."
```

## API Limitations

- **Free Tier**: 1,000 API calls per day
- **Rate Limit**: 60 calls per minute
- **Forecast**: Maximum 5 days (3-hour intervals)
- **Caching**: 10-minute default TTL to minimize API usage

## Error Handling

The server handles common errors gracefully:
- Invalid location names
- API rate limiting
- Network timeouts
- Invalid API responses

All errors are logged and returned with descriptive messages.

## Development

### Testing
```bash
# Run test mode
python weather_server.py --test

# Test with FastMCP client
python -c "
import asyncio
from fastmcp import Client

async def test():
    async with Client('weather_server.py') as client:
        result = await client.call_tool(
            'get_current_weather',
            {'location': 'London'}
        )
        print(result.data)

asyncio.run(test())
"
```

### Logging
Enable debug logging by setting in `.env`:
```env
LOG_LEVEL=DEBUG
```

## License

MIT

## Support

- [OpenWeatherMap API Docs](https://openweathermap.org/api)
- [FastMCP Documentation](https://docs.fastmcp.com)
- [MCP Protocol](https://modelcontextprotocol.io)