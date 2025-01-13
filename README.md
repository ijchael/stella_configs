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

### 1. Resource Naming
The system uses two naming formats:
- camelCase in code (e.g., `jcCasagan`)
- kebab-case in UI and files (e.g., `jc-casagan`)

This conversion is handled automatically:
```dart
// In code (camelCase)
const String jcCasagan = '''
JC Casagan is Stella's developer.
''';

// In config.json (camelCase keys)
{
  "resourceMappings": {
    "goalSettingResource": ["goal", "achieve"],
    "toddlersResource": ["toddler", "child"]
  }
}
```

## How to Update

### Using the Configuration Editor (Recommended)

1. Open the configuration editor in the app
2. Add new resource mappings in the JSON editor
3. Save changes - new resources appear in the dropdown
4. Select the resource and add its content
5. Save again to update all files
6. Changes are immediately available to the AI

### Manual Updates (Alternative)

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

1. Use the configuration editor to modify content
2. Or edit content in `lib/resources/stellains.dart`
3. Save changes or run `dart tools/split_config.dart`
4. Push changes

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
- Use the configuration editor for easy updates
- Let the editor handle JSON formatting
- Don't manually edit JSON files

### Resource Organization
- Keep content focused and modular
- Use descriptive resource names
- Choose relevant keywords
- Test keywords with sample questions

### Token Efficiency
- Use token counter in the editor
- Keep resources concise
- Focus on essential content
- Consider token limits when writing

## Troubleshooting

### Configuration Editor Issues
1. Clear app cache if changes don't appear
2. Ensure valid JSON in resource mappings
3. Check for proper keyword formatting
4. Verify content saves successfully

### Resource Recognition Issues
1. Check resource mappings in config.json
2. Verify keywords match user questions
3. Clear caches if updates don't appear
4. Test with sample questions

### Cache Management
1. Changes should appear immediately
2. If not, try saving again
3. Check for error messages
4. Clear app cache manually if needed

### Performance Optimization
1. Keep resources focused
2. Use token counter
3. Test with various scenarios
4. Monitor resource usage

## Security and Reliability
- Proper JSON escaping
- Token-aware selection
- Automatic formatting
- Multiple backup levels
- Built-in configuration editor
- Automatic cache management
