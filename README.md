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

### 1. Configuration Editor
The app includes a built-in configuration editor that provides:
- Real-time resource editing
- Automatic JSON formatting
- Dynamic resource dropdown
- Token counting for content

To add new resources:
1. Open the configuration editor
2. Add the resource to "Resource Mappings (JSON)":
   ```json
   {
     "resourceMappings": {
       "newResource": ["keyword1", "keyword2"]
     }
   }
   ```
3. Click Save - the new resource will appear in the dropdown
4. Select it from the dropdown to add its content

### 2. Source Files
Resources are maintained in `lib/resources/stellains.dart` using triple quotes for readability:
```dart
const String goalSettingResource = '''
Your content here
Multiple lines are fine
''';
```

### 3. JSON Generation
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

### 4. Resource Mappings
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

### Using the Configuration Editor (Recommended)

1. Open the configuration editor in the app
2. Add new resource mappings in the JSON editor
3. Save changes - new resources appear in the dropdown
4. Select the resource and add its content
5. Save again to update all files

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

### JSON Formatting Issues
1. Use the configuration editor
2. Don't manually edit JSON files
3. Let the editor handle formatting
4. Run split_config.dart if needed

### Resource Selection Issues
1. Check keyword mappings
2. Verify JSON formatting
3. Test with sample questions
4. Monitor token usage

### Performance Optimization
1. Keep resources focused
2. Use token counter
3. Test with various scenarios
4. Verify JSON parsing

## Security and Reliability
- Proper JSON escaping
- Token-aware selection
- Automatic formatting
- Multiple backup levels
- Built-in configuration editor
