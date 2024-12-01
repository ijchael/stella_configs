# Stella Configuration System

This directory contains the configuration files for the Stella AI assistant. The configuration is split into multiple files for better maintainability and real-time updates.

## Directory Structure

```
stella_configs/
├── config.json           # Main config with API key
├── resources/           # Individual resource files
│   ├── aiPrompt.json
│   ├── goalSetting.json
│   ├── toddlers.json
│   └── ...
└── README.md
```

## How to Update

### Updating API Key

1. Run the encryption tool:
   ```bash
   dart tools/encrypt_config.dart
   ```
2. Enter your new API key when prompted
3. Push changes:
   ```bash
   git add config.json
   git commit -m "Update API key"
   git push origin main
   ```

### Updating Resources

1. Edit the content in `lib/resources/stellains.dart`
2. Run the split script:
   ```bash
   dart tools/split_config.dart
   ```
3. Push changes:
   ```bash
   git add resources/
   git commit -m "Update resources"
   git push origin main
   ```

## How It Works

1. Resources are maintained in `stellains.dart` using triple quotes (''') format for readability
2. The `split_config.dart` script converts these into separate JSON files
3. The app fetches these files from GitHub Pages
4. Changes take effect within 5 minutes (cache duration)
5. If GitHub is unreachable, falls back to local content

## Best Practices

1. Always edit content in `stellains.dart`
2. Run `split_config.dart` after any changes
3. Test changes locally before pushing
4. Use clear commit messages
5. Update one section at a time

## File Format

Each resource file follows this format:
```json
{
  "content": """
  Your resource content here
  Can span multiple lines
  Preserves formatting
  """
}
```

## Security

- API key is encrypted before storage
- Resources are versioned through Git
- Multiple backup levels (GitHub, cache, local)
- Automatic fallback if GitHub is unreachable
