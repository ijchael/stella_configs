# Stella Configuration System

This directory contains the configuration files for the Stella AI assistant. The configuration is split into multiple files for better maintainability and real-time updates.

## Directory Structure

```
stella_configs/
├── config.json           # Main config with API key and resource mappings
├── resources/           # Individual resource files
│   ├── ai-prompt.json
│   ├── goal-setting.json
│   ├── toddlers.json
│   └── ...
└── README.md
```

## Smart Resource Management

### Token Management
The system automatically manages GPT-4's token limit (4096 tokens):
- Counts tokens for all messages using tiktoken
- Includes estimated response tokens (2000)
- Dynamically adjusts resource inclusion when near limits
- Prioritizes most relevant resources when reducing content

### Resource Prioritization
Resources are selected and prioritized based on:
1. Keyword matches in the question
2. Number of matching keywords (higher = more relevant)
3. Token budget availability

Example:
```dart
Question: "How can I help my child achieve their goals at school?"
Matches:
- goalSettingResource: ["goal", "achieve"] = 2 matches
- schoolSuccess: ["school"] = 1 match
- childLearns: ["learn"] = 0 matches
Priority: goalSettingResource > schoolSuccess > childLearns
```

## Configuration Components

### 1. Resource Content (resources/*.json)
Individual resource files containing the actual content.

### 2. Resource Mappings (config.json)
Maps keywords to resources for dynamic content selection:
```json
{
  "resourceMappings": {
    "goalSettingResource": ["goal", "achieve"],
    "schoolSuccess": ["school", "study", "education"],
    // ... other mappings
  }
}
```

## How to Update

### Adding New Resources

1. Add your new resource in `lib/resources/stellains.dart`:
   ```dart
   const String newResource = '''
   Your new resource content here
   Using triple quotes for formatting
   ''';
   ```

2. Add keywords mapping in `stella_configs/config.json`:
   ```json
   {
     "resourceMappings": {
       "newResource": ["keyword1", "keyword2"]
     }
   }
   ```

3. Run the split script:
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

### Updating Resource Content

1. Edit content in `lib/resources/stellains.dart`
2. Run the split script:
   ```bash
   dart tools/split_config.dart
   ```
3. Push changes

### Updating Resource Mappings

1. Edit `stella_configs/config.json`:
   ```json
   {
     "resourceMappings": {
       "resourceName": ["new", "keywords", "here"]
     }
   }
   ```
2. Push changes

## How It Works

1. Question Processing:
   - User asks a question
   - System tokenizes the question
   - Matches keywords against mappings
   - Sorts resources by relevance

2. Token Management:
   - Counts tokens for system prompt
   - Counts tokens for user info
   - Counts tokens for each resource
   - Reserves tokens for response
   - Adjusts included resources if needed

3. Resource Selection:
   - Prioritizes resources with most keyword matches
   - Includes resources until token limit
   - Falls back to most relevant if over limit
   - Always includes system prompt and user info

4. Real-time Updates:
   - Resource content updates
   - Keyword mapping updates
   - No app release needed

## Best Practices

### Resource Content
- Keep content concise but informative
- Structure content for easy reading
- Consider token usage when writing

### Keyword Mapping
- Choose specific, relevant keywords
- Order keywords by importance
- Test with various questions
- Monitor token usage

### Token Management
- Monitor average token usage
- Keep resources focused
- Split large resources if needed
- Test with long conversations

## Troubleshooting

### Resource Selection Issues
1. Check keyword mappings
2. Verify token counts
3. Test with different questions
4. Monitor resource prioritization

### Token Limit Issues
1. Check resource sizes
2. Monitor conversation history
3. Consider splitting resources
4. Verify token counting

### Performance Optimization
1. Cache token counts
2. Prioritize essential resources
3. Monitor API response times
4. Test with various scenarios

## Security and Reliability
- Token-aware resource selection
- Graceful degradation under limits
- Fallback to essential content
- Automatic resource prioritization
