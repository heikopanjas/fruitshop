# 🍎 Fruitshop - Apple Podcast Directory Scanner

A powerful command-line tool for scanning, collecting, and managing podcast metadata from the Apple Podcasts directory. Fruitshop systematically discovers podcasts across all categories and retrieves detailed information including names, IDs, and RSS feed URLs.

## 🚀 Features

- **🔍 Directory Scanning**: Automatically scan Apple Podcasts directory by categories, letters, and pagination
- **📊 Data Management**: Store and manage podcast metadata in SQL Server database
- **🔧 Batch Processing**: Resolve podcast details in configurable batches with rate limiting
- **📤 Export/Import**: Export data to CSV format and import from external sources
- **🔎 Search**: Find podcasts by name or other criteria
- **📈 Status Monitoring**: Track collection progress and database statistics
- **⚙️ Configuration**: Flexible configuration for storage, database, and user agent settings

## 🛠️ Installation

### Prerequisites

- .NET 7.0 Runtime
- SQL Server (for data storage)
- macOS/Linux/Windows

### Build from Source

```bash
# Clone the repository
git clone https://github.com/ultralove/fruitshop.git
cd fruitshop

# Build using the provided script
./build.sh

# Or build manually
dotnet build -c Release
dotnet publish -c Release -r osx-x64 --self-contained
```

The build script will create a self-contained executable in `~/.local/bin/fruitshop` by default.

## 📖 Usage

### Basic Commands

```bash
# Show help
fruitshop --help

# Show current database status
fruitshop status

# Scan Apple Podcasts directory for new podcasts
fruitshop scan

# Resolve metadata for discovered podcasts
fruitshop resolve --count 100

# Export podcast data to CSV
fruitshop export --output podcasts.csv

# Import podcast data from CSV
fruitshop import --input podcasts.csv

# Search for podcasts
fruitshop find "tech" "programming"
```

### Advanced Usage

#### Scanning Specific Categories

```bash
# Scan a specific Apple Podcasts category
fruitshop scan --source "https://podcasts.apple.com/de/genre/podcasts-technology/id1318"
```

#### Batch Resolution with Rate Limiting

```bash
# Resolve 500 podcasts in batches of 50 with 2-second delays
fruitshop resolve --count 500 --size 50 --delay 2000

# Resolve all pending podcasts
fruitshop resolve --all

# Skip retries for failed requests
fruitshop resolve --no-retries
```

#### Configuration Management

```bash
# View current configuration
fruitshop config --get

# Set custom storage path
fruitshop config --storagePath "/custom/path"

# Set custom user agent
fruitshop config --userAgent "MyBot/1.0"

# Export configuration to file
fruitshop config --dump
```

## 🗄️ Database Schema

Fruitshop uses SQL Server with the following main tables:

- **fruitshop_scans**: Tracks scanning sessions
- **fruitshop_collections**: Stores podcast metadata
- **fruitshop_collection_ids**: Manages podcast IDs and their status

### Database Setup

1. Create the database:

```sql
-- Run scripts/mssql-create-database.sql
```

2. Create the schema:

```sql
-- Run scripts/mssql-create-schema.sql
```

## 📁 Project Structure

```text
fruitshop/
├── Program.cs              # Main entry point
├── Commands/               # Command implementations
│   ├── ScanCommand.cs      # Directory scanning
│   ├── ResolveCommand.cs   # Metadata resolution
│   ├── StatusCommand.cs    # Status reporting
│   ├── FindCommand.cs      # Search functionality
│   ├── ExportCommand.cs    # CSV export
│   ├── ImportCommand.cs    # CSV import
│   └── ConfigCommand.cs    # Configuration management
├── Core/
│   ├── Collection.cs       # Podcast data model
│   ├── Scanner.cs          # Web scraping logic
│   ├── Resolver.cs         # iTunes API client
│   ├── Repository.cs       # Data access layer
│   └── Configuration.cs    # Settings management
├── Data/
│   ├── SqlServerReaderWriter.cs  # SQL Server operations
│   └── FileReaderWriter.cs       # File operations
├── scripts/               # Database scripts
└── build.sh              # Build automation
```

## 🔧 Configuration

Fruitshop stores configuration in `~/.config/fruitshop/config.json`:

```json
{
  "StoragePath": "~/.local/share/fruitshop",
  "DatabaseEngine": "SqlServer",
  "UserAgent": "fruitshop/1.0"
}
```

### Configuration Options

- **StoragePath**: Directory for local data storage
- **DatabaseEngine**: Database type (currently SqlServer)
- **UserAgent**: HTTP User-Agent for API requests

## 🚦 Workflow

1. **Scan**: Discover podcast IDs from Apple Podcasts directory
2. **Resolve**: Fetch detailed metadata using iTunes API
3. **Store**: Save data to SQL Server database
4. **Export**: Generate CSV reports for analysis
5. **Monitor**: Track progress and statistics

## 📊 Data Model

### Collection

```csharp
public class Collection
{
    public string CollectionId { get; set; }    // Apple Podcasts ID
    public string CollectionName { get; set; }  // Podcast title
    public string FeedUrl { get; set; }         // RSS feed URL
}
```

## 🔗 APIs Used

- **Apple Podcasts Directory**: Web scraping for podcast discovery
- **iTunes Search API**: Metadata resolution (`https://itunes.apple.com/lookup`)

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- Built with .NET 7.0 and C#
- Uses HtmlAgilityPack for web scraping
- McMaster.Extensions.CommandLineUtils for CLI
- Dapper for database operations
- FileHelpers for CSV processing

---

**Note**: This tool is for educational and research purposes. Please respect Apple's terms of service and implement appropriate rate limiting when using their services.
