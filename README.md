# Stella Configuration System

This directory contains the configuration files for the Stella AI assistant. The configuration is split into multiple files for better maintainability and real-time updates.

## Directory Structure

```
stella_configs/
├── config.json           # Main config with API key and resource mappings
├── resources/           # Individual resource files
│   ├── goal-setting-resource.json
│   ├── toddlers-resource.json
│   └── ...
└── README.md
```

## Resource Management

### 1. Source Files
Resources are maintained in `lib/resources/stellains.dart` using triple quotes for readability:
```dart
const String goalSettingResource = '''
Your content here
Multiple lines are fine
''';
```

### 2. JSON Generation
The split_config.dart script automatically:
- Converts triple-quoted content to proper JSON format
- Escapes special characters
- Handles newlines and quotes correctly
- Creates individual JSON files:
```json
{
  "content": "Your content here\nMultiple lines are fine"
}
```

### 3. Resource Mappings
Keywords are mapped to resources in config.json:
```json
{
  "resourceMappings": {
    "goalSettingResource": ["goal", "achieve"],
    "toddlersResource": ["toddler", "child"]
  }
}
```

## How to Update

### Adding New Resources

1. Add content in `lib/resources/stellains.dart`:
   ```dart
   const String newResource = '''
   Your new resource content here
   Using triple quotes for easy editing
   ''';
   ```

2. Add keywords in `stella_configs/config.json`:
   ```json
   {
     "resourceMappings": {
       "newResource": ["keyword1", "keyword2"]
     }
   }
   ```

3. Generate JSON files:
   ```bash
   dart tools/split_config.dart
   ```

4. Push changes:
   ```bash
   cd stella_configs
   git add .
   git commit -m "Add new resource"
   git push origin main
   ```

### Updating Existing Resources

1. Edit content in `lib/resources/stellains.dart`
2. Run `dart tools/split_config.dart`
3. Push changes

## Token Management

The system automatically:
- Counts tokens using tiktoken
- Manages GPT-4's 4096 token limit
- Prioritizes resources by relevance
- Adjusts content when near limits

### Token Optimization
1. System messages (prompt, user info)
2. Most relevant resources (by keyword matches)
3. Conversation history
4. Reserved space for response

## Best Practices

### Content Formatting
- Use triple quotes in stellains.dart
- Let split_config.dart handle JSON formatting
- Don't manually edit JSON files

### Resource Organization
- Keep content focused and modular
- Use descriptive resource names
- Choose relevant keywords

### Token Efficiency
- Keep resources concise
- Focus on essential content
- Consider token limits when writing

## Troubleshooting

### JSON Formatting Issues
1. Only edit in stellains.dart
2. Use triple quotes for content
3. Run split_config.dart to generate JSON
4. Never manually edit JSON files

### Resource Selection Issues
1. Check keyword mappings
2. Verify JSON formatting
3. Test with sample questions
4. Monitor token usage

### Performance Optimization
1. Keep resources focused
2. Monitor token counts
3. Test with various scenarios
4. Verify JSON parsing

## Security and Reliability
- Proper JSON escaping
- Token-aware selection
- Automatic formatting
- Multiple backup levels
